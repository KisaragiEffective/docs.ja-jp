---
title: 既定のマーシャリングの動作
ms.date: 06/26/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interop marshaling, default
- interoperation with unmanaged code, marshaling
- marshaling behavior
ms.assetid: c0a9bcdf-3df8-4db3-b1b6-abbdb2af809a
ms.openlocfilehash: abb8b507b21ca8f40461192c37e6c2fbe73b684e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123599"
---
# <a name="default-marshaling-behavior"></a>既定のマーシャリングの動作
相互運用マーシャリングは、メソッドのパラメーターに関連付けられたデータが、マネージド メモリとアンマネージド メモリの間で渡されるときに、どのように動作するかを指示する規則に従って機能します。 これらの組み込みの規則は、データ型の変換などのマーシャリング動作、呼び出し先が渡されたデータを変更してその変更を呼び出し元にこ返すことが可能かどうか、およびどのような状況のときにマーシャラーがパフォーマンスの最適化を実現するかを制御します。  
  
 このセクションでは、相互運用マーシャリング サービスの既定の動作特性を示します。 ここでは、配列、ブール型、char 型、デリゲート、クラス、オブジェクト、文字列、および構造体のマーシャリングに関する詳細情報を示します。  
  
> [!NOTE]
> ジェネリック型のマーシャリングはサポートされていません。 詳しくは、「[ジェネリック型を使用する相互運用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100))」を参照してください。  
  
## <a name="memory-management-with-the-interop-marshaler"></a>相互運用マーシャラーによるメモリ管理  
 相互運用マーシャラーは、アンマネージ コードによって割り当てられたメモリを常に解放しようとします。 この動作は、COM メモリの管理規則に準拠していますが、ネイティブ C++ を制御する規則とは異なります。  
  
 ネイティブ C++ の動作 (メモリを解放しない) を予期している場合、ポインターのメモリを自動的に解放するプラットフォーム呼び出しを使用すると、混乱が生じることがあります。 たとえば、C++ DLL からの次のアンマネージ メソッドを呼び出しても、メモリは自動的に解放されません。  
  
### <a name="unmanaged-signature"></a>アンマネージ シグネチャ  
  
```cpp  
BSTR MethodOne (BSTR b) {  
     return b;  
}  
```  
  
 ただし、メソッドをプラットフォーム呼び出しのプロトタイプとして定義する場合は、各 **BSTR** 型を <xref:System.String> 型に置き換えて、`MethodOne` を呼び出します。共通言語ランタイムは、`b` の解放を 2 回試行します。 **String** 型ではなく <xref:System.IntPtr> 型を使用することにより、マーシャリングの動作を変更できます。  
  
 ランタイムは、常に **CoTaskMemFree** メソッドを使用してメモリを解放します。 使用しているメモリが **CoTaskMemAlloc** メソッドで割り当てられていない場合、**IntPtr** を使用し、適切なメソッドを使用して手動でメモリを解放する必要があります。 同様に、カーネル メモリへのポインターを返す **GetCommandLine** 関数を Kernel32.dll から使用するときなど、メモリを解放してはいけない状況のときには、自動的なメモリの解放を防止できます。 手動でメモリを解放する方法について詳しくは、「[Buffers サンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/x3txb6xc(v=vs.100))」を参照してください。  
  
## <a name="default-marshaling-for-classes"></a>クラスに対する既定のマーシャリング  
 クラスは、COM 相互運用でのみマーシャリングすることができ、常にインターフェイスとしてマーシャリングされます。 クラスをマーシャリングするために使用されるインターフェイスが、クラス インターフェイスと呼ばれる場合があります。 クラス インターフェイスを任意のインターフェイスでオーバーライドする方法について詳しくは、「[クラス インターフェイスの概要](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)」をご覧ください。  
  
