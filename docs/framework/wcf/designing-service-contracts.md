---
title: サービス コントラクトの設計
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF]
ms.assetid: 8e89cbb9-ac84-4f0d-85ef-0eb6be0022fd
ms.openlocfilehash: 27f867bbf079c2e202d93425ddb951fc0df7784a
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72318382"
---
# <a name="designing-service-contracts"></a>サービス コントラクトの設計
ここでは、サービス コントラクトの概要、定義方法、使用できる操作 (および基になるメッセージ交換の影響)、使用するデータ型、およびシナリオの要件を満たす操作を設計する際に役立つその他の問題について説明します。  
  
## <a name="creating-a-service-contract"></a>サービス コントラクトの作成  
 サービスは複数の操作を公開します。 Windows Communication Foundation (WCF) アプリケーションでは、メソッドを作成し、<xref:System.ServiceModel.OperationContractAttribute> 属性を使用して操作を定義します。 次に、サービス コントラクトを作成するために、<xref:System.ServiceModel.ServiceContractAttribute> 属性でマークされたインターフェイス内で操作を宣言するか、この属性でマークされたクラス内で操作を定義することにより、操作をグループ化します (基本的な例については、「[方法: サービスコントラクトを定義](how-to-define-a-wcf-service-contract.md)する」を参照してください)。  
  
 <xref:System.ServiceModel.OperationContractAttribute> 属性を持たないメソッドはサービス操作ではなく、WCF サービスによって公開されません。  
  
 ここでは、サービス コントラクトの設計時に決定すべき以下のポイントについて説明します。  
  
- クラスとインターフェイスのどちらを使用するか  
  
- 交換するデータ型の指定方法  
  
- 使用できる交換パターンの種類  
  
- コントラクトのセキュリティ要件の部分を明示的に作成できるかどうか  
  
- 操作の入力と出力の制限  
  
## <a name="classes-or-interfaces"></a>クラスとインターフェイス  
 クラスとインターフェイスはどちらも機能のグループ化を表し、その両方を使用して WCF サービスコントラクトを定義できます。 ただし、インターフェイスはサービス コントラクトを直接モデル化するため、インターフェイスを使用することをお勧めします。 実装のないインターフェイスは、特定のシグネチャを持つメソッドのグループ化を定義しているにすぎません。 サービスコントラクトインターフェイスを実装し、WCF サービスを実装しました。  
  
 サービス コントラクト インターフェイスには、次のようにマネージド インターフェイスのあらゆる利点がもたらされます。  
  
- サービス コントラクト インターフェイスでは、他のサービス コントラクト インターフェイスをいくつでも拡張できます。  
  
- これらのサービス コントラクト インターフェイスを実装することにより、1 つのクラスでサービス コントラクトをいくつでも実装できます。  
  
- インターフェイスの実装を変更することにより、同じサービス コントラクトを引き続き使用したまま、そのコントラクトの実装を変更できます。  
  
- 以前のインターフェイスと新しいインターフェイスを実装することで、サービスをバージョン管理できます。 以前のクライアントは元のバージョンに接続し、新しいクライアントは新しいバージョンに接続できます。  
  
> [!NOTE]
> 他のサービス コントラクト インターフェイスから継承した場合、操作のプロパティ (名前や名前空間など) をオーバーライドすることはできません。 これを行う場合は、現在のサービス コントラクトに新しい操作を作成します。  
  
 インターフェイスを使用してサービスコントラクトを作成する例については、「[方法: コントラクトインターフェイスを使用してサービスを作成](./feature-details/how-to-create-a-service-with-a-contract-interface.md)する」を参照してください。  
  
 クラスを使用すると、サービス コントラクトの定義と実装を一度に行うことができます。 <xref:System.ServiceModel.ServiceContractAttribute> と <xref:System.ServiceModel.OperationContractAttribute> をそれぞれクラスとクラスのメソッドに直接適用してサービスを作成する方法には、サービスを迅速かつ簡単に作成できるという利点があります。 欠点は、マネージド クラスでは複数の継承をサポートしていないため、サービス コントラクトを一度に 1 つしか実装できないことです。 また、クラスまたはメソッド シグネチャに変更を加えると、そのサービスのパブリック コントラクトが変更されるため、変更されていないクライアントがサービスを使用できなくなることがあります。 詳細については、「[サービスコントラクトの実装](implementing-service-contracts.md)」を参照してください。  
  
 クラスを使用してサービスコントラクトを作成し、同時に実装する例については、「[方法: コントラクトクラスを使用してサービスを作成](./feature-details/how-to-create-a-wcf-contract-with-a-class.md)する」を参照してください。  
  
 これで、サービス コントラクトを定義する際に、インターフェイスを使用した場合とクラスを使用した場合の違いがわかりました。 次に、サービスとクライアント間で受け渡しできるデータを決定します。  
  
