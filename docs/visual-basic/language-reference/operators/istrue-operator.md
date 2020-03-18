---
title: IsTrue 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.istrue
helpviewer_keywords:
- IsTrue operator [Visual Basic]
- OrElse operator [Visual Basic]
ms.assetid: b6cec0f2-61b1-4331-a7f0-4d07ee3179d6
ms.openlocfilehash: 4b863eed8406a10b9a44d7139f8659ac5cb758ad
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349510"
---
# <a name="istrue-operator-visual-basic"></a>IsTrue 演算子 (Visual Basic)
式が `True`かどうかを判断します。  
  
 コード内で `IsTrue` を明示的に呼び出すことはできませんが、Visual Basic コンパイラはそれを使用して `OrElse` 句からコードを生成できます。 クラスまたは構造体を定義し、その型の変数を `OrElse` 句で使用する場合は、そのクラスまたは構造体で `IsTrue` を定義する必要があります。  
  
 コンパイラは、`IsTrue` と `IsFalse` 演算子を一致する*ペア*と見なします。 これは、そのいずれかを定義する場合は、もう一方も定義する必要があることを意味します。  
  
## <a name="compiler-use-of-istrue"></a>IsTrue のコンパイラの使用  
 クラスまたは構造体を定義したら、その型の変数を `For`、`If`、`Else If`、または `While` ステートメントで使用することも、`When` 句で使用することもできます。 これを行う場合、コンパイラは、条件をテストできるように、型を `Boolean` 値に変換する演算子を必要とします。 適切な演算子を次の順序で検索します。  
  
1. クラスまたは構造体から `Boolean`への拡大変換演算子。  
  
2. クラスまたは構造体から `Boolean?`への拡大変換演算子。  
  
3. クラスまたは構造体の `IsTrue` 演算子。  
  
4. `Boolean` から `Boolean?`への変換が関係しない `Boolean?` への縮小変換。  
  
5. クラスまたは構造体から `Boolean`への縮小変換演算子。  
  
 `Boolean` または `IsTrue` 演算子への変換が定義されていない場合、コンパイラはエラーを通知します。  
  
> [!NOTE]
> `IsTrue` 演算子は*オーバーロード*することができます。つまり、クラスまたは構造体がそのクラスまたは構造体の型を持つ場合に、クラスまたは構造体がその動作を再定義できます。 コードでこのようなクラスまたは構造体に対してこの演算子を使用する場合は、再定義された動作を理解していることを確認してください。 詳細については、「[演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)」を参照してください。  
  
## <a name="example"></a>例  
 次のコード例では、`IsFalse` 演算子と `IsTrue` 演算子の定義を含む構造体のアウトラインを定義します。  
  
 [!code-vb[VbVbalrOperators#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>参照

- [IsFalse 演算子](../../../visual-basic/language-reference/operators/isfalse-operator.md)
- [方法 : 演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [OrElse 演算子](../../../visual-basic/language-reference/operators/orelse-operator.md)