### <a name="passing-classes-to-com"></a>クラスを COM に渡す  
 マネージド クラスが COM に渡されると、相互運用マーシャラーは自動的にクラスを COM プロキシでラップし、プロキシによって生成されたクラス インターフェイスを COM メソッド呼び出しに渡します。 その後、プロキシは、クラス インターフェイス上のすべての呼び出しを、マネージド オブジェクトにデリゲートして戻します。 プロキシはまた、クラスによって明示的に実装されていない他のインターフェイスも公開します。 プロキシは、クラスの代わりに、**IUnknown** や **IDispatch** などのインターフェイスを自動的に実装します。  
  
### <a name="passing-classes-to-net-code"></a>クラスを .NET Code に渡す  
 コクラスは、通常、COM のメソッド引数として使用されません。 代わりに、通常は既定のインターフェイスがコクラスの代わりに渡されます。  
  
 インターフェイスがマネージド コードに渡されると、相互運用マーシャラーは、インターフェイスを適切なラッパーでラップし、そのラッパーをマネージド メソッドに渡すことを担当します。 どのラッパーを使用するかは難しい判断となることがあります。 COM オブジェクトのすべてのインスタンスは、オブジェクトが実装するインターフェイスの数に関係なく、1 つの一意のラッパーを持ちます。 たとえば、5 つの異なるインターフェイスを実装する 1 つの COM オブジェクトには 1 つのラッパーだけがあります。 同一のラッパーが、5 つのすべてのインターフェイスを公開します。 COM オブジェクトの 2 つのインスタンスが作成される場合、ラッパーの 2 つのインスタンスが作成されます。  
  
 ラッパーがその有効期間中は同じ型を維持するように、相互運用マーシャラーは、オブジェクトによって公開されるインターフェイスが初めてマーシャラーを介して渡されるときに、正しいラッパーを識別する必要があります。 マーシャラーは、オブジェクトが実装するインターフェイスの 1 つに注目することにより、オブジェクトを識別します。  
  
 たとえば、マーシャラーは、マネージド コードに渡されたインターフェイスをラップするために、クラス ラッパーを使用する必要があると判別します。 インターフェイスが初めてマーシャラーを介して渡されるとき、マーシャラーは、インターフェイスが既知のオブジェクトからのものかどうかを確認します。 このチェックは、以下の 2 つの状況で発生します。  
  
- インターフェイスは、他の場所で COM に渡された別のマネージド オブジェクトによって実装されています。 マーシャラーは、マネージド オブジェクトによって公開されるインターフェイスを簡単に特定し、インターフェイスと実装を提供するマネージド オブジェクトとで突き合わせすることができます。 その後、マネージド オブジェクトはメソッドに渡され、ラッパーは必要ありません。  
  
- 既ににラップされているオブジェクトは、インターフェイスを実装しています。 このような状況かどうかを確認するために、マーシャラーは **IUnknown** インターフェイスのためにオブジェクトのクエリを実行して、返されたインターフェイスを既にラップされているその他のオブジェクトのインターフェイスと比較します。 インターフェイスが別のラッパーのものと同じ場合、それらのオブジェクトには同じ ID があり、既存のラッパーがメソッドに渡されます。  
  
 インターフェイスが既知のオブジェクトからのものではない場合、マーシャラーは以下を実行します。  
  
1. マーシャラーは **IProvideClassInfo2** インターフェイスのためにオブジェクトのクエリを実行します。 提供された場合、マーシャラーは、**IProvideClassInfo2.GetGUID** から返される CLSID を使用して、インターフェイスを提供するコクラスを識別します。 CLSID によって、マーシャラーは、アセンブリが事前に登録されている場合にレジストリからラッパーを見つけることができます。  
  
2. マーシャラーは **IProvideClassInfo** インターフェイスのためにインターフェイスのクエリを実行します。 提供された場合、マーシャラーは、**IProvideClassInfo.GetClassinfo** から返される **ITypeInfo** を使用して、インターフェイスを公開するクラスの CLSID を決定します。 マーシャラーは、CLSID を使用して、ラッパーのメタデータを見つけることができます。  
  
