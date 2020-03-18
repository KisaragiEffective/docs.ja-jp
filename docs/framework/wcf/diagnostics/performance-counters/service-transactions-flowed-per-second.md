---
title: 'サービス : 1 秒あたりのトランザクション フロー'
ms.date: 03/30/2017
ms.assetid: ec72eb49-2942-4811-91df-d6e5dad81fd8
ms.openlocfilehash: e8fbc314321a46fd3539998bd3dd22770086ca37
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76164009"
---
# <a name="service-transactions-flowed-per-second"></a>サービス : 1 秒あたりのトランザクション フロー
カウンター名 : 1 秒あたりのトランザクション フロー。  
  
## <a name="description"></a>説明  
 このサービスでの操作に対して実行された 1 秒あたりのトランザクションの数です。  
  
 このカウンターは、次の式を使用して計算された値を持つ、パフォーマンスカウンターの種類[PERF_COUNTER_COUNTER](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))です。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
