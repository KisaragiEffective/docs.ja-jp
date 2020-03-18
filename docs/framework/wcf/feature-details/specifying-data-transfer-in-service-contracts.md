---
title: サービス コントラクトでのデータ転送の指定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], data transfer
ms.assetid: 7c5a26c8-89c9-4bcb-a4bc-7131e6d01f0c
ms.openlocfilehash: 50f2444764ddb212513550ff0a62fcfecab2c45a
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347990"
---
# <a name="specifying-data-transfer-in-service-contracts"></a>サービス コントラクトでのデータ転送の指定
Windows Communication Foundation (WCF) は、メッセージングインフラストラクチャと考えることができます。 サービス操作では、メッセージを受信し、それらのメッセージを処理し、送信することができます。 メッセージは、操作コントラクトを使用して記述されます。 たとえば、次のようなコントラクトがあるとします。  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(string fromCity, string toCity);  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
  
    <OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
End Interface  
```  
  
 この場合、`GetAirfare` 操作では、`fromCity` と `toCity` に関する情報を含むメッセージを受け入れ、数値を含むメッセージを返します。  
  
 ここでは、操作コントラクトでメッセージを記述するさまざまな方法について説明します。  
  
## <a name="describing-messages-by-using-parameters"></a>パラメーターを使用したメッセージの記述  
 メッセージを記述する最も簡単な方法は、パラメーター リストと戻り値を使用することです。 前の例では、`fromCity` と `toCity` の 2 つの文字列パラメーターを使用して要求メッセージを記述し、float 型の戻り値を使用して応答メッセージを記述しました。 戻り値だけでは応答メッセージを記述できない場合は、out パラメーターを使用できます。 たとえば、次の操作は、`fromCity` と `toCity` を要求メッセージに含め、数値と通貨を応答メッセージに含めます。  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, out string currency);  
```  
  
```vb  
<OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
```  
  
 また、参照パラメーターを使用すると、要求メッセージと応答メッセージの両方のパラメーター部分を作成できます。 パラメーターは、シリアル化 (XML への変換) が可能な型にする必要があります。 既定では、WCF は <xref:System.Runtime.Serialization.DataContractSerializer> クラスと呼ばれるコンポーネントを使用して、この変換を実行します。 ほとんどのプリミティブ型 (`int`、`string`、`float`、`DateTime` など) がサポートされます。 ユーザー定義型には、通常、データ コントラクトが存在する必要があります。 詳細については、「[データコントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)」を参照してください。  
  
```csharp
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
  
    [DataContract]  
    public class Itinerary  
    {  
        [DataMember]  
        public string fromCity;  
        [DataMember]  
        public string toCity;  
   }  
}  
```  
  
```vb  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
    <DataContract()>  
    Class Itinerary  
  
        <DataMember()>  
        Public fromCity As String  
        <DataMember()>  
        Public toCity As String  
    End Class  
End Interface  
```  
  
 場合によっては、使用する型のシリアル化に `DataContractSerializer` が適さないことがあります。 WCF では、代替のシリアル化エンジンである <xref:System.Xml.Serialization.XmlSerializer>をサポートしています。これは、パラメーターをシリアル化するためにも使用できます。 <xref:System.Xml.Serialization.XmlSerializer> では、`XmlAttributeAttribute` などの属性を使用することによって、結果の XML をより細かく制御できます。 特定の操作やサービス全体で <xref:System.Xml.Serialization.XmlSerializer> を使用するように切り替えるには、<xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性を操作またはサービスに適用します。 例:  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    [XmlSerializerFormat]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
}  
public class Itinerary  
{  
    public string fromCity;  
    public string toCity;  
    [XmlAttribute]  
    public bool isFirstClass;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    <XmlSerializerFormat>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
End Interface  
  
Class Itinerary  
  
