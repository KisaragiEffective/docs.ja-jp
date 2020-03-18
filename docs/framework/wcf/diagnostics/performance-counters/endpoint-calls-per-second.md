---
title: 'エンドポイント : 1 秒あたりの呼び出し回数'
ms.date: 03/30/2017
ms.assetid: ca0fc06d-d68f-4236-bd5f-c7ff6214acdd
ms.openlocfilehash: d639d881bcedd336c7bbabd28ec385f60ff54d43
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76163788"
---
# <a name="endpoint-calls-per-second"></a>エンドポイント : 1 秒あたりの呼び出し回数
カウンター名 : 1 秒あたりの呼び出し  
  
## <a name="description"></a>説明  
 このエンドポイントが 1 秒あたりに呼び出される回数です。  
  
 このカウンターは、次の式を使用して計算された値を持つ、パフォーマンスカウンターの種類[PERF_COUNTER_COUNTER](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))です。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
