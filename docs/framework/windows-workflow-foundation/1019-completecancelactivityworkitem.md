---
description: '詳細情報: 1019 - CompleteCancelActivityWorkItem'
title: 1019 - CompleteCancelActivityWorkItem
ms.date: 03/30/2017
ms.assetid: 75a4a1ab-e5a3-4f4e-88e4-d19806e671d7
ms.openlocfilehash: 33e58931562d7244987dd8c0d9cf5d34fb894190
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99792870"
---
# <a name="1019---completecancelactivityworkitem"></a>1019 - CompleteCancelActivityWorkItem

## <a name="properties"></a>プロパティ  
  
|||  
|-|-|  
|id|1019|  
|キーワード|WFRuntime|  
|Level|"詳細"|  
|チャネル|Microsoft-Windows-Application Server-Applications/Debug|  
  
## <a name="description"></a>説明  

 CancelActivityWorkItem が完了したことを示します。  
  
## <a name="message"></a>メッセージ  

 Activity '%1'、DisplayName: '%2'、InstanceId: '%3' の CancelActivityWorkItem が完了しました。  
  
## <a name="details"></a>詳細  
  
|データ項目名|データ項目の型|説明|  
|--------------------|--------------------|-----------------|  
|アクティビティ|xs:string|アクティビティの型名。|  
|表示名|xs:string|アクティビティの表示名。|  
|InstanceId|xs:string|アクティビティのインスタンス ID。|  
|AppDomain|xs:string|AppDomain.CurrentDomain.FriendlyName で返される文字列。|
