---
title: '方法 : 符号なしの型を使用する Windows の機能を呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- Windows functions [Visual Basic], calling
- unsigned data types [Visual Basic]
- UShort data type [Visual Basic], using
- functions [Visual Basic], calling Windows functions
- ULong data type [Visual Basic], using
- UInteger data type [Visual Basic], using
- data types [Visual Basic], using
- unsigned types [Visual Basic]
- data types [Visual Basic], unsigned
- data types [Visual Basic], numeric
- unsigned types [Visual Basic], using
ms.assetid: c2c0e712-8dc2-43b9-b4c6-345fbb02e7ce
ms.openlocfilehash: 790c680744e2100a40a7cea8b8cef80c68d586bb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348732"
---
# <a name="how-to-call-a-windows-function-that-takes-unsigned-types-visual-basic"></a>方法: 符号なしの型を使用する Windows の機能を呼び出す (Visual Basic)

符号なし整数型のメンバーを持つクラス、モジュール、または構造体を使用している場合、これらのメンバーには Visual Basic でアクセスできます。

## <a name="to-call-a-windows-function-that-takes-an-unsigned-type"></a>符号なしの型を受け取る Windows 関数を呼び出すには

1. [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)を使用して、どのライブラリに関数が格納されているか、そのライブラリ内でその名前がどのようなものであるか、呼び出し元のシーケンスについて、および文字列を呼び出すときに文字列を変換する方法を Visual Basic 通知します。

2. `Declare` ステートメントでは、符号なしの型を持つ各パラメーターに対して、必要に応じて `UInteger`、`ULong`、`UShort`、または `Byte` を使用します。

3. 使用している定数の名前と値を調べるには、呼び出し元の Windows 関数のドキュメントを参照してください。 これらの多くは、WinUser.h ファイルで定義されています。

4. コードで必要な定数を宣言します。 多くの Windows 定数は、32ビットの符号なしの値であるため、これらの `As UInteger`を宣言する必要があります。

5. 通常の方法で関数を呼び出します。 次の例では、Windows 関数 `MessageBox`を呼び出します。この関数は、符号なし整数引数を受け取ります。

    ```vb
    Public Class windowsMessage
        Private Declare Auto Function mb Lib "user32.dll" Alias "MessageBox" (
            ByVal hWnd As Integer,
            ByVal lpText As String,
            ByVal lpCaption As String,
            ByVal uType As UInteger) As Integer
        Private Const MB_OK As UInteger = 0
        Private Const MB_ICONEXCLAMATION As UInteger = &H30
        Private Const IDOK As UInteger = 1
        Private Const IDCLOSE As UInteger = 8
        Private Const c As UInteger = MB_OK Or MB_ICONEXCLAMATION
        Public Function messageThroughWindows() As String
            Dim r As Integer = mb(0, "Click OK if you see this!",
                "Windows API call", c)
            Dim s As String = "Windows API MessageBox returned " &
                 CStr(r)& vbCrLf & "(IDOK = " & CStr(IDOK) &
                 ", IDCLOSE = " & CStr(IDCLOSE) & ")"
            Return s
        End Function
    End Class
    ```

     関数 `messageThroughWindows` をテストするには、次のコードを使用します。

    ```vb
    Public Sub consumeWindowsMessage()
        Dim w As New windowsMessage
        w.messageThroughWindows()
    End Sub
    ```

    > [!CAUTION]
    > `UInteger`、`ULong`、`UShort`、および `SByte` の各データ型は、言語に[依存](../../../standard/language-independence-and-language-independent-components.md)しないコンポーネント (cls) の一部ではないため、cls 準拠のコードはそれらを使用するコンポーネントを使用できません。

    > [!IMPORTANT]
    > Windows アプリケーション プログラミング インターフェイス (API) などのアンマネージ コードを呼び出すと、コードは潜在的なセキュリティリスクにさらされます。

    > [!IMPORTANT]
    > Windows API を呼び出すには、アンマネージコードのアクセス許可が必要です。これは、部分信頼状況での実行に影響を与える可能性があります。 詳細については、「<xref:System.Security.Permissions.SecurityPermission>」および「[コードアクセス許可](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h846e9b3(v=vs.100))」を参照してください。

## <a name="see-also"></a>参照

- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [Integer データ型](../../../visual-basic/language-reference/data-types/integer-data-type.md)
- [UInteger データ型](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)
- [Declare Statement](../../../visual-basic/language-reference/statements/declare-statement.md)
- [チュートリアル : Windows API の呼び出し](../../../visual-basic/programming-guide/com-interop/walkthrough-calling-windows-apis.md)
