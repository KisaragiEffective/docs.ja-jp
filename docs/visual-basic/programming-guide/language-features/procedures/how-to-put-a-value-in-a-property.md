---
title: '方法 : プロパティに値を格納する'
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- Visual Basic code, properties
- properties [Visual Basic], values
ms.assetid: c39401e5-b5fc-4439-8f31-ed640f7ce6ed
ms.openlocfilehash: ad0d0e81f94dd3dead50f21c3bd6ff580c004dd6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346054"
---
# <a name="how-to-put-a-value-in-a-property-visual-basic"></a>方法: プロパティに値を格納する (Visual Basic)
プロパティに値を格納するには、プロパティ名を代入ステートメントの左側に配置します。  
  
 プロパティの `Set` プロシージャには値が格納されますが、名前で明示的に呼び出すことはありません。 変数を使用する場合と同じように、プロパティを使用します。 Visual Basic によって、プロパティのプロシージャが呼び出されます。  
  
### <a name="to-store-a-value-in-a-property"></a>プロパティに値を格納するには  
  
1. 代入ステートメントの左側にあるプロパティ名を使用します。  
  
     次の例では、Visual Basic `TimeOfDay` プロパティの値を正午に設定し、その `Set` プロシージャを暗黙的に呼び出します。  
  
     [!code-vb[VbVbcnProcedures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#11)]  
  
2. プロパティが引数を受け取る場合は、プロパティ名の後にかっこを付けて引数リストを囲みます。 引数がない場合は、必要に応じてかっこを省略できます。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、プロパティが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
4. 代入ステートメントの右辺に生成される値は、プロパティに格納されます。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.DateAndTime.TimeOfDay%2A>
- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
