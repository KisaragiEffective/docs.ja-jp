---
title: エントリ ポイントの指定
ms.date: 03/30/2017
helpviewer_keywords:
- EntryPoint field
- platform invoke, attribute fields
- attribute fields in platform invoke, EntryPoint
ms.assetid: d1247f08-0965-416a-b978-e0b50652dfe3
ms.openlocfilehash: a55e460f565c33731c5b0b29ab42b8263d3690e2
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125167"
---
# <a name="specifying-an-entry-point"></a>エントリ ポイントの指定

エントリ ポイントは、DLL 内の関数の位置を識別します。 マネージド プロジェクト内では、対象となる関数の元の名前または序数エントリ ポイントによって、その関数が相互運用の境界にまたがって識別されます。 また、エントリ ポイントを別の名前に割り当てて、関数の名前を事実上変更できます。  
  
 DLL 関数の名前を変更する考えられる原因の一覧を次に示します。  
  
- 大文字と小文字が区別される API 関数名を使わないようにするため  
  
- 既存の名前付け標準に合わせるため  
  
- 異なるデータ型を受け取る複数の関数を共存させるため (同じ DLL 関数の複数のバージョンを宣言することによって)  
  
- ANSI バージョンと Unicode バージョンを持つ API の使用を簡単にするため  
  
 このトピックでは、マネージド コード内の DLL 関数の名前を変更する方法について説明します。  
  
## <a name="renaming-a-function-in-visual-basic"></a>Visual Basic での関数名の変更  
 
Visual Basic で <xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint?displayProperty=nameWithType> フィールドを設定するには、**Declare** ステートメントで **Function** キーワードを使います。 基本的な宣言を次の例に示します。  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function MessageBox Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
定義に **Alias** キーワードを含めることで、**MessageBox** エントリ ポイントを **MsgBox** に置き換えることができます。その例を次に示します。 どちらの例でも、**Auto** キーワードを使って、エントリ ポイントの文字セットのバージョンを指定する手間を省いています。 文字セットの選択の詳細については、「[文字セットの指定](specifying-a-character-set.md)」を参照してください。  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function MsgBox _
        Lib "user32.dll" Alias "MessageBox" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
## <a name="renaming-a-function-in-c-and-c"></a>C# および C++ での関数名の変更  
 DLL 関数を名前または序数で指定するには、<xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint?displayProperty=nameWithType> フィールドを使います。 メソッド定義内の関数の名前が DLL 内のエントリ ポイントと同じである場合は、その関数を **EntryPoint** フィールドで明示的に識別する必要はありません。 同じでない場合は、次のいずれかの属性書式を使って、名前または序数を指示します。  
  
```csharp
[DllImport("DllName", EntryPoint = "Functionname")]
[DllImport("DllName", EntryPoint = "#123")]
```
  
 序数の前にシャープ記号 (#) を付ける必要があることに注意してください。  
  
 **EntryPoint** フィールドを使ってコード内の **MessageBoxA** を **MsgBox** に置き換える方法を次の例に示します。  
  
```csharp
using System;
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll", EntryPoint = "MessageBoxA")]
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```
  
```cpp
using namespace System;
using namespace System::Runtime::InteropServices;

typedef void* HWND;
[DllImport("user32", EntryPoint = "MessageBoxA")]
extern "C" int MsgBox(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);
```
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- [マネージド コードでのプロトタイプの作成](creating-prototypes-in-managed-code.md)
- [プラットフォーム呼び出しの例](platform-invoke-examples.md)
- [プラットフォーム呼び出しによるデータのマーシャリング](marshaling-data-with-platform-invoke.md)
