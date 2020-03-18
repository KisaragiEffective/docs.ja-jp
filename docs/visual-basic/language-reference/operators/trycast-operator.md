---
title: TryCast 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.trycast
- trycast
helpviewer_keywords:
- TryCast keyword [Visual Basic]
ms.assetid: d1ef5d47-fef4-491e-b014-1d910628f65c
ms.openlocfilehash: 53306575cfc385039be3939fd87cf993b4509af4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348208"
---
# <a name="trycast-operator-visual-basic"></a>TryCast 演算子 (Visual Basic)
例外をスローしない型変換操作を導入します。  
  
## <a name="remarks"></a>コメント  
 変換の試行が失敗した場合、`CType` と `DirectCast` の両方で <xref:System.InvalidCastException> エラーがスローされます。 これにより、アプリケーションのパフォーマンスが低下する可能性があります。 `TryCast` は [Nothing](../../../visual-basic/language-reference/nothing.md) を返します。そのため、考えられる例外を処理する代わりに、返された結果を `Nothing`に対してのみテストする必要があります。  
  
 `TryCast` キーワードは、[CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)と [DirectCast 演算子](../../../visual-basic/language-reference/operators/directcast-operator.md)キーワードを使用するのと同じ方法で使用します。最初の引数として式を指定し、2 番目の引数として変換する型を指定します。`TryCast` は、クラスやインターフェイスなどの参照型に対してのみ動作します。2 つの型の間の継承または実装関係が必要です。これは、一方の型が他の型を継承または実装する必要があることを意味します。  

  
## <a name="errors-and-failures"></a>エラーとエラー  
 `TryCast` は、継承または実装関係が存在しないことを検出すると、コンパイラエラーを生成します。 ただし、コンパイラエラーがないからといって、正常に変換できるとは限りません。 目的の変換が縮小されている場合は、実行時に失敗する可能性があります。 この場合、`TryCast` は [Nothing](../../../visual-basic/language-reference/nothing.md) を返します。  
  
## <a name="conversion-keywords"></a>変換キーワード  
 型変換のキーワードの比較を次に示します。  
  
|Keyword|データの種類|引数のリレーションシップ|実行時エラー|  
|---|---|---|---|  
|[CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)|任意のデータ型|拡大または縮小変換は、2 つのデータ型の間で定義する必要があります|<xref:System.InvalidCastException> をスロー|  
|[DirectCast 演算子](../../../visual-basic/language-reference/operators/directcast-operator.md)|任意のデータ型|一方の型は、他の型を継承するか、他の型を実装する必要があります|<xref:System.InvalidCastException> をスロー|  
|`TryCast`|参照型のみ|一方の型は、他の型を継承するか、他の型を実装する必要があります|[Nothing](../../../visual-basic/language-reference/nothing.md) を返す|  
  
## <a name="example"></a>例  
 次の例は、`TryCast` を使用する方法を示しています。  
  
 [!code-vb[VbVbalrKeywords#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>参照

- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
