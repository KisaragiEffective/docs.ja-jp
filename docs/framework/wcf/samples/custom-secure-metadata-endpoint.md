---
title: カスタム セキュア メタデータ エンドポイント
ms.date: 03/30/2017
ms.assetid: 9e369e99-ea4a-49ff-aed2-9fdf61091a48
ms.openlocfilehash: 92fa468caf331fadcd6cab0ab57b34858053c1b5
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715447"
---
# <a name="custom-secure-metadata-endpoint"></a>カスタム セキュア メタデータ エンドポイント
このサンプルでは、メタデータ以外の交換バインディングの1つを使用するセキュリティで保護されたメタデータエンドポイントを使用してサービスを実装する方法と、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)またはクライアントを構成して、そのようなメタデータエンドポイントからメタデータをフェッチする方法を示します。 メタデータ エンドポイントを公開する場合に使用できるシステム指定のバインディングには、mexHttpBinding と mexHttpsBinding の 2 つがあります。 mexHttpBinding は、メタデータ エンドポイントをセキュリティ保護されない HTTP を介して公開する場合に使用します。 mexHttpsBinding は、メタデータ エンドポイントをセキュリティ保護される HTTPS を介して公開する場合に使用します。 このサンプルでは、<xref:System.ServiceModel.WSHttpBinding> を使用してセキュア メタデータ エンドポイントを公開する方法を示します。 この方法は、HTTPS を使用せずにバインディングのセキュリティ設定を変更する場合に使用します。 mexHttpsBinding を使用すると、メタデータ エンドポイントがセキュリティ保護されますが、バインディング設定を変更できなくなります。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
## <a name="service"></a>サービス  
 このサンプルのサービスには 2 つのエンドポイントがあります。 アプリケーション エンドポイントは、`ICalculator` が有効で証明書による `WSHttpBinding` セキュリティを使用する `ReliableSession` に、`Message` コントラクトを提供します。 メタデータ エンドポイントでも、同じセキュリティが設定されている `WSHttpBinding` を使用しますが、`ReliableSession` は無効です。 関連する構成は次のとおりです。  
  
```xml  
<services>   
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
     <!-- use base address provided by host -->  
     <endpoint address=""  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding2"  
       contract="Microsoft.ServiceModel.Samples.ICalculator" />  
     <endpoint address="mex"  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding1"  
       contract="IMetadataExchange" />  
     </service>  
 </services>  
 <bindings>  
   <wsHttpBinding>  
     <binding name="Binding1">  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
     <binding name="Binding2">  
       <reliableSession inactivityTimeout="00:01:00" enabled="true" />  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
   </wsHttpBinding>  
 </bindings>  
```  
  
 他の多くのサンプルでは、メタデータ エンドポイントには、セキュリティ保護されていない既定の `mexHttpBinding` が使用されます。 このサンプルのメタデータは、`WSHttpBinding` セキュリティが設定されている `Message` を使用してセキュリティ保護されます。 メタデータ クライアントがこのメタデータを取得するには、照合バインディングを使用して構成する必要があります。 このサンプルでは、こうした 2 つのクライアントを示します。  
  
 最初のクライアントでは Svcutil.exe を使用し、デザイン時にメタデータを取得してクライアント コードと構成を生成します。 サービスはメタデータの既定以外のバインディングを使用するので、Svcutil.exe ツールはこのバインディングを使用してサービスからメタデータを取得できるよう、特に構成される必要があります。  
  
 2 番目のクライアントでは `MetadataResolver` を使用し、既知のコントラクトのメタデータを動的にフェッチして、動的に生成されたクライアントでの操作を呼び出します。  
  
## <a name="svcutil-client"></a>Svcutil クライアント  
 既定のバインディングを使用して `IMetadataExchange` エンドポイントをホストする場合、このエンドポイントのアドレスを使用して次のように Svcutil.exe を実行できます。  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 これでツールが動作します。 ただしこのサンプルのサーバーでは、メタデータをホストするときに既定以外のエンドポイントを使用しています。 そのため、正しいバインディングが使用されるように Svcutil.exe を指定する必要があります。 これは、Svcutil.exe.config ファイルを使用して実行できます。  
  
 Svcutil.exe.config ファイルは、通常のクライアント構成ファイルに似ています。 異なる点は、次に示すようにクライアントのエンドポイント名とエンドポイント コントラクトだけです。  
  
