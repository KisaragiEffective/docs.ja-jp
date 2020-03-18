---
title: 例外とエラーの処理
ms.date: 03/30/2017
ms.assetid: a64d01c6-f221-4f58-93e5-da4e87a5682e
ms.openlocfilehash: 2886463510a2237834529e1ec61c73ec7251e621
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937717"
---
# <a name="handling-exceptions-and-faults"></a>例外とエラーの処理
例外は、サービスまたはクライアント実装内でエラーをローカルに伝達するために使用されます。 一方、エラーは、サーバーからクライアントまたはクライアントからサーバーのように、サービス境界を越えてエラーを伝達するために使用されます。 このようなエラーに加え、多くの場合、トランスポート チャネルはトランスポート固有の機構を使用して、トランスポート レベルのエラーを伝達します。 たとえば、HTTP トランスポートは、404 などのステータス コードを使用して、存在しないエンドポイントの URL (エラーを返信するエンドポイントが存在しないこと) を伝達します。 このドキュメントは、カスタム チャネル作成者にガイダンスを示す 3 つのセクションで構成されています。 最初のセクションでは、例外を定義しスローする状況と方法に関するガイダンスを示します。 2 番目のセクションでは、エラーの生成と使用に関するガイダンスを示します。 3 番目のセクションでは、実行中のアプリケーションのトラブルシューティングを行う際に、カスタム チャネルのユーザーにとって役立つトレース情報を提供する方法について説明します。  
  
## <a name="exceptions"></a>例外  
 例外をスローする場合、2 つの点に留意します。まず、例外は、ユーザーがその例外に適切に対処できる正しいコードを作成できるような種類であることが必要です。 もう 1 つは、例外では、問題点、エラーの影響、およびエラーの修正方法をユーザーが理解できるだけの情報を提供する必要があります。 次のセクションでは、Windows Communication Foundation (WCF) チャネルの例外の種類とメッセージに関するガイダンスを提供します。 また、「Design Guidelines for Exceptions」に記載された .NET の例外に関する一般的なガイダンスも示します。  
  
### <a name="exception-types"></a>例外の種類  
 チャネルがスローするすべての例外は、<xref:System.TimeoutException?displayProperty=nameWithType>、<xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType>、または <xref:System.ServiceModel.CommunicationException> から派生した種類のいずれかである必要があります (<xref:System.ObjectDisposedException> のような例外をスローすることもできますが、これは呼び出し元のコードがチャネルを誤用したことを示す場合だけです。 チャネルが正しく使用されている場合は、指定された例外のみをスローする必要があります)。WCF には <xref:System.ServiceModel.CommunicationException> から派生する7つの例外の種類が用意されており、チャネルで使用するように設計されています。 <xref:System.ServiceModel.CommunicationException> から派生した例外には、システムのその他の部分で使用するように設計されているものもあります。 これらの例外の種類を以下に示します。  
  
