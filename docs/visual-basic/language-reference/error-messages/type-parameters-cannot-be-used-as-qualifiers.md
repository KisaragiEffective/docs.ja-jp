---
title: 型パラメーターは修飾子として使用できません。
ms.date: 07/20/2015
f1_keywords:
- vbc32098
- bc32098
helpviewer_keywords:
- BC32098
ms.assetid: bab05325-dde8-4621-a5f6-368b5b7b2d76
ms.openlocfilehash: 3ff4b189539bf119351a94dabadd596c336ac723
ms.sourcegitcommit: d7c298f6c2e3aab0c7498bfafc0a0a94ea1fe23e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2019
ms.locfileid: "72250336"
---
# <a name="type-parameters-cannot-be-used-as-qualifiers"></a>型パラメーターは修飾子として使用できません。

プログラミング要素は、型パラメーターを含む修飾文字列で修飾されます。

型パラメーターは、ジェネリック型が構築されるときに指定される型の要件を表します。 定義されている特定の型を表していません。 修飾文字列には、コンパイル時に定義された要素のみを含める必要があります。

次のコードでは、このエラーが発生する可能性があります。

```vb  
Public Function CheckText(Of c As System.Windows.Forms.Control)(
    badText As String) As Boolean
  
    Dim saveText As c.Text  
    ' Insert code to look for badText within saveText.
End Function  
```  
  
 **エラー ID:** BC32098  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. 修飾文字列から型パラメーターを削除するか、定義された型で置き換えます。  
  
2. 構築された型を使用して、修飾するプログラミング要素を見つける必要がある場合は、追加のプログラムロジックを使用する必要があります。  
  
## <a name="see-also"></a>関連項目

- [宣言された要素の参照](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Visual Basic におけるジェネリック型](../../programming-guide/language-features/data-types/generic-types.md)
- [型リスト](../statements/type-list.md)