```xml  
<endpoint name="http"  
          binding="wsHttpBinding"  
          bindingConfiguration="Binding1"  
          behaviorConfiguration="ClientCertificateBehavior"  
          contract="IMetadataExchange" />  
```  
  
 エンドポイント名は、メタデータがホストされるアドレスのスキーマの名前である必要があります。また、エンドポイント コントラクトは `IMetadataExchange` であることが必要です。 したがって、Svcutil.exe をコマンド ラインで実行する場合は次のようになります。  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 ここでは、"http" という名前のエンドポイントと、このメタデータ エンドポイントとの通信交換のバインディングと動作を構成するコントラクト `IMetadataExchange` が検索されます。 このサンプルでは、Svcutil.exe.config ファイルの残りの部分でバインド構成と動作の資格情報を指定して、サーバーのメタデータ エンドポイントの構成と照合します。  
  
 Svcutil.exe が Svcutil.exe.config の構成を使用するには、Svcutil.exe がこの構成ファイルと同じディレクトリにある必要があります。 したがって、Svcutil.exe をインストール場所から Svcutil.exe.config ファイルが含まれるディレクトリにコピーする必要があります。 その後、そのディレクトリで次のコマンドを実行します。  
  
```console  
.\svcutil.exe http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 先頭の ".\\"は、このディレクトリ (対応する Svcutil.exe を持つ Svcutil.exe) の Svcutil.exe のコピーが実行されることを保証します。  
  
## <a name="metadataresolver-client"></a>MetadataResolver クライアント  
 クライアントで、コントラクトおよびメタデータと対話する方法がデザイン時に認識されている場合、クライアントは `MetadataResolver` を使用してアプリケーション エンドポイントのバインディングとアドレスを動的に検索することができます。 その例として、このクライアントのサンプルでは、`MetadataResolver` を作成して構成することにより、`MetadataExchangeClient` で使用されるバインディングと資格情報を構成する方法を示します。  
  
 Svcutil.exe.config には、同じバインディングと資格情報についての情報が示されており、`MetadataExchangeClient` でこれを次のように強制的に使用できます。  
  
```csharp  
// Specify the Metadata Exchange binding and its security mode  
WSHttpBinding mexBinding = new WSHttpBinding(SecurityMode.Message);  
mexBinding.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a MetadataExchangeClient for retrieving metadata, and set the // certificate details  
MetadataExchangeClient mexClient = new MetadataExchangeClient(mexBinding);  
mexClient.SoapCredentials.ClientCertificate.SetCertificate(    StoreLocation.CurrentUser, StoreName.My,  
    X509FindType.FindBySubjectName, "client.com");  
mexClient.SoapCredentials.ServiceCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.PeerOrChainTrust;  
mexClient.SoapCredentials.ServiceCertificate.SetDefaultCertificate(    StoreLocation.CurrentUser, StoreName.TrustedPeople,  
    X509FindType.FindBySubjectName, "localhost");  
