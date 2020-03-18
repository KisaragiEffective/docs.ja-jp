---
title: 構造体の受け渡し
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- platform invoke, calling unmanaged functions
ms.assetid: 9b92ac73-32b7-4e1b-862e-6d8d950cf169
ms.openlocfilehash: 8fde48f0697d986c5fc7f6d7059b6b45a6af1488
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124976"
---
# <a name="passing-structures"></a>構造体の受け渡し
多くのアンマネージド 関数では、構造体のメンバー (Visual Basic ではユーザー定義型) またはマネージド コードで定義されたクラスのメンバーがパラメーターとして渡されることを期待しています。 プラットフォーム呼び出しを使って構造体またはクラスをアンマネージ コードに渡す場合は、元のレイアウトやアラインメントを保持するための追加情報を提供する必要があります。 このトピックでは、フォーマットされた型を定義するために使用する <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性について説明します。 マネージ構造体やマネージド クラスの場合は、**LayoutKind** 列挙型によって提供される想定されたレイアウト動作から選択できます。  
  
 ここで説明する概念の中心は、構造体の型とクラスの型の相違点です。 構造体は値型であり、クラスは参照型です。クラスは常に最低 1 つのレベルのメモリの間接参照 (値へのポインター) を提供します。 アンマネージ関数は、次の表の第 1 列のシグネチャに示されているように、間接参照を必要とすることが多いため、この違いは重要です。 他の列のマネージ構造体およびマネージド クラスの宣言は、宣言でどの程度間接参照のレベルを調整できるかを示しています。宣言は、Visual Basic と Visual C# の両方で指定されています。  
  
|アンマネージ シグネチャ|マネージド宣言 <br />間接参照なし<br />`Structure MyType`<br />`struct MyType;`|マネージド宣言 <br />1 レベルの間接参照<br />`Class MyType`<br />`class MyType;`|  
|-------------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|  
|`DoWork(MyType x);`<br /><br /> 0 レベルの間接参照を必要とします。|`DoWork(ByVal x As MyType)` <br /> `DoWork(MyType x)`<br /><br /> 0 レベルの間接参照を追加します。|既に 1 レベルの間接参照が存在するため調整できません。|  
|`DoWork(MyType* x);`<br /><br /> 1 レベルの間接参照を必要とします。|`DoWork(ByRef x As MyType)` <br /> `DoWork(ref MyType x)`<br /><br /> 1 レベルの間接参照を追加します。|`DoWork(ByVal x As MyType)` <br /> `DoWork(MyType x)`<br /><br /> 0 レベルの間接参照を追加します。|  
|`DoWork(MyType** x);`<br /><br /> 2 レベルの間接参照を必要とします。|**ByRef** **ByRef** または `ref` `ref` を使用できないため調整できません。|`DoWork(ByRef x As MyType)` <br /> `DoWork(ref MyType x)`<br /><br /> 1 レベルの間接参照を追加します。|  
  
 この表は、プラットフォーム呼び出しの宣言について次のようなガイドラインを示しています。  
  
- アンマネージ関数が間接参照を必要としない場合は、値渡しによる構造体を使用します。  
  
- アンマネージ関数が 1 レベルの間接参照を必要とする場合は、参照渡しによる構造体または値渡しによるクラスのいずれかを使用します。  
  
- アンマネージ関数が 2 レベルの間接参照を必要とする場合は、参照渡しによるクラスを使用します。  
  
## <a name="declaring-and-passing-structures"></a>構造体の宣言と受け渡し  
 マネージド コードで `Point` 型および `Rect` 型を定義し、これらの型を User32.dll ファイル内の **PtInRect** 関数にパラメーターとして渡す方法の例を次に示します。 **PtInRect** は、次に示すアンマネージ シグネチャを持っています。  
  
```cpp
BOOL PtInRect(const RECT *lprc, POINT pt);  
```  
  
 この関数は RECT 型へのポインターを期待しているため、Rect 構造体は参照渡しする必要があることに注意してください。  
  
```vb  
Imports System.Runtime.InteropServices  
  
<StructLayout(LayoutKind.Sequential)> Public Structure Point  
    Public x As Integer  
    Public y As Integer  
End Structure  
  
Public Structure <StructLayout(LayoutKind.Explicit)> Rect  
    <FieldOffset(0)> Public left As Integer  
    <FieldOffset(4)> Public top As Integer  
    <FieldOffset(8)> Public right As Integer  
    <FieldOffset(12)> Public bottom As Integer  
End Structure  
  
Friend Class NativeMethods      
    Friend Declare Auto Function PtInRect Lib "user32.dll" (
        ByRef r As Rect, p As Point) As Boolean  
End Class  
```  
  