    Public fromCity As String  
    Public toCity As String  
    <XmlSerializerFormat()>  
    Public isFirstClass As Boolean  
End Class  
```  
  
 詳細については、「 [XmlSerializer クラスの使用](../../../../docs/framework/wcf/feature-details/using-the-xmlserializer-class.md)」を参照してください。 「XmlSerializer の使用」で説明するような明確な理由がある場合を除き、ここで示す <xref:System.Xml.Serialization.XmlSerializer> への手動での切り替えはお勧めしません。  
  
 .NET パラメーター名をコントラクト名から分離するには、<xref:System.ServiceModel.MessageParameterAttribute> 属性を使用し、`Name` プロパティを使用してコントラクト名を設定します。 たとえば、次の操作コントラクトは、このトピックの最初の例に相当します。  
  
```csharp  
[OperationContract]  
public float GetAirfare(  
    [MessageParameter(Name="fromCity")] string originCity,  
    [MessageParameter(Name="toCity")] string destinationCity);  
```  
  
```vb  
<OperationContract()>  
  Function GetAirfare(<MessageParameter(Name := "fromCity")> fromCity As String, <MessageParameter(Name := "toCity")> toCity As String) As Double  
```  
  
## <a name="describing-empty-messages"></a>空のメッセージの記述  
 空の要求メッセージを記述するには、入力パラメーターや参照パラメーターを一切指定しません。 たとえば、 C#次のようになります。  
  
 `[OperationContract]`  
  
 `public int GetCurrentTemperature();`  
  
 たとえば、Visual Basic の場合は次のようになります。  
  
 `<OperationContract()>`  
  
 `Function GetCurrentTemperature() as Integer`  
  
 空の応答メッセージを記述するには、戻り値の型として `void` を指定し、出力パラメーターや参照パラメーターを一切指定しません。 次の場合:  
  
```csharp  
[OperationContract]  
public void SetTemperature(int temperature);  
```  
  
```vb  
<OperationContract()>  
Sub SetTemperature(temperature As Integer)  
```  
  
 これは、次のような一方向操作コントラクトと異なります。  
  
```csharp  
[OperationContract(IsOneWay=true)]  
public void SetLightbulbStatus(bool isOn);  
```  
  
```vb  
<OperationContract(IsOneWay:=True)>  
Sub SetLightbulbStatus(isOne As Boolean)  
```  
  
 `SetTemperatureStatus` 操作は空のメッセージを返します。 入力メッセージの処理中にエラーが発生した場合は、代わりにエラーを返す可能性があります。 `SetLightbulbStatus` 操作は何も返しません。 この操作からエラー状態を伝達することはできません。  
  
## <a name="describing-messages-by-using-message-contracts"></a>メッセージ コントラクトを使用したメッセージの記述  
 1 つの型を使用してメッセージ全体を表現する場合があります。 この場合、データ コントラクトを使用することもできますが、メッセージ コントラクトを使用する方法をお勧めします。この方法を使用すると、結果の XML での不要なレベルのラップを回避できます。 さらに、メッセージ コントラクトを使用すると、結果のメッセージをより細かく制御できます。 たとえば、メッセージの本文とヘッダーに含める情報をそれぞれ決めることができます。 次に、メッセージ コントラクトの使用例を示します。  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    GetAirfareResponse GetAirfare(GetAirfareRequest request);  
}  
  
[MessageContract]  
public class GetAirfareRequest  
{  
    [MessageHeader] public DateTime date;  
    [MessageBodyMember] public Itinerary itinerary;  
}  
  
[MessageContract]  
public class GetAirfareResponse  
{  
    [MessageBodyMember] public float airfare;  
    [MessageBodyMember] public string currency;  
}  
  
[DataContract]  
public class Itinerary  
{  
    [DataMember] public string fromCity;  
    [DataMember] public string toCity;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    Function GetAirfare(request As GetAirfareRequest) As GetAirfareResponse  
End Interface  
  
<MessageContract()>  
Public Class GetAirfareRequest  
    <MessageHeader()>   
    Public Property date as DateTime  
    <MessageBodyMember()>  
    Public Property itinerary As Itinerary  
End Class  
  
<MessageContract()>  
Public Class GetAirfareResponse  
    <MessageBodyMember()>  
    Public Property airfare As Double  
    <MessageBodyMember()> Public Property currency As String  
End Class  
  
<DataContract()>  
Public Class Itinerary  
    <DataMember()> Public Property fromCity As String  
    <DataMember()> Public Property toCity As String  
End Class  
```  
  
