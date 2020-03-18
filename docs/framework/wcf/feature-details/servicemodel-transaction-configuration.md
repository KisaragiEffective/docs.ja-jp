---
title: ServiceModel トランザクションの構成
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], ServiceModel configuration
ms.assetid: 5636067a-7fbd-4485-aaa2-8141c502acf3
ms.openlocfilehash: e8c8c9ebff259ccd991768afb8cdf9925a66aad0
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141611"
---
# <a name="servicemodel-transaction-configuration"></a>ServiceModel トランザクションの構成
Windows Communication Foundation (WCF) には、サービスのトランザクションを構成するための3つの属性 (`transactionFlow`、`transactionProtocol`、および `transactionTimeout`) が用意されています。  
  
## <a name="configuring-transactionflow"></a>transactionFlow の構成  
 WCF に用意されている定義済みバインディングのほとんどには、`transactionFlow` と `transactionProtocol` の属性が含まれているため、特定のトランザクションフロープロトコルを使用して特定のエンドポイントの受信トランザクションを受け入れるようにバインドを構成できます。 さらに、`transactionFlow` 要素とその `transactionProtocol` 属性を使用して、ユーザー独自のカスタム バインドを構築できます。 構成要素の設定の詳細については、「 [\<binding >](../../configure-apps/file-schema/wcf/bindings.md) 」および「 [WCF 構成スキーマ](../../../../docs/framework/configure-apps/file-schema/wcf/index.md)」を参照してください。  
  
 `transactionFlow` 属性は、バインディングを使用するサービス エンドポイントに対してトランザクション フローを有効にするかどうかを指定します。  
  
## <a name="configuring-transactionprotocol"></a>transactionProtocol の構成  
 `transactionProtocol` 属性は、バインディングを使用するサービス エンドポイントで使用されるトランザクション プロトコルを指定します。  
  
 以下に、指定したバインディングがトランザクション フローをサポートし、WS-AtomicTransaction プロトコルを使用するように構成する構成セクションの例を示します。  
  
```xml  
<netNamedPipeBinding>  
   <binding name="test"  
      closeTimeout="00:00:10"  
      openTimeout="00:00:20"   
      receiveTimeout="00:00:30"  
      sendTimeout="00:00:40"  
      transactionFlow="true"  
      transactionProtocol="WSAtomicTransactionOctober2004"  
      hostNameComparisonMode="WeakWildcard"  
      maxBufferSize="1001"  
      maxConnections="123"   
      maxReceivedMessageSize="1000">  
   </binding>  
</netNamedPipeBinding>  
```  
  
## <a name="configuring-transactiontimeout"></a>transactionTimeout の構成  
 WCF サービスの `transactionTimeout` 属性は、構成ファイルの `behavior` 要素で構成できます。 次のコードでは、この設定方法について説明します。  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <behaviors>  
         <behavior name="NewBehavior" transactionTimeout="00:01:00" /> <!-- 1 minute timeout -->  
      </behaviors>  
   </system.serviceModel>  
</configuration>  
```  
  
 `transactionTimeout` 属性は、サービスで作成された新しいトランザクションを完了させる期間を指定します。 この属性は、新しいトランザクションを確立するすべての操作の <xref:System.Transactions.TransactionScope> タイムアウトとして使用され、<xref:System.ServiceModel.OperationBehaviorAttribute> が適用されている場合、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> プロパティは `true` に設定されます。  
  
 このタイムアウトは、2 フェーズ コミット プロトコルにおける、トランザクションの作成からフェーズ 1 の完了までの期間を指定します。  
  
 この属性を `service` 構成セクション内に設定する場合は、対応するサービスの 1 つ以上のメソッドで、<xref:System.ServiceModel.OperationBehaviorAttribute> プロパティが <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> に設定された `true` を適用する必要があります。  
  
 使用されるタイムアウト値は常に、この `transactionTimeout` 構成設定と <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> プロパティの小さい方の値になります。  
  
## <a name="see-also"></a>関連項目

- [\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)
- [WCF 構成スキーマ](../../../../docs/framework/configure-apps/file-schema/wcf/index.md)
