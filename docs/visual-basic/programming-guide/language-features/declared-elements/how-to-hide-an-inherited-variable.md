---
title: '方法 : 継承された変数を隠す'
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declaration statements [Visual Basic], declared elements
- referencing declared elements [Visual Basic]
- declared elements [Visual Basic], referencing
- declared elements [Visual Basic], about declared elements
- variables [Visual Basic], hiding inherited
ms.assetid: 765728d9-7351-4a30-999d-b5f34f024412
ms.openlocfilehash: c20c36b26c90c82da4e8836799f499498ccc40e4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345350"
---
# <a name="how-to-hide-an-inherited-variable-visual-basic"></a>方法: 継承された変数を隠す (Visual Basic)

派生クラスは、その基本クラスのすべての定義を継承します。 基底クラスの要素と同じ名前を使用して変数を定義する場合は、派生クラスで変数を定義するときに、その基底クラスの要素を非表示にしたり、*シャドウ*したりすることができます。 これを行うと、シャドウ機構を明示的にバイパスしない限り、派生クラスのコードは変数にアクセスします。

継承された変数を非表示にするもう1つの理由は、基本クラスのリビジョンに対して保護することです。 基底クラスには、継承している要素を変更する変更が加えられる場合があります。 この場合、`Shadows` 修飾子は、派生クラスからの参照を、基底クラスの要素ではなく、変数に対して強制的に解決します。

## <a name="to-hide-an-inherited-variable"></a>継承された変数を非表示にするには

1. 非表示にする変数がクラスレベル (プロシージャ以外) で宣言されていることを確認してください。 それ以外の場合は、非表示にする必要はありません。
  
2. 派生クラス内で、変数を宣言する[Dim ステートメント](../../../language-reference/statements/dim-statement.md)を記述します。 継承された変数と同じ名前を使用します。

3. [Shadows](../../../language-reference/modifiers/shadows.md)キーワードを宣言に含めます。

     派生クラスのコードが変数名を参照すると、コンパイラは変数への参照を解決します。

     次の例は、継承された変数のシャドウを示しています。
  
    ```vb  
    Public Class ShadowBaseClass  
        Public shadowString As String = "This is the base class string."  
    End Class  
    Public Class ShadowDerivedClass  
        Inherits ShadowBaseClass  
        Public Shadows shadowString As String = "This is the derived class string."  
        Public Sub ShowStrings()  
            Dim s As String = $"Unqualified shadowString: {shadowString}{vbCrLf}MyBase.shadowString: {MyBase.shadowString}"
            MsgBox(s)  
        End Sub  
    End Class  
    ```  
  
     前の例では、基底クラスで `shadowString` 変数を宣言し、派生クラスでそれをシャドウしています。 派生クラスのプロシージャ `ShowStrings` は、名前 `shadowString` が修飾されていない場合に、文字列のシャドウバージョンを表示します。 `shadowString` が `MyBase` キーワードで修飾されている場合は、シャドウされたバージョンが表示されます。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング

シャドウでは、同じ名前の変数の複数のバージョンが導入されます。 コードステートメントが変数名を参照する場合、コンパイラが参照を解決するバージョンは、コードステートメントの場所や修飾文字列の存在などの要因によって異なります。 これにより、シャドウされた変数の意図しないバージョンを参照するリスクが増加する可能性があります。 シャドウされた変数へのすべての参照を完全に修飾することで、そのリスクを軽減することができます。

## <a name="see-also"></a>参照

- [宣言された要素の参照](references-to-declared-elements.md)
- [Visual Basic でのシャドウ処理](shadowing.md)
- [シャドウとオーバーライドの違い](differences-between-shadowing-and-overriding.md)
- [方法: 自分で宣言した変数と同じ名前の変数を隠す](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [方法: 派生クラスによって非表示になっている変数にアクセスする](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Overrides](../../../../visual-basic/language-reference/modifiers/overrides.md)
- [Me、My、MyBase、および MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [継承の基本](../objects-and-classes/inheritance-basics.md)