 詳細については、「[メッセージコントラクトの使用](../../../../docs/framework/wcf/feature-details/using-message-contracts.md)」を参照してください。  
  
 前の例でも <xref:System.Runtime.Serialization.DataContractSerializer> クラスが既定で使用されます。 メッセージ コントラクトで、<xref:System.Xml.Serialization.XmlSerializer> クラスを使用することもできます。 これを行うには、操作またはコントラクトに <xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性を適用し、メッセージ ヘッダーと本文メンバーで <xref:System.Xml.Serialization.XmlSerializer> クラスと互換性のある型を使用します。  
  
## <a name="describing-messages-by-using-streams"></a>ストリームを使用したメッセージの記述  
 操作でメッセージを記述する場合は、<xref:System.IO.Stream> クラスまたはその派生クラスを操作コントラクトで使用したり、メッセージ コントラクト本文メンバー (この場合、唯一のメンバーである必要があります) として使用したりする方法もあります。 受信メッセージの場合、型は `Stream` である必要があり、派生クラスを使用することはできません。  
  
 WCF は、シリアライザーを呼び出す代わりに、ストリームからデータを取得し、送信メッセージに直接格納するか、受信メッセージからデータを取得してストリームに直接格納します。 次に、ストリームの使用例を示します。  
  
```csharp  
[OperationContract]  
public Stream DownloadFile(string fileName);  
```  
  
```vb  
<OperationContract()>  
Function DownloadFile(fileName As String) As String  
```  
  
 1 つのメッセージ本文で `Stream` と非ストリーム データを組み合わせることはできません。 追加データをメッセージ ヘッダーに配置するには、メッセージ コントラクトを使用します。 操作コントラクトを定義するときのストリームの不適切な使用例を次に示します。  
  
```csharp  
//Incorrect:  
// [OperationContract]  
// public void UploadFile (string fileName, Stream fileData);  
```  
  
```vb  
'Incorrect:  
    '<OperationContract()>  
    Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
```  
  
 操作コントラクトを定義するときのストリームの正しい使用例を次に示します。  
  
```csharp  
[OperationContract]  
public void UploadFile (UploadFileMessage message);  
//code omitted  
[MessageContract]  
public class UploadFileMessage  
{  
    [MessageHeader] public string fileName;  
    [MessageBodyMember] public Stream fileData;  
}  
```  
  
```vb  
<OperationContract()>  
Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
'Code Omitted  
<MessageContract()>  
Public Class UploadFileMessage  
   <MessageHeader()>  
    Public Property fileName As String  
    <MessageBodyMember()>  
    Public Property fileData As Stream  
End Class  
```  
  
 詳細については、「 [Large Data And Streaming](../../../../docs/framework/wcf/feature-details/large-data-and-streaming.md)」を参照してください。  
  
## <a name="using-the-message-class"></a>メッセージ クラスの使用  
 送受信されるメッセージをプログラムによって完全に制御するには、次のコード例に示すように <xref:System.ServiceModel.Channels.Message> クラスを直接使用します。  
  
```csharp  
[OperationContract]  
public void LogMessage(Message m);  
```  
  
```vb  
<OperationContract()>  
Sub LogMessage(m As Message)  
```  
  