## <a name="parameters-and-return-values"></a>パラメーターと戻り値  
 各操作は戻り値とパラメーターを持ちます (戻り値とパラメーターが `void` の場合も含まれます)。 ただし、オブジェクトへの参照をオブジェクト間で渡すことができるローカル メソッドとは異なり、サービス操作ではオブジェクトへの参照を渡しません。 代わりに、オブジェクトのコピーを渡します。  
  
 これが重要なのは、パラメーターまたは戻り値で使用される各型がシリアル化可能でなければならないためです。つまり、その型のオブジェクトをバイト ストリームに変換でき、バイト ストリームからオブジェクトに変換できる必要があります。  
  
 プリミティブ型は既定でシリアル化可能であり、.NET Framework の多くの型も同様です。  
  
> [!NOTE]
> 操作シグネチャのパラメーター名の値はコントラクトに含まれ、大文字と小文字が区別されます。 ローカルでは同じパラメーター名を使用するが、公開されるメタデータでは名前を変更する場合は、<xref:System.ServiceModel.MessageParameterAttribute?displayProperty=nameWithType> を参照してください。  
  
#### <a name="data-contracts"></a>データ コントラクト  
 Windows Communication Foundation (WCF) アプリケーションなどのサービス指向アプリケーションは、Microsoft と Microsoft 以外の両方のプラットフォームで最も多くのクライアントアプリケーションと相互運用できるように設計されています。 最大限の相互運用性を実現するために、使用する型を <xref:System.Runtime.Serialization.DataContractAttribute> 属性と <xref:System.Runtime.Serialization.DataMemberAttribute> 属性でマークして、データ コントラクトを作成することをお勧めします。データ コントラクトは、サービス コントラクトの一部であり、サービス操作で交換するデータを記述したものです。  
  
 データ コントラクトは opt-in 方式のコントラクトです。つまり、データ コントラクト属性を明示的に適用しない限り、型またはデータ メンバーはシリアル化されません。 データ コントラクトはマネージド コードのアクセス スコープとして関連付けられていません。プライベートのデータ メンバーはシリアル化され、パブリックにアクセスされる他の場所に送信されます (データコントラクトの基本的な例については、「[方法: クラスまたは構造体の基本的なデータコントラクトを作成](./feature-details/how-to-create-a-basic-data-contract-for-a-class-or-structure.md)する」を参照してください)。WCF では、基になる SOAP メッセージの定義を処理します。これにより、操作の機能を有効にすると共に、メッセージの本文との間でデータ型をシリアル化することができます。 使用するデータ型がシリアル化可能であれば、操作の設計時に、基盤となるメッセージ交換インフラストラクチャについて考える必要はありません。  
  
 一般的な WCF アプリケーションでは、<xref:System.Runtime.Serialization.DataContractAttribute> と <xref:System.Runtime.Serialization.DataMemberAttribute> の属性を使用して操作のデータコントラクトを作成しますが、他のシリアル化メカニズムを使用することもできます。 <xref:System.Runtime.Serialization.ISerializable>、<xref:System.SerializableAttribute>、および <xref:System.Xml.Serialization.IXmlSerializable> の各標準機構はすべて、基になる SOAP メッセージへのデータ型のシリアル化を処理します。このメッセージはアプリケーション間でデータ型を伝達します。 使用するデータ型で特別なサポートが必要な場合は、さらに多くのシリアル化方法を使用できます。 WCF アプリケーションでのデータ型のシリアル化の選択肢の詳細については、「[サービスコントラクトでのデータ転送の指定](./feature-details/specifying-data-transfer-in-service-contracts.md)」を参照してください。  
  
#### <a name="mapping-parameters-and-return-values-to-message-exchanges"></a>メッセージ交換へのパラメーターと戻り値のマッピング  
 サービス操作は、特定の標準セキュリティ、トランザクション、およびセッション関連の機能をサポートするためにアプリケーションが必要とするデータに加え、アプリケーション データをやり取りする SOAP メッセージの基になる交換によってサポートされます。 この場合、サービス操作の署名によって、データ転送をサポートできる特定の基になる*メッセージ交換パターン*(mep) と、操作に必要な機能が決まります。 WCF プログラミングモデルでは、要求/応答、一方向、および双方向のメッセージパターンの3つのパターンを指定できます。  
  
