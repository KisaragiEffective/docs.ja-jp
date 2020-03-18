---
title: AddressOf 演算子
ms.date: 07/20/2015
f1_keywords:
- AddressOf
- vb.AddressOf
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- addresses, passing to API procedures
ms.assetid: 8105a59d-60d8-4ab5-b221-5899cdfacbf4
ms.openlocfilehash: e88520bd7e731a35b98c1d40c5210dc5d1314911
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350283"
---
# <a name="addressof-operator-visual-basic"></a>AddressOf 演算子 (Visual Basic)
特定のプロシージャを参照するデリゲートインスタンスを作成します。  
  
## <a name="syntax"></a>構文  
  
```vb  
AddressOf procedurename  
```  
  
## <a name="parts"></a>指定項目  
 `procedurename`  
 必須。 新しく作成されたデリゲートによって参照されるプロシージャを指定します。  
  
## <a name="remarks"></a>コメント  
 `AddressOf` 演算子は、`procedurename`によって指定されたサブまたは関数を指すデリゲートを作成します。 指定されたプロシージャがインスタンスメソッドの場合、デリゲートはインスタンスとメソッドの両方を参照します。 次に、デリゲートが呼び出されると、指定したインスタンスの指定したメソッドが呼び出されます。  
  
 `AddressOf` 演算子は、デリゲートコンストラクターのオペランドとして使用することも、コンパイラによってデリゲートの型を決定できるコンテキストで使用することもできます。  
  
## <a name="example"></a>例  
 この例では、`AddressOf` 演算子を使用して、ボタンの `Click` イベントを処理するデリゲートを指定します。  
  
 [!code-vb[VbVbalrDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#8)]  
  
## <a name="example"></a>例  
 次の例では、`AddressOf` 演算子を使用して、スレッドのスタートアップ関数を指定します。  
  
 [!code-vb[VbVbalrDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>関連項目

- [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)
- [デリゲート](../../../visual-basic/programming-guide/language-features/delegates/index.md)
