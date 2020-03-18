---
title: WSStreamedHttpBinding
ms.date: 03/30/2017
ms.assetid: 97ce4d3d-ca6f-45fa-b33b-2429bb84e65b
ms.openlocfilehash: 36b4db5270205aec55ad52db40500c8d434a1560
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714548"
---
# <a name="wsstreamedhttpbinding"></a>WSStreamedHttpBinding

このサンプルでは、HTTP トランスポート使用時にストリーミングをサポートする目的でデザインされたバインディングを作成する方法を示します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Binding\WSStreamedHttpBinding`

 新しい標準バインディングを作成して構成する手順は、次のとおりです。

1. 新しい標準バインディングを作成する

    BasicHttpBinding、netTcpBinding などの Windows Communication Foundation (WCF) の標準バインディングでは、特定の要件に応じて基になるトランスポートとチャネルスタックを構成します。 このサンプルでは、`WSStreamedHttpBinding` は、ストリーミングをサポートするようチャネル スタックを構成します。 既定では、WS-Security と信頼できるメッセージ機能はチャネル スタックに追加されません。どちらの機能もストリーミングではサポートされないためです。 新しいバインディングは、`WSStreamedHttpBinding` から派生するクラス <xref:System.ServiceModel.Channels.Binding> に実装されます。 `WSStreamedHttpBinding` には、<xref:System.ServiceModel.Channels.HttpTransportBindingElement>、<xref:System.ServiceModel.Channels.HttpsTransportBindingElement>、<xref:System.ServiceModel.Channels.TransactionFlowBindingElement>、および <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> のバインド要素が含まれます。 このクラスは、作成されるバインディング スタックを構成する `CreateBindingElements()` メソッドを提供します。次のサンプル コードを参照してください。

    ```csharp
    public override BindingElementCollection CreateBindingElements()
    {
        // return collection of BindingElements
        BindingElementCollection bindingElements = new BindingElementCollection();

        // the order of binding elements within the collection is important: layered channels are applied in the order included, followed by
        // the message encoder, and finally the transport at the end
        if (flowTransactions)
        {
            bindingElements.Add(transactionFlow);
        }
        bindingElements.Add(textEncoding);

        // add transport (http or https)
        bindingElements.Add(transport);
        return bindingElements.Clone();
    }
    ```

2. 構成サポートを追加する

    構成を使用してトランスポートを公開するため、サンプルではさらに、`WSStreamedHttpBindingConfigurationElement` クラスと `WSStreamedHttpBindingSection` クラスという 2 つのクラスを実装します。 クラス `WSStreamedHttpBindingSection` は、WCF 構成システムに `WSStreamedHttpBinding` を公開する <xref:System.ServiceModel.Configuration.StandardBindingCollectionElement%602> です。 実装の大部分は `WSStreamedHttpBindingConfigurationElement` で代行されます。これは <xref:System.ServiceModel.Configuration.StandardBindingElement> の派生です。 クラス `WSStreamedHttpBindingConfigurationElement` には `WSStreamedHttpBinding` のプロパティに対応するプロパティがあり、各構成要素をバインディングにマップする関数があります。

    次のセクションをサービスの構成ファイルに追加することにより、ハンドラを構成システムに登録します。

    ```xml
    <configuration>
      <system.serviceModel>
        <extensions>
          <bindingExtensions>
            <add name="wsStreamedHttpBinding" type="Microsoft.ServiceModel.Samples.WSStreamedHttpBindingCollectionElement, WSStreamedHttpBinding, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null" />
          </bindingExtensions>
        </extensions>
      </system.serviceModel>
    </configuration>
    ```

    ハンドラーは serviceModel 構成セクションから参照できます。

    ```xml
    <configuration>
      <system.serviceModel>
        <client>
          <endpoint
                    address="https://localhost/servicemodelsamples/service.svc"
                    bindingConfiguration="Binding"
                    binding="wsStreamedHttpBinding"
                    contract="Microsoft.ServiceModel.Samples.IStreamedEchoService"/>
        </client>
      </system.serviceModel>
    </configuration>
    ```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. [Windows Communication Foundation のサンプルについ](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)ては、1回限りのセットアップ手順に記載されている手順を実行したことを確認します。

3. [インターネットインフォメーションサービス (IIS) サーバー証明書のインストール手順](../../../../docs/framework/wcf/samples/iis-server-certificate-installation-instructions.md)を実行していることを確認します。

4. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

5. 複数コンピューター構成でサンプルを実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

6. クライアント ウィンドウが表示されたら、「Sample.txt」と入力します。 ディレクトリで "Sample.txt のコピー" を検索する必要があります。

## <a name="the-wsstreamedhttpbinding-sample-service"></a>WSStreamedHttpBinding サービスのサンプル

`WSStreamedHttpBinding` を使用するサービスのサンプルは、サービス サブディレクトリにあります。 `OperationContract` の実装は `MemoryStream` を使用して、最初に受信ストリームからすべてのデータを取得し、その後 `MemoryStream` を返します。 このサンプル サービスは、インターネット インフォメーション サービス (IIS) によってホストされます。

```csharp
[ServiceContract]
public interface IStreamedEchoService
{
    [OperationContract]
    Stream Echo(Stream data);
}

public class StreamedEchoService : IStreamedEchoService
{
    public Stream Echo(Stream data)
    {
        MemoryStream dataStorage = new MemoryStream();
        byte[] byteArray = new byte[8192];
        int bytesRead = data.Read(byteArray, 0, 8192);
        while (bytesRead > 0)
        {
            dataStorage.Write(byteArray, 0, bytesRead);
            bytesRead = data.Read(byteArray, 0, 8192);
        }
        data.Close();
        dataStorage.Seek(0, SeekOrigin.Begin);

        return dataStorage;
    }
}
```

## <a name="the-wsstreamedhttpbinding-sample-client"></a>WSStreamedHttpBinding クライアントのサンプル

`WSStreamedHttpBinding` を使用してサービスとやり取りするクライアントは、クライアント サブディレクトリにあります。 このサンプルで使用される証明書は、Makecert で作成されたテスト証明書であるため、 https://localhost/servicemodelsamples/service.svc などのブラウザーで HTTPS アドレスにアクセスしようとすると、セキュリティ警告が表示されます。 WCF クライアントがテスト証明書を使用できるようにするために、セキュリティの警告を抑制するために、クライアントに追加のコードがいくつか追加されています。 そのためのコードとそれに必要なクラスは、本運用の証明書を使用するときには不要です。

```csharp
// WARNING: This code is only required for test certificates such as those created by makecert. It is
// not recommended for production code.
PermissiveCertificatePolicy.Enact("CN=ServiceModelSamples-HTTPS-Server");
```
