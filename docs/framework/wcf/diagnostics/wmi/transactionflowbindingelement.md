---
description: '詳細情報: TransactionFlowBindingElement'
title: TransactionFlowBindingElement
ms.date: 03/30/2017
ms.assetid: 0a9656fe-2400-45ca-ad79-92715c8cf190
ms.openlocfilehash: 6a96a83c563affdaef01a12d64bc1c13ab2f719e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99757138"
---
# <a name="transactionflowbindingelement"></a>TransactionFlowBindingElement

TransactionFlowBindingElement  
  
## <a name="syntax"></a>構文  
  
```csharp
class TransactionFlowBindingElement : BindingElement  
{  
  string IssuedTokens;  
  string TransactionProtocol;  
  boolean Transactions;  
};  
```  
  
## <a name="methods"></a>メソッド  

 TransactionFlowBindingElement クラスは、メソッドを一切定義しません。  
  
## <a name="properties"></a>プロパティ  

 TransactionFlowBindingElement クラスには、次のプロパティがあります。  
  
### <a name="issuedtokens"></a>IssuedTokens  

 データ型: 文字列  
  
 アクセスの種類: 読み取り専用  
  
 発行されるセキュリティ トークン ヘッダー (WS-Trust からの IssuedTokens) の要件を指定します。  
  
### <a name="transactionprotocol"></a>TransactionProtocol  

 データ型: 文字列  
  
 アクセスの種類: 読み取り専用  
  
 トランザクションをフローさせるためにサービスによって使用されるトランザクション プロトコルです。  
  
### <a name="transactions"></a>トランザクション  

 データ型 : boolean  
  
 アクセスの種類: 読み取り専用  
  
 受信トランザクションをサポートするかどうかを示します。  
  
## <a name="requirements"></a>要件  
  
|MOF|Servicemodel.mof にて宣言済み。|  
|---------|-----------------------------------|  
|名前空間|root\ServiceModel で定義|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.TransactionFlowBindingElement>
