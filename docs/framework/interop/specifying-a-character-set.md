---
title: 文字セットの指定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- platform invoke, attribute fields
- attribute fields in platform invoke, CharSet
- CharSet field
ms.assetid: a8347eb1-295f-46b9-8a78-63331f9ecc50
ms.openlocfilehash: 0db1cd8d75b45f6d718168793c873e5867028269
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125174"
---
# <a name="specifying-a-character-set"></a>文字セットの指定
<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> フィールドは文字列のマーシャリングを制御し、DLL の関数名をプラットフォーム呼び出しが見つけるしくみを決定します。 このトピックでは、両方の動作について説明します。  
  
 一部の API は、文字列引数、ナロー (ANSI) とワイド (Unicode) を受け取る 2 種類の関数をエクスポートします。 たとえば、Windows API には、**MessageBox** 関数の次のエントリ ポイント名が含まれています。  
  
- **MessageBoxA**  
  
     1 バイト文字の ANSI 書式設定を提供します。エントリ ポイント名に "A" が追加されます。 **MessageBoxA** を呼び出すと、常に ANSI 形式で文字列がマーシャリングされます。  
  
- **MessageBoxW**  
  
     2 バイト文字の Unicode 書式設定を提供します。エントリ ポイント名に "W" が追加されます。 **MessageBoxW** を呼び出すと、常に Unicode 形式で文字列がマーシャリングされます。  
  
## <a name="string-marshaling-and-name-matching"></a>文字列のマーシャリングと名前の一致  
 `CharSet` フィールドは次の値を受け取ります。  
  
 <xref:System.Runtime.InteropServices.CharSet.Ansi> (既定値)  
  
- 文字列のマーシャリング  
  
     プラットフォーム呼び出しは、その管理対象形式 (Unicode) から ANSI 形式に文字列をマーシャリングします。  
  
- 名前の一致  
  
     <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling?displayProperty=nameWithType> フィールドが `true` の場合 (Visual Basic ではこれが既定値)、プラットフォーム呼び出しによって指定した名前のみが検索されます。 たとえば、**MessageBox** を指定した場合、プラットフォーム呼び出しは **MessageBox** を検索し、厳密に一致する綴りが見つからない場合、検索失敗となります。  
  
     `ExactSpelling` フィールドが `false` のとき (C++ と C# で既定)、プラットフォーム呼び出しは最初に修飾なしのエイリアスを探し (**MessageBox**)、見つからなければ、修飾ありの名前を探します (**MessageBoxA**)。 ANSI の名前一致動作と Unicode の名前一致動作は異なることにご注意ください。  
  
 <xref:System.Runtime.InteropServices.CharSet.Unicode>  
  
- 文字列のマーシャリング  
  
     プラットフォーム呼び出しは、その管理対象形式 (Unicode) から Unicode 形式に文字列をコピーします。  
  
- 名前の一致  
  
     `ExactSpelling` フィールドが `true` の場合 (Visual Basic ではこれが既定値)、プラットフォーム呼び出しによって指定した名前のみが検索されます。 たとえば、**MessageBox** を指定した場合、プラットフォーム呼び出しは **MessageBox** を検索し、厳密に一致する綴りが見つからない場合、検索失敗となります。  
  
     `ExactSpelling` フィールドが `false` のとき (C++ と C# で既定)、プラットフォーム呼び出しは最初に修飾ありの名前を探し (**MessageBoxW**)、見つからなければ、修飾なしのエイリアスを探します (**MessageBox**)。 Unicode の名前一致動作と ANSI の名前一致動作は異なることにご注意ください。  
  
 <xref:System.Runtime.InteropServices.CharSet.Auto>  
  
- プラットフォーム呼び出しは、対象プラットフォームに基づき、ANSI 形式または Unicode 形式を選択します。  
  
## <a name="specifying-a-character-set-in-visual-basic"></a>Visual Basic で文字セットを指定する  
 次の例では **MessageBox** 関数を 3 回宣言しています。宣言のたびに文字セット動作が変わっています。 Visual Basic では、文字セット動作を指定できます。宣言ステートメントにキーワードとして **Ansi**、**Unicode**、**Auto** を追加します。  
  
 最初の宣言ステートメントのように、文字セット キーワードを省略した場合、<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> フィールドは既定で ANSI 文字セットに設定されます。 例の 2 番目と 3 番目のステートメントは、キーワードで文字セットを明示的に指定しています。  
  
```vb
Friend Class NativeMethods
    Friend Declare Function MessageBoxA Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer

    Friend Declare Unicode Function MessageBoxW Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer

    Friend Declare Auto Function MessageBox Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
## <a name="specifying-a-character-set-in-c-and-c"></a>C# と C++ で文字セットを指定する  
 <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> フィールドは、基礎となる文字セットとして ANSI または Unicode を識別します。 この文字セットは、メソッドの文字列引数をマーシャリングする方法を制御します。 次の形式の 1 つを使用し、文字セットを指示します。  
  
```csharp
[DllImport("DllName", CharSet = CharSet.Ansi)]
[DllImport("DllName", CharSet = CharSet.Unicode)]
[DllImport("DllName", CharSet = CharSet.Auto)]
```
  
```cpp
[DllImport("DllName", CharSet = CharSet::Ansi)]
[DllImport("DllName", CharSet = CharSet::Unicode)]
[DllImport("DllName", CharSet = CharSet::Auto)]
```
  
 次の例では、**MessageBox** 関数の 3 つの管理対象定義を確認できます。これにより文字セットが指定されます。 最初の定義で、その省略により、`CharSet` フィールドは ANSI 文字セットに初期設定されます。  
  
```csharp  
using System;
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll")]
    internal static extern int MessageBoxA(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);

    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    internal static extern int MessageBoxW(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);

    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```  
  
```cpp
typedef void* HWND;

// Can use MessageBox or MessageBoxA.
[DllImport("user32")]
extern "C" int MessageBox(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);

// Can use MessageBox or MessageBoxW.
[DllImport("user32", CharSet = CharSet::Unicode)]
extern "C" int MessageBoxW(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);

// Must use MessageBox.
[DllImport("user32", CharSet = CharSet::Auto)]
extern "C" int MessageBox(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);
```
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- [マネージド コードでのプロトタイプの作成](creating-prototypes-in-managed-code.md)
- [プラットフォーム呼び出しの例](platform-invoke-examples.md)
- [プラットフォーム呼び出しによるデータのマーシャリング](marshaling-data-with-platform-invoke.md)
