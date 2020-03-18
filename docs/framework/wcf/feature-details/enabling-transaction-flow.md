---
title: トランザクション フローの有効化
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], enabling flow
ms.assetid: a03f5041-5049-43f4-897c-e0292d4718f7
ms.openlocfilehash: 8aff6afb09c97d7d01f5e7b7f1b92ae24bb99fb7
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141753"
---
# <a name="enabling-transaction-flow"></a>トランザクション フローの有効化
Windows Communication Foundation (WCF) では、トランザクションフローを制御するための柔軟なオプションが用意されています。 サービスのトランザクション フローの設定は、属性と構成の組み合わせを使用して表すことができます。  
  
## <a name="transaction-flow-settings"></a>トランザクション フローの設定  
 トランザクション フローの設定は、次の 3 つの値の積集合の結果として、サービス エンドポイントに対して生成されます。  
  
- サービス コントラクトの各メソッドに指定された <xref:System.ServiceModel.TransactionFlowAttribute> 属性。  
  
- 特定のバインディングの `TransactionFlow` バインディング プロパティ。  
  
- 特定のバインディングの `TransactionFlowProtocol` バインディング プロパティ。 `TransactionFlowProtocol` バインディング プロパティでは、トランザクションをフローさせるために使用できる 2 つのトランザクション プロトコルのいずれかを選択できます。 次のセクションでは、これらのプロパティについてそれぞれ簡単に説明します。  
  
### <a name="ws-atomictransaction-protocol"></a>WS-AtomicTransaction プロトコル  
 サード パーティのプロトコル スタックとの相互運用性が必要なシナリオでは、WS-AT (WS-AtomicTransaction) プロトコルが有用です。  
  
### <a name="oletransactions-protocol"></a>OleTransactions プロトコル  
 OleTransactions プロトコルは、サードパーティのプロトコル スタックとの相互運用性が必要ではなく、WS-AT プロトコル サービスがローカルで無効になっていること、または既存のネットワーク トポロジが WS-AT の使用に向いていないことがサービスを配置する前に既にわかっている場合に有効です。  
  
 これらのさまざまな組み合わせを使用して生成できるトランザクション フローの種類を次の表に示します。  
  
|TransactionFlow<br /><br /> バインド|TransactionFlow バインディング プロパティ|TransactionFlowProtocol バインディング プロパティ|トランザクション フローの種類|  
|---------------------------------|--------------------------------------|----------------------------------------------|------------------------------|  
|必須|TRUE|WS-AT|トランザクションは、相互運用可能な WS-AT 形式でフローさせる必要があります。|  
|必須|TRUE|OleTransactions|トランザクションは、WCF OleTransactions 形式でフローさせる必要があります。|  
|必須|False|利用不可|この構成は無効なため、適用できません。|  
|Allowed|TRUE|WS-AT|トランザクションは、相互運用可能な WS-AT 形式でフローさせることができます。|  
|Allowed|TRUE|OleTransactions|トランザクションは、WCF OleTransactions 形式でフローさせることができます。|  
|Allowed|False|任意の値|トランザクションは送信されません。|  
|NotAllowed|任意の値|任意の値|トランザクションは送信されません。|  
  
 メッセージ処理の結果を次の表にまとめます。  
  
|受信メッセージ|トランザクション フローの設定|トランザクション ヘッダー|メッセージ処理の結果|  
|----------------------|-----------------------------|------------------------|-------------------------------|  
|トランザクションは予期されたプロトコル形式に一致します|Allowed または Mandatory|`MustUnderstand` と `true` は等しい。|プロセス|  
|トランザクションは予期されたプロトコル形式に一致しません|必須|`MustUnderstand` と `false` は等しい。|トランザクションが必須のため拒否|  
|トランザクションは予期されたプロトコル形式に一致しません|Allowed|`MustUnderstand` と `false` は等しい。|ヘッダーが認識されないため拒否|  
|任意のプロトコル形式を使用しているトランザクション|NotAllowed|`MustUnderstand` と `false` は等しい。|ヘッダーが認識されないため拒否|  
|トランザクションなし|必須|N/A|トランザクションが必須のため拒否|  
|トランザクションなし|Allowed|N/A|プロセス|  
|トランザクションなし|NotAllowed|N/A|プロセス|  
  
 コントラクトの各メソッドには、トランザクション フローに関するさまざまな要件を割り当てることができますが、トランザクション フローのプロトコル設定のスコープは、バインディングのレベルになります。 このため、同じエンドポイント (ひいては同じバインディング) を共有するすべてのメソッドは、トランザクション フローを許可または必要とする同じポリシー、および同じトランザクション プロトコル (該当する場合) を共有します。  
  
