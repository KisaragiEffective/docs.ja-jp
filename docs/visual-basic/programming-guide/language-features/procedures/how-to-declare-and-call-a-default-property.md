---
title: '方法: 既定のプロパティを宣言して呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- defaults [Visual Basic], properties
- properties [Visual Basic], default
- procedures [Visual Basic], defining
- default properties [Visual Basic], in Visual Basic
- Visual Basic code, procedures
- Visual Basic code, properties
- default properties
ms.assetid: 68b4026e-09ef-4613-808e-f6287494ff63
ms.openlocfilehash: b01188ed8a9ff4da95a6975dcac3509625fdffb2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349682"
---
# <a name="how-to-declare-and-call-a-default-property-in-visual-basic"></a>方法: 既定のプロパティを宣言して呼び出す (Visual Basic)
*既定のプロパティ*は、コードがそれを指定せずにアクセスできるクラスまたは構造体のプロパティです。 コードでクラスまたは構造体を呼び出すが、プロパティにはアクセスできない場合、Visual Basic は、そのクラスまたは構造体の既定のプロパティへのアクセスを解決します (存在する場合)。  
  
 クラスまたは構造体は、最大で1つの既定のプロパティを持つことができます。 ただし、既定のプロパティをオーバーロードして、複数のバージョンを持つことができます。  
  
 詳細については、「 [Default](../../../../visual-basic/language-reference/modifiers/default.md)」を参照してください。  
  
### <a name="to-declare-a-default-property"></a>既定のプロパティを宣言するには  
  
1. 通常の方法でプロパティを宣言します。 `Shared` または `Private` キーワードを指定しないでください。  
  
2. プロパティ宣言に `Default` キーワードを含めます。  
  
3. プロパティのパラメーターを少なくとも1つ指定してください。 少なくとも1つの引数を受け取らない既定のプロパティを定義することはできません。  
  
     [!code-vb[VbVbcnProcedures#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#17)]  
  
### <a name="to-call-a-default-property"></a>既定のプロパティを呼び出すには  
  
1. 含んでいるクラスまたは構造体型の変数を宣言します。  
  
     [!code-vb[VbVbcnProcedures#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#16)]  
  
2. 変数名は、通常、プロパティ名を含める式でのみ使用します。  
  
     [!code-vb[VbVbcnProcedures#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#21)]  
  
3. 変数名の後にかっこで囲んだ引数リストを指定します。 既定のプロパティは、少なくとも1つの引数を受け取る必要があります。  
  
     [!code-vb[VbVbcnProcedures#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#20)]  
  
4. 既定のプロパティ値を取得するには、式または代入ステートメントで等号 (`=`) に続く変数名を引数リストと共に使用します。  
  
     [!code-vb[VbVbcnProcedures#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#15)]  
  
5. 既定のプロパティ値を設定するには、代入ステートメントの左側にある引数リストを持つ変数名を使用します。  
  
     [!code-vb[VbVbcnProcedures#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#14)]  
  
6. 他のプロパティにアクセスする場合と同じように、既定のプロパティ名は常に変数名と共に指定できます。  
  
     [!code-vb[VbVbcnProcedures#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#19)]  
  
## <a name="example"></a>例  
 次の例では、クラスの既定のプロパティを宣言しています。  
  
 [!code-vb[VbVbcnProcedures#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#12)]  
  
## <a name="example"></a>例  
 次の例は、クラス `class1`で既定のプロパティ `myProperty` を呼び出す方法を示しています。 3つの代入ステートメントは `myProperty`に値を格納し、<xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 呼び出しは値を読み取ります。  
  
 [!code-vb[VbVbcnProcedures#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#13)]  
  
 既定のプロパティの最も一般的な用途は、さまざまなコレクションクラスの <xref:Microsoft.VisualBasic.Collection.Item%2A> プロパティです。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 既定のプロパティを使用すると、ソースコード文字が小さくなることがありますが、コードの読み取りが困難になる可能性があります。 呼び出し元のコードがクラスまたは構造体に精通していない場合、クラスまたは構造体の名前への参照を作成するときに、その参照がクラスまたは構造体自体、または既定のプロパティにアクセスするかどうかを特定できません。 これにより、コンパイラエラーまたは微妙な実行時のロジックエラーが発生する可能性があります。  
  
 [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)を常に使用してコンパイラの型チェックを `On`に設定することにより、既定のプロパティエラーが発生する可能性を多少減らすことができます。  
  
 定義済みのクラスまたは構造体をコード内で使用する予定の場合は、既定のプロパティがあるかどうかを判断し、存在する場合はその名前を確認する必要があります。  
  
 これらの欠点があるため、既定のプロパティを定義しないことを検討してください。 コードを読みやすくするために、常にすべてのプロパティを明示的に参照することも検討する必要があります。既定のプロパティも同様です。  
  
## <a name="see-also"></a>参照

- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Shared](../../../../visual-basic/language-reference/modifiers/default.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
