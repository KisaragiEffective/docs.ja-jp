---
title: 'カスタム メッセージ エンコーダー : 圧縮エンコーダー'
ms.date: 03/30/2017
ms.assetid: 57450b6c-89fe-4b8a-8376-3d794857bfd7
ms.openlocfilehash: 80dd29569897be501d76024a081f38ec5add4ff7
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716847"
---
# <a name="custom-message-encoder-compression-encoder"></a>カスタム メッセージ エンコーダー : 圧縮エンコーダー

このサンプルでは、Windows Communication Foundation (WCF) プラットフォームを使用してカスタムエンコーダーを実装する方法を示します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`

## <a name="sample-details"></a>サンプルの詳細

このサンプルは、クライアント コンソール プログラム (.exe)、自己ホスト型サービス コンソール プログラム (.exe)、および圧縮メッセージ エンコーダー ライブラリ (.dll) で構成されています。 サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 コントラクトは `ISampleServer` インターフェイスによって定義されます。このインターフェイスでは、基本的な文字列のエコー操作 (`Echo` と `BigEcho`) が公開されます。 クライアントは指定された操作を同期要求し、サービスはクライアントにメッセージをそのまま戻すことによって応答します。 クライアント アクティビティとサービス アクティビティは、コンソール ウィンドウに表示されます。 このサンプルの目的は、カスタム エンコーダを記述する方法と、ネットワーク上でメッセージを圧縮したときの影響を示すことです。 圧縮メッセージ エンコーダーにインストルメンテーションを追加すると、メッセージのサイズ、処理時間、またはその両方を計算することができます。

> [!NOTE]
> .NET Framework 4 では、サーバーが (GZip や Deflate などのアルゴリズムで作成された) 圧縮された応答を送信する場合、WCF クライアントで自動展開が有効になっています。 このサービスがインターネット インフォメーション サービス (IIS) で Web ホストされる場合は、サービスが圧縮された応答を送信するように IIS を構成することができます。 このサンプルは、クライアントとサービスの両方で圧縮および展開を行うという要件がある場合、またはサービスが自己ホスト型である場合に使用できます。

このサンプルでは、カスタムメッセージエンコーダーを構築し、WCF アプリケーションに統合する方法を示します。 GZipEncoder.dll ライブラリは、クライアントとサービスの両方で配置されます。 また、このサンプルではメッセージを圧縮したときの影響も示します。 GZipEncoder.dll のコードでは、次が示されます。

- カスタム エンコーダーおよびエンコーダー ファクトリの作成。

- カスタム エンコーダのバインド要素の開発。

- カスタム バインド要素を統合するためのカスタム バインド構成の使用。

- カスタム バインド要素のファイル構成を可能にするカスタム構成ハンドラーの開発。

前に示したように、カスタム エンコーダにはいくつかのレイヤが実装されています。 こうした各レイヤ間の関係をわかりやすく説明するために、サービス起動のイベントの順序を簡略化した一覧を次に示します。

1. サーバーが起動します。

2. 構成情報が読み取られます。

    1. サービス構成がカスタム構成ハンドラを登録します。

    2. サービス ホストが作成されて開かれます。

    3. カスタム構成要素がカスタム バインド要素を作成して返します。

    4. カスタム バインド要素がメッセージ エンコーダ ファクトリを作成して返します。

3. メッセージが受信されます。

4. メッセージ エンコーダ ファクトリは、メッセージを読み込んで応答を出力するためのメッセージ エンコーダを返します。

5. エンコーダ レイヤはクラス ファクトリとして実装されます。 カスタム エンコーダ用にパブリックに公開する必要があるのはエンコーダのクラス ファクトリだけです。 <xref:System.ServiceModel.ServiceHost> オブジェクトまたは <xref:System.ServiceModel.ChannelFactory%601> オブジェクトが作成されると、ファクトリ オブジェクトがバインド要素によって返されます。 メッセージ エンコーダは、バッファ モードまたはストリーミング モードで動作できます。 このサンプルでは、バッファ モードとストリーミング モードの両方を示します。

各モードの `ReadMessage` 抽象クラスには、関連する `WriteMessage` メソッドと `MessageEncoder` メソッドがあります。 エンコード処理の大部分はこれらのメソッドで行われます。 サンプルでは、既存のテキスト エンコーダとバイナリ メッセージ エンコーダをラップします。 これにより、内部のエンコーダがネットワーク上でのメッセージの表現の読み取りと書き込みを代行し、圧縮エンコーダがその結果を圧縮または解凍できます。 メッセージエンコード用のパイプラインがないため、WCF で複数のエンコーダーを使用する唯一のモデルとなります。 メッセージが解凍されると、結果として得られたメッセージは、処理対象のチャネル スタックの上にスタックとして渡されます。 圧縮中は、結果として得られる圧縮メッセージは指定されたストリームに直接書き込まれます。

このサンプルは、ヘルパー メソッド (`CompressBuffer` と `DecompressBuffer`) を使用して、バッファを、`GZipStream` クラスを使用するストリームに変換します。

バッファ内の `ReadMessage` クラスと `WriteMessage` クラスは、`BufferManager` クラスを使用します。 エンコーダにはエンコーダ ファクトリを通じてのみアクセスできます。 `MessageEncoderFactory` 抽象クラスは、現在のエンコーダにアクセスするための `Encoder` という名前のプロパティと、セッションをサポートするエンコーダを作成するための `CreateSessionEncoder` という名前のメソッドを提供します。 チャネルがセッションをサポートし、順序付けされて信頼できるシナリオでは、このようなエンコーダを使用できます。 このシナリオでは、ネットワークに書き込まれるデータの各セッションを最適化できます。 これが必要でない場合は、基本メソッドをオーバーロードしないでください。 `Encoder` プロパティは、セッションのないエンコーダにアクセスする機構を備えており、`CreateSessionEncoder` メソッドの既定の実装では、このプロパティの値が返されます。 サンプルでは既存のエンコーダをラップして圧縮を行うので、`MessageEncoderFactory` の実装では、内部のエンコーダ ファクトリを表す `MessageEncoderFactory` が受け入れられます。

エンコーダーとエンコーダーファクトリが定義されたので、それらを WCF クライアントおよびサービスと共に使用できます。 ただし、これらのエンコーダをチャネル スタックに追加する必要があります。 <xref:System.ServiceModel.ServiceHost> クラスと <xref:System.ServiceModel.ChannelFactory%601> クラスの派生クラスを作成して `OnInitialize` メソッドをオーバーライドすると、このエンコーダ ファクトリを手動で追加することができます。 また、カスタム バインド要素を介してエンコーダ ファクトリを公開することもできます。

新しいカスタム バインド要素を作成するには、<xref:System.ServiceModel.Channels.BindingElement> クラスの派生クラスを作成します。 ただし、バインド要素には複数の型があります。 カスタム バインド要素がメッセージ エンコード バインド要素として認識されるには、さらに <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> も実装する必要があります。 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> は、新しいメッセージ エンコーダ ファクトリ (`CreateMessageEncoderFactory`) を作成するためのメソッドを公開します。このメソッドを実装すると、一致するメッセージ エンコーダ ファクトリのインスタンスが返されます。 また、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement> にはアドレス バージョンを示すプロパティがあります。 このサンプルでは既存のエンコーダをラップするので、サンプルの実装では既存のエンコーダ バインド要素もラップし、内部のエンコーダ バインド要素をコンストラクタへのパラメータとして設定して、プロパティを介して公開します。 `GZipMessageEncodingBindingElement` クラスを実装する方法を次のサンプル コードに示します。

```csharp
public sealed class GZipMessageEncodingBindingElement
                        : MessageEncodingBindingElement //BindingElement
                        , IPolicyExportExtension
{

    //We use an inner binding element to store information
    //required for the inner encoder.
    MessageEncodingBindingElement innerBindingElement;

        //By default, use the default text encoder as the inner encoder.
        public GZipMessageEncodingBindingElement()
            : this(new TextMessageEncodingBindingElement()) { }

    public GZipMessageEncodingBindingElement(MessageEncodingBindingElement messageEncoderBindingElement)
    {
        this.innerBindingElement = messageEncoderBindingElement;
    }

    public MessageEncodingBindingElement InnerMessageEncodingBindingElement
    {
        get { return innerBindingElement; }
        set { innerBindingElement = value; }
    }

    //Main entry point into the encoder binding element.
    // Called by WCF to get the factory that creates the
    //message encoder.
    public override MessageEncoderFactory CreateMessageEncoderFactory()
    {
        return new
GZipMessageEncoderFactory(innerBindingElement.CreateMessageEncoderFactory());
    }

    public override MessageVersion MessageVersion
    {
        get { return innerBindingElement.MessageVersion; }
        set { innerBindingElement.MessageVersion = value; }
    }

    public override BindingElement Clone()
    {
        return new
        GZipMessageEncodingBindingElement(this.innerBindingElement);
    }

    public override T GetProperty<T>(BindingContext context)
    {
        if (typeof(T) == typeof(XmlDictionaryReaderQuotas))
        {
            return innerBindingElement.GetProperty<T>(context);
        }
        else
        {
            return base.GetProperty<T>(context);
        }
    }

    public override IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelFactory<TChannel>();
    }

    public override IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelListener<TChannel>();
    }

    public override bool CanBuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.CanBuildInnerChannelListener<TChannel>();
    }

    void IPolicyExportExtension.ExportPolicy(MetadataExporter exporter, PolicyConversionContext policyContext)
    {
        if (policyContext == null)
        {
            throw new ArgumentNullException("policyContext");
        }
       XmlDocument document = new XmlDocument();
       policyContext.GetBindingAssertions().Add(document.CreateElement(
            GZipMessageEncodingPolicyConstants.GZipEncodingPrefix,
            GZipMessageEncodingPolicyConstants.GZipEncodingName,
            GZipMessageEncodingPolicyConstants.GZipEncodingNamespace));
    }
}
```

`GZipMessageEncodingBindingElement` クラスは `IPolicyExportExtension` インターフェイスを実装しているので、次の例に示すように、このバインド要素はメタデータ内のポリシーとしてエクスポートできます。

```xml
<wsp:Policy wsu:Id="BufferedHttpSampleServer_ISampleServer_policy">
    <wsp:ExactlyOne>
      <wsp:All>
        <gzip:text xmlns:gzip=
        "http://schemas.microsoft.com/ws/06/2004/mspolicy/netgzip1" />
       <wsaw:UsingAddressing />
     </wsp:All>
   </wsp:ExactlyOne>
