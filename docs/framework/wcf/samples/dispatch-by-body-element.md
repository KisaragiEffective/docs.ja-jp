---
title: 本文要素別のディスパッチ
ms.date: 03/30/2017
ms.assetid: f64a3c04-62b4-47b2-91d9-747a3af1659f
ms.openlocfilehash: 307d6bbbab118392ef079942eae367a4c6792c22
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74712029"
---
# <a name="dispatch-by-body-element"></a>本文要素別のディスパッチ
このサンプルでは、入力メッセージを操作に割り当てるための代替アルゴリズムを実装する方法を示します。  
  
 既定では、サービス モデル ディスパッチャは、メッセージの WS-Addressing の "Action" ヘッダーまたは HTTP SOAP 要求でのこれと同等の情報に基づいて、入力メッセージの適切な処理メソッドを選択します。  
  
 WS-I Basic Profile 1.1 のガイドラインに従っていない SOAP 1.1 Web サービス スタックの一部では、Action URI に基づくのではなく、SOAP 本文内にある最初の要素の XML 修飾名に基づいてメッセージをディスパッチします。 同様に、クライアント側のこうしたスタックでは、空または任意の HTTP SoapAction ヘッダーが含まれるメッセージを送信する場合があります。これは SOAP 1.1 の仕様で許可されていました。  
  
 メッセージをメソッドにディスパッチする方法を変更するために、このサンプルでは <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 拡張インターフェイスを `DispatchByBodyElementOperationSelector` に実装します。 このクラスは、メッセージ本文の最初の要素に基づいて操作を選択します。  
  
 クラス コンストラクタには、`XmlQualifiedName` と文字列のペアが記録されたディクショナリが必要です。この修飾名は SOAP 本文の最初の子の名前を示し、文字列は該当する操作名を示します。 `defaultOperationName` は、このディクショナリで照合できないすべてのメッセージを受信する操作の名前です。  
  
```csharp
class DispatchByBodyElementOperationSelector : IDispatchOperationSelector  
{  
    Dictionary<XmlQualifiedName, string> dispatchDictionary;  
    string defaultOperationName;  
  
    public DispatchByBodyElementOperationSelector(Dictionary<XmlQualifiedName,string> dispatchDictionary, string defaultOperationName)  
    {  
        this.dispatchDictionary = dispatchDictionary;  
        this.defaultOperationName = defaultOperationName;  
    }  
}
```  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> インターフェイスにはメソッドが 1 つしかないので、<xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector.SelectOperation%2A> を実装すると、ビルドが非常に簡単です。 このメソッドでの処理は、入力メッセージを検査し、現在のエンドポイントのサービス コントラクトでのメソッド名と同じ文字列を返すことです。  
  
 このサンプルでは、操作セレクタは <xref:System.Xml.XmlDictionaryReader> を通じて、入力メッセージの本文の <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> を取得します。 このメソッドは既にリーダーをメッセージ本文の最初の子に配置しています。そのため、現在の要素の名前と名前空間の URI を取得して、それらを `XmlQualifiedName` に結合することは簡単です。その後、これは操作セレクタによって保持されているディクショナリから対応する操作を検索する際に使用されます。  
  
```csharp
public string SelectOperation(ref System.ServiceModel.Channels.Message message)  
{  
    XmlDictionaryReader bodyReader = message.GetReaderAtBodyContents();  
    XmlQualifiedName lookupQName = new  
       XmlQualifiedName(bodyReader.LocalName, bodyReader.NamespaceURI);  
    message = CreateMessageCopy(message,bodyReader);  
    if (dispatchDictionary.ContainsKey(lookupQName))  
    {  
         return dispatchDictionary[lookupQName];  
    }  
    else  
    {  
        return defaultOperationName;  
    }  
}  
```  
  
 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A>、またはメッセージ本文のコンテンツへのアクセスを提供する他のメソッドを使用してメッセージ本文にアクセスすると、そのメッセージは "read" とマークされます。つまり、メッセージは以降の処理に対して無効になります。 したがって、操作セレクタは次のコードに示すメソッドを使用して入力メッセージのコピーを作成します。 リーダーの位置は検査中には変更されていないので、新しく作成されたメッセージはこれを参照できます。入力メッセージのプロパティとヘッダーもこの新しいメッセージにコピーされるので、その結果、元のメッセージの完全な複製ができあがります。  
  
