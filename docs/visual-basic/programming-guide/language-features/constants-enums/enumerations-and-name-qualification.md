---
title: 列挙型と名前の修飾
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- Imports statement [Visual Basic], namespace declarations
- declaring namespaces [Visual Basic], enumerations
- name collisions
- ambiguous names [Visual Basic], enumerations
- enumerations [Visual Basic], name qualification
- names [Visual Basic], avoiding conflicts
- namespaces [Visual Basic], declaring
- naming conflicts, enumerations
- naming conflicts, qualifying names
- declaring enumerations
- references [Visual Basic], enumeration members
- naming conventions [Visual Basic], naming conflicts
- declarations [Visual Basic], namespaces
ms.assetid: 08ba2738-df52-4140-bc55-f57c871c9b73
ms.openlocfilehash: 4121266447b771ba954ad52a46e0d8b88de3f9cc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347494"
---
# <a name="enumerations-and-name-qualification-visual-basic"></a>列挙型と名前修飾 (Visual Basic)
通常、列挙体のメンバーを参照する場合は、列挙名でメンバー名を修飾する必要があります。 たとえば、`Days` 列挙体の `Sunday` メンバーを参照するには、次の構文を使用します。  
  
 [!code-vb[VbEnumsTask#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#18)]  
  
## <a name="using-the-imports-statement"></a>Imports ステートメントの使用  
 次の例のように、コードの名前空間宣言セクションに `Imports` ステートメントを追加することで、完全修飾名を使用しないようにすることができます。  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 `Imports` ステートメントは、参照されるプロジェクトおよびアセンブリから名前空間名をインポートし、ステートメントが記述されているモジュールと同じプロジェクト内から名前空間名をインポートします。 このステートメントを追加すると、次の例のように、修飾子を使用せずに列挙メンバーを参照できます。  
  
 [!code-vb[VbEnumsTask#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#24)]  
  
 一連の関連する定数を列挙にまとめることによって、異なるコンテキストで同じ定数名を使用することができます。 たとえば、`Days` と `WorkDays` の列挙体の曜日定数に同じ名前を使用できます。 `Imports` ステートメントを列挙体と共に使用する場合は、あいまいな参照を避けるために注意する必要があります。 次に例を示します。  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 [!code-vb[VbEnumsTask#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#25)]  
  
 `Monday` が `Days` 列挙と `Workdays` 列挙の両方のメンバーであると仮定すると、このコードはコンパイラエラーを生成します。 個々の定数を参照するときにあいまいな参照を回避するには、定数名を列挙体で修飾します。 次のコードは、`Days` と `WorkDays` 列挙体の `Saturday` 定数を参照します。  
  
 [!code-vb[VbEnumsTask#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#32)]  
  
## <a name="see-also"></a>参照

- [定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)
- [方法: 列挙型を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [方法 : 列挙型のメンバーを参照する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [方法: Visual Basic 内の列挙体を反復処理する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-iterate-through-an-enumeration.md)
- [方法 : 列挙値に関連付けられている文字列を確認する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [列挙型を使用する状況](../../../../visual-basic/programming-guide/language-features/constants-enums/when-to-use-an-enumeration.md)
- [定数とリテラルのデータ型](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)
- [Enum ステートメント](../../../../visual-basic/language-reference/statements/enum-statement.md)
- [Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