3. マーシャラーは、まだクラスを識別できない場合には、**System.__ComObject** と呼ばれるジェネリック ラッパー クラスを使用してインターフェイスをラップします。  
  
## <a name="default-marshaling-for-delegates"></a>デリゲートに対する既定のマーシャリング  
 マネージド デリゲートは、呼び出し元のメカニズムに基づいて、COM インターフェイスまたは関数ポインターとしてマーシャリングされます。  
  
- プラットフォーム呼び出しの場合、デリゲートは、既定でアンマネージ関数ポインターとしてマーシャリングされます。  
  
- COM 相互運用の場合、デリゲートは、既定で **_Delegate** 型の COM インターフェイスとしてマーシャリングされます。 **_Delegate** インターフェイスは Mscorlib.tlb のタイプ ライブラリで定義され、デリゲートが参照するメソッドの呼び出しを可能にする <xref:System.Delegate.DynamicInvoke%2A?displayProperty=nameWithType> メソッドが含まれています。  
  
 次の表は、マネージド デリゲート データ型のマーシャリング オプションを示しています。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は、デリゲートをマーシャリングする <xref:System.Runtime.InteropServices.UnmanagedType> 列挙値を提供します。  
  
|列挙型|アンマネージ形式の説明|  
|----------------------|-------------------------------------|  
|**UnmanagedType.FunctionPtr**|アンマネージ関数ポインター。|  
|**UnmanagedType.Interface**|Mscorlib.tlb で定義されている、 **_Delegate** 型のインターフェイス。|  
  
 `DelegateTestInterface` のメソッドが COM タイプ ライブラリにエクスポートされる、次のコード例を検討してください。 **ref** (または **ByRef**) キーワードでマークされたデリゲートだけが、In/Out パラメーターとして渡されることに注意してください。  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public interface DelegateTest {  
void m1(Delegate d);  
void m2([MarshalAs(UnmanagedType.Interface)] Delegate d);     
void m3([MarshalAs(UnmanagedType.Interface)] ref Delegate d);    
void m4([MarshalAs(UnmanagedType.FunctionPtr)] Delegate d);   
void m5([MarshalAs(UnmanagedType.FunctionPtr)] ref Delegate d);     
}  
```  
  
### <a name="type-library-representation"></a>タイプ ライブラリの表現  
  
```cpp  
importlib("mscorlib.tlb");  
interface DelegateTest : IDispatch {  
[id(…)] HRESULT m1([in] _Delegate* d);  
[id(…)] HRESULT m2([in] _Delegate* d);  
[id(…)] HRESULT m3([in, out] _Delegate** d);  
[id()] HRESULT m4([in] int d);  
[id()] HRESULT m5([in, out] int *d);  
   };  
