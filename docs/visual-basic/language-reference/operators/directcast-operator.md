---
title: DirectCast 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.directCast
- directCast
helpviewer_keywords:
- DirectCast keyword [Visual Basic]
ms.assetid: 63e5a1d0-4d9e-4732-bf8f-e90c0c8784b8
ms.openlocfilehash: 8ea29b80cf27bbb2c21a8cebbfaa0a294e05f11d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331314"
---
# <a name="directcast-operator-visual-basic"></a>DirectCast 演算子 (Visual Basic)
継承または実装に基づく型変換操作を導入します。  
  
## <a name="remarks"></a>コメント  
 `DirectCast` では、変換に Visual Basic ランタイムヘルパールーチンを使用しないため、データ型 `Object`との間で変換を行う場合は `CType` よりも多少優れたパフォーマンスが得られます。  
  
 [CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)および [TryCast 演算子](../../../visual-basic/language-reference/operators/trycast-operator.md)キーワードの使用方法と同様に、`DirectCast` キーワードを使用します。 最初の引数として式を指定し、2 番目の引数として変換する型を指定します。 `DirectCast` には、2 つの引数のデータ型間の継承または実装関係が必要です。 これは、一方の型が他の型を継承または実装する必要があることを意味します。  
  
## <a name="errors-and-failures"></a>エラーとエラー  
 `DirectCast` は、継承または実装関係が存在しないことを検出すると、コンパイラエラーを生成します。 ただし、コンパイラエラーが発生しても、正常に変換できないことは保証されません。 目的の変換が縮小されている場合は、実行時に失敗する可能性があります。 これが発生すると、ランタイムは <xref:System.InvalidCastException> エラーをスローします。  
  
## <a name="conversion-keywords"></a>変換キーワード  
 型変換のキーワードの比較を次に示します。  
  
|Keyword|データの種類|引数のリレーションシップ|実行時エラー|  
|---|---|---|---|  
|[CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)|任意のデータ型|拡大または縮小変換は、2 つのデータ型の間で定義する必要があります|<xref:System.InvalidCastException> をスロー|  
|`DirectCast`|任意のデータ型|一方の型は、他の型を継承するか、他の型を実装する必要があります|<xref:System.InvalidCastException> をスロー|  
|[TryCast 演算子](../../../visual-basic/language-reference/operators/trycast-operator.md)|参照型のみ|一方の型は、他の型を継承するか、他の型を実装する必要があります|[Nothing](../../../visual-basic/language-reference/nothing.md) を返す|  
  
## <a name="example"></a>例  
 次の例は、`DirectCast`の2つの使用方法を示しています。1つは実行時に失敗し、もう1つは成功します。  
  
 [!code-vb[VbVbalrKeywords#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#1)]  
  
 前の例では、`q` の実行時の型が `Double`になっています。 `Double` は `Integer`に変換できるため、`CType` は成功します。 ただし、変換が存在する場合でも、`Double` の実行時の型には `Integer`との継承関係がないため、実行時に最初の `DirectCast` は失敗します。 2番目の `DirectCast` は、<xref:System.Windows.Forms.Form> が継承する型 <xref:System.Windows.Forms.Form> から型 <xref:System.Windows.Forms.Control>に変換するため、成功します。  
  
## <a name="see-also"></a>参照

- <xref:System.Convert.ChangeType%2A?displayProperty=nameWithType>
- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