##### <a name="requestreply"></a>要求/応答  
 要求/応答パターンでは、要求の送信側 (クライアント アプリケーション) は、その要求に関連付けられた応答を受信します。 このパターンでは、1 つの操作において、1 つ以上のパラメーターを操作に渡し、戻り値を呼び出し元に返すことができるため、このパターンが既定の MEP となります。 たとえば、次の C# コード例は、文字列を 1 つ受け取り、文字列を返す基本的なサービス操作を示しています。  
  
```csharp  
[OperationContractAttribute]  
string Hello(string greeting);  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<OperationContractAttribute()>  
Function Hello (ByVal greeting As String) As String  
```  
  
 この操作シグネチャは、基になるメッセージ交換の形式を指定しています。 関連付けが存在しない場合、WCF は戻り値の対象となる操作を特定できません。  
  
 別の基になるメッセージパターンを指定していない場合は、`void` (Visual Basic で`Nothing`) を返すサービス操作でも、要求/応答メッセージ交換が行われることに注意してください。 クライアントが操作を非同期で呼び出していない場合、通常、メッセージが空の場合でも、戻りメッセージを受信するまでクライアントは処理を中止します。 クライアントが応答で空のメッセージを受信するまで制御が戻らない操作の C# コード例を次に示します。  
  
```csharp  
[OperationContractAttribute]  
void Hello(string greeting);  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<OperationContractAttribute()>  
Sub Hello (ByVal greeting As String)  
```  
  
 上記の例では、実行に時間のかかる操作の場合に、クライアントのパフォーマンスと応答性が低下するおそれがありますが、要求/応答操作で `void` を返す場合でも、この操作には利点があります。 最も明らかな利点は、応答メッセージで SOAP エラーを返すことが可能であるということです。これにより、通信と処理のどちらで発生したかに関係なく、サービス関連の何らかのエラー状態が発生したことがわかります。 サービス コントラクトに指定された SOAP エラーは、<xref:System.ServiceModel.FaultException%601> オブジェクトとしてクライアント アプリケーションに渡されます。このオブジェクトの型パラメーターは、サービス コントラクトで指定された型です。 これにより、クライアントに WCF サービスのエラー状態が簡単に通知されるようになります。 例外、SOAP エラー、およびエラー処理の詳細については、「[コントラクトとサービスのエラーの指定と処理](specifying-and-handling-faults-in-contracts-and-services.md)」を参照してください。 要求/応答サービスとクライアントの例については、「[方法: 要求/応答コントラクトを作成](./feature-details/how-to-create-a-request-reply-contract.md)する」を参照してください。 要求/応答パターンに関する問題の詳細については、「[要求/応答サービス](./feature-details/request-reply-services.md)」を参照してください。  
  
##### <a name="one-way"></a>一方向  
 WCF サービスアプリケーションのクライアントが操作の完了を待機せず、SOAP エラーを処理しない場合、操作は一方向のメッセージパターンを指定できます。 一方向の操作とは、クライアントが操作を呼び出し、WCF によってメッセージがネットワークに書き込まれた後も処理を続行する操作です。 通常、これは、送信メッセージで送信するデータが膨大な量でない限り、(データ送信時にエラーが発生しなければ) クライアントはほぼすぐに実行を続けることを意味します。 この種のメッセージ交換パターンでは、クライアントからサービス アプリケーションへのイベントのような動作をサポートします。  
  
 1 つのメッセージを送信し、何も受信しないメッセージ交換では、`void` 以外の戻り値を指定したサービス操作をサポートすることはできません。この場合、<xref:System.InvalidOperationException> 例外がスローされます。  
  
 戻りメッセージがないということは、処理または通信時のエラーを示すために SOAP エラーを返すこともできないということです (操作が一方向操作の場合にエラー情報を伝達するには、双方向メッセージ交換パターンが必要です)。  
  
 `void` を返す操作で一方向メッセージ交換を指定するには、次の C# コード例に示すように、<xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> プロパティを `true` に設定します。  
  
