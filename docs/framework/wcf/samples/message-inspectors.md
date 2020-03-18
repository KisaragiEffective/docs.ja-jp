---
title: メッセージ インスペクター
ms.date: 03/30/2017
ms.assetid: 9bd1f305-ad03-4dd7-971f-fa1014b97c9b
ms.openlocfilehash: 29c7fd9729cbdcc99a05d01f717c1cc548e8d9ea
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714826"
---
# <a name="message-inspectors"></a>メッセージ インスペクター
このサンプルでは、クライアントとサービスのメッセージ インスペクタを実装して構成する方法を示します。  
  
 メッセージ インスペクタは拡張オブジェクトで、サービス モデルのクライアント ランタイムで使用し、プログラムまたは構成を使用してランタイムをディスパッチします。それにより、受信後または送信前にメッセージの検査および変更を行うことができます。  
  
 このサンプルでは、クライアントとサービスの基本的なメッセージ検証機構を実装します。この機構は、構成可能な XML スキーマ ドキュメント セットに照らして受信メッセージを検証します。 このサンプルでのメッセージ検証は、操作ごとに行われません。 これは意図的に単純化されたサンプルであるためです。  
  
## <a name="message-inspector"></a>メッセージ インスペクタ  
 クライアント メッセージ インスペクタには、<xref:System.ServiceModel.Dispatcher.IClientMessageInspector> インターフェイスが実装され、サービス メッセージ インスペクタには <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> インターフェイスが実装されます。 こうした実装を 1 つのクラスに結合すると、クライアントとサービスの両方で使用できるメッセージ インスペクタとして設定できます。 このサンプルでは、このように結合されたメッセージ インスペクタを実装します。 このインスペクタは、受信メッセージと送信メッセージが検証されるスキーマ セットを渡すように構築されます。開発者はインスペクタを使用して、受信メッセージまたは送信メッセージを検証するかどうか、およびインスペクタをディスパッチ モードとクライアント モードのどちらにするかを指定できます。この指定内容は、このトピックで後述するようにエラー処理に影響します。  
  
```csharp
public class SchemaValidationMessageInspector : IClientMessageInspector, IDispatchMessageInspector  
{  
    XmlSchemaSet schemaSet;  
    bool validateRequest;  
    bool validateReply;  
    bool isClientSide;  
    [ThreadStatic]  
    bool isRequest;  
  
    public SchemaValidationMessageInspector(XmlSchemaSet schemaSet,  
         bool validateRequest, bool validateReply, bool isClientSide)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = validateReply;  
        this.validateRequest = validateRequest;  
        this.isClientSide = isClientSide;  
    }  
```  
  
 サービス (ディスパッチャ) メッセージ インスペクタには、<xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> と <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> の、2 つの <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29> メソッドを実装する必要があります。  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> がディスパッチャによって呼び出されるのは、メッセージが到着してチャネル スタックで処理され、サービスに割り当てられたときですが、メッセージが逆シリアル化されて操作にディスパッチされる前です。 受信メッセージが暗号化されていた場合、そのメッセージはメッセージ インスペクタに到着するときには既に復号化されています。 このメソッドは、参照パラメータとして渡された `request` メッセージを取得します。これにより、必要に応じてメッセージを調査、操作または置き換えることができます。 戻り値は任意のオブジェクトで、サービスが現在のメッセージに応答を返すときに <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> に渡される相関状態オブジェクトとして使用されます。 サンプルでは、<xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> はメッセージの検査 (検証) をプライベートでローカルのメソッド `ValidateMessageBody` に代行させ、相関状態オブジェクトを返しません。 このメソッドは、無効なメッセージがサービスに渡されないようにします。  
  
