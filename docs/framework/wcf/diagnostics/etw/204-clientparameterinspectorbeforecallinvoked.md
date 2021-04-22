---
description: '詳細情報: 204 - ClientParameterInspectorBeforeCallInvoked'
title: 204 - ClientParameterInspectorBeforeCallInvoked
ms.date: 03/30/2017
ms.assetid: 8253555a-9002-4565-8ede-33d7a33a895f
ms.openlocfilehash: fa5646da7c48f624dc40f4fcc54a890c0758b4ba
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644997"
---
# <a name="204---clientparameterinspectorbeforecallinvoked"></a>204 - ClientParameterInspectorBeforeCallInvoked

## <a name="properties"></a>プロパティ  
  
|||  
|-|-|  
|id|204|  
|キーワード|Troubleshooting、ServiceModel|  
|レベル|説明|  
|チャネル|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>説明  

 このイベントは、クライアント パラメーター インスペクターで Service Model が `BeforeCall` メソッドを呼び出した後に生成されます。  
  
## <a name="message"></a>メッセージ  

 ディスパッチャーが型 '%1' の ClientParameterInspector で 'BeforeCall' を呼び出しました。  
  
## <a name="details"></a>詳細  
  
|データ項目名|データ項目の型|説明|  
|--------------------|--------------------|-----------------|  
|TypeName|`xs:string`|呼び出されたインスペクターの型の CLR FullName。|  
|HostReference|`xs:string`|Web ホスト サービスの場合は、このフィールドにより、サービスが Web 階層内で一意に識別されます。 その形式は、"Web サイト名アプリケーション仮想パス&#124;サービス仮想パス&#124;サービス名" と定義されます。 例: 'Default Web Site/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService'。|  
|AppDomain|`xs:string`|AppDomain.CurrentDomain.FriendlyName で返される文字列。|