```csharp  
[OperationContractAttribute(IsOneWay=true)]  
void Hello(string greeting);  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<OperationContractAttribute(IsOneWay := True)>  
Sub Hello (ByVal greeting As String)  
```  
  
 このメソッドは、前述の要求/応答の例と同じです。ただし、<xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> プロパティを `true` に設定するということは、メソッドは同じでも、サービス操作は戻りメッセージを送信せず、送信メッセージがチャネル レイヤーに渡されると、すぐにクライアントに制御が戻ることを意味します。 例については、「[方法: 一方向コントラクトを作成](./feature-details/how-to-create-a-one-way-contract.md)する」を参照してください。 一方向のパターンの詳細については、「[一方向サービス](./feature-details/one-way-services.md)」を参照してください。  
  
##### <a name="duplex"></a>二重  
 双方向パターンの特徴は、一方向メッセージングと要求/応答メッセージングのどちらを使用しているかに関係なく、サービスとクライアントが共に独立して、相互にメッセージを送信できるという点です。 二方向通信のこの形式は、サービスがクライアントと直接通信する必要がある場合や、イベントのような動作など、メッセージを交換するどちらの側も非同期で動作できるようにする場合に役立ちます。  
  
 双方向パターンでは、クライアントと通信するための機構が別途必要になるため、要求/応答パターンや一方向パターンに比べると若干複雑になります。  
  
 双方向コントラクトを設計する場合、コールバック コントラクトも設計し、そのコールバック コントラクトの型を、サービス コントラクトをマークする <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A> 属性の <xref:System.ServiceModel.ServiceContractAttribute> プロパティに割り当てる必要があります。  
  
 双方向パターンを実装するには、クライアントで呼び出されるメソッド宣言を含む 2 つ目のインターフェイスを作成する必要があります。  
  
 サービスを作成する例と、そのサービスにアクセスするクライアントの例については、「[方法: 双方向コントラクトを作成](./feature-details/how-to-create-a-duplex-contract.md)する」および「[方法: 双方向コントラクトを使用してサービスにアクセスする](./feature-details/how-to-access-services-with-a-duplex-contract.md)」を参照してください。 実際のサンプルについては、「[双](./samples/duplex.md)方向」を参照してください。 双方向コントラクトの使用に関する問題の詳細については、「[双方向サービス](./feature-details/duplex-services.md)」を参照してください。  
  
> [!CAUTION]
> サービスは、双方向メッセージを受信すると、その受信メッセージの `ReplyTo` 要素を参照して応答の送信先を決定します。 メッセージの受信に使用するチャネルがセキュリティで保護されていない場合、信頼関係のないクライアントが対象コンピューターの `ReplyTo` を使用して悪意のあるメッセージを送信し、その対象コンピューターのサービス拒否 (DOS: Denial Of Service) を引き起こすおそれがあります。  
  
##### <a name="out-and-ref-parameters"></a>Out パラメーターと Ref パラメーター  
 ほとんどの場合、パラメーター (Visual Basic では`ByVal`) を使用して、`out` および `ref` のパラメーター (`ByRef` で Visual Basic) を `in` できます。 `out` パラメーターと `ref` パラメーターは、操作からデータが返されることを示すため、操作シグネチャが `void` を返す場合でも、次のような操作シグネチャによって要求/応答操作が必要であることを指定します。  
  
```csharp  
[ServiceContractAttribute]  
public interface IMyContract  
{  
  [OperationContractAttribute]  
  public void PopulateData(ref CustomDataType data);  
}  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface IMyContract  
  <OperationContractAttribute()> _  
  Public Sub PopulateData(ByRef data As CustomDataType)  
End Interface  
```  
  
 唯一の例外は、シグネチャに特定の構造体が含まれている場合です。 たとえば、<xref:System.ServiceModel.NetMsmqBinding> バインディングを使用してクライアントと通信できるのは、操作の宣言に使用されたメソッドが `void` を返す場合だけです。戻り値、`ref` パラメーター、または `out` パラメーターのいずれであるかに関係なく、出力値を指定することはできません。  
  
 また、`out` パラメーターまたは `ref` パラメーターを使用する場合、操作には基になる応答メッセージが必要です。このメッセージが変更されたオブジェクトを返します。 操作が一方向操作の場合、実行時に <xref:System.InvalidOperationException> 例外がスローされます。  
  
