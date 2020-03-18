---
title: NamedPipe アクティベーション
ms.date: 03/30/2017
ms.assetid: f3c0437d-006c-442e-bfb0-6b29216e4e29
ms.openlocfilehash: a562ec51d35af08f49e89b652670e9a57b0f00c2
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837858"
---
# <a name="namedpipe-activation"></a>NamedPipe アクティベーション

このサンプルでは、名前付きパイプを介して通信するサービスをアクティブ化するために、Windows プロセス アクティブ化サービス (WAS: Windows Process Activation Service) を使用してサービスをホストする方法を示します。 このサンプルは[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいており、Windows Vista を実行する必要があります。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WASHost\NamedPipeActivation`

## <a name="sample-details"></a>サンプルの詳細

このサンプルは、クライアント コンソール プログラム (.exe) と、Windows プロセス アクティブ化サービス (WAS) によってアクティブ化されるワーカー プロセス内でホストされるサービス ライブラリ (.dll) で構成されています。 クライアント アクティビティは、コンソール ウィンドウに表示されます。

サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 コントラクトは `ICalculator` インターフェイスによって定義されており、算術演算 (Add、Subtract、Multiply、および Divide) を公開しています。次のサンプル コードを参照してください。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

クライアントは指定された算術演算を同期要求し、サービスの実装は計算を行って結果を返します。

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

このサンプルでは、セキュリティ保護を行わないように変更された `netNamedPipeBinding` バインディングを使用します。 バインディングは、クライアントとサービスの構成ファイルに指定されます。 サービスのバインディングの種類は、エンドポイント要素の `binding` 属性に指定されます。次のサンプル構成を参照してください。

セキュリティ保護された名前付きパイプ バインディングを使用する場合は、サーバーのセキュリティ モードを必要なセキュリティ設定に変更し、クライアントで svcutil.exe を再実行して更新されたクライアントの構成ファイルを取得します。

```xml
<system.serviceModel>
        <services>
            <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">

        <!-- This endpoint is exposed at the base address provided by host: net.pipe://localhost/servicemodelsamples/service.svc  -->
        <endpoint address=""
                  binding="netNamedPipeBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- the mex endpoint is exposed at net.pipe://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexNamedPipeBinding"
                  contract="IMetadataExchange" />
      </service>
        </services>
        <bindings>
            <netNamedPipeBinding>
                <binding name="Binding1" >
                    <security mode = "None">
                    </security>
                </binding >
            </netNamedPipeBinding>
        </bindings>

    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

クライアントのエンドポイント情報が構成されます。次のサンプル コードを参照してください。

```xml
<system.serviceModel>

    <client>
      <endpoint name=""
                          address="net.pipe://localhost/servicemodelsamples/service.svc"
                          binding="netNamedPipeBinding"
                          bindingConfiguration="Binding1"
                          contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </client>

    <bindings>

      <!--  Following is the expanded configuration section for a NetNamedPipeBinding.
            Each property is configured with the default value. -->

      <netNamedPipeBinding>
        <binding name="Binding1"
                         maxBufferSize="65536"
                         maxConnections="10">
          <security mode = "None">
          </security>
        </binding >

      </netNamedPipeBinding>
    </bindings>

  </system.serviceModel>
```

このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. IIS 7.0 がインストールされていることを確認します。 WAS のアクティブ化には IIS 7.0 が必要です。

2. [Windows Communication Foundation のサンプルに対して1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

    さらに、WCF 非 HTTP アクティブ化コンポーネントをインストールする必要があります。

    1. **[スタート]** メニューの **[コントロール パネル]** をクリックします。

    2. **[プログラムと機能]** を選択します。

    3. [ **Windows コンポーネントの有効化または無効化] を**クリックします。

    4. **[Microsoft .NET Framework 3.0]** ノードを展開し、 **[WINDOWS COMMUNICATION FOUNDATION の非 HTTP アクティブ化]** 機能をオンにします。

3. 名前付きパイプのアクティブ化をサポートするように Windows プロセス アクティブ化サービス (WAS) を構成します。

    便宜上、次の 2 つの手順が、サンプル ディレクトリにある AddNetPipeSiteBinding.cmd というバッチ ファイルに実装されています。

    1. net.pipe のアクティブ化をサポートするには、既定の Web サイトをあらかじめ net.pipe プロトコルにバインドしておく必要があります。 これは、IIS7.0 管理ツール セットと共にインストールされる appcmd.exe を使用して行います。 権限のレベルが高い (管理者の) コマンド プロンプトで、次のコマンドを実行します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

        このコマンドによって、既定の Web サイトに net.pipe サイト バインディングを追加します。

    2. サイト内のすべてのアプリケーションが同じ net.pipe バインディングを共有しますが、net.pipe サポートの有効化はアプリケーションごとに指定できます。 /servicemodelsamples アプリケーションで net.pipe を有効にするには、権限のレベルが高いコマンド プロンプトから、次のコマンドを実行します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.pipe
        ```

        > [!NOTE]
        > このコマンドはテキスト 1 行です。

        このコマンドにより、`http://localhost/servicemodelsamples` と `net.tcp://localhost/servicemodelsamples`の両方を使用して/servicemodelsamples アプリケーションにアクセスできるようになります。

4. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

5. このサンプル用に追加した net.pipe サイト バインディングを削除します。

    便宜上、次の 2 つの手順が、サンプル ディレクトリにある RemoveNetPipeSiteBinding.cmd というバッチ ファイルに実装されています。

    1. 権限のレベルが高いコマンド プロンプトから次のコマンドを実行して、有効なプロトコルの一覧から net.tcp を削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > このコマンドは、全体で 1 行のテキストになるように入力する必要があります。

    2. 権限のレベルが高いコマンド プロンプトから次のコマンドを実行して、net.tcp サイト バインディングを削除します。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > このコマンドは、全体で 1 行のテキストになるように入力する必要があります。

## <a name="see-also"></a>参照

- [AppFabric のホスティングと永続化のサンプル](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
