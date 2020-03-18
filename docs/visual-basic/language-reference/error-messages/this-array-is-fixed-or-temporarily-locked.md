---
title: この配列は固定か、または一時的にロックされています。
ms.date: 07/20/2015
f1_keywords:
- vbrID10
ms.assetid: de6713a6-51d7-4edb-8515-d5fb544e2091
ms.openlocfilehash: 8d5e4add2d92a575126fb934ac3874a2e37685f5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350782"
---
# <a name="this-array-is-fixed-or-temporarily-locked-visual-basic"></a>この配列は固定か、または一時的にロックされています。(Visual Basic)
このエラーには、次のような原因が考えられます。  
  
- `ReDim` を使用して、固定サイズの配列の要素数を変更します。  
  
- モジュールレベルの動的配列を再配置します。この配列では、1つの要素がプロシージャに引数として渡されています。 要素が渡された場合、配列は、プロシージャ内の参照パラメーターのメモリの解放を防ぐためにロックされます。  
  
- 配列を含む `Variant` 変数に値を割り当てようとしましたが、`Variant` は現在ロックされています。  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. `ReDim` (配列がプロシージャ内で宣言されている場合) で宣言するか、要素の数を指定せずに宣言することによって、元の配列を固定ではなく動的にします (配列がモジュールレベルで宣言されている場合)。  
  
2. 要素を渡す必要があるかどうかを判断します。これは、モジュール内のすべてのプロシージャ内に表示されるためです。  
  
3. `Variant` をロックしているものを特定し、それを修復します。  
  
## <a name="see-also"></a>関連項目

- [配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)
