---
title: ラムダ式内で繰り返し変数を使用すると、予期しない結果が発生する可能性があります。
ms.date: 07/20/2015
f1_keywords:
- vbc42324
- bc42324
helpviewer_keywords:
- BC42324
ms.assetid: b5c2c4bd-3b2a-4a73-aaeb-55728eb03b68
ms.openlocfilehash: 3335da503b6fb9c33e44266997cc945214a3a365
ms.sourcegitcommit: e08b319358a8025cc6aa38737854f7bdb87183d6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2019
ms.locfileid: "64913088"
---
# <a name="using-the-iteration-variable-in-a-lambda-expression-may-have-unexpected-results"></a>ラムダ式内で繰り返し変数を使用すると、予期しない結果が発生する可能性があります。
ラムダ式で繰り返し変数を使用する必要があります予期しない結果。 代わりに、ループ内でローカル変数を作成し、反復変数の値を割り当てます。  
  
 ループ内で宣言されているラムダ式でループの反復変数を使用すると、この警告が表示されます。 たとえば、次の例に表示される警告が発生します。  
  
```vb  
For i As Integer = 1 To 10  
    ' The warning is given for the use of i.  
    Dim exampleFunc As Func(Of Integer) = Function() i  
Next  
```  
  
 次の例では、発生する可能性が予期しない結果を示します。  
  
```vb  
Module Module1  
    Sub Main()  
        Dim array1 As Func(Of Integer)() = New Func(Of Integer)(4) {}  
  
        For i As Integer = 0 To 4  
            array1(i) = Function() i  
        Next  
  
        For Each funcElement In array1  
            System.Console.WriteLine(funcElement())  
        Next  
  
    End Sub  
End Module  
```  
  
 `For`ループがループの反復変数の値を返します、ラムダ式の配列を作成します`i`します。 ラムダ式を評価するときに、`For Each`ループされる予定である 0、1、2、3、および 4 の連続する値を表示するを参照してください`i`で、`For`ループします。 最終的な値を表示する代わりに、 `i` 5 回表示されます。  
  
 `5`  
  
 `5`  
  
 `5`  
  
 `5`  
  
 `5`  
  
 既定では、このメッセージは警告です。 警告を表示しない方法や、警告をエラーとして扱う方法の詳細については、「[Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC42324  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 反復変数の値をローカル変数に割り当てるし、ローカル変数をラムダ式で使用します。  
  
```vb  
Module Module1  
    Sub Main()  
        Dim array1 As Func(Of Integer)() = New Func(Of Integer)(4) {}  
  
        For i As Integer = 0 To 4  
            Dim j = i  
            array1(i) = Function() j  
        Next  
  
        For Each funcElement In array1  
            System.Console.WriteLine(funcElement())  
        Next  
  
    End Sub  
End Module  
```  
  
## <a name="see-also"></a>関連項目

- [ラムダ式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