|例外の種類|説明|内部例外の内容|回復戦略|  
|--------------------|-------------|-----------------------------|-----------------------|  
|<xref:System.ServiceModel.AddressAlreadyInUseException>|リッスン用に指定されたエンドポイント アドレスは既に使用されています。|エラーの詳細情報が得られるようであれば、この例外の原因となったトランスポート エラーについての情報を提供します。 たとえば、Windows 認証を使用する HTTP アダプタでメッセージが受信された場合、送信者の SSID は domain\username になります。 <xref:System.IO.PipeException>、<xref:System.Net.HttpListenerException>、または <xref:System.Net.Sockets.SocketException>。|別のアドレスで実行してください。|  
|<xref:System.ServiceModel.AddressAccessDeniedException>|このプロセスは、リッスン用に指定されたエンドポイント アドレスへのアクセスを許可されていません。|エラーの詳細情報が得られるようであれば、この例外の原因となったトランスポート エラーについての情報を提供します。 例 : <xref:System.IO.PipeException> または <xref:System.Net.HttpListenerException>|別の資格情報を使用して試行します。|  
|<xref:System.ServiceModel.CommunicationObjectFaultedException>|使用されているは、Faulted状態です(詳細については、「[状態の変更について](understanding-state-changes.md)」を参照してください<xref:System.ServiceModel.ICommunicationObject> )。 複数の呼び出しが保留になっているオブジェクトが Faulted 状態に遷移した場合、エラーに関連する例外をスローする呼び出しは 1 つだけであり、それ以外の呼び出しは <xref:System.ServiceModel.CommunicationObjectFaultedException> をスローします。 通常、この例外がスローされるのは、アプリケーションが何らかの例外を認識しておらず、おそらく最初の例外をキャッチしたスレッド以外のスレッドで、既に Faulted 状態のオブジェクトを使用しようとしたためです。|詳細情報を使用できる場合は、内部例外についての詳細を示します。|新規オブジェクトを作成します。 <xref:System.ServiceModel.ICommunicationObject> にエラーが発生した第一の原因によっては、回復するために他の作業が必要になることがあります。|  
|<xref:System.ServiceModel.CommunicationObjectAbortedException>|使用されている <xref:System.ServiceModel.ICommunicationObject> が中止されました (詳細については、「[状態の変更につい](understanding-state-changes.md)て」を参照してください)。 <xref:System.ServiceModel.CommunicationObjectFaultedException> と同様に、この例外は、アプリケーションがおそらく別のスレッドからオブジェクトに対して <xref:System.ServiceModel.ICommunicationObject.Abort%2A> を呼び出したときに、オブジェクトが既に Aborted 状態であるため、使用できなくなっていることを示しています。|詳細情報を使用できる場合は、内部例外についての詳細を示します。|新規オブジェクトを作成します。 <xref:System.ServiceModel.ICommunicationObject> が中止された第一の原因によっては、回復するために他の作業が必要になることがあります。|  
|<xref:System.ServiceModel.EndpointNotFoundException>|対象のリモート エンドポイントがリッスンしていません。 これは、エンドポイント アドレスの一部に誤りがある、エンドポイント アドレスの一部を解決できない、またはエンドポイントがダウンしていることが原因として考えられます (DNS エラー、キュー マネージャーを使用できない、サービスが実行されていないなど)。|この内部例外では、一般に、基になるトランスポートの詳細が示されます。|別のアドレスで実行してください。 また、サービスがダウンしている場合は、送信元がしばらく待機し、再試行することもできます。|  
|<xref:System.ServiceModel.ProtocolException>|エンドポイントのポリシーで示されている通信プロトコルがエンドポイント間で一致していません (フレーム コンテンツの種類の不一致や、最大メッセージ サイズの超過など)。|詳細情報を使用できる場合は、特定のプロトコル エラーの詳細を示します。 たとえば、MaxReceivedMessageSize を超えていることがエラーの原因である場合、<xref:System.ServiceModel.QuotaExceededException> が内部例外です。|回復 : 受信したプロトコル設定が送信側と一致していることを確認します。 この方法の 1 つとして、サービス エンドポイントのメタデータ (ポリシー) を再度インポートし、生成されたバインディングを使用してチャネルを再作成します。|  
|<xref:System.ServiceModel.ServerTooBusyException>|リモート エンドポイントはリッスンしていますが、メッセージを処理する準備ができていません。|詳細情報を使用できる場合は、内部例外で SOAP エラーまたはトランスポート レベルのエラーの詳細を示します。|回復 : しばらく待機し、後で操作を再試行します。|  
|<xref:System.TimeoutException>|タイムアウト期間内に操作を完了できませんでした。|タイムアウトの詳細を示します。|しばらく待機し、後で操作を再試行します。|  
  
 新しい例外の種類を定義するのは、その種類が既存の例外の種類のいずれとも異なる回復方法に対応する場合だけです。 新しい例外の種類を定義する場合は、<xref:System.ServiceModel.CommunicationException> または派生クラスのいずれかから派生する必要があります。  
  
