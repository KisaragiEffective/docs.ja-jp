---
title: '方法 : オブジェクト変数で参照している型を確認する'
ms.date: 07/20/2015
helpviewer_keywords:
- TypeOf operator [Visual Basic], determining object variable type
- variables [Visual Basic], object
- object variables [Visual Basic], determining type
ms.assetid: 6f6a138d-58a4-40d1-9f4e-0a3c598eaf81
ms.openlocfilehash: b3778a170759f685db78e7dcde219138196f9eca
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344200"
---
# <a name="how-to-determine-what-type-an-object-variable-refers-to-visual-basic"></a>方法: オブジェクト変数で参照している型を確認する (Visual Basic)

オブジェクト変数には、他の場所に格納されているデータへのポインターが含まれています。 データの種類は、実行時に変更される可能性があります。 現時点では、<xref:System.Type.GetTypeCode%2A> メソッドを使用して現在のランタイム型を判断したり、 [TypeOf 演算子](../../../../visual-basic/language-reference/operators/typeof-operator.md)を使用して、現在の実行時の型が指定した型と互換性があるかどうかを調べることができます。

### <a name="to-determine-the-exact-type-an-object-variable-currently-refers-to"></a>オブジェクト変数で現在参照されている正確な型を確認するには

1. オブジェクト変数で、<xref:System.Object.GetType%2A> メソッドを呼び出して、<xref:System.Type?displayProperty=nameWithType> オブジェクトを取得します。

    ```vb
    Dim myObject As Object
    myObject.GetType()
    ```

2. <xref:System.Type?displayProperty=nameWithType> クラスで、共有メソッド <xref:System.Type.GetTypeCode%2A> を呼び出して、オブジェクトの型の <xref:System.TypeCode> 列挙値を取得します。

    ```vb
    Dim myObject As Object
    Dim datTyp As Integer = Type.GetTypeCode(myObject.GetType())
    MsgBox("myObject currently has type code " & CStr(datTyp))
    ```

    <xref:System.TypeCode> 列挙値は、`Double`など、目的の列挙体のメンバーに対してテストできます。

### <a name="to-determine-whether-an-object-variables-type-is-compatible-with-a-specified-type"></a>オブジェクト変数の型が指定した型と互換性があるかどうかを判断するには

- `TypeOf` 演算子を[Is 演算子](../../../../visual-basic/language-reference/operators/is-operator.md)と組み合わせて使用し、`TypeOf`...`Is` 式を使用してオブジェクトをテストします。

    ```vb
    If TypeOf objA Is System.Windows.Forms.Control Then
        MsgBox("objA is compatible with the Control class")
    End If
    ```

    `TypeOf`...`Is` 式は、オブジェクトの実行時の型が指定した型と互換性がある場合に `True` を返します。

    互換性の基準は、指定された型がクラス、構造体、またはインターフェイスのいずれであるかによって異なります。 一般に、オブジェクトがと同じ型であるか、指定された型を継承するか、または実装する場合、型は互換性があります。 詳細については、「 [TypeOf 演算子](../../../../visual-basic/language-reference/operators/typeof-operator.md)」を参照してください。

## <a name="compile-the-code"></a>コードのコンパイル

指定された型を変数または式にすることはできません。 クラス、構造体、インターフェイスなど、定義された型の名前である必要があります。 これには、`Integer` や `String`などの組み込み型が含まれます。

## <a name="see-also"></a>関連項目

- <xref:System.Object.GetType%2A>
- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Type.GetTypeCode%2A>
- <xref:System.TypeCode>
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の値](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [Object Data Type](../../../../visual-basic/language-reference/data-types/object-data-type.md)