 これは高度なシナリオです。詳細については、「 [Message クラスの使用](../../../../docs/framework/wcf/feature-details/using-the-message-class.md)」を参照してください。  
  
## <a name="describing-fault-messages"></a>エラー メッセージの記述  
 一方向ではない操作では、戻り値と出力パラメーターまたは参照パラメーターによって記述されるメッセージに加え、通常の応答メッセージとエラー メッセージの少なくとも 2 つのメッセージを返すことができます。 たとえば、次の操作コントラクトがあるとします。  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
```  
  
```vb  
<OperationContract()>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime)  
```  
  
 この操作は、`float` 型の数値を含む通常のメッセージか、エラー コードと説明を含むエラー メッセージのいずれかを返すことができます。 これは、サービス実装で <xref:System.ServiceModel.FaultException> をスローすることによって実現できます。  
  
 <xref:System.ServiceModel.FaultContractAttribute> 属性を使用すると、追加のエラー メッセージを指定できます。 追加のエラーは、次のコード例に示すように、<xref:System.Runtime.Serialization.DataContractSerializer> を使用してシリアル化できる必要があります。  
  
```csharp  
[OperationContract]  
[FaultContract(typeof(ItineraryNotAvailableFault))]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
  
//code omitted  
  
[DataContract]  
public class ItineraryNotAvailableFault  
{  
    [DataMember]  
    public bool IsAlternativeDateAvailable;  
  
    [DataMember]  
    public DateTime alternativeSuggestedDate;  
}  
```  
  
```vb  
<OperationContract()>  
<FaultContract(GetType(ItineraryNotAvailableFault))>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime) As Double  
  
'Code Omitted  
<DataContract()>  
Public Class  
  <DataMember()>  
  Public Property IsAlternativeDateAvailable As Boolean  
  <DataMember()>  
  Public Property alternativeSuggestedDate As DateTime  
End Class  
```  
  
 これらの追加エラーは、適切なデータ コントラクト型の <xref:System.ServiceModel.FaultException%601> をスローすることによって生成できます。 詳細については、「[例外とエラーの処理](../../../../docs/framework/wcf/extending/handling-exceptions-and-faults.md)」を参照してください。  
  
 <xref:System.Xml.Serialization.XmlSerializer> クラスを使用してエラーを記述することはできません。 <xref:System.ServiceModel.XmlSerializerFormatAttribute> は、エラー コントラクトに影響を及ぼしません。  
  
## <a name="using-derived-types"></a>派生型の使用  
 操作コントラクトやメッセージ コントラクトで基本型を使用し、その後、実際に操作を呼び出すときに派生型を使用する場合があります。 この場合、<xref:System.ServiceModel.ServiceKnownTypeAttribute> 属性または何らかの代替機構を使用して、派生型を使用できるようにする必要があります。 次の操作があるとします。  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
```  
  
```vb
<OperationContract()>  
    Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
```  
  
 `Book` から派生する `Magazine` と `LibraryItem` の 2 つの型があると想定します。 `IsLibraryItemAvailable` 操作でこれらの型を使用するには、操作を次のように変更します。  
  
 `[OperationContract]`  
  
 `[ServiceKnownType(typeof(Book))]`  
  
 `[ServiceKnownType(typeof(Magazine))]`  
  
 `public bool IsLibraryItemAvailable(LibraryItem item);`  
  
 既定の <xref:System.Runtime.Serialization.KnownTypeAttribute> を使用している場合は、次のコード例に示すように <xref:System.Runtime.Serialization.DataContractSerializer> 属性を使用できます。  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
  
// code omitted   
  
[DataContract]  
[KnownType(typeof(Book))]  
[KnownType(typeof(Magazine))]  
public class LibraryItem  
{  
    //code omitted  
}  
```  
  
```vb  
<OperationContract()>  
Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
  
'Code Omitted  
<DataContract()>  
<KnownType(GetType(Book))>  
<KnownType(GetType(Magazine))>  
Public Class LibraryItem  
  'Code Omitted  