## <a name="enabling-transaction-flow-at-the-method-level"></a>メソッド レベルでのトランザクション フローの有効化  
 トランザクション フローの要件は、サービス コントラクトのすべてのメソッドで常に同じであるとは限りません。 そのため、WCF では、各メソッドのトランザクションフローの基本設定を表すことができる属性ベースの機構も提供されます。 これは、サービス操作がトランザクション ヘッダーを受け入れるレベルを指定する <xref:System.ServiceModel.TransactionFlowAttribute> によって実現されます。 トランザクション フローを有効にする場合は、この属性を使用してサービス コントラクト メソッドをマークする必要があります。 この属性は、<xref:System.ServiceModel.TransactionFlowOption> 列挙値のいずれかを取り、既定値は <xref:System.ServiceModel.TransactionFlowOption.NotAllowed> です。 <xref:System.ServiceModel.TransactionFlowOption.NotAllowed> 以外の値を指定する場合、メソッドは一方向ではない必要があります。 開発者は、この属性を使用して、メソッド レベルのトランザクション フローに関する要件や制約をデザイン時に指定できます。  
  
## <a name="enabling-transaction-flow-at-the-endpoint-level"></a>エンドポイント レベルでのトランザクション フローの有効化  
 <xref:System.ServiceModel.TransactionFlowAttribute> 属性によって提供されるメソッドレベルのトランザクションフロー設定に加えて、WCF では、管理者が上位レベルでトランザクションフローを制御できるようにするためのエンドポイント全体のトランザクションフローの設定が提供されます。  
  
 これは、<xref:System.ServiceModel.Channels.TransactionFlowBindingElement> によって実現され、エンドポイントのバインディング設定で受信トランザクション フローを有効化または無効化する他に、受信トランザクションに必要なトランザクション プロトコル形式を指定できるようになります。  
  
 バインディングによりトランザクション フローが無効にされているとき、サービス コントラクトのいずれかの操作が受信トランザクションを必要とした場合は、サービスの起動時に検証例外がスローされます。  
  
 WCF に用意されているほとんどの継続的バインディングには、`transactionFlow` 属性と `transactionProtocol` 属性が含まれており、受信トランザクションを受け入れるように特定のバインディングを構成できます。 構成要素の設定の詳細については、「 [\<binding >](../../configure-apps/file-schema/wcf/bindings.md)」を参照してください。  
  
 管理者や展開担当者は、エンドポイント レベルのトランザクション フローを使用することで、展開時に構成ファイルを使用してトランザクション フローの要件や制約を構成できます。  
  
## <a name="security"></a>セキュリティ  
 システムのセキュリティと整合性を確保するために、アプリケーション間でトランザクションをフローさせるときは、メッセージ交換をセキュリティで保護する必要があります。 同じトランザクションに参加する資格のないアプリケーションには、トランザクションの詳細をフローさせたり、公開したりしないでください。  
  
 メタデータ交換を使用して、不明な、または信頼されていない Web サービスに WCF クライアントを生成する場合、これらの Web サービスに対する呼び出しでは、可能であれば現在のトランザクションを抑制する必要があります。 この方法を次の例に示します。  
  
```csharp
//client code which has an ambient transaction  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Suppress))  
{  
    // No transaction will flow to this operation  
    untrustedProxy.Operation1(...);  
    scope.Complete();  
}  
//remainder of client code  
```  
  
 さらに、サービスは、認証および承認済みのクライアントからの受信トランザクションのみを受け入れるように構成する必要があります。 受信トランザクションは、高信頼クライアントから送られてきた場合にのみ受け入れるようにする必要があります。  
  
## <a name="policy-assertions"></a>ポリシー アサーション  
 WCF では、ポリシーアサーションを使用してトランザクションフローを制御します。 ポリシー アサーションは、コントラクト、構成、および属性を集約して生成される、サービスのポリシー ドキュメントに含まれています。 クライアントは、HTTP GET または WS-MetadataExchange 要求/応答を使用して、サービスのポリシー ドキュメントを取得できます。 クライアントは、取得したポリシー ドキュメントを処理して、トランザクション フローをサポートまたは必要とする可能性があるサービス コントラクトの操作を判別できます。  
  
 トランザクション フローのポリシー アサーションは、クライアントがサービスに送信する必要がある、トランザクションを表す SOAP ヘッダーを指定することで、トランザクション フローに影響します。 すべてのトランザクション ヘッダーの `MustUnderstand` は、`true` に設定する必要があります。 これ以外の値に設定されたメッセージは、SOAP エラーによりすべて拒否されます。  
  
 トランザクション関連のポリシー アサーションは、1 つの操作に 1 つしか存在できません。 1つの操作に複数のトランザクションアサーションがあるポリシードキュメントは無効と見なされ、WCF によって拒否されます。 さらに、トランザクション プロトコルも各ポートの種類の内部に 1 つしか存在できません。 1つのポートの種類で複数のトランザクションプロトコルを参照する操作を含むポリシードキュメントは無効と見なされ、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)によって拒否されます。 また、出力メッセージや一方向の入力メッセージに関するトランザクション アサーションが存在するポリシー ドキュメントも無効と見なされます。
