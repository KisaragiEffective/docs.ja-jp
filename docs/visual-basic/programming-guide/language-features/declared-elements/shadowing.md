---
title: シャドウ
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], shadowing
- overriding, and shadowing
- shadowing
- duplicate names [Visual Basic]
- shadowing, by inheritance
- declared elements [Visual Basic], referencing
- shadowing, by scope
- declared elements [Visual Basic], hiding
- naming conflicts, shadowing
- declared elements [Visual Basic], shadowing
- shadowing, and overriding
- scope [Visual Basic], shadowing
- Shadows keyword [Visual Basic], about
- objects [Visual Basic], names
- names [Visual Basic], shadowing
ms.assetid: 54bb4c25-12c4-4181-b4a0-93546053964e
ms.openlocfilehash: 034b5c0ecf3be6e77048fb7318e931801575f07a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345324"
---
# <a name="shadowing-in-visual-basic"></a>Visual Basic におけるシャドウ
2つのプログラミング要素が同じ名前を共有している場合、そのうちの1つは、もう一方を非表示にしたり*影*を付けることができます。 このような状況では、シャドウされた要素を参照することはできません。代わりに、コードで要素名を使用すると、Visual Basic コンパイラによってシャドウ要素に解決されます。  
  
## <a name="purpose"></a>用途  
 シャドウの主な目的は、クラスメンバーの定義を保護することです。 基本クラスには、既に定義されているものと同じ名前の要素を作成する変更が含まれる場合があります。 この場合、`Shadows` 修飾子は、新しい基底クラス要素ではなく、定義したメンバーに解決されるように、クラスを通じて参照を強制します。  
  
## <a name="types-of-shadowing"></a>シャドウの種類  
 要素は、2つの異なる方法で別の要素をシャドウできます。 シャドウ要素は、シャドウされた要素を含む領域のサブ領域内で宣言できます。この場合、シャドウは*スコープによって*行われます。 または、派生クラスで基底クラスのメンバーを再定義できます。この場合、シャドウは*継承によって*行われます。  
  
### <a name="shadowing-through-scope"></a>スコープによるシャドウ処理  
 同じモジュール、クラス、または構造体のプログラミング要素が同じ名前でスコープが異なる場合があります。 2つの要素がこのように宣言されていて、コードが共有する名前を参照している場合、狭いスコープの要素は他の要素をシャドウします (ブロックスコープは最も狭いものです)。  
  
 たとえば、モジュールは `temp`という名前の `Public` 変数を定義できます。モジュール内のプロシージャは、`temp`という名前のローカル変数も宣言できます。 プロシージャ内から `temp` への参照はローカル変数にアクセスしますが、プロシージャの外部から `temp` への参照は `Public` 変数にアクセスします。 この場合、プロシージャ変数 `temp`、モジュール変数 `temp`をシャドウします。  
  
 次の図は、両方とも `temp`という名前の2つの変数を示しています。 ローカル変数は、独自のプロシージャ `p`内からアクセスされるときに、メンバー変数 `temp` `temp` シャドウします。 ただし、`MyClass` キーワードはシャドウをバイパスし、メンバー変数にアクセスします。  
  
 ![スコープによるシャドウ処理を示すグラフィック。](./media/shadowing/shadow-scope-diagram.gif)
  
 スコープを使用したシャドウの例については、「[方法: 変数と同じ名前の変数を非表示に](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-a-variable-with-the-same-name-as-your-variable.md)する」を参照してください。  
  
### <a name="shadowing-through-inheritance"></a>継承によるシャドウ処理  
 派生クラス内の複数の要素を持つ同じ基底クラス要素をシャドウする場合、シャドウされる要素は、その要素のオーバーロードされたバージョンになります。詳細については、「[プロシージャのオーバーロード](../../../../visual-basic/programming-guide/language-features/procedures/procedure-overloading.md)」を参照してください。 
  
 次の図は、基本クラス `b` と、`b`から継承する派生クラス `d` を示しています。 基底クラスは `proc`という名前のプロシージャを定義し、派生クラスは同じ名前の別のプロシージャでそれをシャドウします。 最初の `Call` ステートメントは、派生クラスのシャドウ `proc` にアクセスします。 ただし、`MyBase` キーワードはシャドウをバイパスし、基本クラスのシャドウされたプロシージャにアクセスします。  
  
 ![継承によるシャドウのグラフィック ダイアグラム](./media/shadowing/shadowing-inherit-diagram.gif)  
  
 継承によるシャドウ処理の例については、「[方法: 変数と同じ名前の変数を非表示](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-a-variable-with-the-same-name-as-your-variable.md)にする」および「[方法: 継承された変数を非表示にする](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-hide-an-inherited-variable.md)」を参照してください。  
  
#### <a name="shadowing-and-access-level"></a>シャドウとアクセスレベル  
 シャドウ要素は、派生クラスを使用してコードから常にアクセスできるとは限りません。 たとえば、`Private`として宣言されている可能性があります。 このような場合は、シャドウが解消され、シャドウが存在しない場合と同じ要素への参照がコンパイラによって解決されます。 この要素は、アクセス可能な要素であり、シャドウクラスから逆方向に derivational ステップが最も少ない。 シャドウされた要素がプロシージャの場合、解決策は、同じ名前、パラメーターリスト、および戻り値の型を持つアクセス可能な最も近いバージョンになります。  
  
 次の例は、3つのクラスの継承階層を示しています。 各クラスは `Sub` プロシージャ `display`を定義します。各派生クラスは、基本クラスの `display` プロシージャをシャドウします。  
  