```csharp
private Message CreateMessageCopy(Message message,   
                                     XmlDictionaryReader body)  
{  
    Message copy = Message.CreateMessage(message.Version,message.Headers.Action,body);  
    copy.Headers.CopyHeaderFrom(message,0);  
    copy.Properties.CopyProperties(message.Properties);  
    return copy;  
}  
```  
  
## <a name="adding-an-operation-selector-to-a-service"></a>サービスへの操作セレクタの追加  
 サービスディスパッチ操作セレクターは、Windows Communication Foundation (WCF) ディスパッチャーの拡張機能です。 二重のコントラクトのコールバック チャネルでメソッドを選択する場合、クライアントの操作セレクタも存在します。この操作セレクタはここで説明するディスパッチ操作セレクタと非常によく似ていますが、このサンプルでは明示的には説明しません。  
  
 ディスパッチ操作セレクタはほとんどのサービス モデル拡張と同様、動作を使用してディスパッチャに追加されます。 *動作*は、ディスパッチランタイム (またはクライアントランタイム) に1つ以上の拡張機能を追加する構成オブジェクトであり、それ以外の場合はその設定を変更します。  
  
 操作セレクタにはコントラクトのスコープがあるので、ここで実装する適切な動作は <xref:System.ServiceModel.Description.IContractBehavior> です。 このインターフェイスは、次のコードに示すように <xref:System.Attribute> 派生クラスに実装されているため、この動作は任意のサービス コントラクトに宣言として追加できます。 <xref:System.ServiceModel.ServiceHost> が開いてディスパッチ ランタイムがビルドされるたびに、コントラクト、操作、およびサービス実装の属性の形をとるか、またはサービス構成内の要素の形をとるすべての動作が自動的に追加され、その後拡張機能の支援または既定の構成の変更を求められます。  
  
 次のコードの抜粋では、簡略化のため、メソッド <xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> の実装のみを示します。これは、このサンプルのディスパッチャの構成変更に影響します。 他のメソッドが表示されていないのは、何の処理も行わずに呼び出し元に返されるためです。  
  
```csharp
[AttributeUsage(AttributeTargets.Class|AttributeTargets.Interface)]  
class DispatchByBodyElementBehaviorAttribute : Attribute, IContractBehavior  
{  
    // public void AddBindingParameters(...)   
    // public void ApplyClientBehavior(...)  
    // public void Validate(...)  
```  
  
 最初に、<xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> を実装し、サービス エンドポイントの <xref:System.ServiceModel.Description.OperationDescription> の <xref:System.ServiceModel.Description.ContractDescription> 要素を反復処理することによって、操作セレクタの検索ディクショナリを設定します。 次に、各操作の記述に `DispatchBodyElementAttribute` 動作が存在するかどうかが検査されます。この動作は、このサンプルでも定義されている <xref:System.ServiceModel.Description.IOperationBehavior> の実装です。 このクラスも動作の 1 つですが、パッシブな動作です。ディスパッチ ランタイムに対して構成変更をアクティブに支援することはありません。 このすべてのメソッドは、アクションを起こすことなく呼び出し元に返されます。 この操作の動作は、新しいディスパッチ機構に必要なメタデータ、つまり見つかった場合に操作で選択される本文要素の修飾名を、該当する操作に関連付ける目的でのみ存在します。  
  
 このような動作が見つかると、XML 修飾名 (`QName` プロパティ) と操作名 (`Name` プロパティ) から作成された値のペアがディクショナリに追加されます。  
  
 ディクショナリが設定されると、新しい `DispatchByBodyElementOperationSelector` がこの情報で構築され、ディスパッチ ランタイムの操作セレクタとして設定されます。  
  
