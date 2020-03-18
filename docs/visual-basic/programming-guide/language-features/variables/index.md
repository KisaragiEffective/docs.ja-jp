---
title: '変数:'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic]
- values [Visual Basic], storing
ms.assetid: 4cfaa06d-4ae3-4307-897b-cf599dc24caa
ms.openlocfilehash: 26433acea2b98ad0e67b9c35bec4eb8e88f7b7b6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352406"
---
# <a name="variables-in-visual-basic"></a>Visual Basic における変数
Visual Basic で計算を実行するときは、多くの場合、値を格納する必要があります。 たとえば、いくつかの値を計算し、比較し、比較の結果に応じて、さまざまな操作を実行する必要があるとします。 比較する場合には、その値を保持する必要があります。  
  
## <a name="usage"></a>使用方法  
 Visual Basic は、ほとんどのプログラミング言語と同様に、値を格納するために変数を使用します。 *変数*には (変数に含まれる値を参照するために使用する語である) 名前があります。 変数には、(変数に格納できるデータの種類を決定する) データ型もあります。 密接に関連するデータ項目のインデックス セットを格納する必要がある場合、変数で配列を表現することもできます。  
  
 ローカル型推論では、データ型を明示的に指定せずに変数を宣言できます。 代わりに、コンパイラは、初期化式の型から変数の型を推測します。 詳細については、「[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」と「[Option Infer ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)」を参照してください。  
  
## <a name="assigning-values"></a>値の代入  
 次の例のように、計算を実行し、結果を変数に割り当てるために、代入ステートメントを使用できます。  
  
 [!code-vb[VbVbalrVariables#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrVariables/VB/Class1.vb#1)]  
  
> [!NOTE]
> この例の等号 (`=`) は、等値演算子ではなく代入演算子です。 この値は、変数 `applesSold` に割り当てられます。  
  
 詳細については、「[方法 : 変数に値を格納する、および変数から値を取得する](../../../../visual-basic/programming-guide/language-features/variables/how-to-move-data-into-and-out-of-a-variable.md)」を参照してください。  
  
## <a name="variables-and-properties"></a>変数とプロパティ  
 変数と同様に、*プロパティ*はアクセス可能な値です。 ただし、変数よりも複雑です。 プロパティは、その値を設定し取得する方法を制御するコード ブロックを使用します。 詳細については、「[Visual Basic のプロパティと変数の違い](../../../../visual-basic/programming-guide/language-features/procedures/differences-between-properties-and-variables.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [変数のトラブルシューティング](../../../../visual-basic/programming-guide/language-features/variables/troubleshooting-variables.md)
- [方法 : 変数に値を格納する、および変数から値を取得する](../../../../visual-basic/programming-guide/language-features/variables/how-to-move-data-into-and-out-of-a-variable.md)
- [Visual Basic のプロパティと変数の違い](../../../../visual-basic/programming-guide/language-features/procedures/differences-between-properties-and-variables.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
