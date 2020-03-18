---
title: カスタム サービス ホスト
ms.date: 03/30/2017
ms.assetid: fe16ff50-7156-4499-9c32-13d8a79dc100
ms.openlocfilehash: 271233015739024428a7a29815f66278c9d7aa04
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789928"
---
# <a name="custom-service-host"></a>カスタム サービス ホスト
このサンプルでは、<xref:System.ServiceModel.ServiceHost> クラスから派生したカスタムのサービス ホストを使用して、サービスの実行時動作を変更する方法を示します。 この方法は、多数のサービスを共通方式で構成するという方法の代わりに使用でき、再利用可能です。 このサンプルでは、<xref:System.ServiceModel.Activation.ServiceHostFactory> クラスを使用して、カスタムの ServiceHost を、インターネット インフォメーション サービス (IIS) または Windows プロセス アクティブ化サービス (WAS) でホストされる環境で使用する方法も示します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Hosting\CustomServiceHost`  
  
## <a name="about-the-scenario"></a>シナリオについて
 機密性の高いサービスメタデータが誤って公開されるのを防ぐために、Windows Communication Foundation (WCF) サービスの既定の構成では、メタデータの公開が無効になっています。 この動作は、既定の設定ではセキュリティで保護されますが、同時に、サービスの構成の中でメタデータ発行の動作が明示的に有効化されない限り、サービスの呼び出しに必要なクライアント コードをメタデータ インポート ツール (Svcutil.exe など) を使用して生成できないことも意味します。  
  
 多数のサービスのメタデータ公開を有効にすると、同一の構成要素が各サービスに追加されます。この結果、本質的に同じ構成情報が大量に発生することになります。 各サービスを個別に構成する代わりに、メタデータ公開を有効化する命令型コードを一度だけプログラミングして、そのコードを複数のサービスで再利用するという方法もあります。 この方法を使用するには、<xref:System.ServiceModel.ServiceHost> から派生する新しいクラスを作成し、`ApplyConfiguration`() メソッドをオーバーライドして命令型コードでメタデータ公開動作を追加します。  
  
> [!IMPORTANT]
> このサンプルでは、わかりやすくするために、セキュリティで保護されていないメタデータ公開エンドポイントを作成する方法を示します。 このようなエンドポイントを利用するコンシューマーは、匿名で、認証を受けていない可能性もあります。したがって、エンドポイントを配置する前には注意を払い、サービスのメタデータをパブリックに開示することが適切であることを確認する必要があります。  
  
## <a name="implementing-a-custom-servicehost"></a>カスタム ServiceHost の実装
 <xref:System.ServiceModel.ServiceHost> クラスは、便利な仮想メソッドを多数公開しています。継承側でこの仮想メソッドをオーバーライドして、サービスの実行時動作を変更することができます。 たとえば、`ApplyConfiguration`() メソッドはサービスの構成情報を構成ストアから読み取り、それに応じてホストの <xref:System.ServiceModel.Description.ServiceDescription> を変更します。 既定の実装は構成をアプリケーションの構成ファイルから読み取りますが、 カスタム実装では、`ApplyConfiguration`() をオーバーライドすれば、命令型コードを使用して <xref:System.ServiceModel.Description.ServiceDescription> にさらに変更を加えることや、既定の構成ストア全体を置き換えることも可能です ( たとえば、サービスのエンドポイント構成を、アプリケーションの構成ファイルではなくデータベースから読み取るようにする場合)。  
  
 このサンプルでは、ServiceMetadataBehavior (メタデータ公開を有効にする動作) がサービスの構成ファイルに明示的に追加されていない場合でもこの動作を追加する、カスタムの ServiceHost を構築します。 これを実現するには、<xref:System.ServiceModel.ServiceHost> から継承する新しいクラスを作成して `ApplyConfiguration`() をオーバーライドします。  
  
```csharp
class SelfDescribingServiceHost : ServiceHost  
{  
    public SelfDescribingServiceHost(Type serviceType, params Uri[] baseAddresses)  
        : base(serviceType, baseAddresses) { }  
  
    //Overriding ApplyConfiguration() allows us to   
    //alter the ServiceDescription prior to opening  
    //the service host.   
    protected override void ApplyConfiguration()  
    {  
        //First, we call base.ApplyConfiguration()  
        //to read any configuration that was provided for  
        //the service we're hosting. After this call,  
        //this.Description describes the service  
        //as it was configured.  
        base.ApplyConfiguration();       
  
        //(rest of implementation elided for clarity)  
    }  
}  
```  
  
 アプリケーションの構成ファイルで指定されている構成が無視されないようにしたいので、`ApplyConfiguration`() のオーバーライドで最初に行うことは、基本実装の呼び出しです。 このメソッドが完了した後で、次の命令型コードを使用して、サービス記述に <xref:System.ServiceModel.Description.ServiceMetadataBehavior> を追加します。  
  
```csharp
ServiceMetadataBehavior mexBehavior = this.Description.Behaviors.Find<ServiceMetadataBehavior>();  
if (mexBehavior == null)  
{  
    mexBehavior = new ServiceMetadataBehavior();  
    this.Description.Behaviors.Add(mexBehavior);  
}  
else  
{  
    //Metadata behavior has already been configured,   
    //so we do not have any work to do.  
    return;  
}  
```  
  
 `ApplyConfiguration`() のオーバーライドでの最後の処理は、既定のメタデータ エンドポイントの追加です。 通常は、サービス ホストの BaseAddresses コレクション内の各 URI に対して 1 つずつメタデータ エンドポイントを作成します。  
  
```csharp
//Add a metadata endpoint at each base address  
//using the "/mex" addressing convention  
foreach (Uri baseAddress in this.BaseAddresses)  
{  
    if (baseAddress.Scheme == Uri.UriSchemeHttp)  
    {  
        mexBehavior.HttpGetEnabled = true;  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexHttpBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeHttps)  
    {  
        mexBehavior.HttpsGetEnabled = true;  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexHttpsBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeNetPipe)  
    {  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexNamedPipeBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeNetTcp)  
    {  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexTcpBinding(),  
                                "mex");  
    }  
}  
```  
  
## <a name="using-a-custom-servicehost-in-self-host"></a>自己ホストでのカスタム ServiceHost の使用  
 カスタム ServiceHost の実装が完成したので、これを使用して任意のサービスにメタデータ公開動作を追加できるようになりました。追加するには、`SelfDescribingServiceHost` のインスタンス内でサービスをホストします。 カスタム ServiceHost を自己ホストのシナリオで使用する方法を次のコードに示します。  
  
```csharp
SelfDescribingServiceHost host =   
         new SelfDescribingServiceHost( typeof( Calculator ) );  
