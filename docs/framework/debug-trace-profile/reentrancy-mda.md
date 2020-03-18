---
title: reentrancy MDA
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged code, debugging
- transitioning threads unmanaged to managed code
- reentrancy MDA
- reentrancy without an orderly transition
- managed debugging assistants (MDAs), reentrancy
- illegal reentrancy
- MDAs (managed debugging assistants), reentrancy
- threading [.NET Framework], managed debugging assistants
- managed code, debugging
- native debugging, MDAs
ms.assetid: 7240c3f3-7df8-4b03-bbf1-17cdce142d45
ms.openlocfilehash: 8f1621090079c030e3c055a417ed9bcad882bf78
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217234"
---
# <a name="reentrancy-mda"></a>reentrancy MDA
`reentrancy` マネージド デバッグ アシスタント (MDA) は、前のマネージド コードからネイティブ コードへの切り替えが遷移順序を守って実行されなかった場合に、ネイティブ コードからマネージド コードへの遷移が試みられるとアクティブになります。  
  
## <a name="symptoms"></a>現象  
 ネイティブ コードからマネージド コードに遷移するときに、オブジェクト ヒープが壊れたり、他の重大なエラーが発生しています。  
  
 ネイティブ コードとマネージド コードの間でどちらかの方向に切り替えるスレッドは、適切な遷移順序を実行する必要があります。 ただし、ベクトル化例外ハンドラーなど、オペレーティング システムの特定の低レベル拡張ポイントでは、適切な遷移順序を実行しなくてもマネージド コードからネイティブ コードに切り替えることができます。  これらの切り替えは、共通言語ランタイム (CLR) の管理下ではなく、オペレーティング システムの管理下にあります。  これらの拡張ポイント内で実行されるネイティブ コードでは、マネージド コードへのコールバックを避ける必要があります。  
  
## <a name="cause"></a>原因  
 ベクトル化例外ハンドラーなどの低レベルのオペレーティング システム拡張ポイントは、マネージド コードの実行中にアクティブになります。  そのような拡張ポイントを介して呼び出されたアプリケーション コードが、マネージド コードにコールバックしようとしています。  
  
 この問題は常に、アプリケーション コードが原因で発生します。  
  
## <a name="resolution"></a>解決策  
 この MDA をアクティブにしたスレッドのスタック トレースを確認します。  スレッドがマネージド コードの不正な呼び出しを試みています。  スタック トレースでは、この拡張ポイントを使っているアプリケーションのコード、この拡張ポイントを提供しているオペレーティング システムのコード、および拡張ポイントによって中断されたマネージド コードが、示されているはずです。  
  
 たとえば、ベクトル化例外ハンドラー内からのマネージド コードの呼び出しの試みによってアクティブ化された MDA が表示されます。  スタックには、オペレーティング システムの例外処理コードと、<xref:System.DivideByZeroException> や <xref:System.AccessViolationException> などの例外をトリガーするマネージド コードが表示されます。  
  
 この例の正しい解決策は、アンマネージ コードでベクトル化例外ハンドラーを完全に実装することです。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。  
  
## <a name="output"></a>出力  
 MDA は、無効な再入が試みられていることを報告します。  これが発生している理由と、問題の解決方法を確認するには、スレッドのスタックを調べます。 サンプルの出力を次に示します。  
  
```output
Additional Information: Attempting to call into managed code without   
transitioning out first.  Do not attempt to run managed code inside   
low-level native extensibility points. Managed Debugging Assistant   
'Reentrancy' has detected a problem in 'D:\ConsoleApplication1\  
ConsoleApplication1\bin\Debug\ConsoleApplication1.vshost.exe'.  
```  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <reentrancy />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 次のコード例では、<xref:System.AccessViolationException> がスローされます。  ベクトル化例外処理をサポートする Windows のバージョンでは、これによりマネージド ベクトル化例外ハンドラーが呼び出されます。  `reentrancy` MDA が有効にされている場合、オペレーティング システムのベクトル化例外処理サポート コードから `MyHandler` の呼び出しが試みられると、MDA がアクティブになります。  
  
```csharp
using System;  
public delegate int ExceptionHandler(IntPtr ptrExceptionInfo);  
  
public class Reenter   
{  
    public static ExceptionHandler keepAlive;  
  
    [System.Runtime.InteropServices.DllImport("kernel32", ExactSpelling=true,   
        CharSet=System.Runtime.InteropServices.CharSet.Auto)]  
    public static extern IntPtr AddVectoredExceptionHandler(int bFirst,   
        ExceptionHandler handler);  
  
    static int MyHandler(IntPtr ptrExceptionInfo)   
    {  
        // EXCEPTION_CONTINUE_SEARCH  
        return 0;  
    }  
    void Run() {}  
  
    static void Main()   
    {  
        keepAlive = new ExceptionHandler(Reenter.MyHandler);  
        IntPtr ret = AddVectoredExceptionHandler(1, keepAlive);  
        try   
        {  
            // Dispatch on null should AV.  
            Reenter r = null;   
            r.Run();  
        }   
        catch { }  
    }  
}  
```  
  
## <a name="see-also"></a>参照

- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
