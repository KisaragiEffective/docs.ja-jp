---
title: '方法 : 変数のスコープを制御する'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], scope
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- variables [Visual Basic], visibility
- scope [Visual Basic], declared elements
- scope [Visual Basic], variables
- scope [Visual Basic], Visual Basic
- declared elements [Visual Basic], visibility
- visibility [Visual Basic], variables
ms.assetid: 44b7f62a-cb5c-4d50-bce9-60ae68f87072
ms.openlocfilehash: 0ee6ce183310aa836ecdbbc0bc819e0e83d1872d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345377"
---
# <a name="how-to-control-the-scope-of-a-variable-visual-basic"></a>方法: 変数のスコープを制御する (Visual Basic)
通常、変数は、宣言する領域全体で*スコープ*内にあるか、参照用に表示されます。 場合によっては、変数の*アクセスレベル*がそのスコープに影響を与えることがあります。  
  
 詳細については、「[Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)」を参照してください。  
  
## <a name="scope-at-block-or-procedure-level"></a>ブロックレベルまたはプロシージャレベルのスコープ  
  
#### <a name="to-make-a-variable-visible-only-within-a-block"></a>変数がブロック内でのみ表示されるようにするには  
  
- `For` ループの `For` ステートメントと `Next` ステートメントの間で、そのブロックの開始宣言ステートメントと終了宣言ステートメントの間に変数の[Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を配置します。  
  
     変数は、ブロック内からのみ参照できます。  
  
#### <a name="to-make-a-variable-visible-only-within-a-procedure"></a>プロシージャ内でのみ変数を表示するには  
  
- プロシージャ内に変数の `Dim` ステートメントを配置しますが、ブロックの外側 (`With`...`End With` ブロックなど) の内側に配置します。  
  
     プロシージャ内の変数を参照できるのは、プロシージャに含まれている任意のブロック内からです。  
  
## <a name="scope-at-module-or-namespace-level"></a>モジュールまたは名前空間レベルのスコープ  
 便宜上、単項*モジュールレベル*はモジュール、クラス、および構造体にも同様に適用されます。 モジュールレベル変数のアクセスレベルによってそのスコープが決まります。 モジュール、クラス、または構造体を含む名前空間もスコープに影響します。  
  
#### <a name="to-make-a-variable-visible-throughout-a-module-class-or-structure"></a>モジュール、クラス、または構造体全体で変数を表示するには  
  
1. 変数の `Dim` ステートメントをモジュール、クラス、または構造体の内部に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Private](../../../../visual-basic/language-reference/modifiers/private.md)キーワードを含めます。  
  
3. 変数は、モジュール、クラス、または構造体内のどこからでも参照できますが、外部からは参照できません。  
  
#### <a name="to-make-a-variable-visible-throughout-a-namespace"></a>名前空間全体で変数を表示するには  
  
1. 変数の `Dim` ステートメントをモジュール、クラス、または構造体の内部に配置しますが、プロシージャの外側に配置します。  
  
2. `Dim` ステートメントに[Friend](../../../../visual-basic/language-reference/modifiers/friend.md)キーワードまたは[Public](../../../../visual-basic/language-reference/modifiers/public.md)キーワードを含めます。  
  
3. 変数は、モジュール、クラス、または構造体を含む名前空間内の任意の場所から参照できます。  
  
## <a name="example"></a>例  
 次の例では、モジュールレベルで変数を宣言し、その可視性をモジュール内のコードに制限しています。  
  
```vb  
Module demonstrateScope  
    Private strMsg As String  
    Sub initializePrivateVariable()  
        strMsg = "This variable cannot be used outside this module."  
    End Sub  
    Sub usePrivateVariable()  
        MsgBox(strMsg)  
    End Sub  
End Module  
```  
  
 前の例では、モジュール `demonstrateScope` で定義されているすべてのプロシージャが、`String` 変数 `strMsg`を参照できます。 `usePrivateVariable` プロシージャが呼び出されると、ダイアログボックスに `strMsg` 文字列変数の内容が表示されます。  
  
 前の例を次のように変更すると、`strMsg` 文字列変数は、宣言の名前空間内の任意の場所でコードによって参照できます。  
  
```vb  
Public strMsg As String  
```  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 変数の範囲が狭いほど、同じ名前を持つ別の変数の代わりに、誤って変数を参照してしまう可能性が低くなります。 参照の一致に関する問題を最小限に抑えることもできます。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 変数の範囲が狭いほど、悪意のあるコードによって不適切に使用される可能性が小さくなります。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
- [Visual Basic における有効期間](../../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)
- [Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [変数](../../../../visual-basic/programming-guide/language-features/variables/index.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)