```  
  
 他のアンマネージ関数ポインターが逆参照できるのと同様に、関数ポインターは逆参照できます。  

この例では、2 つのデリゲートが <xref:System.Runtime.InteropServices.UnmanagedType.FunctionPtr?displayProperty=nameWithType> としてマーシャリングされると、その結果は `int` および `int` へのポインターとなります。 デリゲートの型はマーシャリング中であるため、`int` はここでは、メモリ内のデリゲートのアドレスである void (`void*`) を指すポインターを表します。 つまり、この結果は 32 ビット Windows システムに固有のものとなります。`int` がここでは関数ポインターのサイズを表すからです。

> [!NOTE]
> アンマネージド コードで保持されている、マネージド デリゲートへの関数ポインターに対する参照は、共通言語ランタイムがマネージド オブジェクトでガベージ コレクションを実行することを防止しません。  
  
 たとえば、`SetChangeHandler` メソッドに渡される `cb` オブジェクトへの参照は `Test` メソッドの有効期間を超えて `cb` を有効のまま保持しないので、次のコードは正しくありません。 `cb` オブジェクトでガベージ コレクションを実行した後、`SetChangeHandler` に渡された関数ポインターは無効になります。  
  
```csharp  
public class ExternalAPI {  
   [DllImport("External.dll")]  
   public static extern void SetChangeHandler(  
      [MarshalAs(UnmanagedType.FunctionPtr)]ChangeDelegate d);  
}  
public delegate bool ChangeDelegate([MarshalAs(UnmanagedType.LPWStr) string S);  
public class CallBackClass {  
   public bool OnChange(string S){ return true;}  
}  
internal class DelegateTest {  
   public static void Test() {  
      CallBackClass cb = new CallBackClass();  
      // Caution: The following reference on the cb object does not keep the   
      // object from being garbage collected after the Main method   
      // executes.  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));     
   }  
}  
```  
  
 予期しないガベージ コレクションを補正するために、呼び出し元は、アンマネージ関数ポインターの使用中は `cb` オブジェクトが有効のまま保持されるようにする必要があります。 必要に応じて、次の例に示すように、関数ポインターが不要になった際にアンマネージド コードがマネージド コードに通知するようにすることができます。  
  
```csharp  
internal class DelegateTest {  
   CallBackClass cb;  
   // Called before ever using the callback function.  
   public static void SetChangeHandler() {  
      cb = new CallBackClass();  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));  
   }  
   // Called after using the callback function for the last time.  
   public static void RemoveChangeHandler() {  
      // The cb object can be collected now. The unmanaged code is   
      // finished with the callback function.  
      cb = null;  
   }  
}  
```  
  
## <a name="default-marshaling-for-value-types"></a>値型に対する既定のマーシャリング  
 整数や浮動小数点数など、ほとんどの値型は、[blittable](blittable-and-non-blittable-types.md) であり、マーシャリングを必要としません。 その他の [non-blittable](blittable-and-non-blittable-types.md) 型は、マネージド メモリとアンマネージド メモリに類似しない表現があり、マーシャリングが必要です。 さらに他の型は、相互運用性の境界を越えて、明示的な書式設定が必要です。  
  
 このセクションでは、次の書式設定された値に関する情報を提供します。  
  
- [プラットフォーム呼び出しで使用される値型](#value-types-used-in-platform-invoke)  
  
- [COM 相互運用で使用される値型](#value-types-used-in-com-interop)  
  
 書式指定された型について説明することに加えて、このトピックでは、マーシャリング動作が通常ではない[システム値型](#system-value-types)について示します。  
  
 書式設定された型は、メモリ内のメンバーのレイアウトを明示的に制御するための情報を含む複合型です。 メンバーのレイアウト情報は、<xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を使用して示すことができます。 レイアウトは、次のいずれかの <xref:System.Runtime.InteropServices.LayoutKind> 列挙値です。  
  
- **LayoutKind.Automatic**  
  
     共通言語ランタイムで、効率を上げるのために、型のメンバーを自由に再配置できることを示します。 ただし、値型がアンマネージ コードに渡されると、メンバーのレイアウトは予測可能です。 このような構造体を自動的にマーシャリングしようとすると、例外が発生します。  
  
- **LayoutKind.Sequential**  
  
     型のメンバーは、マネージド型定義での順序と同じ順序で、アンマネージド メモリにレイアウトされることを示します。  
  
- **LayoutKind.Explicit**  
  
     メンバーが、各フィールドに指定された <xref:System.Runtime.InteropServices.FieldOffsetAttribute> に基づいてレイアウトされることを示します。  
  
### <a name="value-types-used-in-platform-invoke"></a>プラットフォーム呼び出しで使用される値型  
 次の例では、`Point` と `Rect` 型が、**StructLayoutAttribute** を使用してメンバーのレイアウト情報を提供します。  
  
```vb  
Imports System.Runtime.InteropServices  
<StructLayout(LayoutKind.Sequential)> Public Structure Point  
   Public x As Integer  
   Public y As Integer  
End Structure  
<StructLayout(LayoutKind.Explicit)> Public Structure Rect  
   <FieldOffset(0)> Public left As Integer  
   <FieldOffset(4)> Public top As Integer  
   <FieldOffset(8)> Public right As Integer  
   <FieldOffset(12)> Public bottom As Integer  
