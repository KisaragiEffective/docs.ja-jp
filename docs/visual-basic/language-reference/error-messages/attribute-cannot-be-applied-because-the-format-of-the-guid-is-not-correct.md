---
title: GUID '<attribute>' の形式が正しくないため、'<number>' を適用できません。
ms.date: 07/20/2015
f1_keywords:
- vbc32500
- bc32500
helpviewer_keywords:
- BC32500
ms.assetid: 6fa34c55-368e-4d7d-b488-07a3fffe045f
ms.openlocfilehash: f7b6e42480075666ce9f7e8fc6966bd4bb6b888a
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73977316"
---
# <a name="attribute-cannot-be-applied-because-the-format-of-the-guid-number-is-not-correct"></a>GUID '\<number > ' の形式が正しくないため、'\<属性 > ' を適用できません

`COMClassAttribute` 属性ブロックで、GUID の適切な形式に準拠していないグローバル一意識別子 (GUID) が指定されています。 `COMClassAttribute` は、Guid を使用して、クラス、インターフェイス、および作成イベントを一意に識別します。  
  
 GUID は、16 バイトで構成されます。前半の 8 バイトは数値、後半の 8 バイトはバイナリです。 このファイルは、uuidgen.exe などの Microsoft ユーティリティによって生成され、スペースと時間において一意であることが保証されています。  
  
 **エラー ID:** BC32500  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. COM オブジェクトを識別するために必要な正しい GUID または guid を特定します。  
  
2. `COMClassAttribute` 属性ブロックに表示される GUID 文字列が正しくコピーされていることを確認します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Guid>
- [属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)