</wsp:Policy>
```

`GZipMessageEncodingBindingElementImporter` クラスは `IPolicyImportExtension` インターフェイスを実装しています。このクラスは `GZipMessageEncodingBindingElement` のポリシーをインポートします。 Svcutil.exe ツールを使用すると、ポリシーを構成ファイルにインポートして `GZipMessageEncodingBindingElement` を処理できます。これを行うには、Svcutil.exe.config に次のコードを追加する必要があります。

```xml
<configuration>
  <system.serviceModel>
    <extensions>
      <bindingElementExtensions>
        <add name="gzipMessageEncoding"
          type=
            "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      </bindingElementExtensions>
    </extensions>
    <client>
      <metadata>
        <policyImporters>
          <remove type=
"System.ServiceModel.Channels.MessageEncodingBindingElementImporter, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
          <extension type=
"Microsoft.ServiceModel.Samples.GZipMessageEncodingBindingElementImporter, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </policyImporters>
      </metadata>
    </client>
  </system.serviceModel>
</configuration>
```

これで、一致する圧縮エンコーダのバインド要素ができるので、新しいカスタム バインド オブジェクトを作成してそのカスタム バインド要素を追加すると、プログラムによって圧縮エンコーダのバインド要素をサービスまたはクライアントにフックできます。次のサンプル コードを参照してください。

```csharp
ICollection<BindingElement> bindingElements = new List<BindingElement>();
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();
GZipMessageEncodingBindingElement compBindingElement = new GZipMessageEncodingBindingElement ();
bindingElements.Add(compBindingElement);
bindingElements.Add(httpBindingElement);
CustomBinding binding = new CustomBinding(bindingElements);
binding.Name = "SampleBinding";
binding.Namespace = "http://tempuri.org/bindings";
```

ほとんどのユーザー シナリオではこのコードで十分ですが、サービスが Web ホストの場合はファイル構成のサポートが重要になります。 Web ホストのシナリオをサポートするには、カスタム構成ハンドラを開発して、カスタム バインディング要素をファイル内で構成できるようにする必要があります。

構成システム上にバインド要素の構成ハンドラーを作成できます。 バインド要素の構成ハンドラーは、<xref:System.ServiceModel.Configuration.BindingElementExtensionElement> クラスから派生する必要があります。 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.BindingElementType?displayProperty=nameWithType> は、このセクション用に作成するバインド要素の型を構成システムに通知します。 設定可能な `BindingElement` のすべての側面を、<xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 派生クラスのプロパティとして公開する必要があります。 <xref:System.Configuration.ConfigurationPropertyAttribute> は、属性がない場合に、構成要素の属性をプロパティにマップし、既定値を設定するのに役立ちます。 構成から値が読み込まれてプロパティに適用されると、<xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A?displayProperty=nameWithType> メソッドが呼び出されます。このメソッドは、プロパティをバインディング要素の具体的なインスタンスに変換します。 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.ApplyConfiguration%2A?displayProperty=nameWithType> メソッドは、<xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 派生クラスのプロパティを、新しく作成されたバインド要素に設定される値に変換するために使用されます。

`GZipMessageEncodingElement` を実装するサンプル コードを次に示します。

```csharp
public class GZipMessageEncodingElement : BindingElementExtensionElement
{
    public GZipMessageEncodingElement()
    {
    }

//Called by the WCF to discover the type of binding element this
//config section enables
    public override Type BindingElementType
    {
        get { return typeof(GZipMessageEncodingBindingElement); }
    }