End Structure  
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
```  
  
 アンマネージ コードにマーシャリングされるとき、これらの書式指定された型は C スタイルの構造体としてマーシャリングされます。 これは、構造体の引数を持つアンマネージ API を呼び出すための、簡単な方法を提供します。 たとえば、次のようにして、`POINT` と `RECT` 構造体を Microsoft Windows API **PtInRect** 関数に渡すことができます。  
  
```cpp  
BOOL PtInRect(const RECT *lprc, POINT pt);  
```  
  
 次のプラットフォーム呼び出し定義を使用して、構造体を渡すことができます  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function PtInRect Lib "User32.dll" (
        ByRef r As Rect, p As Point) As Boolean
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("User32.dll")]
   internal static extern bool PtInRect(ref Rect r, Point p);
}
```
  
 アンマネージ API では、`RECT` へのポインターが関数に渡されることが予期されているので、`Rect` 値型は参照によって渡す必要があります。 アンマネージ API では、`POINT` がスタックで渡されることが予期されているので、`Point` 値型は値によって渡します。 この微妙な違いは、非常に重要です。 参照は、ポインターとしてアンマネージ コードに渡されます。 値は、スタックでアンマネージ コードに渡されます。  
  
> [!NOTE]
> 書式設定された型が構造体としてマーシャリングされると、型内のフィールドだけがアクセス可能となります。 型にメソッド、プロパティ、またはイベントがある場合、それらにはアンマネージ コードからアクセスできません。  
  
 クラスも、固定のメンバー レイアウトがあれば、C スタイルの構造体としてアンマネージ コードにマーシャリングできます。 クラスのメンバー レイアウト情報も、<xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性で提供されます。 固定レイアウトの値型と固定レイアウトのクラスの主な違いは、それらがアンマネージ コードにマーシャリングされる方法です。 値型は (スタックで) 値によって渡されるので、呼び出し先が型のメンバーに変更を加えても、その変更は呼び出し元に示されません。 参照型は参照によって渡される (型への参照がスタックで渡される) ので、呼び出し先が型の blittable 型のメンバーに加えるすべての変更は呼び出し元に示されます。  
  
