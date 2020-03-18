---
title: 匿名型の定義
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [Visual Basic], type definition
ms.assetid: 7a8a0ddc-55ba-4d67-869e-87a84d938bac
ms.openlocfilehash: f8ac26577a7fbef865605a7ecf643fa733b2c2c0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344927"
---
# <a name="anonymous-type-definition-visual-basic"></a>匿名型の定義 (Visual Basic)

匿名型のインスタンスの宣言に応答して、コンパイラは、型の指定されたプロパティを含む新しいクラス定義を作成します。

## <a name="compiler-generated-code"></a>コンパイラで生成されたコード

次の `product`の定義では、コンパイラは、`Name`、`Price`、および `OnHand`プロパティを含む新しいクラス定義を作成します。

[!code-vb[VbVbalrAnonymousTypes#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#25)]

クラス定義には、次のようなプロパティ定義が含まれています。 キープロパティに `Set` メソッドがないことに注意してください。 キープロパティの値は読み取り専用です。

```vb
Public Class $Anonymous1
    Private _name As String
    Private _price As Double
    Private _onHand As Integer
     Public ReadOnly Property Name() As String
        Get
            Return _name
        End Get
    End Property

    Public ReadOnly Property Price() As Double
        Get
            Return _price
        End Get
    End Property

    Public Property OnHand() As Integer
        Get
            Return _onHand
        End Get
        Set(ByVal Value As Integer)
            _onHand = Value
        End Set
    End Property

End Class
```

また、匿名型の定義には、パラメーターなしのコンストラクターが含まれています。 パラメーターを必要とするコンストラクターは使用できません。

匿名型の宣言に少なくとも1つのキープロパティが含まれている場合、型定義は <xref:System.Object>: <xref:System.Object.Equals%2A>、<xref:System.Object.GetHashCode%2A>、および <xref:System.Object.ToString%2A>から継承された3つのメンバーをオーバーライドします。 キープロパティが宣言されていない場合は、<xref:System.Object.ToString%2A> のみがオーバーライドされます。 オーバーライドは、次の機能を提供します。

- 2つの匿名型のインスタンスが同じインスタンスである場合、または次の条件を満たしている場合、`Equals` は `True` を返します。

  - これらのプロパティの数は同じです。

  - プロパティは同じ順序で、同じ名前と推論された型を使用して宣言されます。 名前比較では、大文字と小文字は区別されません。

  - 少なくとも1つのプロパティがキープロパティであり、`Key` キーワードが同じプロパティに適用されています。

  - キープロパティの対応する各ペアの比較によって `True`が返されます。

    たとえば、次の例では、`Equals` は `employee01` と `employee08`に対してのみ `True` を返します。 各行の前のコメントは、新しいインスタンスが `employee01`と一致しない理由を指定します。

    [!code-vb[VbVbalrAnonymousTypes#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#24)]

- `GetHashcode` には、適切に一意の GetHashCode アルゴリズムが用意されています。 アルゴリズムでは、キープロパティのみを使用してハッシュコードを計算します。

- `ToString` は、次の例に示すように、連結されたプロパティ値の文字列を返します。 キーとキー以外のプロパティの両方が含まれます。

  [!code-vb[VbVbalrAnonymousTypes#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#29)]

匿名型の明示的に名前が付けられたプロパティは、これらの生成されたメソッドと競合しません。 つまり、`.Equals`、`.GetHashCode`、または `.ToString` を使用してプロパティの名前を指定することはできません。

少なくとも1つのキープロパティを含む匿名型定義は、<xref:System.IEquatable%601?displayProperty=nameWithType> インターフェイスも実装します。 `T` は匿名型の型です。

> [!NOTE]
> 匿名型の宣言では、同じアセンブリ内に存在する場合にのみ、同じ匿名型が作成され、そのプロパティの名前と推論される型は同じで、プロパティは同じ順序で宣言され、同じプロパティがキープロパティとしてマークされます。

## <a name="see-also"></a>参照

- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [方法 : 匿名型の宣言におけるプロパティ名と型を推論する](../../../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
