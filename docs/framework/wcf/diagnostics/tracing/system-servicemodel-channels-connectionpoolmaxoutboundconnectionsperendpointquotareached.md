---
description: '詳細情報: Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure'
title: Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure
ms.date: 03/30/2017
ms.assetid: 1b9f5139-e122-4716-9ef7-2f38e1813993
ms.openlocfilehash: f0645d0f7bfc2787f4bce254ea8d6e3143dc0406
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644607"
---
# <a name="microsofttransactionstransactionbridgeenlisttransactionfailure"></a>Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure

WS-AT プロトコル サービスは、指定されたコーディネーション コンテキストを使用してトランザクションに参加することができませんでした。  
  
## <a name="description"></a>説明  

 指定された 2PC プロトコルのトランザクションに MSDTC が参加できない場合にトレースされます。  これは、トランザクションが存在しなくなった場合、参加が許可されなくなった場合、または既に参加が多すぎる場合に発生する可能性があります。  
  
## <a name="troubleshooting"></a>トラブルシューティング  

 トレース メッセージ内のステータス文字列を調べて、アクション可能な項目が存在するかどうかを確認します。  
  
## <a name="see-also"></a>関連項目

- [トレース](index.md)
- [トレースを使用したアプリケーションのトラブルシューティング](using-tracing-to-troubleshoot-your-application.md)
- [管理と診断](../index.md)