> [!NOTE]
> 参照型に非 blittable 型のメンバーがある場合、変換は 2 回必要となります。1 回目は引数がアンマネージ サイトに渡されるときで、2 回目は呼び出しから返されるときです。 この追加のオーバーヘッドがあるために、呼び出し先による変更を呼び出し元が参照できるようにする場合は、In/Out パラメーターを引数に明示的に適用する必要があります。  
  
 次の例では、`SystemTime` クラスにシーケンシャル メンバー レイアウトがあり、それを Windows API **GetSystemTime** 関数に渡すことができます。  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class SystemTime  
   Public wYear As System.UInt16  
   Public wMonth As System.UInt16  
   Public wDayOfWeek As System.UInt16  
   Public wDay As System.UInt16  
   Public wHour As System.UInt16  
   Public wMinute As System.UInt16  
   Public wSecond As System.UInt16  
   Public wMilliseconds As System.UInt16  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
   public class SystemTime {  
   public ushort wYear;   
   public ushort wMonth;  
   public ushort wDayOfWeek;   
   public ushort wDay;   
   public ushort wHour;   
   public ushort wMinute;   
   public ushort wSecond;   
   public ushort wMilliseconds;   
}  
```  
  
 **GetSystemTime** 関数は次のように定義されています。  
  
```cpp  
void GetSystemTime(SYSTEMTIME* SystemTime);  
```  
  
 **GetSystemTime** に対する同等のプラットフォーム呼び出し定義は、次のようになります。  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Sub GetSystemTime Lib "Kernel32.dll" (
        ByVal sysTime As SystemTime)
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("Kernel32.dll", CharSet = CharSet.Auto)]
   internal static extern void GetSystemTime(SystemTime st);
}
```
  
 `SystemTime` は値型ではなくクラスなので、`SystemTime` 引数は参照引数として型指定されていないことに注意してください。 値型とは異なり、クラスは常に参照によって渡されます。  
  
 次のコード例は、`SetXY` と呼ばれるメソッドを持つ、異なる `Point` クラスを示しています。 型にはシーケンシャル レイアウトがあるため、それをアンマネージ コードに渡して、構造体としてマーシャリングすることができます。 ただし、`SetXY` メンバーは、オブジェクトが参照によって渡される場合でも、アンマネージ コードから呼び出すことができません。  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class Point  
   Private x, y As Integer  
   Public Sub SetXY(x As Integer, y As Integer)  
      Me.x = x  
      Me.y = y  
   End Sub  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public class Point {  
   int x, y;  
   public void SetXY(int x, int y){   
      this.x = x;  
      this.y = y;  
   }  
}  
```  
  
### <a name="value-types-used-in-com-interop"></a>COM 相互運用で使用される値型  
 書式指定された型は、COM 相互運用のメソッドの呼び出しに渡すこともできます。 実際には、タイプ ライブラリにエクスポートされると、値型は構造体に自動的に変換されます。 次の例に示すように、`Point` 値型は `Point` という名前の型定義 (typedef) になります。 タイプ ライブラリ内の他の場所にある `Point` 値型へのすべての参照は、`Point` typedef に置き換えられます。  
  
 **タイプ ライブラリの表現**  
  
```cpp  
typedef struct tagPoint {  
   int x;  
   int y;  
} Point;  
interface _Graphics {  
   …  
   HRESULT SetPoint ([in] Point p)  
   HRESULT SetPointRef ([in,out] Point *p)  
   HRESULT GetPoint ([out,retval] Point *p)  
}  
```  
  
 値と参照をプラットフォーム呼び出しにマーシャリングする際に使用されるものと同じ規則が、COM インターフェイスを介してマーシャリングする際にも使用されます。 たとえば、`Point` 値型のインスタンスが .NET Framework から COM に渡されるとき、`Point` は値によって渡されます。 `Point` 値型が参照によって渡される場合、`Point` へのポインターはスタックで渡されます。 相互運用マーシャラーは、どちらの方向でも、より高いレベルの間接参照 (**Point** \*\*) はサポートしていません。  
  
> [!NOTE]
> <xref:System.Runtime.InteropServices.LayoutKind> 列挙値が **Explicit** に設定された構造体は、エクスポートされたタイプ ライブラリが明示的なレイアウトを表現できないので、COM 相互運用で使用することはできません。  
  
### <a name="system-value-types"></a>システムの値型  
 <xref:System> 名前空間には、ランタイムのプリミティブ型のボックス化された形式を表す、いくつかの値型があります。 たとえば、値型 <xref:System.Int32?displayProperty=nameWithType> 構造体は、**ELEMENT_TYPE_I4** のボックス化された形式を表します。 これらの型は、書式設定された他の型のように構造体としてマーシャリングするのではなく、それらがボックス化するプリミティブ型と同じ方法でマーシャリングします。 そのため、**System.Int32** は、**long** 型の 1 つのメンバーを含む構造体としてではなく、**ELEMENT_TYPE_I4** としてマーシャリングされます。 次の表には、プリミティブ型のボックス化された表現である、**System** 名前空間にある値型の一覧が含まれています。  
  
|システムの値型|要素型|  
|-----------------------|------------------|  
|<xref:System.Boolean?displayProperty=nameWithType>|**ELEMENT_TYPE_BOOLEAN**|  
|<xref:System.SByte?displayProperty=nameWithType>|**ELEMENT_TYPE_I1**|  
|<xref:System.Byte?displayProperty=nameWithType>|**ELEMENT_TYPE_UI1**|  
|<xref:System.Char?displayProperty=nameWithType>|**ELEMENT_TYPE_CHAR**|  
|<xref:System.Int16?displayProperty=nameWithType>|**ELEMENT_TYPE_I2**|  
|<xref:System.UInt16?displayProperty=nameWithType>|**ELEMENT_TYPE_U2**|  
|<xref:System.Int32?displayProperty=nameWithType>|**ELEMENT_TYPE_I4**|  
|<xref:System.UInt32?displayProperty=nameWithType>|**ELEMENT_TYPE_U4**|  
|<xref:System.Int64?displayProperty=nameWithType>|**ELEMENT_TYPE_I8**|  
|<xref:System.UInt64?displayProperty=nameWithType>|**ELEMENT_TYPE_U8**|  
|<xref:System.Single?displayProperty=nameWithType>|**ELEMENT_TYPE_R4**|  
|<xref:System.Double?displayProperty=nameWithType>|**ELEMENT_TYPE_R8**|  
|<xref:System.String?displayProperty=nameWithType>|**ELEMENT_TYPE_STRING**|  
|<xref:System.IntPtr?displayProperty=nameWithType>|**ELEMENT_TYPE_I**|  
|<xref:System.UIntPtr?displayProperty=nameWithType>|**ELEMENT_TYPE_U**|  
  
 **System** 名前空間にある他の値型は、処理が異なります。 アンマネージ コードではこれらの型の形式が既に確立されているため、マーシャラーには、それらをマーシャリングするための特別な規則があります。 次の表では、**System** 名前空間にある特殊な値型、およびそれらがマーシャリングされるアンマネージ型を一覧で示します。  
  
|システムの値型|IDL 型|  
|-----------------------|--------------|  
|<xref:System.DateTime?displayProperty=nameWithType>|**DATE**|  
|<xref:System.Decimal?displayProperty=nameWithType>|**DECIMAL**|  
|<xref:System.Guid?displayProperty=nameWithType>|**GUID**|  
|<xref:System.Drawing.Color?displayProperty=nameWithType>|**OLE_COLOR**|  
  
 次のコードでは、Stdole2 タイプ ライブラリにあるアンマネージ型の **DATE**、**GUID**、**DECIMAL**、および **OLE_COLOR** の定義を示します。  
  
#### <a name="type-library-representation"></a>タイプ ライブラリの表現  
  
```cpp  
typedef double DATE;  
typedef DWORD OLE_COLOR;  
  
