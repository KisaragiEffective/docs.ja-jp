---
title: '方法 : 変数に値を格納する、および変数から値を取得する'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], retrieving values
- variables [Visual Basic], storing data
ms.assetid: 93744f46-bf78-4fa0-9640-1de01bc38d9a
ms.openlocfilehash: bc5a7377a5e2e4c7ebe7291fd5f0093c4d6e996d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346904"
---
# <a name="how-to-move-data-into-and-out-of-a-variable-visual-basic"></a>方法: 変数に値を格納する、および変数から値を取得する (Visual Basic)

変数に値を格納するには、代入ステートメントの左側に変数名を指定します。

## <a name="putting-data-in-a-variable"></a>変数にデータを格納する

#### <a name="to-store-a-value-in-a-variable"></a>変数に値を格納するには

- 代入ステートメントの左側にある変数名を使用します。

    次の例では、変数 `alpha`の値を設定します。

    ```vb
    alpha = (beta * 6.27) / (gamma + 2.1)
    ```

    代入ステートメントの右辺に生成される値は、変数に格納されます。

## <a name="getting-data-from-a-variable"></a>変数からのデータの取得

変数の値を取得するには、式に変数名を含めます。

#### <a name="to-retrieve-a-value-from-a-variable"></a>変数から値を取得するには

- 式では変数名を使用します。 定数またはリテラルを使用できる場所であればどこでも変数を使用できます。ただし、定数の値を定義する式では使用できません。

  \- または -

- 代入ステートメントで等号 (`=`) 記号に続く変数名を使用します。

  次の例では `startValue` 変数の値を読み取り、式で `counter` 変数の値を使用します。

  ```vb
  counter = startValue
  cellValue = (counter + 5) ^ 2
  ```

  変数の値は定数と同様に式に参加し、代入ステートメントの左側にある変数またはプロパティに格納されます。

## <a name="see-also"></a>参照

- [変数](../../../../visual-basic/programming-guide/language-features/variables/index.md)
- [変数宣言](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