### <a name="specify-message-protection-level-on-the-contract"></a>コントラクトでのメッセージ保護レベルの指定  
 コントラクトの設計時に、コントラクトを実装するサービスのメッセージ保護レベルも決定する必要があります。 これは、メッセージ セキュリティをコントラクトのエンドポイントのバインディングに適用する場合にのみ必要です。 バインディングのセキュリティが無効になっている場合 (つまり、システム指定のバインディングで <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType> の値が <xref:System.ServiceModel.SecurityMode.None?displayProperty=nameWithType> に設定されている場合)、コントラクトのメッセージ保護レベルを決定する必要はありません。 ほとんどの場合、メッセージ レベルのセキュリティが適用されたシステム指定のバインディングは、十分な保護レベルを備えているため、操作ごとまたはメッセージごとに保護レベルを検討する必要はありません。  
  
 保護レベルは、サービスをサポートするメッセージ (またはメッセージ部分) が署名されるのか、署名および暗号化されるのか、または署名と暗号化なしで送信されるのかを指定する値です。 保護レベルは、さまざまなスコープ (サービス レベル、特定の操作、その操作内のメッセージ、またはメッセージ部分) で設定できます。 あるスコープで設定された値は、明示的にオーバーライドしない限り、そのスコープよりも小さなスコープの既定値になります。 コントラクトに必要とされる最小限の保護レベルをバインド構成で提供できない場合は、例外がスローされます。 保護レベルの値がコントラクトで明示的に設定されていない場合、バインディングのメッセージ セキュリティが有効であれば、バインド構成によってすべてのメッセージの保護レベルが制御されます。 これが既定の動作です。  
  
> [!IMPORTANT]
> 一般に、コントラクトのさまざまなスコープを完全な保護レベルである <xref:System.Net.Security.ProtectionLevel.EncryptAndSign?displayProperty=nameWithType> よりも下のレベルに明示的に設定するかどうかは、パフォーマンスの向上と引き換えに、ある程度のセキュリティで妥協できるかどうかという判断によって決まります。 このような場合、操作および操作で交換するデータの価値に焦点を絞って判断を下す必要があります。 詳細については、「[サービスのセキュリティ保護](securing-services.md)」を参照してください。  
  
 たとえば、次のコード例では、<xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> も、コントラクトの <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A> プロパティも設定していません。  
  
```csharp  
[ServiceContract]  
public interface ISampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute]  
  public int GetInt();    
}  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface ISampleService  
  
  <OperationContractAttribute()> _  
  Public Function GetString()As String  
  
  <OperationContractAttribute()> _  
  Public Function GetData() As Integer  
  
End Interface  
```  
  
 既定の `ISampleService` (既定の <xref:System.ServiceModel.WSHttpBinding> は <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType>) を使用するエンドポイントの <xref:System.ServiceModel.SecurityMode.Message> 実装とやり取りする場合は、暗号化と署名が既定の保護レベルであるため、すべてのメッセージが暗号化および署名されます。 ただし、既定の `ISampleService` (既定の <xref:System.ServiceModel.BasicHttpBinding> は <xref:System.ServiceModel.SecurityMode>) を使用して <xref:System.ServiceModel.SecurityMode.None> サービスを使用すると、すべてのメッセージがテキストとして送信されます。これは、このバインディングにはセキュリティがないため、保護レベルが無視されるためです (つまり、メッセージの暗号化も署名も行われません)。 <xref:System.ServiceModel.SecurityMode> を <xref:System.ServiceModel.SecurityMode.Message> に変更すると、これがこのバインディングの既定の保護レベルになるため、これらのメッセージは暗号化および署名されるようになります。  
  
 コントラクトの保護要件を明示的に指定または調整する場合は、<xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> プロパティ (またはより小さなスコープの `ProtectionLevel` プロパティのいずれか) をサービス コントラクトに必要なレベルに設定します。 この場合、明示的な設定を使用するには、使用するスコープに少なくともその設定をサポートするバインディングが必要です。 たとえば、次のコード例では、<xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A> 操作の `GetGuid` の値を明示的に指定しています。  
  
```csharp  
[ServiceContract]  
public interface IExplicitProtectionLevelSampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.None)]  
  public int GetInt();    
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.EncryptAndSign)]  
  public int GetGuid();    
}  
```  
  
 次のコードは同等の Visual Basic コードです。  
  
```vb  
<ServiceContract()> _   
Public Interface IExplicitProtectionLevelSampleService   
    <OperationContract()> _   
    Public Function GetString() As String   
    End Function   
  
    <OperationContract(ProtectionLevel := ProtectionLevel.None)> _   
    Public Function GetInt() As Integer   
    End Function   
  
    <OperationContractAttribute(ProtectionLevel := ProtectionLevel.EncryptAndSign)> _   
    Public Function GetGuid() As Integer   
    End Function   
  
End Interface  
```  
  
 この `IExplicitProtectionLevelSampleService` コントラクトを実装し、既定の <xref:System.ServiceModel.WSHttpBinding> (既定の <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType> は <xref:System.ServiceModel.SecurityMode.Message>) を使用するエンドポイントを持つサービスの動作は次のようになります。  
  