    //The only property we need to configure for our binding element is
    //the type of inner encoder to use. Here, we support text and
    //binary.
    [ConfigurationProperty("innerMessageEncoding",
                         DefaultValue = "textMessageEncoding")]
    public string InnerMessageEncoding
    {
        get { return (string)base["innerMessageEncoding"]; }
        set { base["innerMessageEncoding"] = value; }
    }

    //Called by the WCF to apply the configuration settings (the
    //property above) to the binding element
    public override void ApplyConfiguration(BindingElement bindingElement)
    {
        GZipMessageEncodingBindingElement binding =
                (GZipMessageEncodingBindingElement)bindingElement;
        PropertyInformationCollection propertyInfo =
                    this.ElementInformation.Properties;
        if (propertyInfo["innerMessageEncoding"].ValueOrigin !=
                                     PropertyValueOrigin.Default)
        {
            switch (this.InnerMessageEncoding)
            {
                case "textMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                      new TextMessageEncodingBindingElement();
                    break;
                case "binaryMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                         new BinaryMessageEncodingBindingElement();
                    break;
            }
        }
    }

    //Called by the WCF to create the binding element
    protected override BindingElement CreateBindingElement()
    {
        GZipMessageEncodingBindingElement bindingElement =
                new GZipMessageEncodingBindingElement();
        this.ApplyConfiguration(bindingElement);
        return bindingElement;
    }
}
```

この構成ハンドラーは、サービスまたはクライアントの App.config または Web.config の次の表現にマップされます。

```xml
<gzipMessageEncoding innerMessageEncoding="textMessageEncoding" />
```

この構成ハンドラーを使用するには、次のサンプル構成に示すように、 [\<system.servicemodel >](../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md)要素内に登録する必要があります。

```xml
<extensions>
    <bindingElementExtensions>
       <add
           name="gzipMessageEncoding"
           type=
           "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement,
           GZipEncoder, Version=1.0.0.0, Culture=neutral,
           PublicKeyToken=null" />
      </bindingElementExtensions>
</extensions>
```

このサーバーを実行する場合は、操作要求および応答はコンソール ウィンドウに表示されます。 サーバーをシャットダウンするには、ウィンドウで Enter キーを押します。

```console
Press Enter key to Exit.

        Server Echo(string input) called:
        Client message: Simple hello

        Server BigEcho(string[] input) called:
        64 client messages
```

このクライアントを実行する場合は、操作要求および応答はコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

```console
Calling Echo(string):
Server responds: Simple hello Simple hello

Calling BigEcho(string[]):
Server responds: Hello 0

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

3. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`