```vb  
Public Class firstClass  
    Public Sub display()  
        MsgBox("This is firstClass")  
    End Sub  
End Class  
Public Class secondClass  
    Inherits firstClass  
    Private Shadows Sub display()  
        MsgBox("This is secondClass")  
    End Sub  
End Class  
Public Class thirdClass  
    Inherits secondClass  
    Public Shadows Sub display()  
        MsgBox("This is thirdClass")  
    End Sub  
End Class  
Module callDisplay  
    Dim first As New firstClass  
    Dim second As New secondClass  
    Dim third As New thirdClass  
    Public Sub callDisplayProcedures()  
        ' The following statement displays "This is firstClass".  
        first.display()  
        ' The following statement displays "This is firstClass".  
        second.display()  
        ' The following statement displays "This is thirdClass".  
        third.display()  
    End Sub  
End Module  
```  
  
 前の例では、派生クラス `secondClass`、`Private` プロシージャと共に `display` シャドウしています。 モジュール `callDisplay` が `secondClass`で `display` を呼び出すと、呼び出し元のコードが `secondClass` 外部にあるため、プライベート `display` プロシージャにアクセスできません。 シャドウが解消され、コンパイラが基底クラス `display` プロシージャへの参照を解決します。  
  
 ただし、さらに派生したクラス `thirdClass` は `display` を `Public`として宣言するので、`callDisplay` のコードからアクセスできます。  
  
## <a name="shadowing-and-overriding"></a>シャドウとオーバーライド  
 シャドウは、オーバーライドと混同しないようにしてください。 どちらも、派生クラスが基底クラスから継承し、宣言された1つの要素を別の要素で再定義するときに使用されます。 ただし、2つの間には大きな違いがあります。 比較については、「[シャドウとオーバーライドの違い](../../../../visual-basic/programming-guide/language-features/declared-elements/differences-between-shadowing-and-overriding.md)」を参照してください。  
  
## <a name="shadowing-and-overloading"></a>シャドウとオーバーロード  
 派生クラス内の複数の要素を持つ同じ基底クラス要素をシャドウする場合、シャドウされる要素は、その要素のオーバーロードされたバージョンになります。 詳細については、「[プロシージャのオーバーロード](../../../../visual-basic/programming-guide/language-features/procedures/procedure-overloading.md)」を参照してください。  
  
## <a name="accessing-a-shadowed-element"></a>シャドウ要素へのアクセス  
 派生クラスから要素にアクセスする場合、通常は、その派生クラスの現在のインスタンスを使用します。そのためには、要素名を `Me` キーワードで修飾します。 派生クラスが基底クラスの要素をシャドウする場合は、`MyBase` キーワードを使用して修飾することで、基底クラスの要素にアクセスできます。  
  
 シャドウされた要素にアクセスする例については、「[方法: 派生クラスによって非表示になっている変数にアクセスする](../../../../visual-basic/programming-guide/language-features/declared-elements/how-to-access-a-variable-hidden-by-a-derived-class.md)」を参照してください。  
  
### <a name="declaration-of-the-object-variable"></a>オブジェクト変数の宣言  
 オブジェクト変数を作成する方法は、派生クラスがシャドウ要素またはシャドウされた要素にアクセスするかどうかにも影響します。 次の例では、派生クラスから2つのオブジェクトを作成しますが、一方のオブジェクトは基底クラスとして、もう1つは派生クラスとして宣言されています。  
  
```vb  
Public Class baseCls  
    ' The following statement declares the element that is to be shadowed.  
    Public z As Integer = 100  
End Class  
Public Class dervCls  
    Inherits baseCls  
    ' The following statement declares the shadowing element.  
    Public Shadows z As String = "*"  
End Class  
Public Class useClasses  
    ' The following statement creates the object declared as the base class.  
    Dim basObj As baseCls = New dervCls()  
    ' Note that dervCls widens to its base class baseCls.  
    ' The following statement creates the object declared as the derived class.  
    Dim derObj As dervCls = New dervCls()  
    Public Sub showZ()   
    ' The following statement outputs 100 (the shadowed element).  
        MsgBox("Accessed through base class: " & basObj.z)  
    ' The following statement outputs "*" (the shadowing element).  
        MsgBox("Accessed through derived class: " & derObj.z)  
    End Sub  
End Class  
```  
  
 前の例では、変数 `basObj` が基本クラスとして宣言されています。 `dervCls` オブジェクトをこのオブジェクトに割り当てると、拡大変換が構成されるため、有効になります。 ただし、基底クラスは派生クラスの変数 `z` のシャドウバージョンにアクセスできないため、コンパイラは `basObj.z` を元の基底クラスの値に解決します。  
  
## <a name="see-also"></a>参照

- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic 内のスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
- [拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)
- [Overrides](../../../../visual-basic/language-reference/modifiers/overrides.md)
- [Me、My、MyBase、および MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [継承の基本](../../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
