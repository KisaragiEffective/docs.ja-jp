---
description: '詳細情報: 203 - ClientParameterInspectorAfterCallInvoked'
title: 203 - ClientParameterInspectorAfterCallInvoked
ms.date: 03/30/2017
ms.assetid: b9592212-07e2-43e0-8b00-affd195cf55a
ms.openlocfilehash: bacdc2a74fc2cef6d7e473fa58c0cbbcffffcf16
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99645023"
---
# <a name="203---clientparameterinspectoraftercallinvoked"></a>203 - ClientParameterInspectorAfterCallInvoked

## <a name="properties"></a>プロパティ  
  
|||  
|-|-|  
|id|203|  
|キーワード|Troubleshooting、ServiceModel|  
|レベル|説明|  
|チャネル|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>説明  

 このイベントは、クライアント パラメーター インスペクターで Service Model が `AfterCall` メソッドを呼び出した後に生成されます。  
  
## <a name="message"></a>メッセージ  

 ディスパッチャーが型 '%1' の ClientParameterInspector で 'AfterCall' を呼び出しました。  
  
## <a name="details"></a>詳細  
  
|データ項目名|データ項目の型|説明|  
|--------------------|--------------------|-----------------|  
|TypeName|`xs:string`|呼び出されたインスペクターの型の CLR FullName。|  
|HostReference|`xs:string`|Web ホスト サービスの場合は、このフィールドにより、サービスが Web 階層内で一意に識別されます。 その形式は、"Web サイト名アプリケーション仮想パス&#124;サービス仮想パス&#124;サービス名" と定義されます。 例: 'Default Web Site/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService'。|  
|AppDomain|`xs:string`|AppDomain.CurrentDomain.FriendlyName で返される文字列。|
