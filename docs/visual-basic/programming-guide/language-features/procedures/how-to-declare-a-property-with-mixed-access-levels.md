---
title: '方法 : 複数のアクセス レベルを持つプロパティを宣言する'
ms.date: 07/20/2015
helpviewer_keywords:
- access levels [Visual Basic], properties
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- mixed access levels
- Visual Basic code, properties
- properties [Visual Basic], access levels
- Property statement [Visual Basic], declaring mixed access levels
ms.assetid: fdbb2d97-279a-4956-b26c-cbdfbc34915a
ms.openlocfilehash: d74e23f33fbf7d9d29ab84b9b1bd4fc08863ac48
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349697"
---
# <a name="how-to-declare-a-property-with-mixed-access-levels-visual-basic"></a>方法: 複数のアクセス レベルを持つプロパティを宣言する (Visual Basic)
プロパティの `Get` および `Set` プロシージャのアクセスレベルを変更するには、`Property` ステートメントでより制限の緩いレベルを使用し、`Get` または `Set` ステートメントでより制限の厳しいレベルを使用できます。 プロパティで混合アクセスレベルを使用するのは、コードの特定の部分がプロパティの値を取得できるようにする場合と、コードの他の部分で値を変更できるようにする場合です。  
  
 アクセスレベルの詳細については、「[Visual Basic でのアクセス レベル](../../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
### <a name="to-declare-a-property-with-mixed-access-levels"></a>混合アクセスレベルを持つプロパティを宣言するには  
  
1. 通常の方法でプロパティを宣言し、`Property` ステートメントで制限の緩いアクセスレベル (`Public`など) を指定します。  
  
2. より厳しいアクセスレベル (`Friend`など) を指定する `Get` または `Set` プロシージャを宣言します。  
  
3. 他のプロパティプロシージャにアクセスレベルを指定しないでください。 `Property` ステートメントで宣言されたアクセスレベルを前提としています。 アクセスは、プロパティプロシージャのいずれか1つに対してのみ制限できます。  
  
     [!code-vb[VbVbcnProcedures#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#10)]  
  
     前の例では、`Get` プロシージャは、プロパティ自体と同じ `Protected` アクセスを持ちますが、`Set` プロシージャは `Private` アクセスします。 `employee` から派生したクラスは `salary` 値を読み取ることができますが、設定できるのは `employee` クラスだけです。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
