---
title: Static
ms.date: 07/20/2015
f1_keywords:
- vb.Static
helpviewer_keywords:
- static modifier
- Static keyword [Visual Basic]
ms.assetid: 19013910-4658-47b6-a22e-1744b527979e
ms.openlocfilehash: f020756466888f51298abb423997906ddc7caff7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350760"
---
# <a name="static-visual-basic"></a>Static (Visual Basic)
1つ以上の宣言されたローカル変数が引き続き存在し、宣言されているプロシージャの終了後もその最新の値を保持することを指定します。  
  
## <a name="remarks"></a>コメント  
 通常、プロシージャ内のローカル変数は、プロシージャが停止するとすぐに存在しなくなります。 静的変数は引き続き存在し、最新の値が保持されます。 次回コードがプロシージャを呼び出したとき、変数は再初期化されず、割り当てた最新の値も保持されます。 静的変数は、その変数が定義されているクラスまたはモジュールの有効期間にわたって存在し続けます。  
  
## <a name="rules"></a>ルール  
  
- **宣言コンテキスト。** `Static` は、ローカル変数に対してのみ使用できます。 つまり、`Static` 変数の宣言コンテキストはプロシージャ内のプロシージャまたはブロックである必要があり、ソースファイル、名前空間、クラス、構造体、またはモジュールにすることはできません。  
  
     構造体プロシージャ内で `Static` を使用することはできません。  
  
- `Static` ローカル変数のデータ型を推論できません。 詳細については、「[ローカル型の推論](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。  
  
- **結合された修飾子。** 同じ宣言内で `ReadOnly`、`Shadows`、または `Shared` と共に `Static` を指定することはできません。  
  
## <a name="behavior"></a>動作  
 `Shared` プロシージャで静的変数を宣言する場合、アプリケーション全体で使用できる静的変数のコピーは1つだけです。 クラスのインスタンスを参照する変数ではなく、クラス名を使用して `Shared` プロシージャを呼び出します。  
  
 `Shared`ていないプロシージャで静的変数を宣言した場合、クラスの各インスタンスに対して使用できる変数のコピーは1つだけです。 非共有プロシージャを呼び出すには、クラスの特定のインスタンスを指す変数を使用します。  
  
## <a name="example"></a>例  
 次の例は、`Static` の使い方を示しています。  
  
 [!code-vb[VbVbalrKeywords#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#5)]  
  
 `Static` 変数 `totalSales` は、1回だけ0に初期化されます。 `updateSales`を入力するたびに、`totalSales` に計算した最新の値がまだ含まれています。  
  
 このコンテキストでは、`Static` 修飾子を使用できます。  
  
 [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)  
  
## <a name="see-also"></a>参照

- [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)
- [Shared](../../../visual-basic/language-reference/modifiers/shared.md)
- [Visual Basic の有効期間](../../../visual-basic/programming-guide/language-features/declared-elements/lifetime.md)
- [変数宣言](../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [ローカル型の推論](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
