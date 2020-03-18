---
title: 'カスタム メッセージ エンコーダー : カスタム テキスト エンコーダー'
ms.date: 03/30/2017
ms.assetid: 68ff5c74-3d33-4b44-bcae-e1d2f5dea0de
ms.openlocfilehash: ae9d7f3d24e70c5cc74378eaaaa224910ada8d25
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347946"
---
# <a name="custom-message-encoder-custom-text-encoder"></a>カスタム メッセージ エンコーダー : カスタム テキスト エンコーダー

このサンプルでは、Windows Communication Foundation (WCF) を使用してカスタムテキストメッセージエンコーダーを実装する方法を示します。

> [!WARNING]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
> 
> `<InstallDrive>:\WF_WCF_Samples`
> 
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
> 
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Text`

WCF の <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> では、UTF-8、UTF-16、およびビッグエンディアンの Unicode エンコーディングのみがサポートされています。 このサンプルのカスタムテキストメッセージエンコーダーでは、相互運用性のために必要なプラットフォームでサポートされているすべての文字エンコーディングがサポートされています。 このサンプルは、クライアントコンソールプログラム (.exe)、インターネットインフォメーションサービス (IIS) によってホストされるサービスライブラリ (.dll)、およびテキストメッセージエンコーダーライブラリ (.dll) で構成されています。 サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 このコントラクトは `ICalculator` インターフェイスによって定義されており、算術演算 (加算、減算、乗算、および 除算) を公開しています。 クライアントは指定された算術演算を同期要求し、サービスは結果と共に応答します。 クライアントとサービスはどちらも、既定の `CustomTextMessageEncoder` の代わりに <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> を使用します。

カスタム エンコーダーの実装は、メッセージ エンコーダー ファクトリ、メッセージ エンコーダー、メッセージ エンコーディング バインド要素、および構成ハンドラーで構成され、次が示されます。

- カスタム エンコーダーおよびエンコーダー ファクトリの作成。

- カスタム エンコーダーのバインド要素の作成。

- カスタム バインド要素を統合するためのカスタム バインド構成の使用。

- カスタム バインド要素のファイル構成を可能にするカスタム構成ハンドラーの開発。

## <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

3. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

## <a name="message-encoder-factory-and-the-message-encoder"></a>メッセージ エンコーダー ファクトリとメッセージ エンコーダー

<xref:System.ServiceModel.ServiceHost> またはクライアント チャネルが開かれると、デザイン時コンポーネント `CustomTextMessageBindingElement` は `CustomTextMessageEncoderFactory` を作成します。 このファクトリは、`CustomTextMessageEncoder` を作成します。 メッセージ エンコーダーは、ストリーミング モードとバッファー モードの両方で動作し、 <xref:System.Xml.XmlReader> と <xref:System.Xml.XmlWriter> を使用してメッセージの読み取りと書き込みを行います。 UTF-8、UTF-16、ビッグエンディアン Unicode のみをサポートする WCF の最適化された XML リーダーとライターではなく、これらのリーダーとライターは、プラットフォームでサポートされているすべてのエンコーディングをサポートしています。

カスタム テキスト メッセージ エンコーダーのコード例を次に示します。

```csharp
public class CustomTextMessageEncoder : MessageEncoder
{
    private CustomTextMessageEncoderFactory factory;
    private XmlWriterSettings writerSettings;
    private string contentType;

    public CustomTextMessageEncoder(CustomTextMessageEncoderFactory factory)
    {
        this.factory = factory;

        this.writerSettings = new XmlWriterSettings();
        this.writerSettings.Encoding = Encoding.GetEncoding(factory.CharSet);
        this.contentType = $"{this.factory.MediaType}; charset={this.writerSettings.Encoding.HeaderName}";
    }

    public override string ContentType
    {
        get
        {
            return this.contentType;
        }
    }

    public override string MediaType
    {
        get 
        {
            return factory.MediaType;
        }
    }

    public override MessageVersion MessageVersion
    {
        get 
        {
            return this.factory.MessageVersion;
        }
    }

    public override Message ReadMessage(ArraySegment<byte> buffer, BufferManager bufferManager, string contentType)
    {   
        byte[] msgContents = new byte[buffer.Count];
        Array.Copy(buffer.Array, buffer.Offset, msgContents, 0, msgContents.Length);
        bufferManager.ReturnBuffer(buffer.Array);

        MemoryStream stream = new MemoryStream(msgContents);
        return ReadMessage(stream, int.MaxValue);
    }

    public override Message ReadMessage(Stream stream, int maxSizeOfHeaders, string contentType)
    {
        XmlReader reader = XmlReader.Create(stream);
        return Message.CreateMessage(reader, maxSizeOfHeaders, this.MessageVersion);
    }

    public override ArraySegment<byte> WriteMessage(Message message, int maxMessageSize, BufferManager bufferManager, int messageOffset)
    {
        MemoryStream stream = new MemoryStream();
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);
        message.WriteMessage(writer);
        writer.Close();

        byte[] messageBytes = stream.GetBuffer();
        int messageLength = (int)stream.Position;
        stream.Close();

        int totalLength = messageLength + messageOffset;
        byte[] totalBytes = bufferManager.TakeBuffer(totalLength);
        Array.Copy(messageBytes, 0, totalBytes, messageOffset, messageLength);

        ArraySegment<byte> byteArray = new ArraySegment<byte>(totalBytes, messageOffset, messageLength);
        return byteArray;
    }

    public override void WriteMessage(Message message, Stream stream)
    {
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);
        message.WriteMessage(writer);
        writer.Close();
    }
}
```

メッセージ エンコーダー ファクトリを作成する方法を次のコード例に示します。

```csharp
public class CustomTextMessageEncoderFactory : MessageEncoderFactory
{
    private MessageEncoder encoder;
    private MessageVersion version;
    private string mediaType;
    private string charSet;

