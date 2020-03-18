---
title: 1 秒あたりのインスタンス
ms.date: 03/30/2017
ms.assetid: 74579397-1058-4278-80cf-2d00854a480f
ms.openlocfilehash: 3ae0b83066b22187ce062596a62e9d17aa2acd45
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76163801"
---
# <a name="instances-per-second"></a>1 秒あたりのインスタンス
カウンター名 : 1 秒あたりに作成されたインスタンス。  
  
## <a name="description"></a>説明  
 1 秒あたりに作成されたサービス インスタンスの合計数です。  
  
 このカウンターは、次の式を使用して計算された値を持つ、パフォーマンスカウンターの種類[PERF_COUNTER_COUNTER](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))です。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