typedef struct tagDEC {  
    USHORT    wReserved;  
    BYTE      scale;  
    BYTE      sign;  
    ULONG     Hi32;  
    ULONGLONG Lo64;  
} DECIMAL;  
  
typedef struct tagGUID {  
    DWORD Data1;  
    WORD  Data2;  
    WORD  Data3;  
    BYTE  Data4[ 8 ];  
} GUID;  
```  
  
 次のコードでは、マネージド `IValueTypes` インターフェイスでの対応する定義を示します。  
  
```vb  
Public Interface IValueTypes  
   Sub M1(d As System.DateTime)  
   Sub M2(d As System.Guid)  
   Sub M3(d As System.Decimal)  
   Sub M4(d As System.Drawing.Color)  
End Interface  
```  
  
```csharp  
public interface IValueTypes {  
   void M1(System.DateTime d);  
   void M2(System.Guid d);  
   void M3(System.Decimal d);  
   void M4(System.Drawing.Color d);  
}  
```  
  
#### <a name="type-library-representation"></a>タイプ ライブラリの表現  
  
```cpp  
[…]  
interface IValueTypes : IDispatch {  
   HRESULT M1([in] DATE d);  
   HRESULT M2([in] GUID d);  
   HRESULT M3([in] DECIMAL d);  
   HRESULT M4([in] OLE_COLOR d);  
};  
```  
  
## <a name="see-also"></a>関連項目

- [Blittable 型と非 Blittable 型](blittable-and-non-blittable-types.md)
- [コピーと固定](copying-and-pinning.md)
- [配列に対する既定のマーシャリング](default-marshaling-for-arrays.md)
- [オブジェクトに対する既定のマーシャリング](default-marshaling-for-objects.md)
- [文字列に対する既定のマーシャリング](default-marshaling-for-strings.md)
