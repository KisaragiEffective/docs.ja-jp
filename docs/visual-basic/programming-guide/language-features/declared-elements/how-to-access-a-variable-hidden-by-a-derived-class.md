---
title: '方法: 派生クラスによって非表示になっている変数にアクセスする'
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- base classes [Visual Basic], accessing elements
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declared elements [Visual Basic], referencing
- variables [Visual Basic], accessing hidden
ms.assetid: ae21a8ac-9cd4-4fba-a3ec-ecc4321ef93c
ms.openlocfilehash: 276cb1411c66e1f7205507a1b1053cc8642c7cb0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345407"
---
# <a name="how-to-access-a-variable-hidden-by-a-derived-class-visual-basic"></a>方法: 派生クラスによって非表示になっている変数にアクセスする (Visual Basic)

派生クラスのコードが変数にアクセスすると、コンパイラは通常、最も近いアクセス可能なバージョン (アクセス可能なバージョン) への参照を解決します。これは、アクセスしているクラスからさかのぼって最も少ない derivational 手順です。 変数が派生クラスで定義されている場合、コードは通常、その定義にアクセスします。

派生クラス変数が基底クラスの変数をシャドウすると、基底クラスのバージョンが非表示になります。 ただし、基底クラスの変数にアクセスするには、`MyBase` キーワードを使用して修飾する必要があります。

### <a name="to-access-a-base-class-variable-hidden-by-a-derived-class"></a>派生クラスによって非表示にされた基本クラス変数にアクセスするには

- 式または代入ステートメントでは、変数名の前に `MyBase` キーワードとピリオド (`.`) を指定します。

    コンパイラは、変数の基本クラスバージョンへの参照を解決します。

    次の例は、継承によるシャドウ処理を示しています。 2つの参照が作成されます。1つはシャドウを行う変数にアクセスし、もう1つはシャドウ処理をバイパスします。

    ```vb
    Public Class shadowBaseClass
        Public shadowString As String = "This is the base class string."
    End Class
    Public Class shadowDerivedClass
        Inherits shadowBaseClass
        Public Shadows shadowString As String = "This is the derived class string."
        Public Sub showStrings()
            Dim s As String = "Unqualified shadowString: " & shadowString &
                vbCrLf & "MyBase.shadowString: " & MyBase.shadowString
            MsgBox(s)
        End Sub
    End Class
    ```

    前の例では、基底クラスで `shadowString` 変数を宣言し、派生クラスでそれをシャドウしています。 派生クラスのプロシージャ `showStrings` は、名前 `shadowString` が修飾されていない場合に、文字列のシャドウバージョンを表示します。 `shadowString` が `MyBase` キーワードで修飾されている場合は、シャドウされたバージョンが表示されます。

## <a name="robust-programming"></a>信頼性の高いプログラミング

シャドウされた変数の意図しないバージョンを参照する危険性を低くするために、シャドウされた変数へのすべての参照を完全修飾することができます。 シャドウでは、同じ名前の変数の複数のバージョンが導入されます。 コードステートメントが変数名を参照する場合、コンパイラが参照を解決するバージョンは、コードステートメントの場所や修飾文字列の存在などの要因によって異なります。 これにより、変数の間違ったバージョンを参照するリスクが増加する可能性があります。

## <a name="see-also"></a>関連項目

- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic でのシャドウ処理](../../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
- [シャドウとオーバーライドの違い](../../../../visual-basic/programming-guide/language-features/declared-elements/differences-between-shadowing-and-overriding.md)
- [方法: 自分で宣言した変数と同じ名前の変数を隠す](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [方法: 継承された変数を隠す](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-an-inherited-variable.md)
- [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)
- [Overrides](../../../../visual-basic/language-reference/modifiers/overrides.md)
- [Me、My、MyBase、および MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [継承の基本](../../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
