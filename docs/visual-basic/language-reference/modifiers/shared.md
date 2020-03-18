---
title: Shared (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Shared
helpviewer_keywords:
- Shared keyword [Visual Basic]
- members [Visual Basic], shared
- shared members
- nonshared
- shared [elements VB]
- elements [Visual Basic], shared
ms.assetid: 2bf7cf2c-b0dd-485e-8749-b5d674dab4cd
ms.openlocfilehash: 98fa25d2283408dfb80e82fbc620a1b284e5c530
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349124"
---
# <a name="shared-visual-basic"></a>Shared (Visual Basic)
1つ以上の宣言されたプログラミング要素が、クラスまたは構造体の特定のインスタンスではなく、大規模なクラスまたは構造体に関連付けられていることを指定します。  
  
## <a name="remarks"></a>コメント  
  
## <a name="when-to-use-shared"></a>共有を使用する場合  
 クラスまたは構造体のメンバーを共有すると、共有ではなく、すべてのインスタンスで使用できるようになります。この*場合、各*インスタンスは独自のコピーを保持します。 これは、変数の値がアプリケーション全体に適用される場合などに便利です。 この変数を `Shared`するように宣言すると、すべてのインスタンスが同じストレージの場所にアクセスし、1つのインスタンスが変数の値を変更すると、すべてのインスタンスが更新された値にアクセスします。  
  
 共有によってメンバーのアクセスレベルが変更されることはありません。 たとえば、クラスメンバーは、共有およびプライベート (クラス内からのみアクセス可能)、または非共有およびパブリックにすることができます。 詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
  
- **宣言コンテキスト。** `Shared` は、モジュール レベルでのみ使用できます。 つまり、`Shared` 要素の宣言コンテキストは、クラスまたは構造体である必要があり、ソースファイル、名前空間、またはプロシージャにすることはできません。  
  
- **修飾子の結合。** 同じ宣言で、[Overrides](../../../visual-basic/language-reference/modifiers/overrides.md)、 [Overridable](../../../visual-basic/language-reference/modifiers/overridable.md)、 [NotOverridable](../../../visual-basic/language-reference/modifiers/notoverridable.md)、 [MustOverride](../../../visual-basic/language-reference/modifiers/mustoverride.md)、または[Static](../../../visual-basic/language-reference/modifiers/static.md) と共に `Shared` を指定することはできません。  
  
- **アクセス。 ** 共有要素には、そのクラスまたは構造体の特定のインスタンスの変数名ではなく、そのクラスまたは構造体の名前を修飾することによってアクセスします。 クラスまたは構造体のインスタンスを作成しなくても、その共有メンバーにアクセスできます。  
  
     次の例では、<xref:System.Double> 構造によって公開される <xref:System.Double.IsNaN%2A> 共有プロシージャを呼び出します。  
  
     `If Double.IsNaN(result) Then MsgBox("Result is mathematically undefined.")`  
  
- **暗黙の共有。** [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)では `Shared` 修飾子を使用できませんが、定数は暗黙的に共有されます。 同様に、モジュールまたはインターフェイスのメンバーを `Shared`するように宣言することはできませんが、暗黙的に共有されます。  
  
## <a name="behavior"></a>動作  
  
- **ストレージ。 ** 共有変数またはイベントは、クラスまたは構造体に生成されたインスタンスの数に関係なく、メモリに 1 回だけ格納されます。 同様に、共有プロシージャまたはプロパティは、ローカル変数のセットを 1 つだけ保持します。  
  
- **インスタンス変数を使用したアクセス。** 共有要素にアクセスするには、そのクラスまたは構造体の特定のインスタンスを含む変数の名前を指定します。 これは通常、想定どおりに動作しますが、コンパイラは警告メッセージを生成し、変数ではなくクラスまたは構造体の名前を使用してアクセスを行います。  
  
- **インスタンス式を使用したアクセス。** クラスまたは構造体のインスタンスを返す式を使用して共有要素にアクセスする場合、コンパイラは式を評価するのではなく、クラスまたは構造体の名前を使用してアクセスを行います。 これにより、他のアクションを実行するための式を作成する場合や、インスタンスを返す場合に、予期しない結果が発生します。 これを次の例に示します。  
  
    ```vb
    Sub main()  
        shareTotal.total = 10  
        ' The preceding line is the preferred way to access total.  
        Dim instanceVar As New shareTotal  
        instanceVar.total += 100  
        ' The preceding line generates a compiler warning message and  
        ' accesses total through class shareTotal instead of through  
        ' the variable instanceVar. This works as expected and adds  
        ' 100 to total.  
        returnClass().total += 1000  
        ' The preceding line generates a compiler warning message and  
        ' accesses total through class shareTotal instead of calling  
        ' returnClass(). This adds 1000 to total but does not work as  
        ' expected, because the MsgBox in returnClass() does not run.  
        MsgBox("Value of total is " & CStr(shareTotal.total))  
    End Sub  
    Public Function returnClass() As shareTotal  
        MsgBox("Function returnClass() called")  
        Return New shareTotal  
    End Function  
    Public Class shareTotal  
        Public Shared total As Integer  
    End Class  
    ```  
  
     前の例では、コードがインスタンスを介して `total` 共有変数にアクセスするときに、コンパイラによって警告メッセージが生成されます。 各ケースでは、クラス `shareTotal` を通じて直接アクセスを行い、インスタンスを使用しません。 このプロシージャ `returnClass`の呼び出しの場合、これは `returnClass`の呼び出しを生成しないことを意味するため、"関数 returnClass ()" を表示する追加のアクションは実行されません。  
  
 `Shared` 修飾子は、次のコンテキストで使用できます。  
  
 [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)  
  
 [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)  
  
 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>参照

- [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)
- [Static](../../../visual-basic/language-reference/modifiers/static.md)
- [Visual Basic における有効期間](../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)
- [Visual Basic におけるプロシージャ](../../../visual-basic/programming-guide/language-features/procedures/index.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
