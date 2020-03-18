---
title: マネージド コードでのプロトタイプの作成
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- prototypes in managed code
- COM interop, DLL functions
- unmanaged functions
- platform invoke, creating prototypes
- COM interop, platform invoke
- interoperation with unmanaged code, DLL functions
- interoperation with unmanaged code, platform invoke
- platform invoke, object fields
- DLL functions
- object fields in platform invoke
ms.assetid: ecdcf25d-cae3-4f07-a2b6-8397ac6dc42d
ms.openlocfilehash: 712040c3482b51c4dafe0ee87fdda8cd848fb7fc
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123613"
---
# <a name="creating-prototypes-in-managed-code"></a>マネージド コードでのプロトタイプの作成
このトピックは、アンマネージド 関数にアクセスする方法について説明し、マネージド コードでメソッドの定義の注釈を設定するいくつかの属性フィールドを紹介しています。 プラットフォームの起動で使用する NET ベースの宣言を作成する方法を示す例については、「[プラットフォーム呼び出しによるデータのマーシャリング](marshaling-data-with-platform-invoke.md)」を参照してください。  
  
 マネージド コードからアンマネージド DLL 関数にアクセスする前に、関数の名前とエクスポートする DLL の名前を知っている必要があります。 この情報を使用すると、マネージド DLL に実装されているアンマネージド 関数の定義の作成を開始できます。 さらに、プラットフォーム呼び出しが関数を作成し、関数間でデータをマーシャリングする方法を調整できます。  
  
> [!NOTE]
> 文字列を割り当てる Windows API 関数を使用して、`LocalFree` などのメソッドを使用して文字列を解放できます。 プラットフォーム呼び出しは、このようなパラメーターを異なる方法で処理します。 プラットフォーム呼び出しでは、パラメーターを `String` 型の代わりに `IntPtr` 型にします。 <xref:System.Runtime.InteropServices.Marshal?displayProperty=nameWithType> クラスにより提供されるメソッドを使用して、型を手動で文字列に変換し、手動で解放します。  
  
## <a name="declaration-basics"></a>宣言の基本  
 アンマネージド 関数に対するマネージド定義は、次の例で確認できるように、言語に依存します。 完全なコード例については、「[プラットフォーム呼び出しの例](platform-invoke-examples.md)」を参照してください。  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function MessageBox Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
 <xref:System.Runtime.InteropServices.DllImportAttribute.BestFitMapping?displayProperty=nameWithType>、<xref:System.Runtime.InteropServices.DllImportAttribute.CallingConvention?displayProperty=nameWithType>、<xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling?displayProperty=nameWithType>、<xref:System.Runtime.InteropServices.DllImportAttribute.PreserveSig?displayProperty=nameWithType>、<xref:System.Runtime.InteropServices.DllImportAttribute.SetLastError?displayProperty=nameWithType>、または <xref:System.Runtime.InteropServices.DllImportAttribute.ThrowOnUnmappableChar?displayProperty=nameWithType> の各フィールドを Visual Basic の宣言に適用するには、`Declare` ステートメントではなく <xref:System.Runtime.InteropServices.DllImportAttribute> 属性を使用する必要があります。  
  
```vb
Imports System.Runtime.InteropServices

Friend Class NativeMethods
    <DllImport("user32.dll", CharSet:=CharSet.Auto)>
    Friend Shared Function MessageBox(
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
    End Function
End Class
```
  
```csharp
using System;
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll")]
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```
  
```cpp
using namespace System;
using namespace System::Runtime::InteropServices;

[DllImport("user32.dll")]
extern "C" int MessageBox(
    IntPtr hWnd, String* lpText, String* lpCaption, unsigned int uType);
```
  
## <a name="adjusting-the-definition"></a>定義の調整  
 明示的に設定するかどうかに関係なく、属性フィールドは作業はマネージド コードの動作を動作中に定義します。 プラットフォーム呼び出しは、アセンブリ内のメタデータとして存在するさまざまなフィールドで設定された既定値どおりに動作します。 1 つまたは複数のフィールドの値を調整することによって、この既定の動作を変更することができます。 多くの場合、<xref:System.Runtime.InteropServices.DllImportAttribute> を使用して値を設定します。  
  
 次の表では、プラットフォーム呼び出しに関連する属性フィールドの完全なセットを一覧表示します。 各フィールドについては、表に既定値とこれらのフィールドを使用してアンマネージ DLL 関数を定義する方法に関する情報へのリンクが含まれます。  
  