```csharp  
using System.Runtime.InteropServices;  
  
[StructLayout(LayoutKind.Sequential)]  
public struct Point {  
    public int x;  
    public int y;  
}     
  
[StructLayout(LayoutKind.Explicit)]  
public struct Rect {  
    [FieldOffset(0)] public int left;  
    [FieldOffset(4)] public int top;  
    [FieldOffset(8)] public int right;  
    [FieldOffset(12)] public int bottom;  
}     
  
internal static class NativeMethods
{  
    [DllImport("User32.dll")]  
    internal static extern bool PtInRect(ref Rect r, Point p);  
}  
```  
  
## <a name="declaring-and-passing-classes"></a>クラスの宣言と受け渡し  
 クラスが固定したメンバー レイアウトを持っている場合に限り、クラスのメンバーをアンマネージ DLL 関数に渡すことができます。 定義の順序で固定されている `MySystemTime` クラスのメンバーを User32.dll ファイル内の **GetSystemTime** に渡す方法の例を次に示します。 **GetSystemTime** は、次に示すアンマネージ シグネチャを持っています。  
  
```cpp
void GetSystemTime(SYSTEMTIME* SystemTime);  
```  
  
 値型とは異なり、クラスは常に 1 レベルの間接参照を使用します。  
  
```vb  
Imports System.Runtime.InteropServices  
  
<StructLayout(LayoutKind.Sequential)> Public Class MySystemTime  
    Public wYear As Short  
    Public wMonth As Short  
    Public wDayOfWeek As Short   
    Public wDay As Short  
    Public wHour As Short  
    Public wMinute As Short  
    Public wSecond As Short  
    Public wMiliseconds As Short  
End Class  
  
Friend Class NativeMethods  
    Friend Declare Auto Sub GetSystemTime Lib "Kernel32.dll" (
        sysTime As MySystemTime)  
    Friend Declare Auto Function MessageBox Lib "User32.dll" (
        hWnd As IntPtr, lpText As String, lpCaption As String, uType As UInteger) As Integer  
End Class  
  
Public Class TestPlatformInvoke      
    Public Shared Sub Main()  
        Dim sysTime As New MySystemTime()  
        NativeMethods.GetSystemTime(sysTime)  
  
        Dim dt As String  
        dt = "System time is:" & ControlChars.CrLf & _  
              "Year: " & sysTime.wYear & _  
              ControlChars.CrLf & "Month: " & sysTime.wMonth & _  
              ControlChars.CrLf & "DayOfWeek: " & sysTime.wDayOfWeek & _  
              ControlChars.CrLf & "Day: " & sysTime.wDay  
        NativeMethods.MessageBox(IntPtr.Zero, dt, "Platform Invoke Sample", 0)        
    End Sub  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public class MySystemTime {  
    public ushort wYear;   
    public ushort wMonth;  
    public ushort wDayOfWeek;   
    public ushort wDay;   
    public ushort wHour;   
    public ushort wMinute;   
    public ushort wSecond;   
    public ushort wMilliseconds;   
}  
internal static class NativeMethods
{  
    [DllImport("Kernel32.dll")]  
    internal static extern void GetSystemTime(MySystemTime st);  
  
    [DllImport("user32.dll", CharSet = CharSet.Auto)]  
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);  
}  
  
public class TestPlatformInvoke  
{  
    public static void Main()  
    {  
        MySystemTime sysTime = new MySystemTime();  
        NativeMethods.GetSystemTime(sysTime);  
  
        string dt;  
        dt = "System time is: \n" +  
              "Year: " + sysTime.wYear + "\n" +  
              "Month: " + sysTime.wMonth + "\n" +  
              "DayOfWeek: " + sysTime.wDayOfWeek + "\n" +  
              "Day: " + sysTime.wDay;  
        NativeMethods.MessageBox(IntPtr.Zero, dt, "Platform Invoke Sample", 0);  
    }  
}  
```  
  
## <a name="see-also"></a>関連項目

- [DLL 関数の呼び出し](calling-a-dll-function.md)
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- <xref:System.Runtime.InteropServices.FieldOffsetAttribute>
