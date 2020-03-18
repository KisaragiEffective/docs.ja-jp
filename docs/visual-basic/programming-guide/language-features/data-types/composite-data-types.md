---
title: 複合データ型
ms.date: 04/25/2017
helpviewer_keywords:
- classes [Visual Basic], composite data types
- composite types [Visual Basic]
- composite data types [Visual Basic]
- data types [Visual Basic], composite
- arrays [Visual Basic], composite data types
- structures [Visual Basic], composite data types
- classes [Visual Basic], composite types
- types [Visual Basic], composite
ms.assetid: 62970f2e-52c0-4369-8963-613820f1f434
ms.openlocfilehash: 1c099c5082f1c4173a50c70998c99135c94821e6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346376"
---
# <a name="composite-data-types-visual-basic"></a>複合データ型 (Visual Basic)
Visual Basic の基本データ型に加えて、さまざまな型の項目をアセンブルして、構造体、配列、クラスなどの*複合データ型*を作成することもできます。 複合データ型は、基本型および他の複合型から構築できます。 たとえば、構造体要素の配列、または配列メンバーを持つ構造体を定義できます。  
  
## <a name="data-types"></a>データ型  
 複合型は、そのコンポーネントのデータ型とは異なります。 たとえば、`Integer` の要素の配列は、`Integer` データ型ではありません。  
  
 通常、配列のデータ型は、要素の型、かっこ、およびコンマを使用して、必要に応じて表されます。 たとえば、`String` 要素の1次元配列は `String()`として表され、`Boolean` 要素の2次元配列は `Boolean(,)`として表されます。  
  
## <a name="structure-types"></a>構造体の型  
 すべての構造体を構成する1つのデータ型はありません。 2つの構造体が同じ順序で同じ要素を定義している場合でも、構造体の各定義は一意のデータ型を表します。 ただし、同じ構造体の複数のインスタンスを作成した場合、Visual Basic はそれらを同じデータ型であると見なします。  
  
## <a name="tuples"></a>タプル

組は、型が事前定義されている 2 つ以上のフィールドを含む軽量の構造体です。 タプルは Visual Basic 2017 以降でサポートされています。 タプルは、1つのメソッド呼び出しから複数の値を返すために最も一般的に使用されます。参照によって引数を渡したり、返されたフィールドをより重いクラスまたは構造体にパッケージ化したりする必要はありません。 組の詳細については、「[タプル](tuples.md)」を参照してください。

## <a name="array-types"></a>配列型  
 すべての配列を構成する1つのデータ型はありません。 配列の特定のインスタンスのデータ型は、次のように決定されます。  
  
- 配列であるという事実  
  
- 配列のランク (次元数)  
  
- 配列の要素型。  
  
 特に、特定のディメンションの長さは、インスタンスのデータ型の一部ではありません。 これを次の例に示します。  
  
```vb  
Dim arrayA( ) As Byte = New Byte(12) {}  
Dim arrayB( ) As Byte = New Byte(100) {}  
Dim arrayC( ) As Short = New Short(100) {}  
Dim arrayD( , ) As Short  
Dim arrayE( , ) As Short = New Short(4, 10) {}  
```  
  
 前の例では、配列変数 `arrayA` と `arrayB` は、異なる長さに初期化されている場合でも、同じデータ型 (`Byte()`) と見なされます。 `arrayB` と `arrayC` の変数は、要素の型が異なるため、同じ型ではありません。 `arrayC` と `arrayD` の変数は、ランクが異なるため、同じ型ではありません。 `arrayD` がまだ初期化されていない場合でも、`arrayD` と `arrayE` 変数は同じ型 (`Short(,)`) を持ちます。  
  
 配列の詳細については、「[配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)」を参照してください。  
  
## <a name="class-types"></a>クラスの種類  
 すべてのクラスを構成する1つのデータ型はありません。 1つのクラスは別のクラスから継承できますが、それぞれが個別のデータ型です。 同じクラスの複数のインスタンスが同じデータ型です。 あるクラスインスタンス変数を別のクラスインスタンス変数に割り当てると、データ型が同じであるだけでなく、メモリ内の同じクラスインスタンスが参照されます。  
  
 クラスの詳細については、「[オブジェクトとクラス](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [Visual Basic におけるジェネリック型](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [Visual Basic における型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [方法 : 変数内で複数の値を保持する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-hold-more-than-one-value-in-a-variable.md)