|フィールド|説明|  
|-----------|-----------------|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.BestFitMapping>|最適なマッピングを有効または無効にします。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.CallingConvention>|メソッドの引数を渡すときに使用する呼び出し規則を指定します。 既定値は `WinAPI` で、32 ビットの Intel ベース プラットフォームの `__stdcall` に対応します。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet>|名前修飾および文字列の引数を関数にマーシャリングする方法を管理します。 既定値は、 `CharSet.Ansi`です。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint>|呼び出される DLL エントリ ポイントを指定します。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling>|文字セットに対応するエントリ ポイントを変更する必要があるかどうかを制御します。 既定値は、プログラミング言語によって異なります。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.PreserveSig>|マネージド メソッドのシグネチャが、HRESULT を返すアンマネージド シグネチャに変換され、戻り値の追加の [out, retval] 引数を持つかどうかを制御します。<br /><br /> 既定値は `true` です (シグネチャは変換されません)。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.SetLastError>|呼び出し元が `Marshal.GetLastWin32Error` API の関数を使用して、メソッドの実行中にエラーが発生したかどうかを判断できるようにします。 Visual Basic では既定値は `true`、C# および C++ では既定値は `false` です。|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.ThrowOnUnmappableChar>|ANSI の"?" 文字に変換されるマップできない Unicode 文字での例外のスローを制御します。|  
  
 詳細なリファレンス情報については、「<xref:System.Runtime.InteropServices.DllImportAttribute>」を参照してください。  
  
## <a name="platform-invoke-security-considerations"></a>プラットフォーム呼び出しのセキュリティに関する考慮事項  
 <xref:System.Security.Permissions.SecurityAction> 列挙型の `Assert`、`Deny`、および `PermitOnly` のメンバーは、*スタック ウォーク修飾子*と呼ばれます。 プラットフォーム呼び出しの宣言および COM インターフェイス定義言語 (IDL) ステートメントの宣言属性として使用される場合、これらのメンバーは無視されます。  
  
### <a name="platform-invoke-examples"></a>プラットフォーム呼び出しの例  
 このセクションのプラットフォーム呼び出しのサンプルは、`RegistryPermission` スタック ウォーク修飾子を持つ属性の使用方法を示します。  
  
 次のコード例では、<xref:System.Security.Permissions.SecurityAction>`Assert`、`Deny`、および `PermitOnly` 修飾子は無視されます。  
  
```csharp  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionAssert();  
  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Deny, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
```  
  
 ただし、次の例の `Demand` 修飾子は受け入れられます。  
  
```csharp
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
```  
  
 プラットフォーム呼び出しを含む (ラップする) クラスに配置されている場合、<xref:System.Security.Permissions.SecurityAction> 修飾子は正常に機能します。  
  
```cpp  
      [RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
public ref class PInvokeWrapper  
{  
public:  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
};  
```  
  
```csharp  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
class PInvokeWrapper  
{  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
}  
```  
  
 <xref:System.Security.Permissions.SecurityAction> 修飾子は、プラットフォーム呼び出しの呼び出し元に配置されたネストしたシナリオでも正しく動作します。  
  
```cpp  
      {  
public ref class PInvokeWrapper  
public:  
    [DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
  
    [RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    public static bool CallRegistryPermission()  
    {  
     return CallRegistryPermissionInternal();  
    }  
};  
```  
  
```csharp  
class PInvokeScenario  
{  
    [DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionInternal();  
  
    [RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    public static bool CallRegistryPermission()  
    {  
     return CallRegistryPermissionInternal();  
    }  
}  
```  
  
#### <a name="com-interop-examples"></a>COM 相互運用機能の例  
 このセクションの次の COM 相互運用機能の例は、スタック ウォーク修飾子を持つ `RegistryPermission` 属性の使用方法を示します。  
  
 次の COM 相互運用機能のインターフェイスの宣言は、前のセクションのプラットフォーム呼び出しの例と同様に、`Assert`、`Deny`、および `PermitOnly` の修飾子を無視します。  
  
```csharp
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IAssertStubsItf  
{  
[RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Assert, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IDenyStubsItf  
{  
[RegistryPermission(SecurityAction.Deny, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Deny, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IAssertStubsItf  
{  
[RegistryPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
```  
  
 さらに、次の例に示すように、`Demand` 修飾子は COM 相互運用機能のインターフェイスの宣言のシナリオでは許可されません。  
  
```csharp  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IDemandStubsItf  
{  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Demand, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
```  
  
## <a name="see-also"></a>関連項目

- [アンマネージ DLL 関数の処理](consuming-unmanaged-dll-functions.md)
- [エントリ ポイントの指定](specifying-an-entry-point.md)
- [文字セットの指定](specifying-a-character-set.md)
- [プラットフォーム呼び出しの例](platform-invoke-examples.md)
- [プラットフォーム呼び出しのセキュリティに関する考慮事項](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb397754(v=vs.100))
- [DLL 内の関数の識別](identifying-functions-in-dlls.md)
- [DLL 関数を保持するクラスの作成](creating-a-class-to-hold-dll-functions.md)
- [DLL 関数の呼び出し](calling-a-dll-function.md)