```  
  
 `mexClient` が構成されている場合、次のように必要なコントラクトを列挙し、`MetadataResolver` を使用してこれらのコントラクトが含まれるエンドポイントの一覧をフェッチできます。  
  
```csharp  
// The contract we want to fetch metadata for  
Collection<ContractDescription> contracts = new Collection<ContractDescription>();  
ContractDescription contract = ContractDescription.GetContract(typeof(ICalculator));  
contracts.Add(contract);  
// Find endpoints for that contract  
EndpointAddress mexAddress = new EndpointAddress(ConfigurationManager.AppSettings["mexAddress"]);  
ServiceEndpointCollection endpoints = MetadataResolver.Resolve(contracts, mexAddress, mexClient);  
```  
  
 最後に、こうしたエンドポイントからの情報を使用して、アプリケーション エンドポイントと通信するチャネルの作成に使用される、`ChannelFactory` のバインディングとアドレスを初期化できます。  
  
```csharp  
ChannelFactory<ICalculator> cf = new ChannelFactory<ICalculator>(endpoint.Binding, endpoint.Address);  
```  
  
 このクライアント サンプルで重要な点は、`MetadataResolver` を使用しているときにメタデータ交換通信用のカスタム バインドまたはカスタム動作を指定する必要がある場合に、`MetadataExchangeClient` を使用すればこうしたカスタム設定を指定できるということです。  
  
#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
#### <a name="to-run-the-sample-on-the-same-machine"></a>サンプルを同じコンピューターで実行するには  
  
1. Setup.bat をサンプルのインストール フォルダーで実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。 FindPrivateKey ツールを使用することに注意してください。このツールは、 [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)から setupcerttool を実行してインストールされます。  
  
2. \MetadataResolverClient\bin または \SvcutilClient\bin でクライアント アプリケーションを実行します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
3. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
4. サンプルの使用が終わったら、Cleanup.bat を実行して証明書を削除してください。 他のセキュリティ サンプルでも同じ証明書を使用します。  
  
#### <a name="to-run-the-sample-across-machines"></a>サンプルを複数コンピューターで実行するには  
  
1. サーバーで `setup.bat service` を実行します。 `service` 引数を指定して `setup.bat` を実行すると、コンピューターの完全修飾ドメイン名を使用してサービス証明書が作成され、service .cer という名前のファイルにエクスポートされます。  
  
2. サーバーで、Web.config を編集して新しい証明書の名前を反映します。 つまり、 [\<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md)要素の `findValue` 属性をコンピューターの完全修飾ドメイン名に変更します。  
  
3. Service.cer ファイルを、サービス ディレクトリからクライアント コンピューターのクライアント ディレクトリにコピーします。  
  
4. クライアントで `setup.bat client` を実行します。 `client` 引数を指定して `setup.bat` を実行すると、Client.com という名前のクライアント証明書が作成され、クライアント証明書がクライアント .cer という名前のファイルにエクスポートされます。  
  
5. クライアント コンピューターにある `MetadataResolverClient` の App.config ファイルで、MEX エンドポイントのアドレス値をサービスの新しいアドレスに合わせて変更します。 そのためには、localhost をサーバーの完全修飾ドメイン名に置き換えます。 さらに、metadataResolverClient.cs ファイルに記述されている "localhost" を、新しいサービス証明書の名前 (サーバーの完全修飾ドメイン名) に変更します。 SvcutilClient プロジェクトの App.config に対して、同様の手順を行います。  
  
6. Client.cer ファイルを、クライアント ディレクトリからサーバーのサービス ディレクトリにコピーします。  
  
7. クライアントで `ImportServiceCert.bat` を実行します。 これにより、サービス証明書が Service.cer ファイルから CurrentUser - TrustedPeople ストアにインポートされます。  
  
8. サーバーで `ImportClientCert.bat` を実行します。これにより、クライアント証明書が Client.cer ファイルから LocalMachine - TrustedPeople ストアにインポートされます。  
  
9. サービス コンピューターで、Visual Studio でサービス プロジェクトをビルドし、それが実行されていることを Web ブラウザーでヘルプ ページを選択することにより検証します。  
  
10. クライアント コンピューター上の VS で、MetadataResolverClient または SvcutilClient を実行します。  
  
    1. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
- サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
  
    > [!NOTE]
    > このサンプルを別のマシンで実行している場合、このスクリプトはサービス証明書をクライアントから削除しません。 コンピューター間で証明書を使用する Windows Communication Foundation (WCF) サンプルを実行した場合は、CurrentUser-TrustedPeople ストアにインストールされているサービス証明書を必ずオフにしてください。 削除するには、コマンド `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` を実行します。 たとえば、`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` のように指定します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\CustomMexEndpoint`  
