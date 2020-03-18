---
title: 変数 '<variablename>' は、囲まれたブロック内の変数を非表示にします。
ms.date: 07/20/2015
f1_keywords:
- vbc30616
- bc30616
helpviewer_keywords:
- BC30616
ms.assetid: e7658ebc-da45-451b-a409-a0f8915f0beb
ms.openlocfilehash: 4312abef83728f432e2f6a492e5acad3450719b1
ms.sourcegitcommit: 35da8fb45b4cca4e59cc99a5c56262c356977159
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592062"
---
# <a name="variable-variablename-hides-a-variable-in-an-enclosing-block"></a>変数 '\<variablename > ' は、それを囲むブロック内の変数を非表示にします
ブロックで囲まれた変数に、別のローカル変数と同じ名前が指定されています。  
  
 **エラー ID:** BC30616  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 囲まれたブロック内の変数の名前を変更して、他のローカル変数と同じにならないようにします。 例 :  
  
    ```vb  
    Dim a, b, x As Integer  
    If a = b Then  
       Dim y As Integer = 20 ' Uniquely named block variable.  
    End If  
    ```  
  
- このエラーの一般的な原因は、イベントハンドラー内で `Catch e As Exception` を使用することです。 この場合は、`e`ではなく、`Catch` ブロック変数に `ex` という名前を指定します。  
  
- このエラーのもう1つの一般的な原因は、別の `Catch` ブロック内の `Try` ブロック内で宣言されたローカル変数にアクセスしようとすることです。 これを修正するには、`Try...Catch...Finally` 構造体の外で変数を宣言します。  
  
## <a name="see-also"></a>参照

- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
- [変数宣言](../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