### <a name="exception-messages"></a>例外メッセージ  
 例外メッセージは、プログラムではなくユーザーを対象としています。したがって、ユーザーが問題を理解し、解決する上で役立つ十分な情報を提供する必要があります。 優れた例外メッセージに不可欠な 3 つの構成要素を以下に示します。  
  
 問題の内容。 ユーザーの経験に関連する用語を使用して、問題を明確に説明します。 たとえば、"無効な構成セクション" という例外メッセージは適切とはいえません。 このようなメッセージでは、ユーザーは、どの構成セクションに誤りがあり、それがなぜ誤りなのか疑問を抱いたままになります。 強化されたメッセージは、"無効な構成セクション \<customBinding >" になります。 これをさらに改善し、"myBinding というバインディングには myTransport というトランスポートが既に含まれているため、このバインディングに myTransport というトランスポートを追加することはできません" というメッセージにします。 これは、アプリケーションの構成ファイル内でユーザーが容易に特定できる用語と名前を使用した、非常に明確なメッセージです。 ただし、まだ重要な要素がいくつか欠けています。  
  
 エラーの重大度。 メッセージがエラーの意味を明確に示していない場合、ユーザーはそれが致命的なエラーかどうか、または無視できるのかどうか疑問を抱くと考えられます。 一般に、メッセージではエラーの意味や重大度を示す必要があります。 前述の例のメッセージは、"構成エラーが原因で ServiceHost を開くことができませんでした: myBinding というバインディングには myTransport というトランスポートが既に含まれているため、このバインディングに myTransport というトランスポートを追加することはできません" というメッセージに改善できます。  
  
 ユーザーが問題を修正する方法。 メッセージの最も重要な要素は、ユーザーによる問題の修正を支援することにあります。 メッセージには、問題を改善するためのチェックや修正の内容に関する何らかのガイダンスやヒントを含める必要があります。 たとえば、"構成エラーが原因で ServiceHost を開くことができませんでした: myBinding というバインディングには myTransport というトランスポートが既に含まれているため、このバインディングに myTransport というトランスポートを追加することはできません。 バインディングに含まれているトランスポートが 1 つだけであることを確認してください" というメッセージにします。  
  
## <a name="communicating-faults"></a>エラーの伝達  
 SOAP 1.1 および SOAP 1.2 では、エラーの具体的な構造が定義されています。 この 2 つの仕様にはいくつかの違いがありますが、通常、Message 型と MessageFault 型を使用して、エラーを作成および使用します。  
  
 ![例外とエラーの処理](./media/wcfc-soap1-1andsoap1-2faultcomparisonc.gif "wcfc_SOAP1-1AndSOAP1-2FaultComparisonc")  
SOAP 1.2 エラー (左) と SOAP 1.1 エラー (右)。 Fault 要素が名前空間で修飾されているのは SOAP 1.1 のみ。  
  
 SOAP では、エラー メッセージとは、`<env:Fault>` の子要素としてエラー要素 (`<env:Body>` という名前の要素) だけを含むメッセージと定義されています。 図 1 に示すように、エラー要素の内容は SOAP 1.1 と SOAP 1.2 で若干異なります。 ただし、次のように、<xref:System.ServiceModel.Channels.MessageFault?displayProperty=nameWithType> クラスでは、これらの違いを 1 つのオブジェクト モデルに正規化します。  
  
