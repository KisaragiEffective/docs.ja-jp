---
title: '方法 : プロパティから値を取得する'
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- Visual Basic code, properties
- properties [Visual Basic], values
ms.assetid: 3954423e-6ab7-4a4c-b55c-a8d27be47891
ms.openlocfilehash: 85512d4311d3e731a2c4e129d6a01f9b3273b333
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74339826"
---
# <a name="how-to-get-a-value-from-a-property-visual-basic"></a>方法: プロパティから値を取得する (Visual Basic)
プロパティの値を取得するには、プロパティ名を式に含めます。  
  
 プロパティの `Get` プロシージャは値を取得しますが、名前で明示的に呼び出すことはありません。 変数を使用する場合と同じように、プロパティを使用します。 Visual Basic によって、プロパティのプロシージャが呼び出されます。  
  
### <a name="to-retrieve-a-value-from-a-property"></a>プロパティから値を取得するには  
  
1. 変数名を使用する場合と同じように、式でプロパティ名を使用します。 変数または定数を使用できる場所であればどこでもプロパティを使用できます。  
  
     または  
  
     代入ステートメントの等号 (`=`) の後にあるプロパティ名を使用します。  
  
     次の例では、Visual Basic `Now` プロパティの値を読み取り、その `Get` プロシージャを暗黙的に呼び出します。  
  
     [!code-vb[VbVbalrDateProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDateProperties/VB/Module1.vb#4)]  
  
2. プロパティが引数を受け取る場合は、プロパティ名の後にかっこを付けて引数リストを囲みます。 引数がない場合は、必要に応じてかっこを省略できます。  
  
3. 引数リストに引数をコンマで区切ってかっこ内に配置します。 引数は、プロパティが対応するパラメーターを定義する順序と同じ順序で指定してください。  
  
 プロパティの値は、変数または定数と同様に式に参加します。または、代入ステートメントの左側にある変数またはプロパティに格納されます。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
