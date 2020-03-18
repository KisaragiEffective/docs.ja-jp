---
title: '方法: 拡張メソッドを呼び出す (Visual Basic)'
ms.date: 07/20/2015
helpviewer_keywords:
- calling extension methods [Visual Basic]
- extension methods [Visual Basic]
ms.assetid: df07750f-40f4-4c07-a79e-1113a27cfbea
ms.openlocfilehash: a19705a8f90833d48869df26a18d19b0ad1488e0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74340392"
---
# <a name="how-to-call-an-extension-method-visual-basic"></a>方法: 拡張メソッドを呼び出す (Visual Basic)

拡張メソッドを使用すると、既存のクラスにメソッドを追加できます。 拡張メソッドが宣言され、スコープ内に入ると、拡張された型のインスタンスメソッドのように呼び出すことができます。 拡張メソッドを記述する方法の詳細については、「[方法: 拡張メソッドを作成する](./how-to-write-an-extension-method.md)」を参照してください。

 次の手順では、拡張メソッド `PrintAndPunctuate`について説明します。拡張メソッドは、それを呼び出す文字列インスタンスを表示し、その後、2番目のパラメーターの `punc`に送信される任意の値を示します。

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub

End Module
```

メソッドが呼び出されるときは、そのメソッドがスコープ内にある必要があります。

### <a name="to-call-an-extension-method"></a>拡張メソッドを呼び出すには

1. 拡張メソッドの最初のパラメーターのデータ型を持つ変数を宣言します。 `PrintAndPunctuate`には、<xref:System.String> の変数が必要です。

    ```vb
    Dim example = "Ready"
    ```

2. その変数は拡張メソッドを呼び出し、その値は最初のパラメーター `aString`にバインドされます。 次の呼び出しステートメントでは `Ready?`が表示されます。

    ```vb
    example.PrintAndPunctuate("?")
    ```

     この拡張メソッドへの呼び出しは、1つのパラメーターを必要とする <xref:System.String> インスタンスメソッドの呼び出しと同じように見えます。

    ```vb
    example.EndsWith("dy")
    example.IndexOf("R")
    ```

3. 別の文字列変数を宣言し、メソッドを再度呼び出して、任意の文字列で動作することを確認します。

    ```vb
    Dim example2 = " or not"
    example2.PrintAndPunctuate("!!!")
    ```

     この時間は `or not!!!`になります。

## <a name="example"></a>例
 次のコードは、単純な拡張メソッドの作成と使用の完全な例です。

```vb
Imports System.Runtime.CompilerServices
Imports ConsoleApplication1.StringExtensions

Module Module1

    Sub Main()

        Dim example = "Hello"
        example.PrintAndPunctuate(".")
        example.PrintAndPunctuate("!!!!")

        Dim example2 = "Goodbye"
        example2.PrintAndPunctuate("?")
    End Sub

    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub
End Module

' Output:
' Hello.
' Hello!!!!
' Goodbye?
```

## <a name="see-also"></a>関連項目

- [方法: 拡張メソッドを作成する](./how-to-write-an-extension-method.md)
- [拡張メソッド](./extension-methods.md)
- [Visual Basic におけるスコープ](../../../../visual-basic/programming-guide/language-features/declared-elements/scope.md)
