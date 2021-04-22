---
description: '詳細情報: 205 - OperationInvoked'
title: 205 - OperationInvoked
ms.date: 03/30/2017
ms.assetid: 9c8d6c90-dfa5-4ae0-a589-96679a8fb3ba
ms.openlocfilehash: 09afe4c29c5f56b06bbf524dd13d88ddf2319063
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644971"
---
# <a name="205---operationinvoked"></a>205 - OperationInvoked

## <a name="properties"></a>プロパティ  
  
|||  
|-|-|  
|id|205|  
|キーワード|Troubleshooting、ServiceModel|  
|レベル|説明|  
|チャネル|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>説明  

 このイベントは、サービス モデルの既定の `OperationInvoker` がメソッドの呼び出しを開始する直前に生成されます。  
  
## <a name="message"></a>メッセージ  

 OperationInvoker が '%1' メソッドを呼び出しました。 呼び出し元の情報: '%2'。  
  
## <a name="details"></a>詳細  
  
|データ項目名|データ項目の型|説明|  
|--------------------|--------------------|-----------------|  
|メソッド名|`xs:string`|`OperationInvoker` によって呼び出されたメソッドの CLR 名。|  
|呼び出し元情報|`xs:string`|クライアントの IP アドレスとポート番号。'&lt;IP アドレス&gt;:&lt;ポート番号&gt;' の形式で指定します。 この 2 つの値は、操作コンテキスト内の 'System.ServiceModel.Channels.RemoteEndpointMessageProperty' メッセージ プロパティから取得します。 TCP 以外のバインドの場合、この値は `null` になることに注意してください。|  
|HostReference|`xs:string`|Web ホスト サービスの場合は、このフィールドにより、サービスが Web 階層内で一意に識別されます。 その形式は、"Web サイト名アプリケーション仮想パス&#124;サービス仮想パス&#124;サービス名" と定義されます。 例: 'Default Web Site/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService'。|  
|AppDomain|`xs:string`|AppDomain.CurrentDomain.FriendlyName で返される文字列。|