```csharp  
object IDispatchMessageInspector.AfterReceiveRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel, System.ServiceModel.InstanceContext instanceContext)  
{  
    if (validateRequest)  
    {  
        // inspect the message. If a validation error occurs,  
        // the thrown fault exception bubbles up.  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29> は、応答がクライアントに返送される準備が完了するたびに呼び出されます。一方向メッセージの場合は、受信メッセージが処理されるときに呼び出されます。 これにより、この拡張機能は MEP に関係なく対称的に呼び出されるものと想定できます。 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> と同様に、メッセージは参照パラメータとして渡され、検査、変更、または置き換えを行うことができます。 このサンプルで実行されるメッセージの検証も、同様に `ValidMessageBody` メソッドで代行されますが、この場合、検証エラーの処理は若干異なります。  
  
 サービスで検証エラーが発生すると、`ValidateMessageBody` メソッドは <xref:System.ServiceModel.FaultException> から派生した例外をスローします。 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> では、これらの例外をサービス モデル インフラストラクチャに格納できます。例外はここで自動的に SOAP エラーに変換され、クライアントに転送されます。 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> では、<xref:System.ServiceModel.FaultException> 例外はインフラストラクチャに格納できません。メッセージ インスペクタが呼び出される前に、サービスによってスローされたエラー例外が変換されるためです。 したがって、次の実装では、既知の `ReplyValidationFault` 例外がキャッチされ、応答メッセージが明示的なエラー メッセージに置き換えられます。 このメソッドは、無効なメッセージがサービス実装によって返されないようにします。  
  
```csharp  
void IDispatchMessageInspector.BeforeSendReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        // Inspect the reply, catch a possible validation error   
        try  
        {  
            ValidateMessageBody(ref reply, false);  
        }  
        catch (ReplyValidationFault fault)  
        {  
            // if a validation error occurred, the message is replaced  
            // with the validation fault.  
            reply = Message.CreateMessage(reply.Version,   
                    fault.CreateMessageFault(), reply.Headers.Action);  
        }  
    }  