host.Open();  
```  
  
 既定の <xref:System.ServiceModel.ServiceHost> クラスを使用してサービスをホストする場合と同様に、このカスタム ServiceHost も、サービスのエンドポイント構成をアプリケーションの構成ファイルから読み取ります。 ただし、カスタム ホストの内部でメタデータ公開を有効にするというロジックを追加したので、構成の中でメタデータ公開動作を明示的に有効化することは不要になりました。 この方法のメリットがはっきりと現れるのは、開発するアプリケーションに複数のサービスがあり、同じ構成要素を繰り返し記述することなく各サービスでメタデータ公開を有効化できるようにする場合です。  
  
## <a name="using-a-custom-servicehost-in-iis-or-was"></a>IIS または WAS でのカスタム ServiceHost の使用  
 カスタム サービス ホストを自己ホストのシナリオで使用することは簡単です。サービス ホストのインスタンスを作成して開くことは、アプリケーションのコードで行うからです。 ただし、IIS または WAS のホスト環境では、WCF インフラストラクチャは、受信メッセージに応答してサービスのホストを動的にインスタンス化します。 このホスト環境でもカスタム サービス ホストを使用できますが、ServiceHostFactory の形式でコードを追加する必要があります。 次に示すコードは、カスタム <xref:System.ServiceModel.Activation.ServiceHostFactory> のインスタンスを返す、`SelfDescribingServiceHost` の派生クラスの例です。  
  
```csharp
public class SelfDescribingServiceHostFactory : ServiceHostFactory  
{  
    protected override ServiceHost CreateServiceHost(Type serviceType,   
     Uri[] baseAddresses)  
    {  
        //All the custom factory does is return a new instance  
        //of our custom host class. The bulk of the custom logic should  
        //live in the custom host (as opposed to the factory)   
        //for maximum  
        //reuse value outside of the IIS/WAS hosting environment.  
        return new SelfDescribingServiceHost(serviceType,     
                                             baseAddresses);  
    }  
}  
```  
  
 上のコードでわかるように、カスタム ServiceHostFactory の実装は非常に簡単です。 すべてのカスタム ロジックは ServiceHost 実装内部にあり、このファクトリによって派生クラスのインスタンスが返されます。  
  
 サービス実装と共にカスタム ファクトリを使用するには、いくつかのメタデータをサービスの .svc ファイルに追加する必要があります。  
  
```xml
<%@ServiceHost Service="Microsoft.ServiceModel.Samples.CalculatorService"
               Factory="Microsoft.ServiceModel.Samples.SelfDescribingServiceHostFactory"
               language=c# Debug="true" %>
```
  
 ここでは、`Factory` 属性を `@ServiceHost` ディレクティブに追加し、カスタム ファクトリの CLR 型の名前を属性値として渡しました。 IIS または WAS がこのサービスのメッセージを受信すると、WCF ホスティングインフラストラクチャはまず ServiceHostFactory のインスタンスを作成し、次に `ServiceHostFactory.CreateServiceHost()`を呼び出してサービスホスト自体をインスタンス化します。  
  
## <a name="running-the-sample"></a>サンプルの実行  
 このサンプルには完全な機能を持つクライアントとサービスの実装が用意されていますが、ここでの目的は、カスタム ホストを使用してサービスのランタイム動作を変更する方法を示すことです。次の手順を実行します。  
  
### <a name="observe-the-effect-of-the-custom-host"></a>カスタムホストの効果を確認する
  
1. サービスの Web.config ファイルを開き、サービスのメタデータを明示的に有効にする構成がないことを確認します。  
  
2. サービスの .svc ファイルを開き、@ServiceHost ディレクティブに、カスタム ServiceHostFactory の名前を指定するファクトリ属性が含まれていることを確認します。  
  
### <a name="set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行する
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](building-the-samples.md)」の手順に従います。

3. ソリューションがビルドされたら、Setup.exe を実行して IIS 7.0 で ServiceModelSamples アプリケーションを設定します。 これで、ServiceModelSamples ディレクトリが IIS 7.0 アプリケーションとして表示されます。

4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](running-the-samples.md)」の手順に従います。

5. IIS 7.0 アプリケーションを削除するには、*クリーンアップ*を実行します。

## <a name="see-also"></a>関連項目

- [方法 : IIS で WCF サービスをホストする](../feature-details/how-to-host-a-wcf-service-in-iis.md)
