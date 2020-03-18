---
title: 文字列に対する既定のマーシャリング
ms.date: 03/20/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings, interop marshaling
- interop marshaling, strings
ms.assetid: 9baea3ce-27b3-4b4f-af98-9ad0f9467e6f
ms.openlocfilehash: 49f2d871a42db484e20f0bfc35634a0e8b959c2e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123543"
---
# <a name="default-marshaling-for-strings"></a>文字列に対する既定のマーシャリング

<xref:System.String?displayProperty=nameWithType> と <xref:System.Text.StringBuilder?displayProperty=nameWithType> クラスのマーシャリング動作は類似しています。

文字列は、COM スタイル `BSTR` 型または null で終わる文字列 (null 文字で終わる文字配列) としてマーシャリングされます。 文字列内の文字は、Unicode (Windows システムでの既定値) または ANSI としてマーシャリングすることができます。

## <a name="strings-used-in-interfaces"></a>インターフェイスで使用される文字列

次の表は、アンマネージ コードへのメソッド引数としてマーシャリングするときの、文字列データ型のマーシャリングのオプションを示しています。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は、COM インターフェイスへの文字列をマーシャリングする <xref:System.Runtime.InteropServices.UnmanagedType> 列挙値を提供します。

|列挙型|アンマネージ形式の説明|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr` (既定値)|長さと Unicode 文字がプレフィックスされた COM スタイル `BSTR`。|
|`UnmanagedType.LPStr`|ANSI 文字の null で終わる配列へのポインター。|
|`UnmanagedType.LPWStr`|Unicode 文字の null で終わる配列へのポインター。|

この表は <xref:System.String> に適用されます。 <xref:System.Text.StringBuilder> の場合、許可される唯一のオプションは `UnmanagedType.LPStr` と `UnmanagedType.LPWStr` です。

以下の例では、`IStringWorker` インターフェイスで宣言された文字列を示します。

```csharp
public interface IStringWorker
{
    void PassString1(string s);
    void PassString2([MarshalAs(UnmanagedType.BStr)] string s);
    void PassString3([MarshalAs(UnmanagedType.LPStr)] string s);
    void PassString4([MarshalAs(UnmanagedType.LPWStr)] string s);
    void PassStringRef1(ref string s);
    void PassStringRef2([MarshalAs(UnmanagedType.BStr)] ref string s);
    void PassStringRef3([MarshalAs(UnmanagedType.LPStr)] ref string s);
    void PassStringRef4([MarshalAs(UnmanagedType.LPWStr)] ref string s);
}
```

```vb
Public Interface IStringWorker
    Sub PassString1(s As String)
    Sub PassString2(<MarshalAs(UnmanagedType.BStr)> s As String)
    Sub PassString3(<MarshalAs(UnmanagedType.LPStr)> s As String)
    Sub PassString4(<MarshalAs(UnmanagedType.LPWStr)> s As String)
    Sub PassStringRef1(ByRef s As String)
    Sub PassStringRef2(<MarshalAs(UnmanagedType.BStr)> ByRef s As String)
    Sub PassStringRef3(<MarshalAs(UnmanagedType.LPStr)> ByRef s As String)
    Sub PassStringRef4(<MarshalAs(UnmanagedType.LPWStr)> ByRef s As String)