End Class  
```  
  
 <xref:System.Xml.Serialization.XmlIncludeAttribute> を使用する場合は、<xref:System.Xml.Serialization.XmlSerializer> 属性を使用できます。  
  
 <xref:System.ServiceModel.ServiceKnownTypeAttribute> 属性は、操作に適用することもサービス全体に適用することもできます。 この属性は、<xref:System.Runtime.Serialization.KnownTypeAttribute> 属性と同様に、型を受け取るか、既知の型の一覧を取得するために呼び出すメソッドの名前を受け取ります。 詳細については、「[データコントラクトの既知の型](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md)」を参照してください。  
  
## <a name="specifying-the-use-and-style"></a>Use と Style の指定  
 Web サービス記述言語 (WSDL: Web Services Description Language) を使用してサービスを記述するときは、一般にドキュメントとリモート プロシージャ コール (RPC: Remote Procedure Call) の 2 つのスタイルが使用されます。 ドキュメント スタイルでは、スキーマを使用してメッセージ本文全体が記述されます。WSDL では、該当するスキーマの中の要素を参照して、メッセージ本文のさまざまな部分を記述します。 RPC スタイルでは、WSDL は、要素ではなく、メッセージの各部を表すスキーマ型を参照します。 場合によっては、これらのスタイルのいずれかを手動で選択することが必要になります。 これを行うには、<xref:System.ServiceModel.DataContractFormatAttribute> 属性を適用し、`Style` プロパティを設定するか (<xref:System.Runtime.Serialization.DataContractSerializer> を使用している場合)、`Style` 属性の <xref:System.ServiceModel.XmlSerializerFormatAttribute> を設定します (<xref:System.Xml.Serialization.XmlSerializer> を使用している場合)。  
  
 また、<xref:System.Xml.Serialization.XmlSerializer> はシリアル化された XML の形式を 2 とおり (`Literal` および `Encoded`) サポートしています。 `Literal` は最も一般に受け入れられたフォームであり、<xref:System.Runtime.Serialization.DataContractSerializer> がサポートする唯一のフォームです。 `Encoded` は SOAP 仕様のセクション 5 に記載されたレガシ フォームであり、新しいサービスにはお勧めしません。 `Encoded` モードに切り替えるには、`Use` 属性の <xref:System.ServiceModel.XmlSerializerFormatAttribute> プロパティを `Encoded` に設定します。  
  
 ほとんどの場合、`Style` プロパティと `Use` プロパティの既定の設定は変更しないでください。  
  
## <a name="controlling-the-serialization-process"></a>シリアル化プロセスの制御  
 データをシリアル化する方法をカスタマイズする場合、さまざまな設定を行うことができます。  
  
### <a name="changing-server-serialization-settings"></a>サーバーのシリアル化設定の変更  
 既定の <xref:System.Runtime.Serialization.DataContractSerializer> を使用している場合は、<xref:System.ServiceModel.ServiceBehaviorAttribute> 属性をサービスに適用することで、サービスでのシリアル化プロセスの一部の側面を制御できます。 具体的には、`MaxItemsInObjectGraph` プロパティを使用して、<xref:System.Runtime.Serialization.DataContractSerializer> が逆シリアル化するオブジェクトの最大数を制限するクォータを設定できます。 また、`IgnoreExtensionDataObject` プロパティを使用すると、ラウンド トリップ バージョン管理機能を無効にできます。 クォータの詳細については、「[セキュリティに関するデータの考慮事項](../../../../docs/framework/wcf/feature-details/security-considerations-for-data.md)」を参照してください。 ラウンドトリップの詳細については、「[上位互換性のあるデータコントラクト](../../../../docs/framework/wcf/feature-details/forward-compatible-data-contracts.md)」を参照してください。  
  
```csharp  
[ServiceBehavior(MaxItemsInObjectGraph=100000)]  
public class MyDataService:IDataService  
{  
    public DataPoint[] GetData()  
    {  
       // Implementation omitted  
    }  
}  
```  
  
```vb  
<ServiceBehavior(MaxItemsInObjectGraph:=100000)>  
Public Class MyDataService Implements IDataService  
  
    Function GetData() As DataPoint()  
         ‘ Implementation omitted  
    End Function  
