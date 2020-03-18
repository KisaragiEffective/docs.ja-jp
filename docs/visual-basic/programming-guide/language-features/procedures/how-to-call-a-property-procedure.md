---
title: '方法 : プロパティ プロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], property procedures
- procedure calls [Visual Basic], property procedures
ms.assetid: 96bc4d74-d9c3-4b7a-954d-58ac8553cd94
ms.openlocfilehash: 52e6c62ffb81c480ccc1abf06f04eb780218dbf1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340555"
---
# <a name="how-to-call-a-property-procedure-visual-basic"></a>方法: プロパティ プロシージャを呼び出す (Visual Basic)
プロパティプロシージャを呼び出すには、プロパティに値を格納するか、値を取得します。 プロパティには、変数にアクセスするのと同じ方法でアクセスします。  
  
 プロパティの `Set` プロシージャは値を格納し、その `Get` プロシージャは値を取得します。 ただし、これらのプロシージャを名前で明示的に呼び出すことはできません。 変数の値を格納または取得する場合と同様に、代入ステートメントまたは式でプロパティを使用します。 Visual Basic によって、プロパティのプロシージャが呼び出されます。  
  
### <a name="to-call-a-propertys-get-procedure"></a>プロパティの Get プロシージャを呼び出すには  
  
1. 変数名を使用する場合と同じように、式でプロパティ名を使用します。 変数または定数を使用できる場所であればどこでもプロパティを使用できます。  
  
     または  
  
     代入ステートメントの等号 (`=`) の後にあるプロパティ名を使用します。  
  
     次の例では、<xref:Microsoft.VisualBasic.DateAndTime.Now%2A> プロパティの値を読み取り、その `Get` プロシージャを暗黙的に呼び出します。  
  
     [!code-vb[VbVbalrDateProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDateProperties/VB/Module1.vb#4)]  
  
2. プロパティが引数を受け取る場合は、プロパティ名の後にかっこを付けて引数リストを囲みます。 引数がない場合は、必要に応じてかっこを省略できます。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、プロパティが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
 プロパティの値は、変数または定数と同様に式に参加します。または、代入ステートメントの左側にある変数またはプロパティに格納されます。  
  
### <a name="to-call-a-propertys-set-procedure"></a>プロパティの Set プロシージャを呼び出すには  
  
1. 代入ステートメントの左側にあるプロパティ名を使用します。  
  
     次の例では、<xref:Microsoft.VisualBasic.DateAndTime.TimeOfDay%2A> プロパティの値を設定し、`Set` プロシージャを暗黙的に呼び出します。  
  
     [!code-vb[VbVbcnProcedures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#11)]  
  
2. プロパティが引数を受け取る場合は、プロパティ名の後にかっこを付けて引数リストを囲みます。 引数がない場合は、必要に応じてかっこを省略できます。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、プロパティが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
 代入ステートメントの右辺に生成される値は、プロパティに格納されます。  
  
## <a name="see-also"></a>参照

- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
- [Get ステートメント](../../../../visual-basic/language-reference/statements/get-statement.md)
- [Set ステートメント](../../../../visual-basic/language-reference/statements/set-statement.md)
