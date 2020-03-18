---
title: '方法 : 変数内で複数の値を保持する'
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], composite data types
- composite types [Visual Basic]
- composite data types [Visual Basic]
- data types [Visual Basic], composite
- arrays [Visual Basic], composite data types
- structures [Visual Basic], composite data types
- arrays [Visual Basic], compilation errors
- types [Visual Basic], composite
ms.assetid: 5fe0e558-aac2-4a40-b7f2-7cfea7336917
ms.openlocfilehash: d452fbf35f9d200348234b38c40f8636f0ec4b4e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350014"
---
# <a name="how-to-hold-more-than-one-value-in-a-variable-visual-basic"></a>方法: 変数内で複数の値を保持する (Visual Basic)

*複合データ型*として宣言すると、変数に複数の値が保持されます。

[複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)には、構造体、配列、およびクラスが含まれます。 複合データ型の変数は、基本データ型とその他の複合型の組み合わせを保持できます。 構造体とクラスは、データだけでなくコードも保持できます。

## <a name="to-hold-more-than-one-value-in-a-variable"></a>変数に複数の値を格納するには

1. 変数に使用する複合データ型を決定します。

2. 複合データ型がまだ定義されていない場合は、変数で使用できるように定義します。

    - Structure[ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)を使用して構造体を定義します。

    - [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用して配列を定義します。

    - Class[ステートメント](../../../../visual-basic/language-reference/statements/class-statement.md)を使用してクラスを定義します。

3. `Dim` ステートメントを使用して変数を宣言します。

4. 変数名の後に `As` 句を指定します。

5. `As` キーワードに続けて、適切な複合データ型の名前を指定します。

## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [型文字](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
