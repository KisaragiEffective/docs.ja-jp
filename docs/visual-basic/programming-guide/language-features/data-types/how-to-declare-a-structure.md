---
title: '方法 : 構造体を宣言する'
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], structures
- structure statements [Visual Basic]
- statements [Visual Basic], structure
- structures [Visual Basic], declaring
ms.assetid: d5e98381-eb81-47d4-af83-48cc534a2572
ms.openlocfilehash: 41d2d03064dea703909218de56feb863526c220b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350010"
---
# <a name="how-to-declare-a-structure-visual-basic"></a>方法: 構造体を宣言する (Visual Basic)
Structure[ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)を使用して構造体の宣言を開始し、`End Structure` ステートメントで終了します。 これら2つのステートメントの間には、少なくとも1つの*要素*を宣言する必要があります。 要素は任意のデータ型にすることができますが、少なくとも1つは非共有変数または非共有のカスタムイベントのいずれかである必要があります。  
  
 構造体の宣言で構造体の要素を初期化することはできません。 変数を構造体型として宣言する場合は、変数を使用して値にアクセスすることによって、要素に値を代入します。  
  
 構造体とクラスの違いについては、「[構造体とクラス](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-classes.md)」を参照してください。  
  
 デモンストレーションを目的として、従業員の名前、電話の内線番号、給与を追跡する状況について考えてみましょう。 構造体を使用すると、1つの変数でこれを行うことができます。  
  
### <a name="to-declare-a-structure"></a>構造体を宣言するには  
  
1. 構造体の開始および終了ステートメントを作成します。  
  
     [Public](../../../../visual-basic/language-reference/modifiers/public.md)、 [Protected](../../../../visual-basic/language-reference/modifiers/protected.md)、 [Friend](../../../../visual-basic/language-reference/modifiers/friend.md)、または[Private](../../../../visual-basic/language-reference/modifiers/private.md)キーワードを使用して構造体のアクセスレベルを指定できます。また、既定の `Public`に設定することもできます。  
  
    ```vb  
    Private Structure employee  
    End Structure  
    ```  
  
2. 構造体の本体に要素を追加します。  
  
     構造体には、少なくとも1つの要素が必要です。 すべての要素を宣言し、そのアクセスレベルを指定する必要があります。 キーワードを指定せずに[Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用すると、ユーザー補助の既定値は `Public`になります。  
  
    ```vb  
    Private Structure employee  
        Public givenName As String  
        Public familyName As String  
        Public phoneExtension As Long  
        Private salary As Decimal  
        Public Sub giveRaise(raise As Double)  
            salary *= raise  
        End Sub  
        Public Event salaryReviewTime()  
    End Structure  
    ```  
  
     前の例の `salary` フィールドは `Private`です。これは、コンテナークラスからであっても、構造体の外部ではアクセスできないことを意味します。 ただし、`giveRaise` プロシージャは `Public`ため、構造の外部から呼び出すことができます。 同様に、構造の外部から `salaryReviewTime` イベントを発生させることもできます。  
  
     変数、`Sub` プロシージャ、およびイベントに加えて、構造体の定数、`Function` プロシージャ、およびプロパティを定義することもできます。 少なくとも1つの引数を指定する場合は、*既定のプロパティ*として最大で1つのプロパティを指定できます。 [共有](../../../../visual-basic/language-reference/modifiers/shared.md)`Sub` プロシージャを使用して、イベントを処理できます。 詳細については、「[方法: Visual Basic で既定のプロパティを宣言して呼び出す](../../../../visual-basic/programming-guide/language-features/procedures/how-to-declare-and-call-a-default-property.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [構造体変数](../../../../visual-basic/programming-guide/language-features/data-types/structure-variables.md)
- [構造体とその他のプログラミング要素](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-other-programming-elements.md)
- [構造体とクラス](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-classes.md)
- [ユーザー定義型](../../../../visual-basic/language-reference/data-types/user-defined-data-type.md)