    internal CustomTextMessageEncoderFactory(string mediaType, string charSet,
        MessageVersion version)
    {
        this.version = version;
        this.mediaType = mediaType;
        this.charSet = charSet;
        this.encoder = new CustomTextMessageEncoder(this);
    }

    public override MessageEncoder Encoder
    {
        get 
        { 
            return this.encoder;
        }
    }

    public override MessageVersion MessageVersion
    {
        get 
        { 
            return this.version;
        }
    }

    internal string MediaType
    {
        get
        {
            return this.mediaType;
        }
    }

    internal string CharSet
    {
        get
        {
            return this.charSet;
        }
    }
}
```

## <a name="message-encoding-binding-element"></a>メッセージ エンコーディング バインディング要素

バインド要素によって、WCF ランタイムスタックの構成が可能になります。 WCF アプリケーションでカスタムメッセージエンコーダーを使用するには、ランタイムスタックの適切なレベルで適切な設定を使用してメッセージエンコーダーファクトリを作成するバインド要素が必要です。

`CustomTextMessageBindingElement` は <xref:System.ServiceModel.Channels.BindingElement> 基本クラスから派生し、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement> クラスを継承します。 これにより、他の WCF コンポーネントは、このバインド要素をメッセージエンコーディングバインド要素として認識できます。 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A> を実装すると、一致するメッセージ エンコーダー ファクトリのインスタンスが適切に設定されて返されます。

`CustomTextMessageBindingElement` は、プロパティを介して `MessageVersion`、`ContentType`、および `Encoding` の設定を公開します。 エンコーダーは、Soap11Addressing と Soap12Addressing1 の両方のバージョンをサポートします。 既定値は Soap11Addressing1 です。 また、`ContentType` の既定値は "text/xml" です。 `Encoding` プロパティでは、必要な文字エンコーディングの値を設定できます。 サンプルクライアントとサービスでは、ISO-8859-1 (Latin1) 文字エンコードが使用されています。これは、WCF の <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> ではサポートされていません。

カスタム テキスト メッセージ エンコーダーを使用してプログラムでバインディングを作成する方法を次のコードに示します。

```csharp
ICollection<BindingElement> bindingElements = new List<BindingElement>();
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();
CustomTextMessageBindingElement textBindingElement = new CustomTextMessageBindingElement();
bindingElements.Add(textBindingElement);
bindingElements.Add(httpBindingElement);
CustomBinding binding = new CustomBinding(bindingElements);
```

## <a name="adding-metadata-support-to-the-message-encoding-binding-element"></a>メッセージ エンコーディング バインド要素へのメタデータのサポートの追加

<xref:System.ServiceModel.Channels.MessageEncodingBindingElement> から派生するすべての型は、サービスに対して生成される WSDL ドキュメント内の SOAP バインドのバージョンを更新します。 これを行うには、`ExportEndpoint` インターフェイス上に <xref:System.ServiceModel.Description.IWsdlExportExtension> メソッドを実装し、生成された WSDL を変更します。 このサンプルでは、`CustomTextMessageBindingElement` は `TextMessageEncodingBindingElement` からの WSDL エクスポート ロジックを使用します。

このサンプルの場合、クライアント構成は手動構成です。 Svcutil.exe を使用してクライアント構成を生成することはできません。`CustomTextMessageBindingElement` では、動作を記述するポリシー アサーションがエクスポートされないからです。 通常は、カスタム バインド要素上に <xref:System.ServiceModel.Description.IPolicyExportExtension> インターフェイスを実装して、バインド要素によって実装される動作または機能を記述するカスタム ポリシー アサーションをエクスポートする必要があります。 カスタムバインド要素のポリシーアサーションをエクスポートする方法の例については、[トランスポート: UDP](../../../../docs/framework/wcf/samples/transport-udp.md)サンプルを参照してください。

## <a name="message-encoding-binding-configuration-handler"></a>メッセージ エンコーディング バインド構成ハンドラー
前のセクションでは、カスタム テキスト メッセージ エンコーダーをプログラムによって使用する方法を示しました。 `CustomTextMessageEncodingBindingSection` は構成ハンドラーを実装します。この構成ハンドラーにより、カスタム テキスト メッセージ エンコーダーを構成ファイル内で使用することを指定できます。 `CustomTextMessageEncodingBindingSection` クラスは <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> クラスから派生します。 `BindingElementType` プロパティでは、このセクション用に作成するバインディング要素の型が構成システムに通知されます。

`CustomTextMessageBindingElement` によって定義されたすべての設定は、`CustomTextMessageEncodingBindingSection` のプロパティとして公開されます。 <xref:System.Configuration.ConfigurationPropertyAttribute> は、構成要素の属性をプロパティにマップしたり、属性がない場合は既定値を設定する際に役立ちます。 構成から値が読み込まれて型のプロパティに適用されると、<xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A> メソッドが呼び出されます。このメソッドは、プロパティをバインド要素の具体的なインスタンスに変換します。

この構成ハンドラーは、サービスまたはクライアントの App.config または Web.config の次の表現にマップされます。

```xml
<customTextMessageEncoding encoding="utf-8" contentType="text/xml" messageVersion="Soap11Addressing1" />
```

このサンプルでは、ISO-8859-1 エンコーディングを使用します。

この構成ハンドラーを使用するには、次の構成要素を使用して登録する必要があります。

```xml
<extensions>
    <bindingElementExtensions>
        <add name="customTextMessageEncoding" type=" 
Microsoft.ServiceModel.Samples.CustomTextMessageEncodingBindingSection, 
                  CustomTextMessageEncoder" />
    </bindingElementExtensions>
</extensions>
```