End Interface  
```  
  
### <a name="serialization-behaviors"></a>シリアル化の動作  
 WCF では、特定の操作に使用されているシリアライザーに応じて自動的に接続される、<xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> と <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> の2つの動作が使用できます。 これらの動作は自動的に適用されるため、通常、これらの動作を意識する必要はありません。  
  
 ただし、`DataContractSerializerOperationBehavior` には、`MaxItemsInObjectGraph`、`IgnoreExtensionDataObject`、および `DataContractSurrogate` の 3 つのプロパティがあり、これらを使用してシリアル化プロセスをカスタマイズできます。 最初の 2 つのプロパティの意味は、前のセクションの説明と同じです。 `DataContractSurrogate` プロパティを使用すると、シリアル化プロセスをカスタマイズおよび拡張するための強力な機構であるデータ コントラクト サロゲートを有効にできます。 詳細については、「[データコントラクトサロゲート](../../../../docs/framework/wcf/extending/data-contract-surrogates.md)」を参照してください。  
  
 `DataContractSerializerOperationBehavior` を使用すると、クライアントとサーバーの両方のシリアル化をカスタマイズできます。 クライアントで `MaxItemsInObjectGraph` クォータを増やす方法を次の例に示します。  
  
```csharp  
ChannelFactory<IDataService> factory = new ChannelFactory<IDataService>(binding, address);  
foreach (OperationDescription op in factory.Endpoint.Contract.Operations)  
{  
    DataContractSerializerOperationBehavior dataContractBehavior =  
                op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
    if (dataContractBehavior != null)  
    {  
        dataContractBehavior.MaxItemsInObjectGraph = 100000;  
    }  
}  
IDataService client = factory.CreateChannel();  
```  
  
```vb  
Dim factory As ChannelFactory(Of IDataService) = New ChannelFactory(Of IDataService)(binding, address)  
For Each op As OperationDescription In factory.Endpoint.Contract.Operations  
        Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
        If dataContractBehavior IsNot Nothing Then  
            dataContractBehavior.MaxItemsInObjectGraph = 100000  
        End If  
     Next  
    Dim client As IDataService = factory.CreateChannel  
```  
  
次に示すのは、自己ホスト型の場合のサービスの同等のコードです。
  
```csharp  
ServiceHost serviceHost = new ServiceHost(typeof(IDataService))  
foreach (ServiceEndpoint ep in serviceHost.Description.Endpoints)  
{  
foreach (OperationDescription op in ep.Contract.Operations)  
{  
        DataContractSerializerOperationBehavior dataContractBehavior =  
           op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
        if (dataContractBehavior != null)  
        {  
            dataContractBehavior.MaxItemsInObjectGraph = 100000;  
        }  
}  
}  
serviceHost.Open();  
```  
  
```vb  
Dim serviceHost As ServiceHost = New ServiceHost(GetType(IDataService))  
        For Each ep As ServiceEndpoint In serviceHost.Description.Endpoints  
            For Each op As OperationDescription In ep.Contract.Operations  
                Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
  
                If dataContractBehavior IsNot Nothing Then  
                    dataContractBehavior.MaxItemsInObjectGraph = 100000  
                End If  
            Next  
        Next  
        serviceHost.Open()  
```  
  
 Web ホスト型の場合は、新しい `ServiceHost` 派生クラスを作成し、サービス ホスト ファクトリを使用してプラグインする必要があります。  
  
### <a name="controlling-serialization-settings-in-configuration"></a>構成でのシリアル化設定の制御  
 次の例に示すように、`MaxItemsInObjectGraph` と `IgnoreExtensionDataObject` は、`dataContractSerializer` エンドポイントまたはサービスの動作を使用して構成によって制御できます。  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="LargeQuotaBehavior">  
                    <dataContractSerializer  
                      maxItemsInObjectGraph="100000" />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
        <client>  
            <endpoint address="http://example.com/myservice"  
                  behaviorConfiguration="LargeQuotaBehavior"  
                binding="basicHttpBinding" bindingConfiguration=""   
                            contract="IDataService"  
                name="" />  
        </client>  
    </system.serviceModel>  
</configuration>  
```  
  
