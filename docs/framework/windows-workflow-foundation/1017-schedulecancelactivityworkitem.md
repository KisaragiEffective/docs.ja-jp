---
description: '詳細情報: 1017 - ScheduleCancelActivityWorkItem'
title: 1017 - ScheduleCancelActivityWorkItem
ms.date: 03/30/2017
ms.assetid: 864546ab-d65c-4989-8fcb-537ba03a3cdd
ms.openlocfilehash: 04dc4f663ceed1d7350dd14867e4926042bd11af
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99792896"
---
# <a name="1017---schedulecancelactivityworkitem"></a>1017 - ScheduleCancelActivityWorkItem

## <a name="properties"></a>プロパティ  
  
|||  
|-|-|  
|id|1017|  
|キーワード|WFRuntime|  
|Level|"詳細"|  
|チャネル|Microsoft-Windows-Application Server-Applications/Debug|  
  
## <a name="description"></a>説明  

 CancelActivityWorkItem がスケジュールされていることを示します。  
  
## <a name="message"></a>メッセージ  

 Activity '%1'、DisplayName: '%2'、InstanceId: '%3' に対して CancelActivityWorkItem がスケジュールされました。  
  
## <a name="details"></a>詳細  
  
|データ項目名|データ項目の型|説明|  
|--------------------|--------------------|-----------------|  
|アクティビティ|xs:string|アクティビティの型名。|  
|表示名|xs:string|アクティビティの表示名。|  
|InstanceId|xs:string|アクティビティのインスタンス ID。|  
|AppDomain|xs:string|AppDomain.CurrentDomain.FriendlyName で返される文字列。|