- `GetString` 操作のメッセージは、暗号化および署名されます。  
  
- `GetInt` 操作のメッセージは、暗号化も署名もされない (プレーン) テキストとして送信されます。  
  
- `GetGuid` 操作の <xref:System.Guid?displayProperty=nameWithType> は、暗号化および署名されたメッセージで返されます。  
  
 保護レベルとその使用方法の詳細については、「[保護レベル](understanding-protection-level.md)について」を参照してください。 セキュリティの詳細については、「サービスのセキュリティ[保護](securing-services.md)」を参照してください。  
  
##### <a name="other-operation-signature-requirements"></a>操作シグネチャのその他の要件  
 アプリケーションの一部の機能では、特定の種類の操作シグネチャを必要とします。 たとえば、<xref:System.ServiceModel.NetMsmqBinding> バインディングは、永続的なサービスとクライアントをサポートします。永続的なサービスとクライアントでは、通信の途中でアプリケーションを再起動し、メッセージを失うことなく、アプリケーションが中止された場所を検出できます (詳細については、「 [WCF のキュー](./feature-details/queues-in-wcf.md)」を参照してください)。ただし、持続性のある操作では、`in` パラメーターを1つだけ受け取り、戻り値を指定することはできません。  
  
 もう 1 つの例として、操作における <xref:System.IO.Stream> 型の使用が挙げられます。 <xref:System.IO.Stream> パラメーターにはメッセージの本文全体が含まれるため、入力または出力 (つまり、`ref` パラメーター、`out` パラメーター、または戻り値) が <xref:System.IO.Stream> 型である場合、操作で指定された入力または出力に限定する必要があります。 また、パラメーターまたは戻り値の型は <xref:System.IO.Stream>、<xref:System.ServiceModel.Channels.Message?displayProperty=nameWithType>、<xref:System.Xml.Serialization.IXmlSerializable?displayProperty=nameWithType> のいずれかである必要があります。 ストリームの詳細については、「 [Large Data And Streaming](./feature-details/large-data-and-streaming.md)」を参照してください。  
  
##### <a name="names-namespaces-and-obfuscation"></a>名前、名前空間、および隠ぺい  
 コントラクトおよび操作の定義内の .NET 型の名前や名前空間は、コントラクトが WSDL に変換されるとき、およびコントラクト メッセージが作成および送信されるときに重要になります。 したがって、サービス コントラクトの名前と名前空間は、`Name`、`Namespace`、<xref:System.ServiceModel.ServiceContractAttribute>、<xref:System.ServiceModel.OperationContractAttribute> などの、すべてのサポート対象コントラクト属性や、他のコントラクト属性の <xref:System.Runtime.Serialization.DataContractAttribute> プロパティと <xref:System.Runtime.Serialization.DataMemberAttribute> プロパティを使用して明示的に設定することを強くお勧めします。  
  
 この 1 つの結果として、名前と名前空間が明示的に設定されていない場合、アセンブリで IL 難読化を使用すると、コントラクトの型名と名前空間が変更され、その結果、WSDL が変更され、通常はネットワークでのメッセージ交換に失敗します。 コントラクトの名前と名前空間を明示的に設定せずに難読化を使用する場合は、<xref:System.Reflection.ObfuscationAttribute> 属性と <xref:System.Reflection.ObfuscateAssemblyAttribute> 属性を使用して、コントラクトの型名と名前空間が変更されないようにします。  
  
## <a name="see-also"></a>参照

- [方法 : 要求/応答コントラクトを作成する](./feature-details/how-to-create-a-request-reply-contract.md)
- [方法 : 一方向コントラクトを作成する](./feature-details/how-to-create-a-one-way-contract.md)
- [方法 : 双方向コントラクトを作成する](./feature-details/how-to-create-a-duplex-contract.md)
- [Specifying Data Transfer in Service Contracts](./feature-details/specifying-data-transfer-in-service-contracts.md)
- [コントラクトおよびサービスのエラーの指定と処理](specifying-and-handling-faults-in-contracts-and-services.md)
- [セッションの使用](using-sessions.md)
- [同期操作と非同期操作](synchronous-and-asynchronous-operations.md)
- [信頼できるサービス](reliable-services.md)
- [サービスとトランザクション](services-and-transactions.md)