### <a name="shared-type-serialization-object-graph-preservation-and-custom-serializers"></a>共有する型のシリアル化、オブジェクト グラフの保存、およびカスタム シリアライザー  
 <xref:System.Runtime.Serialization.DataContractSerializer> は、.NET 型名ではなく、データ コントラクト名を使用してシリアル化を実行します。 これは、サービス指向アーキテクチャの基本思想と一致しており、高度な柔軟性の実現を可能にします。つまり、ネットワーク コントラクトに影響を及ぼさずに .NET 型を変更できます。 まれなケースとして、実際の .NET 型名をシリアル化することにより、.NET Framework リモート処理テクノロジと同様のクライアントとサーバー間の密結合を導入することが必要になる場合があります。 これは、.NET Framework リモート処理から WCF に移行するときに通常発生するまれなケースを除き、推奨される方法ではありません。 この場合、<xref:System.Runtime.Serialization.NetDataContractSerializer> クラスではなく、<xref:System.Runtime.Serialization.DataContractSerializer> クラスを使用する必要があります。  
  
 <xref:System.Runtime.Serialization.DataContractSerializer> は、通常、オブジェクト グラフをオブジェクト ツリーとしてシリアル化します。 つまり、同じオブジェクトが複数回にわたって参照される場合、そのオブジェクトは複数回にわたってシリアル化されます。 たとえば、`PurchaseOrder` および `billTo` という、Address 型の 2 つのフィールドを持つ `shipTo` インスタンスがあるとします。 この両方のフィールドに同じ Address インスタンスを設定すると、シリアル化および逆シリアル化の後に 2 つの同一の Address インスタンスが生じます。 これは、オブジェクト グラフを XML で表現する相互運用可能な標準の方法がないためです (ただし、<xref:System.Xml.Serialization.XmlSerializer> と `Style` に関する前のセクションで説明した `Use` で使用できる従来の SOAP エンコード標準を除きます)。 オブジェクト グラフをツリーとしてシリアル化する場合、循環参照が含まれたグラフをシリアル化できないなどの欠点があります。 場合によっては、相互運用が可能ではありませんが、真のオブジェクト グラフ シリアル化に切り替えることが必要になります。 これを行うには、<xref:System.Runtime.Serialization.DataContractSerializer> パラメーターを `preserveObjectReferences` に設定して構築した `true` を使用します。  
  
 シナリオによっては、組み込みのシリアライザーでは不十分な場合があります。 ただし、ほとんどの場合、<xref:System.Runtime.Serialization.XmlObjectSerializer> と <xref:System.Runtime.Serialization.DataContractSerializer> の派生元である <xref:System.Runtime.Serialization.NetDataContractSerializer> 抽象クラスを使用できます。  
  
 前の 3 つのケース (.NET 型の保存、オブジェクト グラフの保存、および `XmlObjectSerializer` ベースの完全なカスタム シリアル化) では、必ずカスタム シリアライザーをプラグインする必要があります。 具体的には、次の手順を実行します。  
  
1. <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> から派生する独自の動作を記述します。  
  
2. 独自のシリアライザー (`CreateSerializer`、<xref:System.Runtime.Serialization.NetDataContractSerializer> が <xref:System.Runtime.Serialization.DataContractSerializer> に設定された `preserveObjectReferences`、独自のカスタム `true` のいずれか) を返すように 2 つの <xref:System.Runtime.Serialization.XmlObjectSerializer> メソッドをオーバーライドします。  
  
3. サービス ホストを開いたり、クライアント チャネルを作成したりする前に、既存の <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 動作を削除し、前の手順で作成したカスタム派生クラスをプラグインします。  
  
 高度なシリアル化の概念の詳細については、「[シリアル化と逆シリアル](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md)化」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XmlSerializer クラスの使用](../../../../docs/framework/wcf/feature-details/using-the-xmlserializer-class.md)
- [方法 : ストリーミングを有効にする](../../../../docs/framework/wcf/feature-details/how-to-enable-streaming.md)
- [方法 : クラスまたは構造体に基本的なデータ コントラクトを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-basic-data-contract-for-a-class-or-structure.md)