```csharp
public void ApplyDispatchBehavior(ContractDescription contractDescription, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatchRuntime)  
{  
    Dictionary<XmlQualifiedName,string> dispatchDictionary =   
                     new Dictionary<XmlQualifiedName,string>();  
    foreach( OperationDescription operationDescription in   
                              contractDescription.Operations )  
    {  
        DispatchBodyElementAttribute dispatchBodyElement =   
   operationDescription.Behaviors.Find<DispatchBodyElementAttribute>();  
        if ( dispatchBodyElement != null )  
        {  
             dispatchDictionary.Add(dispatchBodyElement.QName,   
                              operationDescription.Name);  
        }  
    }  
    dispatchRuntime.OperationSelector =   
            new DispatchByBodyElementOperationSelector(  
               dispatchDictionary,   
               dispatchRuntime.UnhandledDispatchOperation.Name);  
    }  
}  
```  
  
## <a name="implementing-the-service"></a>サービスの実装  
 このサンプルに実装されている動作は、通信回線からのメッセージの解釈方法とディスパッチ方法に直接影響します。これはサービス コントラクトの機能です。 そのため、この動作を使用することを選択したサービス実装では、この動作をサービス コントラクト レベルで宣言する必要があります。  
  
 サンプルプロジェクトサービスは、`DispatchByBodyElementBehaviorAttribute` コントラクト動作を `IDispatchedByBody` サービスコントラクトに適用し、2つの操作のそれぞれに `OperationForBodyA()` と、`DispatchBodyElementAttribute` 操作の動作を `OperationForBodyB()` します。 このコントラクトを実装しているサービスのサービス ホストが開かれると、このメタデータは前で説明したようにディスパッチャ ビルダによって取得されます。  
  
 操作セレクタはメッセージ本文の要素のみに基づいてディスパッチし、"Action" を無視するので、返された応答の "Action" ヘッダーをチェックしないようランタイムに通知する必要があります。これを行うには、ワイルドカード "*" を `ReplyAction` の <xref:System.ServiceModel.OperationContractAttribute> プロパティに割り当てます。 さらに、"Action" プロパティがワイルドカード "\*" に設定されている既定の操作が必要です。 既定の操作は、ディスパッチできず、`DispatchBodyElementAttribute` を持たないすべてのメッセージを受信します。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"),  
                            DispatchByBodyElementBehavior]  
public interface IDispatchedByBody  
{  
    [OperationContract(ReplyAction="*"),   
     DispatchBodyElement("bodyA","http://tempuri.org")]  
    Message OperationForBodyA(Message msg);  
    [OperationContract(ReplyAction = "*"),   
     DispatchBodyElement("bodyB", "http://tempuri.org")]  
    Message OperationForBodyB(Message msg);  
    [OperationContract(Action="*", ReplyAction="*")]  
    Message DefaultOperation(Message msg);  
}  
```  
  
 このサンプルのサービス実装は簡単です。 どのメソッドも受信メッセージを応答メッセージにラップし、クライアントに再度エコーします。  
  
## <a name="running-and-building-the-sample"></a>サンプルの実行とビルド  
 このサンプルを実行すると、次に示す (書式設定された) 出力と同様の、操作応答の内容がクライアントのコンソール ウィンドウに表示されます。  
  
 クライアントは 3 つのメッセージをサービスに送信します。このメッセージ本文のコンテンツ要素の名前は、それぞれ `bodyA`、`bodyB`、および `bodyX` です。 以上の説明とサービス コントラクトからわかるように、`bodyA` 要素が含まれる受信メッセージは、`OperationForBodyA()` メソッドにディスパッチされます。 `bodyX` 本文要素が含まれるメッセージについては、明示的なディスパッチ対象がないので、このメッセージは `DefaultOperation()` にディスパッチされます。 各サービス操作は、受信されたメッセージ本文をメソッド固有の要素にラップして返します。このサンプルでは、入力メッセージと出力メッセージを明確に関連付けるためにこの操作を行います。  
  
```xml  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyA xmlns="http://tempuri.org">  
   <q:bodyA xmlns:q="http://tempuri.org">test</q:bodyA>  
</replyBodyA>  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyB xmlns="http://tempuri.org">  
  <q:bodyB xmlns:q="http://tempuri.org">test</q:bodyB>  
</replyBodyB>  
<?xml version="1.0" encoding="IBM437"?>  
<replyDefault xmlns="http://tempuri.org">  
   <q:bodyX xmlns:q="http://tempuri.org">test</q:bodyX>  
</replyDefault>  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Interop\AdvancedDispatchByBody`  
