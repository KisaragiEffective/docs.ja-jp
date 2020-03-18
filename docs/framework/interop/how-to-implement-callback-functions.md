---
title: '方法: コールバック関数を実装する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- callback function, implementing
ms.assetid: e55b3712-b9ea-4453-bd9a-ad5cfa2f6bfa
ms.openlocfilehash: 23355e16127b45c26a1d950c6a8b3cc27e265781
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123888"
---
# <a name="how-to-implement-callback-functions"></a>方法: コールバック関数を実装する
次の手順と例は、マネージド アプリケーションがプラットフォーム呼び出しを使用して、ローカル コンピューター上の各ウィンドウのハンドル値を出力する方法を示しています。 具体的には、この手順と例では **EnumWindows** 関数を使用してウィンドウのリストをステップスルーし、(CallBack という名前の) マネージド コールバック関数を使用してウィンドウ ハンドルの値を出力します。  
  
### <a name="to-implement-a-callback-function"></a>コールバック関数を実装するには  
  
1. 実装を進める前に、**EnumWindows** 関数のシグネチャを見てみます。 **EnumWindows** のシグネチャは次のとおりです。  
  
    ```cpp
    BOOL EnumWindows(WNDENUMPROC lpEnumFunc, LPARAM lParam)
    ```
  
     この関数がコールバックを必要とする 1 つの手掛かりは、**lpEnumFunc** 引数が存在することです。 コールバック関数へのポインターを使用する引数の名前では、**lp** (long pointer) プレフィックスが **Func** サフィックスと組み合わされているのが一般的です。 Win32 の関数に関するドキュメントについては、Microsoft プラットフォーム SDK を参照してください。  
  
2. マネージド コールバック関数を作成します。 この例では、`CallBack` というデリゲート型を宣言しています。この型は 2 つの引数 (**hwnd** と **lparam**) を受け取ります。 最初の引数はウィンドウのハンドル、2 番目の引数はアプリケーションで定義します。 このリリースでは、両方の引数が整数でなければなりません。  
  
     コールバック関数は通常、成功を示す場合は 0 以外の値を返し、失敗を示す場合は 0 を返します。 この例では、列挙を続けるために戻り値を明示的に **true** に設定します。  
  
3. デリゲートを作成し、**EnumWindows** 関数に引数として渡します。 プラットフォーム呼び出しにより、デリゲートは既知のコールバック形式に自動的に変換されます。  
  
4. コールバック関数がその作業を完了する前にガベージ コレクターがデリゲートをクリアしないようにしてください。 デリゲートをパラメーターとして渡した場合、または構造内のフィールドとして含まれるデリゲートを渡した場合、デリゲートは呼び出しの間収集されないままです。 したがって、次の列挙例と同様に、コールバック関数はその作業を呼び出しが戻る前に完了するので、マネージド呼び出し元による追加操作を必要としません。  
  
     ただし、呼び出しが戻った後にコールバック関数を呼び出せる場合、マネージド呼び出し元は、コールバック関数が終了するまでデリゲートが収集されないようにしておくための対策を講じる必要があります。 ガベージ コレクションを回避する方法の詳細については、プラットフォーム呼び出しによる「[相互運用マーシャリング](interop-marshaling.md)」を参照してください。  
  
## <a name="example"></a>例  
  
```vb  
Imports System  
Imports System.Runtime.InteropServices  
  
Public Delegate Function CallBack( _  
hwnd As Integer, lParam As Integer) As Boolean  
  
Public Class EnumReportApp  
  
    Declare Function EnumWindows Lib "user32" ( _  
       x As CallBack, y As Integer) As Integer  
  
    Public Shared Sub Main()  
        EnumWindows(AddressOf EnumReportApp.Report, 0)  
    End Sub 'Main  
  
    Public Shared Function Report(hwnd As Integer, lParam As Integer) _  
    As Boolean  
        Console.Write("Window handle is ")  
        Console.WriteLine(hwnd)  
        Return True  
    End Function 'Report  
End Class 'EnumReportApp  
```  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public delegate bool CallBack(int hwnd, int lParam);  
  
public class EnumReportApp  
{  
    [DllImport("user32")]  
    public static extern int EnumWindows(CallBack x, int y);   
  
    public static void Main()   
    {  
        CallBack myCallBack = new CallBack(EnumReportApp.Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    public static bool Report(int hwnd, int lParam)  
    {   
        Console.Write("Window handle is ");  
        Console.WriteLine(hwnd);  
        return true;  
    }  
}  
```  
  
```cpp  
using namespace System;  
using namespace System::Runtime::InteropServices;  
  
// A delegate type.  
delegate bool CallBack(int hwnd, int lParam);  
  
// Managed type with the method to call.  
ref class EnumReport  
{  
// Report the window handle.  
public:  
    [DllImport("user32")]  
    static int EnumWindows(CallBack^ x, int y);  
  
    static void Main()  
    {  
        EnumReport^ er = gcnew EnumReport;  
        CallBack^ myCallBack = gcnew CallBack(&EnumReport::Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    static bool Report(int hwnd, int lParam)  
    {  
       Console::Write(L"Window handle is ");  
       Console::WriteLine(hwnd);  
       return true;  
    }  
};  
  
int main()  
{  
    EnumReport::Main();  
}  
```  
  
## <a name="see-also"></a>関連項目

- [コールバック関数](callback-functions.md)
- [DLL 関数の呼び出し](calling-a-dll-function.md)