```  
  
 クライアント メッセージ インスペクタも、これとよく似ています。 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector> からは、<xref:System.ServiceModel.Dispatcher.IClientMessageInspector.AfterReceiveReply%2A> および <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> の 2 つのメソッドが実装される必要があります。  
  
 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> は、クライアント アプリケーションまたは操作フォーマッタのどちらかでメッセージが作成されたときに呼び出されます。 ディスパッチャ メッセージ インスペクタと同様、メッセージを検査することも、または完全に置き換えることもできます。 このサンプルのインスペクタは、ディスパッチ メッセージ インスペクタにも使用されている、同じローカルの `ValidateMessageBody` ヘルパー メソッドに代行させます。  
  
 (コンストラクタで指定される) クライアント検証とサービス検証間の動作上の違いは、クライアント検証はユーザー コードに格納するローカル例外をスローする点です。この例外はローカルに発生するもので、サービス エラーではないためです。 通常のルールでは、サービス ディスパッチャ インスペクタがエラーをスローするのに対し、クライアント インスペクタは例外をスローします。  
  
 この <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> の実装は、無効なメッセージがサービスに送信されないようにします。  
  
```csharp  
object IClientMessageInspector.BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
{  
    if (validateRequest)  
    {  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 `AfterReceiveReply` の実装は、サービスから受信された無効なメッセージがクライアント ユーザー コードに転送されないようにします。  
  
```csharp  
void IClientMessageInspector.AfterReceiveReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        ValidateMessageBody(ref reply, false);  
    }  
}  
```  
  
 この特定のメッセージ インスペクタの中心となるのが `ValidateMessageBody` メソッドです。 この動作を実行するには、渡されるメッセージ本文のコンテンツのサブツリーを、<xref:System.Xml.XmlReader> 検証でラップします。 リーダーには、メッセージ インスペクタが保持するスキーマ セットが設定され、検証コールバックは、このメソッドと共に定義される `InspectionValidationHandler` を参照するデリゲートに設定されます。 この検証を実行するためにメッセージが読み取られ、メモリ ストリームを基にする <xref:System.Xml.XmlDictionaryWriter> にスプールされます。 この処理中に検証エラーまたは警告が発生した場合は、コールバック メソッドが呼び出されます。  
  
 エラーが発生しない場合、プロパティとヘッダーが元のメッセージからコピーされた、新しいメッセージが作成されます。このメッセージにはメモリ ストリーム内の現在検証した Infoset が使用され、<xref:System.Xml.XmlDictionaryReader> でラップされて置換メッセージに追加されます。  
  
```csharp  
void ValidateMessageBody(ref System.ServiceModel.Channels.Message message, bool isRequest)  
{  
    if (!message.IsFault)  
    {  
        XmlDictionaryReaderQuotas quotas =   
                new XmlDictionaryReaderQuotas();  
        XmlReader bodyReader =   
            message.GetReaderAtBodyContents().ReadSubtree();  
        XmlReaderSettings wrapperSettings =   
                              new XmlReaderSettings();  
        wrapperSettings.CloseInput = true;  
        wrapperSettings.Schemas = schemaSet;  
        wrapperSettings.ValidationFlags =   
                                XmlSchemaValidationFlags.None;  
        wrapperSettings.ValidationType = ValidationType.Schema;  
        wrapperSettings.ValidationEventHandler += new   
           ValidationEventHandler(InspectionValidationHandler);  
        XmlReader wrappedReader = XmlReader.Create(bodyReader,   
                                            wrapperSettings);  
  
        // pull body into a memory backed writer to validate  
        this.isRequest = isRequest;  
        MemoryStream memStream = new MemoryStream();  
        XmlDictionaryWriter xdw =  
              XmlDictionaryWriter.CreateBinaryWriter(memStream);  
        xdw.WriteNode(wrappedReader, false);  
        xdw.Flush(); memStream.Position = 0;  
        XmlDictionaryReader xdr =   
        XmlDictionaryReader.CreateBinaryReader(memStream, quotas);  
  
        // reconstruct the message with the validated body  
        Message replacedMessage =   
            Message.CreateMessage(message.Version, null, xdr);  
        replacedMessage.Headers.CopyHeadersFrom(message.Headers);  
        replacedMessage.Properties.CopyProperties(message.Properties);  
        message = replacedMessage;  
    }  
}  
```  
  
 `InspectionValidationHandler` メソッドは、スキーマの検証エラーまたは警告が発生するたびに、<xref:System.Xml.XmlReader> 検証によって呼び出されます。 次の実装はエラーにのみ対応し、すべての警告を無視します。  
  
 最初は、<xref:System.Xml.XmlReader> 検証をメッセージ インスペクタを使用してメッセージに挿入し、メッセージの処理時にメッセージをバッファ処理することなく検証できるように思えます。 ただし、この場合、このコールバックがサービス モデル インフラストラクチャの任意の場所に検証例外をスローするか、またはユーザー コードが無効な XML ノードとして検出され、その結果、予測不可能な動作になります。 バッファ処理の方法により、ユーザー コードは無効なメッセージから完全にシールドされます。  
  
 これまでに説明したように、ハンドラによってスローされる例外は、クライアントとサービスとでは異なります。 サービスでの例外は <xref:System.ServiceModel.FaultException> から派生し、クライアントでの例外は標準のカスタム例外です。  
  
```csharp  
        void InspectionValidationHandler(object sender, ValidationEventArgs e)  
{  
    if (e.Severity == XmlSeverityType.Error)  
    {  
        // We are treating client and service side validation errors  
        // differently here. Client side errors cause exceptions  
        // and are thrown straight up to the user code. Service side  
        // validations cause faults.  
        if (isClientSide)  
        {  
            if (isRequest)  
            {  
                throw new RequestClientValidationException(e.Message);  
            }  
            else  
            {  
                throw new ReplyClientValidationException(e.Message);  
            }  
        }  
        else  
        {  
            if (isRequest)  
            {  
                // this fault is caught by the ServiceModel   
                // infrastructure and turned into a fault reply.  
                throw new RequestValidationFault(e.Message);  
             }  
             else  
             {  
                // this fault is caught and turned into a fault message  
                // in BeforeSendReply in this class  
                throw new ReplyValidationFault(e.Message);  
              }  
          }  
      }  
    }  
```  
  
## <a name="behavior"></a>動作  
 メッセージ インスペクタは、クライアント ランタイムまたはディスパッチ ランタイムに対する拡張機能です。 このような拡張機能は、*ビヘイビアー*を使用して構成されます。 動作はサービス モデル ランタイムの動作を変更するクラスです。この変更は、既定の構成を変更するか、または拡張機能 (メッセージ インスペクタなど) を追加したりすることによって行われます。  
  
 次の `SchemaValidationBehavior` クラスは、このサンプルのメッセージ インスペクタをクライアント ランタイムまたはディスパッチ ランタイムに追加するときに使用される動作です。 この実装は、どちらの場合でも基本的な実装です。 <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyClientBehavior%2A> と <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyDispatchBehavior%2A> ではメッセージ インスペクタが作成され、それぞれのランタイムの <xref:System.ServiceModel.Dispatcher.ClientRuntime.MessageInspectors%2A> コレクションに追加されます。  
  
```csharp
public class SchemaValidationBehavior : IEndpointBehavior  
{  
    XmlSchemaSet schemaSet;   
    bool validateRequest;   
    bool validateReply;  
  
    public SchemaValidationBehavior(XmlSchemaSet schemaSet, bool   
                           inspectRequest, bool inspectReply)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = inspectReply;  
        this.validateRequest = inspectRequest;  
    }  
    #region IEndpointBehavior Members  
  
    public void AddBindingParameters(ServiceEndpoint endpoint,   
       System.ServiceModel.Channels.BindingParameterCollection   
                                            bindingParameters)  
    {  
    }  
  
    public void ApplyClientBehavior(ServiceEndpoint endpoint,   
            System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
    {  
        SchemaValidationMessageInspector inspector =   
           new SchemaValidationMessageInspector(schemaSet,   
                      validateRequest, validateReply, true);  
            clientRuntime.MessageInspectors.Add(inspector);  
    }  
  
    public void ApplyDispatchBehavior(ServiceEndpoint endpoint,   
         System.ServiceModel.Dispatcher.EndpointDispatcher   
                                          endpointDispatcher)  
    {  
        SchemaValidationMessageInspector inspector =   
           new SchemaValidationMessageInspector(schemaSet,   
                        validateRequest, validateReply, false);  
   endpointDispatcher.DispatchRuntime.MessageInspectors.Add(inspector);  
    }  
  
   public void Validate(ServiceEndpoint endpoint)  
   {  
   }  
  
    #endregion  
}  
```  
  
> [!NOTE]
> この特定の動作は属性の代わりにはなりません。したがって、サービス型のコントラクト型に宣言して追加することはできません。 これは設計上の判断です。スキーマ コレクションを属性の宣言で読み込むことはできず、この属性で他の構成場所 (アプリケーション設定など) を参照することによって、残りのサービス モデル構成と一貫性を持たない構成要素が作成されるからです。 したがって、この動作は、強制的にコードおよびサービス モデル構成の拡張を使用してのみ追加できます。  
  
## <a name="adding-the-message-inspector-through-configuration"></a>構成を使用したメッセージ インスペクタの追加  
 アプリケーション構成ファイルのエンドポイントでカスタム動作を構成するには、サービスモデルで、<xref:System.ServiceModel.Configuration.BehaviorExtensionElement>から派生したクラスによって表される構成*拡張要素*を作成する実装が必要です。 次に、この拡張機能を拡張用のサービス モデルの構成セクションに追加する必要があります。このセクションで説明する拡張機能を参照してください。  
  
```xml  
<system.serviceModel>  
…  
   <extensions>  
      <behaviorExtensions>  
        <add name="schemaValidator" type="Microsoft.ServiceModel.Samples.SchemaValidationBehaviorExtensionElement, MessageInspectors, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>  
      </behaviorExtensions>  
    </extensions>  
…  
</system.serviceModel>  
```  
  
 拡張機能は、アプリケーションまたは ASP.NET の構成ファイルに追加できます。これが最も一般的な選択肢ですが、コンピュータ上の構成ファイルに追加することもできます。  
  
 拡張機能を構成範囲に追加すると、次のコードに示すように、動作を動作の構成に追加できるようになります。 動作の構成は再使用可能な要素で、必要に応じて複数のエンドポイントに適用できます。 ここで構成される特定の動作は <xref:System.ServiceModel.Description.IEndpointBehavior> を実装するので、構成ファイルの各構成セクションでのみ有効です。  
  
```xml  
<system.serviceModel>  
   <behaviors>  
      …  
     <endpointBehaviors>  
        <behavior name="HelloServiceEndpointBehavior">  
          <schemaValidator validateRequest="True" validateReply="True">  
            <schemas>  
              <add location="messages.xsd" />    
            </schemas>  
          </schemaValidator>  
        </behavior>  
      </endpointBehaviors>  
      …  
    </behaviors>  
</system.serviceModel>  
```  
  
 メッセージ インスペクタを構成する `<schemaValidator>` 要素は、`SchemaValidationBehaviorExtensionElement` クラスによってサポートされます。 このクラスは、`ValidateRequest` と `ValidateReply` という 2 つのブール型パブリック プロパティを公開します。 これらのプロパティは、どちらも <xref:System.Configuration.ConfigurationPropertyAttribute> でマークされます。 この属性は、コードのプロパティと XML 属性との間のリンクを構成するもので、前の XML 構成要素に含まれています。 このクラスには、追加で `Schemas` でマークされる、<xref:System.Configuration.ConfigurationCollectionAttribute> 型のプロパティ `SchemaCollection` もあります。これもこのサンプルに含まれていますが、このドキュメントでは簡略化のために省略されています。 このプロパティは、コレクションおよびコレクション要素クラス `SchemaConfigElement` と共に、前の構成スニペット内の `<schemas>` 要素を補足し、スキーマのコレクションを検証セットに追加できるようにします。  
  
 ランタイムがクライアントまたはエンドポイントを構築するときにその構成データを評価する場合、オーバーライドされた `CreateBehavior` メソッドにより、構成データは動作オブジェクトになります。  
  
```csharp
public class SchemaValidationBehaviorExtensionElement : BehaviorExtensionElement  
{  
    public SchemaValidationBehaviorExtensionElement()  
    {  
    }  
  
    public override Type BehaviorType   
    {   
        get  
        {  
            return typeof(SchemaValidationBehavior);  
        }   
    }  
  
    protected override object CreateBehavior()  
    {  
        XmlSchemaSet schemaSet = new XmlSchemaSet();  
        foreach (SchemaConfigElement schemaCfg in this.Schemas)  
        {  
            Uri baseSchema = new   
                Uri(AppDomain.CurrentDomain.BaseDirectory);  
            string location = new   
                Uri(baseSchema,schemaCfg.Location).ToString();  
            XmlSchema schema =   
                XmlSchema.Read(new XmlTextReader(location), null);  
            schemaSet.Add(schema);  
        }  
     return new   
     SchemaValidationBehavior(schemaSet,ValidateRequest,ValidateReply);  
    }  
  
[ConfigurationProperty("validateRequest",DefaultValue=false,IsRequired=false)]  
public bool ValidateRequest  
{  
    get { return (bool)base["validateRequest"]; }  
    set { base["validateRequest"] = value; }  
}  
  
[ConfigurationProperty("validateReply", DefaultValue = false, IsRequired = false)]  
        public bool ValidateReply  
        {  
            get { return (bool)base["validateReply"]; }  
            set { base["validateReply"] = value; }  
        }  
  
     //Declare the Schema collection property.  
     //Note: the "IsDefaultCollection = false" instructs   
     //.NET Framework to build a nested section of   
     //the kind <Schema> ...</Schema>.  
    [ConfigurationProperty("schemas", IsDefaultCollection = true)]  
    [ConfigurationCollection(typeof(SchemasCollection),  
        AddItemName = "add",  
        ClearItemsName = "clear",  
        RemoveItemName = "remove")]  
    public SchemasCollection Schemas  
    {  
        get  
        {  
            SchemasCollection SchemasCollection =  
            (SchemasCollection)base["schemas"];  
            return SchemasCollection;  
        }  
    }  
}  
```  
  
## <a name="adding-message-inspectors-imperatively"></a>メッセージ インスペクタの強制的な追加  
 属性 (このサンプルでは前の理由により未サポート) と構成を使用せず、極力簡単に動作をクライアント ランタイムとサービス ランタイムに追加するには、強制コードを使用できます。 このサンプルでは、クライアント アプリケーションで強制コードを使用して、クライアント メッセージ インスペクタをテストします。 `GenericClient` クラスは、エンドポイント構成をユーザー コードに対して公開する <xref:System.ServiceModel.ClientBase%601> から派生しています。 クライアントが暗黙に開かれる前に、エンドポイント構成は変更できます。たとえば、次のコードで示すように動作を追加します。 サービスに動作を追加する方法は、ここで示すクライアントの手法と非常に似ており、サービス ホストが開かれる前に実行する必要があります。  
  
```csharp  
try  
{  
    Console.WriteLine("*** Call 'Hello' with generic client, with client behavior");  
    GenericClient client = new GenericClient();  
  
    // Configure client programmatically, adding behavior  
    XmlSchema schema = XmlSchema.Read(new StreamReader("messages.xsd"),   
                                                          null);  
    XmlSchemaSet schemaSet = new XmlSchemaSet();  
    schemaSet.Add(schema);  
    client.Endpoint.Behaviors.Add(new   
                SchemaValidationBehavior(schemaSet, true, true));  
  
    Console.WriteLine("--- Sending valid client request:");  
    GenericCallValid(client, helloAction);  
    Console.WriteLine("--- Sending invalid client request:");  
    GenericCallInvalid(client, helloAction);  
  
    client.Close();  
}  
catch (Exception e)  
{  
    DumpException(e);  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageInspectors`  
