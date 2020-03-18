---
title: ジェネリック プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- generic methods [Visual Basic], type inference
- generics [Visual Basic], type inference
- procedures [Visual Basic], generic
- generic procedures
- type inference, generics
- generic methods [Visual Basic]
- type inference
- generics [Visual Basic], procedures
- generic procedures [Visual Basic], type inference
ms.assetid: 95577b28-137f-4d5c-a149-919c828600e5
ms.openlocfilehash: 16a629e07cf711778b3d8d1863958ec7a6300649
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350085"
---
# <a name="generic-procedures-in-visual-basic"></a>Visual Basic におけるジェネリック プロシージャ
ジェネリック*プロシージャ*(*ジェネリックメソッド*とも呼ばれます) は、少なくとも1つの型パラメーターで定義されたプロシージャです。 これにより、呼び出し元のコードは、プロシージャを呼び出すたびに、データ型を要件に合わせて調整できます。  
  
 ジェネリッククラスまたはジェネリック構造体内で定義されているので、プロシージャはジェネリックではありません。 ジェネリックにするには、プロシージャは、実行される可能性のある通常のパラメーターに加えて、少なくとも1つの型パラメーターを受け取る必要があります。 ジェネリッククラスまたは構造体には、非ジェネリックプロシージャを含めることができ、非ジェネリックのクラス、構造体、またはモジュールにジェネリックプロシージャを含めることができます。  
  
 ジェネリックプロシージャは、通常のパラメーターリスト、戻り値の型 (存在する場合)、およびプロシージャコード内で、その型パラメーターを使用できます。  
  
## <a name="type-inference"></a>型の推定  
 型引数をまったく指定せずにジェネリックプロシージャを呼び出すことができます。 このように呼び出した場合、コンパイラは、プロシージャの型引数に渡す適切なデータ型を決定しようとします。 これは、*型の推論*と呼ばれます。 次のコードは、型パラメーター `t`に型 `String` を渡す必要があることをコンパイラが推論する呼び出しを示しています。  
  
 [!code-vb[VbVbalrDataTypes#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#15)]  
  
 コンパイラは、呼び出しのコンテキストから型引数を推論できない場合、エラーを報告します。 このようなエラーの原因の1つとして、配列のランクの不一致が考えられます。 たとえば、通常のパラメーターを型パラメーターの配列として定義するとします。 異なるランク (次元数) の配列を指定するジェネリックプロシージャを呼び出すと、不一致によって型の推定が失敗します。 次のコードは、1次元配列を必要とするプロシージャに2次元配列が渡される呼び出しを示しています。  
  
```vb  
Public Sub demoSub(Of t)(ByVal arg() As t)
End Sub

Public Sub callDemoSub()
    Dim twoDimensions(,) As Integer
    demoSub(twoDimensions)
End Sub
```
  
 型の推定を呼び出すには、すべての型引数を省略する必要があります。 1つの型引数を指定する場合は、それらをすべて指定する必要があります。  
  
 型の推定は、ジェネリックプロシージャでのみサポートされています。 ジェネリッククラス、構造体、インターフェイス、またはデリゲートの型の推定を呼び出すことはできません。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 次の例では、ジェネリック `Function` プロシージャを定義して、配列内の特定の要素を検索します。 1つの型パラメーターを定義し、それを使用してパラメーターリスト内の2つのパラメーターを構築します。  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbalrDataTypes#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#14)]  
  
### <a name="comments"></a>コメント  
 前の例では、`searchArray`の各要素に対して `searchValue` を比較する機能が必要です。 この機能を保証するために、`T` 型パラメーターを制約して、<xref:System.IComparable%601> インターフェイスを実装します。 このコードでは、`=` 演算子ではなく、<xref:System.IComparable%601.CompareTo%2A> メソッドを使用しています。 `T` に指定された型引数が `=` 演算子をサポートしている保証はないためです。  
  
 `findElement` プロシージャは、次のコードを使用してテストできます。  
  
 [!code-vb[VbVbalrDataTypes#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#13)]  
  
 上記の `MsgBox` を呼び出すと、それぞれ "0"、"1"、および "-1" が表示されます。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるジェネリック型](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [方法 : 複数のデータ型に同一の機能を提供できるクラスを定義する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-define-a-class-that-can-provide-identical-functionality.md)
- [方法 : ジェネリック クラスを使用する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Visual Basic におけるプロシージャ](../../../../visual-basic/programming-guide/language-features/procedures/index.md)
- [プロシージャのパラメーターと引数](../../../../visual-basic/programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)
- [型リスト](../../../../visual-basic/language-reference/statements/type-list.md)
- [パラメーター リスト](../../../../visual-basic/language-reference/statements/parameter-list.md)