End Interface
```

以下の例では、タイプ ライブラリに記述された対応するインターフェイスを示します。

```cpp
interface IStringWorker : IDispatch
{
    HRESULT PassString1([in] BSTR s);
    HRESULT PassString2([in] BSTR s);
    HRESULT PassString3([in] LPStr s);
    HRESULT PassString4([in] LPWStr s);
    HRESULT PassStringRef1([in, out] BSTR *s);
    HRESULT PassStringRef2([in, out] BSTR *s);
    HRESULT PassStringRef3([in, out] LPStr *s);
    HRESULT PassStringRef4([in, out] LPWStr *s);
};
```

## <a name="strings-used-in-platform-invoke"></a>プラットフォーム呼び出しで使用される文字列

プラットフォーム呼び出しは、文字列の引数を、.NET Framework 形式 (Unicode) から、プラットフォーム アンマネージ形式に変換してコピーします。 文字列は不変であり、呼び出しが戻るときに、アンマネージド メモリから元のマネージド メモリにコピーされることはありません。

次の表は、文字列をプラットフォーム呼び出しのメソッド引数としてマーシャリングする際のマーシャリング オプションをリストしています。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は、文字列をマーシャリングする <xref:System.Runtime.InteropServices.UnmanagedType> 列挙値を提供します。

|列挙型|アンマネージ形式の説明|
|----------------------|-------------------------------------|
|`UnmanagedType.AnsiBStr`|長さと ANSI 文字がプレフィックスされた COM スタイル `BSTR`。|
|`UnmanagedType.BStr`|長さと Unicode 文字がプレフィックスされた COM スタイル `BSTR`。|
|`UnmanagedType.LPStr` (既定値)|ANSI 文字の null で終わる配列へのポインター。|
|`UnmanagedType.LPTStr`|プラットフォーム依存文字の null で終わる配列へのポインター。|
|`UnmanagedType.LPUTF8Str`|UTF-8 でエンコードされた文字の NULL で終わる配列へのポインター。|
|`UnmanagedType.LPWStr`|Unicode 文字の null で終わる配列へのポインター。|
|`UnmanagedType.TBStr`|長さとプラットフォーム依存文字がプレフィックスされた COM スタイル `BSTR`。|
|`VBByRefStr`|Visual Basic .NET で、アンマネージド コードの文字列を変更し、結果をマネージド コードに反映できるようにする値。 この値は、プラットフォーム呼び出しでだけサポートされます。 これは、`ByVal` 文字列に対する Visual Basic の既定値です。|

この表は <xref:System.String> に適用されます。 <xref:System.Text.StringBuilder> の場合、許可される唯一のオプションは `LPStr`、`LPTStr`、および `LPWStr` です。

次の型定義は、プラットフォーム呼び出しで `MarshalAsAttribute` を使用するための正しい方法を示しています。

```csharp
class StringLibAPI
{
    [DllImport("StringLib.dll")]
    public static extern void PassLPStr([MarshalAs(UnmanagedType.LPStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPWStr([MarshalAs(UnmanagedType.LPWStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPTStr([MarshalAs(UnmanagedType.LPTStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPUTF8Str([MarshalAs(UnmanagedType.LPUTF8Str)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassBStr([MarshalAs(UnmanagedType.BStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassAnsiBStr([MarshalAs(UnmanagedType.AnsiBStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassTBStr([MarshalAs(UnmanagedType.TBStr)] string s);
}
```

```vb
Class StringLibAPI
    Public Declare Auto Sub PassLPStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPStr)> s As String)
    Public Declare Auto Sub PassLPWStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPWStr)> s As String)
    Public Declare Auto Sub PassLPTStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPTStr)> s As String)
    Public Declare Auto Sub PassLPUTF8Str Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPUTF8Str)> s As String)
    Public Declare Auto Sub PassBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.BStr)> s As String)
    Public Declare Auto Sub PassAnsiBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.AnsiBStr)> s As String)
    Public Declare Auto Sub PassTBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.TBStr)> s As String)
End Class
```

## <a name="strings-used-in-structures"></a>構造体で使用される文字列

文字列は構造体の有効なメンバーです。ただし、<xref:System.Text.StringBuilder> バッファーは構造体では無効です。 次の表は、型をフィールドとしてマーシャリングするときの、<xref:System.String> データ型のマーシャリングのオプションを示しています。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は、文字列をフィールドにマーシャリングする <xref:System.Runtime.InteropServices.UnmanagedType> 列挙値を提供します。

|列挙型|アンマネージ形式の説明|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr`|長さと Unicode 文字がプレフィックスされた COM スタイル `BSTR`。|
|`UnmanagedType.LPStr` (既定値)|ANSI 文字の null で終わる配列へのポインター。|
|`UnmanagedType.LPTStr`|プラットフォーム依存文字の null で終わる配列へのポインター。|
|`UnmanagedType.LPUTF8Str`|UTF-8 でエンコードされた文字の NULL で終わる配列へのポインター。|
|`UnmanagedType.LPWStr`|Unicode 文字の null で終わる配列へのポインター。|
|`UnmanagedType.ByValTStr`|固定長の文字配列。配列の型は、包含構造体の文字セットによって決まります。|

`ByValTStr` 型は、構造体に定義されているインライン固定長文字配列で使用します。 その他の型は、文字列へのポインターを含む構造体に含まれている文字列参照に適用されます。

包含構造体に適用される <xref:System.Runtime.InteropServices.StructLayoutAttribute> の `CharSet` 引数によって、構造体内の文字列の文字形式が決まります。 以下の構造体の例には、文字列参照とインライン文字列、そして ANSI、Unicode、およびプラットフォーム依存文字が含まれています。 タイプ ライブラリ内のこれらの構造体の表現を次の C++ コードに示します。

```cpp
struct StringInfoA
{
    char *  f1;
    char    f2[256];
};

struct StringInfoW
{
    WCHAR * f1;
    WCHAR   f2[256];
    BSTR    f3;
};

struct StringInfoT
{
    TCHAR * f1;
    TCHAR   f2[256];
};
```

次の例は、<xref:System.Runtime.InteropServices.MarshalAsAttribute> を使用して、同一の構造体を複数の異なる形式で定義する方法を示しています。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
struct StringInfoA
{
    [MarshalAs(UnmanagedType.LPStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
struct StringInfoW
{
    [MarshalAs(UnmanagedType.LPWStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
    [MarshalAs(UnmanagedType.BStr)] public string f3;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
struct StringInfoT
{
    [MarshalAs(UnmanagedType.LPTStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}
```

```vb
<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Ansi)> _
Structure StringInfoA
    <MarshalAs(UnmanagedType.LPStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Unicode)> _
Structure StringInfoW
    <MarshalAs(UnmanagedType.LPWStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
<MarshalAs(UnmanagedType.BStr)> Public f3 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Auto)> _
Structure StringInfoT
    <MarshalAs(UnmanagedType.LPTStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure
```

## <a name="fixed-length-string-buffers"></a>固定長文字列バッファー

状況によっては、固定長の文字列バッファーを、操作するためにアンマネージ コードに渡す必要があります。 この場合、呼び出し先は渡されたバッファーの内容を修正できないので、単に文字列を渡すだけでは不十分です。 文字列が参照によって渡された場合でも、バッファーを特定のサイズに初期化する方法はありません。

この解決策は、<xref:System.Text.StringBuilder> バッファーを <xref:System.String> ではなく引数として渡すことです。 呼び出し先は、`StringBuilder` の容量を超えない範囲で、`StringBuilder` を逆参照したり変更したりすることができます。 また、固定長に初期化することもできます。 たとえば、`StringBuilder` バッファーを初期化してその容量を `N` にする場合、マーシャラーは (`N`+1) 文字のサイズのバッファーを提供します。 +1 は、アンマネージ文字列に null 終了文字があることをしめしています。`StringBuilder` にはそれがありません。

たとえば、Windows [`GetWindowText`](/windows/desktop/api/winuser/nf-winuser-getwindowtextw) API 関数 (*winuser.h* で定義されています) では、関数からウィンドウのテキストが出力される固定長の文字バッファーを呼び出し元が渡す必要があります。 `LpString` は、呼び出し元が割り当てたサイズ `nMaxCount` のバッファーを示します。 呼び出し元は、バッファーを割り当てて、`nMaxCount` 引数を割り当てられたバッファーのサイズに設定することが期待されています。 次の例は、*winuser.h* で定義されている `GetWindowText` 関数宣言を示しています。

```cpp
int GetWindowText(
    HWND hWnd,        // Handle to window or control.
    LPTStr lpString,  // Text buffer.
    int nMaxCount     // Maximum number of characters to copy.
);
```

呼び出し先は、`StringBuilder` の容量を超えない範囲で、`StringBuilder` を逆参照したり変更したりすることができます。 次のコード例は、`StringBuilder` を固定長に初期化する方法を示しています。

```csharp
using System;
using System.Runtime.InteropServices;
using System.Text;

internal static class NativeMethods
{
    [DllImport("User32.dll")]
    internal static extern void GetWindowText(IntPtr hWnd, StringBuilder lpString, int nMaxCount);
}

public class Window
{
    internal IntPtr h;        // Internal handle to Window.
    public String GetText()
    {
        StringBuilder sb = new StringBuilder(256);
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1);
        return sb.ToString();
    }
}
```

```vb
Imports System.Text

Friend Class NativeMethods
    Friend Declare Auto Sub GetWindowText Lib "User32.dll" _
        (hWnd As IntPtr, lpString As StringBuilder, nMaxCount As Integer)
End Class

Public Class Window
    Friend h As IntPtr ' Friend handle to Window.
    Public Function GetText() As String
        Dim sb As New StringBuilder(256)
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1)
        Return sb.ToString()
   End Function
End Class
```

## <a name="see-also"></a>関連項目

- [既定のマーシャリング動作](default-marshaling-behavior.md)
- [マーシャリング (文字列の)](marshaling-strings.md)
- [Blittable 型と非 Blittable 型](blittable-and-non-blittable-types.md)
- [方向属性](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [コピーと固定](copying-and-pinning.md)
