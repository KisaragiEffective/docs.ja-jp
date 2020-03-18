---
title: 構造体の変数
ms.date: 07/20/2015
helpviewer_keywords:
- structures [Visual Basic], variables
- structures [Visual Basic], structure variables
- variables [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: 156872f8-aabc-4454-8e2d-f2253c3c13c9
ms.openlocfilehash: 16b6cdc5a849b50f6caa8b7963dac5c12d63cf3e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346305"
---
# <a name="structure-variables-visual-basic"></a>構造体の変数 (Visual Basic)

構造体を作成したら、その型としてプロシージャレベルとモジュールレベルの変数を宣言できます。 たとえば、コンピューターシステムに関する情報を記録する構造体を作成できます。 次に例を示します。

```vb
Public Structure systemInfo
    Public cPU As String
    Public memory As Long
    Public purchaseDate As Date
End Structure
```

これで、その型の変数を宣言できるようになりました。 次の宣言はこれを示しています。

```vb
Dim mySystem, yourSystem As systemInfo
```

> [!NOTE]
> クラスとモジュールでは、 [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)を使用して宣言された構造体は、既定でパブリックアクセスになります。 構造体をプライベートにする場合は、 [private](../../../../visual-basic/language-reference/modifiers/private.md)キーワードを使用して宣言してください。

## <a name="access-to-structure-values"></a>構造体の値へのアクセス

構造体変数の要素の値を割り当てたり、取得したりするには、オブジェクトのプロパティの設定と取得に使用するのと同じ構文を使用します。 メンバーアクセス演算子 (`.`) は、構造体変数名と要素名の間に配置します。 次の例では、以前に型 `systemInfo`として宣言された変数の要素にアクセスします。

```vb
mySystem.cPU = "486"
Dim tooOld As Boolean
If yourSystem.purchaseDate < #1/1/1992# Then tooOld = True
```

## <a name="assigning-structure-variables"></a>構造体変数の割り当て

また、両方が同じ構造体型である場合は、1つの変数を別の変数に割り当てることもできます。 これにより、1つの構造体のすべての要素が、他方の内の対応する要素にコピーされます。 次の宣言はこれを示しています。

```vb
yourSystem = mySystem
```

構造体要素が `String`、`Object`、配列などの参照型である場合、データへのポインターがコピーされます。 前の例では、`systemInfo` にオブジェクト変数が含まれていた場合、前の例ではポインターが `mySystem` から `yourSystem`にコピーされています。また、1つの構造体を介してオブジェクトのデータを変更すると、他の構造体を使用してアクセスしたときに有効になります。

## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [複合データ型](../../../../visual-basic/programming-guide/language-features/data-types/composite-data-types.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [構造体](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [方法 : 構造体を宣言する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [構造体とその他のプログラミング要素](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-other-programming-elements.md)
- [構造体とクラス](../../../../visual-basic/programming-guide/language-features/data-types/structures-and-classes.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