```csharp
public abstract class MessageFault  
{  
    protected MessageFault();  
  
    public virtual string Actor { get; }  
    public virtual string Node { get; }  
    public static string DefaultAction { get; }  
    public abstract FaultCode Code { get; }  
    public abstract bool HasDetail { get; }  
    public abstract FaultReason Reason { get; }  
  
    public T GetDetail<T>();  
    public T GetDetail<T>( XmlObjectSerializer serializer);  
    public System.Xml.XmlDictionaryReader GetReaderAtDetailContents();  
  
    // other methods omitted  
}  
```  
  
 `Code` プロパティは、`env:Code` (または SOAP 1.1 の `faultCode`) に対応し、エラーの種類を識別します。 SOAP 1.2 では、`faultCode` の 5 つの許容値 (Sender や Receiver など) が定義されており、サブコード値を格納できる `Subcode` 要素も定義されています (使用可能なエラーコードとその意味の一覧については、「 [SOAP 1.2 の仕様](https://www.w3.org/TR/soap12-part1/#tabsoapfaultcodes)」を参照してください)。SOAP 1.1 のメカニズムは少し異なります。たとえば、クライアントとサーバーの4つの `faultCode` 値を定義します。これは、まったく新しい値を定義するかドット表記を使用して、クライアント認証など、より具体的な `faultCodes`を作成することによって拡張できます。  
  
 MessageFault を使用してエラーをプログラミングする場合、FaultCode.Name と FaultCode.Namespace は、SOAP 1.2 の `env:Code` または SOAP 1.1 の `faultCode` の名前と名前空間に割り当てられます。 FaultCode.SubCode は、SOAP 1.2 では `env:Subcode` に割り当てられ、SOAP 1.1 では null になります。  
  
 エラーをプログラムによって区別する場合は、新しいエラー サブコード (SOAP 1.1 を使用する場合は新しいエラー コード) を作成します。 これは、新しい例外の種類を作成することに似ています。 SOAP 1.1 エラー コードではドット表記を使用しないようにすることをお勧めします ( [Ws-i Basic Profile](http://www.ws-i.org/Profiles/BasicProfile-1.1-2004-08-24.html#SOAP_Custom_Fault_Codes)では、エラーコードドット表記の使用が推奨されていません)。  
  
```csharp
public class FaultCode  
{  
    public FaultCode(string name);  
    public FaultCode(string name, FaultCode subCode);  
    public FaultCode(string name, string ns);  
    public FaultCode(string name, string ns, FaultCode subCode);  
  
    public bool IsPredefinedFault { get; }  
    public bool IsReceiverFault { get; }  
    public bool IsSenderFault { get; }  
    public string Name { get; }  
    public string Namespace { get; }  
    public FaultCode SubCode { get; }  
  
//  methods omitted  
  
}  
```  
  
 `Reason` プロパティは、`env:Reason` (または SOAP 1.1 の `faultString`) に対応しています。これは、例外のメッセージと同様に、ユーザーが判読できるエラー状態の記述です。 `FaultReason` クラス (および SOAP の `env:Reason/faultString`) には、グローバリゼーションのために、複数の変換を使用するためのサポートが組み込まれています。  
  
```csharp
public class FaultReason  
{  
    public FaultReason(FaultReasonText translation);  
    public FaultReason(IEnumerable<FaultReasonText> translations);  
    public FaultReason(string text);  
  
    public SynchronizedReadOnlyCollection<FaultReasonText> Translations   
    {   
       get;   
    }  
  
 }  
```  
  
 エラーの詳細コンテンツは、`GetDetail`\<T > や `GetReaderAtDetailContents`() などのさまざまなメソッドを使用して、MessageFault で公開されます。 エラーの詳細は、エラーに関する追加情報を伝達するための不透明な要素です。 これは、エラーと共に伝達する任意の構造化された詳細がある場合に役立ちます。  
  
### <a name="generating-faults"></a>エラーの生成  
 このセクションでは、チャネルまたはチャネルによって作成されたメッセージ プロパティで検出されるエラー状態に応じてエラーを生成するプロセスについて説明します。 一般的な例として、無効なデータが含まれた要求メッセージに対するエラーの送信が挙げられます。  
  
 エラーの生成時には、カスタム チャネルからエラーを直接送信するのではなく、例外をスローするようにし、その例外をエラーに変換するかどうかの判断とエラーの送信方法の決定は上位のレイヤーで行います。 この変換を支援するために、チャネルは `FaultConverter` を実装し、カスタム チャネルがスローした例外を適切なエラーに変換できるようにします。 `FaultConverter` は次のように定義します。  
  
```csharp
public class FaultConverter  
{  
    public static FaultConverter GetDefaultFaultConverter(  
                                   MessageVersion version);  
    protected abstract bool OnTryCreateFaultMessage(  
                                   Exception exception,   
                                   out Message message);  
    public bool TryCreateFaultMessage(  
                                   Exception exception,   
                                   out Message message);  
}  
```  
  
 カスタム エラーを生成する各チャネルは `FaultConverter` を実装し、`GetProperty<FaultConverter>` の呼び出しからこれを返す必要があります。 カスタム `OnTryCreateFaultMessage` 実装では、例外をエラーに変換するか、内部チャネルの `FaultConverter` に委任する必要があります。 チャネルがトランスポートの場合は、例外またはデリゲートをエンコーダーの `FaultConverter` に変換するか、WCF で提供される既定の `FaultConverter` に変換する必要があります。 既定の `FaultConverter` は、WS-Addressing および SOAP で指定されたエラー メッセージに対応するエラーを変換します。 `OnTryCreateFaultMessage` 実装の例を次に示します。  
  
```csharp
public override bool OnTryCreateFaultMessage(Exception exception,   
                                             out Message message)  
{  
    if (exception is ...)  
    {  
        message = ...;  
        return true;  
    }  
  
#if IMPLEMENTING_TRANSPORT_CHANNEL  
    FaultConverter encoderConverter =   
                    this.encoder.GetProperty<FaultConverter>();  
    if ((encoderConverter != null) &&               
        (encoderConverter.TryCreateFaultMessage(  
         exception, out message)))  
    {  
        return true;  
    }  
  
    FaultConverter defaultConverter =   
                   FaultConverter.GetDefaultFaultConverter(  
                   this.channel.messageVersion);  
    return defaultConverter.TryCreateFaultMessage(  
                   exception,   
                   out message);  
#else  
    FaultConverter inner =   
                   this.innerChannel.GetProperty<FaultConverter>();  
    if (inner != null)  
    {  
        return inner.TryCreateFaultMessage(exception, out message);  
    }  
    else  
    {  
        message = null;  
        return false;  
    }  
#endif  
}  
```  
  
 このパターンでは、エラーを必要とするエラー状態についてレイヤー間でスローされる例外に、対応するエラー ジェネレーターが適切なエラーを作成できるだけの情報が含まれている必要があります。 カスタム チャネル作成者は、さまざまなエラー状態に対応する例外の種類を定義できます (特定のエラー状態に対応する既存の例外がない場合)。 チャネル レイヤーを越えて伝播する例外では、不明瞭なエラー データではなく、エラー状態を伝える必要があります。  
  
### <a name="fault-categories"></a>エラーのカテゴリ  
 一般に、エラーは次の 3 つのカテゴリに分類されます。  
  
1. スタック全体にわたるエラー。 このようなエラーは、チャネル スタックのどのレイヤーでも発生する可能性があります (例 : InvalidCardinalityAddressingException)。  
  
2. スタックの特定のレイヤーより上位の任意の場所で発生する可能性があるエラー (例 : フローされたトランザクションやセキュリティ ロールに関するエラー)。  
  
3. スタックの単一のレイヤーを対象とするエラー (例 : WS-RM シーケンス番号エラー)。  
  
 カテゴリ1。 一般に、WS-Addressing エラーおよび SOAP エラーです。 WCF によって提供される基本 `FaultConverter` クラスは、WS-ADDRESSING および SOAP によって指定されたエラーメッセージに対応するエラーを変換するため、これらの例外の変換を自分で処理する必要はありません。  
  
 カテゴリ2。 レイヤーに関するメッセージ情報を完全に使用しているわけではないメッセージに対して、そのレイヤーがプロパティを追加したときに発生します。 後で上位のレイヤーがメッセージ情報をさらに処理するために、メッセージ プロパティを要求したときに、エラーが検出されることがあります。 このようなチャネルでは、前述の `GetProperty` を実装して、上位のレイヤーが適切なエラーを返すことができるようにする必要があります。 プロパティの例として、TransactionMessageProperty があります。 このプロパティは、ヘッダー内のすべてのデータが完全には検証されずにメッセージに追加されます (この検証には、分散トランザクション コーディネーター (DTC) への接続が必要になることがあります)。  
  
 カテゴリ 3. エラーは、プロセッサ内の単一のレイヤーによって生成され、送信されるだけです。 したがって、すべての例外はそのレイヤー内に含まれます。 チャネル間の整合性を向上させ、保守を容易にするために、カスタム チャネルでは、前述のパターンを使用して、内部エラーについてもエラー メッセージを生成することをお勧めします。  
  
### <a name="interpreting-received-faults"></a>受信したエラーの解釈  
 このセクションでは、エラー メッセージを受信したときに、適切な例外を生成するためのガイダンスを示します。 スタックのすべてのレイヤーにおけるメッセージ処理に関するデシジョン ツリーを以下に示します。  
  
1. レイヤーがメッセージを無効と見なした場合、レイヤーは "無効なメッセージ" の処理を実行する必要があります。 このような処理はレイヤーに固有のものですが、メッセージの破棄、トレース、エラーに変換される例外のスローなどの処理が可能です。 たとえば、適切に保護されていないメッセージをセキュリティが受信した場合や、RM が不正なシーケンス番号を持つメッセージを受信した場合などです。  
  
2. メッセージが特定のレイヤーに適用されるエラー メッセージであり、そのレイヤーの対話以外では意味がないものである場合、該当のレイヤーがエラー状態を処理する必要があります。 この一例として、RM シーケンス拒否エラーが挙げられます。これは、RM チャネルにエラーが発生し、保留中の操作からスローされていることを示しており、RM チャネルより上位のレイヤーにとっては意味のないエラーです。  
  
3. それ以外の場合は、Request() または Receive() からメッセージを返すようにします。 たとえば、レイヤーはエラーを認識しているが、そのエラーは要求が失敗したことを示しているにすぎず、チャネルにエラーが発生し、保留中の操作からスローされていることを意味しているわけではない場合などです。 このような場合にユーザビリティを向上させるには、レイヤーで `GetProperty<FaultConverter>` を実装し、`FaultConverter` 派生クラスを返すようにします。この派生クラスは、`OnTryCreateException` をオーバーライドすることで、エラーを例外に変換できます。  
  
 次のオブジェクト モデルは、メッセージから例外への変換をサポートします。  
  
```csharp
public class FaultConverter  
{  
    public static FaultConverter GetDefaultFaultConverter(  
                                  MessageVersion version);  
    protected abstract bool OnTryCreateException(  
                                 Message message,   
                                 MessageFault fault,   
                                 out Exception exception);  
    public bool TryCreateException(  
                                 Message message,   
                                 MessageFault fault,   
                                 out Exception exception);  
}  
```  
  
 チャネル レイヤーは、`GetProperty<FaultConverter>` を実装することで、エラー メッセージから例外への変換をサポートできます。 これを行うには、`OnTryCreateException` をオーバーライドし、エラー メッセージを検査します。 エラー メッセージを認識した場合は、変換を実行します。それ以外の場合は、エラー メッセージの変換を内部チャネルに要求します。 トランスポート チャネルでは、`FaultConverter.GetDefaultFaultConverter` に既定の SOAP/WS-Addressing FaultConverter の取得を委任する必要があります。  
  
 一般的な実装は、次のようになります。  
  
```csharp
public override bool OnTryCreateException(  
                            Message message,   
                            MessageFault fault,   
                            out Exception exception)  
{  
    if (message.Action == "...")  
    {  
        exception = ...;  
        return true;  
    }  
    // OR  
    if ((fault.Code.Name == "...") && (fault.Code.Namespace == "..."))  
    {  
        exception = ...;  
        return true;  
    }  
  
    if (fault.IsMustUnderstand)  
    {  
        if (fault.WasHeaderNotUnderstood(  
                   message.Headers, "...", "..."))  
        {  
            exception = new ProtocolException(...);  
            return true;  
        }  
    }  
  
#if IMPLEMENTING_TRANSPORT_CHANNEL  
    FaultConverter encoderConverter =   
              this.encoder.GetProperty<FaultConverter>();  
    if ((encoderConverter != null) &&   
        (encoderConverter.TryCreateException(  
                              message, fault, out exception)))  
    {  
        return true;  
    }  
  
    FaultConverter defaultConverter =  
             FaultConverter.GetDefaultFaultConverter(  
                             this.channel.messageVersion);  
    return defaultConverter.TryCreateException(  
                             message, fault, out exception);  
#else  
    FaultConverter inner =   
                    this.innerChannel.GetProperty<FaultConverter>();  
    if (inner != null)  
    {  
        return inner.TryCreateException(message, fault, out exception);  
    }  
    else  
    {  
        exception = null;  
        return false;  
    }  
#endif  
}  
```  
  
 明確な回復シナリオが用意されたエラー状態については、`ProtocolException` の派生クラスを定義することを検討してください。  
  
### <a name="mustunderstand-processing"></a>MustUnderstand の処理  
 SOAP には、必須のヘッダーが受信側で認識されなかったことを通知するための一般的なエラーが定義されています。 このエラーは、`mustUnderstand` エラーと呼ばれます。 WCF では、カスタムチャネルは `mustUnderstand` エラーを生成しません。 代わりに、wcf 通信スタックの最上位にある WCF ディスパッチャーは、MustUnderstand = true とマークされているすべてのヘッダーが基になるスタックで認識されていることを確認します。 認識されていないヘッダーが見つかった場合、その時点で `mustUnderstand` エラーが生成されます (ユーザーは、この `mustUnderstand` の処理を無効にし、すべてのメッセージ ヘッダーをアプリケーションで受信するようにすることができます。 その場合は、アプリケーションが `mustUnderstand` 処理を実行します)。生成されたエラーには、認識されていない MustUnderstand = true のすべてのヘッダーの名前を含む NotUnderstood ヘッダーが含まれています。  
  
 プロトコル チャネルから MustUnderstand=true でマークされたカスタム ヘッダーを送信し、`mustUnderstand` エラーを受信した場合、そのエラーが送信したヘッダーに起因するものかどうかを確認する必要があります。 `MessageFault` クラスには、このために役立つ 2 つのメンバーが存在します。  
  
```csharp
public class MessageFault  
{  
    ...  
    public bool IsMustUnderstandFault { get; }  
    public static bool WasHeaderNotUnderstood(MessageHeaders headers,   
        string name, string ns) { }  
    ...  
  
}  
```  
  
 エラーが `IsMustUnderstandFault` エラーの場合、`true` は `mustUnderstand` を返します。 指定した名前と名前空間を持つヘッダーが NotUnderstood ヘッダーとしてエラーに含まれている場合、`WasHeaderNotUnderstood` は `true` を返します。  それ以外の場合は、 `false`を返します。  
  
 MustUnderstand = true でマークされたヘッダーをチャネルで出力する場合は、該当のレイヤーも例外生成 API パターンを実装し、出力したヘッダーが原因で発生した `mustUnderstand` エラーを前述のより有用な例外に変換する必要があります。  
  
## <a name="tracing"></a>トレース  
 デバッガーをアタッチしてコードを段階的に実行できない場合に、運用アプリケーションや断続的な問題の診断を支援する 1 つの方法として、.NET Framework にはプログラムの実行をトレースする機構が用意されています。 この機構のコア コンポーネントは <xref:System.Diagnostics?displayProperty=nameWithType> 名前空間にあり、以下で構成されます。  
  
- <xref:System.Diagnostics.TraceSource?displayProperty=nameWithType> は、書き込むトレース情報のソースです。<xref:System.Diagnostics.TraceListener?displayProperty=nameWithType> は、<xref:System.Diagnostics.TraceSource> からトレースする情報を受け取り、リスナー固有の送信先に出力する具象リスナーの抽象基本クラスです。 たとえば、<xref:System.Diagnostics.XmlWriterTraceListener> は、トレース情報を XML ファイルに出力します。 最後に、<xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> を使用すると、アプリケーション ユーザーがトレースの詳細出力レベルを制御できます。通常、このクラスは構成で指定します。  
  
- コアコンポーネントに加えて、[サービストレースビューアーツール (svctraceviewer.exe)](../service-trace-viewer-tool-svctraceviewer-exe.md)を使用して、WCF トレースを表示および検索することもできます。 このツールは、WCF によって生成され、<xref:System.Diagnostics.XmlWriterTraceListener>を使用して出力されるトレースファイル専用に設計されています。 トレースに関与するさまざまなコンポーネントを次の図に示します。  
  
 ![例外とエラーの処理](./media/wcfc-tracinginchannelsc.gif "wcfc_TracingInChannelsc")  
  
### <a name="tracing-from-a-custom-channel"></a>カスタム チャネルからのトレース  
 実行中のアプリケーションにデバッガーをアタッチできないときに問題の診断を支援するために、カスタム チャネルはトレース メッセージを書き込む必要あります。 この場合、<xref:System.Diagnostics.TraceSource> のインスタンス化と、トレースを書き込むためのメソッドの呼び出しの 2 つの高度なタスクが必要となります。  
  
 <xref:System.Diagnostics.TraceSource> をインスタンス化する場合、指定した文字列が対象のソースの名前になります。 この名前を使用して、トレース ソースを構成 (有効化/無効化/トレース レベルの設定) します。 また、トレース出力にもこの名前が表示されます。 カスタム チャネルは、一意のソース名を使用して、トレース出力の利用者にトレース情報のソースがわかるようにする必要があります。 トレース ソースの名前として、情報を書き込むアセンブリの名前を使用するのが一般的です。 たとえば、WCF では、System.servicemodel アセンブリから書き込まれた情報のトレースソースとして System.servicemodel を使用します。  
  
 トレース ソースを用意したら、<xref:System.Diagnostics.TraceSource.TraceData%2A>、<xref:System.Diagnostics.TraceSource.TraceEvent%2A>、または <xref:System.Diagnostics.TraceSource.TraceInformation%2A> の各メソッドを呼び出して、トレース エントリをトレース リスナーに書き込みます。 書き込む各トレース エントリのイベントの種類は、<xref:System.Diagnostics.TraceEventType> に定義されたイベントの種類のいずれかとして分類する必要があります。 この分類と構成でのトレース レベルの設定によって、トレース エントリをリスナーに出力するかどうかが決まります。 たとえば、構成でトレース レベルを `Warning` に設定すると、`Warning`、`Error`、および `Critical` の各トレース エントリを書き込むことができますが、Information エントリと Verbose エントリはブロックされます。 トレース ソースをインスタンス化し、Information レベルでエントリを書き込む例を次に示します。  
  
```csharp
using System.Diagnostics;  
//...  
TraceSource udpSource = new TraceSource("Microsoft.Samples.Udp");  
//...  
udpsource.TraceInformation("UdpInputChannel received a message");  
```  
  
> [!IMPORTANT]
> カスタム チャネル固有のトレース ソース名を指定して、トレース出力の利用者に出力のソースがわかるようにすることを強くお勧めします。  
  
#### <a name="integrating-with-the-trace-viewer"></a>トレース ビューアーとの統合  
 チャネルによって生成されるトレースは、トレースリスナーとして <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType> を使用することによって、[サービストレースビューアーツール (svctraceviewer.exe)](../service-trace-viewer-tool-svctraceviewer-exe.md)で読み取り可能な形式で出力できます。 これは、チャネル開発者が行う必要はありません。 アプリケーション ユーザー (またはアプリケーションのトラブルシューティング担当者) が、アプリケーションの構成ファイルでこのトレース リスナーを構成する必要があります。 たとえば、次の構成では、<xref:System.ServiceModel?displayProperty=nameWithType> と `Microsoft.Samples.Udp` の両方から `TraceEventsFile.e2e` というファイルにトレース情報が出力されます。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <!-- configure System.ServiceModel trace source -->  
      <source name="System.ServiceModel" switchValue="Verbose"   
              propagateActivity="true">  
        <listeners>  
          <add name="e2e" />  
        </listeners>  
      </source>  
      <!-- configure Microsoft.Samples.Udp trace source -->  
      <source name="Microsoft.Samples.Udp" switchValue="Verbose" >  
        <listeners>  
          <add name="e2e" />  
        </listeners>  
      </source>  
    </sources>  
    <!--   
    Define a shared trace listener that outputs to TraceFile.e2e  
    The listener name is e2e   
    -->  
    <sharedListeners>  
      <add name="e2e" type="System.Diagnostics.XmlWriterTraceListener"  
        initializeData=".\TraceFile.e2e"/>  
    </sharedListeners>  
    <trace autoflush="true" />  
  </system.diagnostics>  
</configuration>  
```  
  
#### <a name="tracing-structured-data"></a>構造化されたデータのトレース  
 <xref:System.Diagnostics.TraceSource?displayProperty=nameWithType> には、トレース エントリに含まれる 1 つ以上のオブジェクトを取得する <xref:System.Diagnostics.TraceSource.TraceData%2A> メソッドがあります。 通常、<xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドは各オブジェクトに対して呼び出され、生成された文字列はトレース エントリの一部として書き込まれます。 <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType> を使用してトレースを出力する場合、データ オブジェクトとして <xref:System.Xml.XPath.IXPathNavigable?displayProperty=nameWithType> を <xref:System.Diagnostics.TraceSource.TraceData%2A> に渡すことができます。 生成されるトレース エントリには、<xref:System.Xml.XPath.XPathNavigator?displayProperty=nameWithType> によって提供された XML が含まれます。 XML アプリケーション データが含まれたエントリの例を次に示します。  
  
```xml  
<E2ETraceEvent xmlns="http://schemas.microsoft.com/2004/06/E2ETraceEvent">  
  <System xmlns="...">  
    <EventID>12</EventID>  
    <Type>3</Type>  
    <SubType Name="Information">0</SubType>  
    <Level>8</Level>  
    <TimeCreated SystemTime="2006-01-13T22:58:03.0654832Z" />  
    <Source Name="Microsoft.ServiceModel.Samples.Udp" />  
    <Correlation ActivityID="{00000000-0000-0000-0000-000000000000}" />  
    <Execution  ProcessName="UdpTestConsole"   
                ProcessID="3348" ThreadID="4" />  
    <Channel />  
    <Computer>COMPUTER-LT01</Computer>  
  </System>  
<!-- XML application data -->  
  <ApplicationData>  
  <TraceData>  
   <DataItem>  
   <TraceRecord   
     Severity="Information"  
     xmlns="…">  
        <TraceIdentifier>some trace id</TraceIdentifier>  
        <Description>EndReceive called</Description>  
        <AppDomain>UdpTestConsole.exe</AppDomain>  
        <Source>UdpInputChannel</Source>  
      </TraceRecord>  
    </DataItem>  
  </TraceData>  
  </ApplicationData>  
</E2ETraceEvent>  
```  
  
 WCF トレースビューアーは、前に示した `TraceRecord` 要素のスキーマを認識し、その子要素からデータを抽出して、表形式で表示します。 チャネルで構造化されたアプリケーション データをトレースするときには、このスキーマを使用して Svctraceviewer.exe のユーザーがデータを判読しやすいようにします。
