---
title: dangerousThreadingAPI MDA
ms.date: 03/30/2017
helpviewer_keywords:
- suspending threads
- DangerousThreadingAPI MDA
- managed debugging assistants (MDAs), dangerous threading operations
- threading [.NET Framework], suspending
- MDAs (managed debugging assistants), dangerous threading operations
- Suspend method
- threading [.NET Framework], managed debugging assistants
ms.assetid: 3e5efbc5-92e4-4229-b31f-ce368a1adb96
ms.openlocfilehash: 4e7e858dfb85eeccbadb23da60d081d1407e89d8
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216673"
---
# <a name="dangerousthreadingapi-mda"></a>dangerousThreadingAPI MDA
`dangerousThreadingAPI` マネージド デバッグ アシスタント (MDA) は、現在のスレッド以外のスレッドで <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> メソッドが呼び出されるとアクティブになります。  
  
## <a name="symptoms"></a>現象  
 アプリケーションがいつまでも応答しないかハングアップしています。 システムまたはアプリケーションのデータが一時的に、またはアプリケーションのシャットダウン後も、予期しない状態のままになっている可能性があります。 一部の操作が予期したとおりに完了していません。  
  
 問題に伴うランダム性により、症状は大きく異なる場合があります。  
  
## <a name="cause"></a>原因  
 スレッドは、<xref:System.Threading.Thread.Suspend%2A> メソッドを使用する別のスレッドによって非同期に中断されています。 操作途中である可能性がある別のスレッドを安全に中断できるタイミングを判断する方法はありません。 スレッドを中断すると、データの破損やインバリアントの中断が発生することがあります。 スレッドが中断状態になっていて、<xref:System.Threading.Thread.Resume%2A> メソッドを使用して再開されないと、アプリケーションのハングアップ状態がいつまでも続き、アプリケーション データが損害を受けることがあります。 これらのメソッドは不使用とマークされています。  
  
 同期プリミティブがターゲット スレッドによって保持されている場合は、中断の間も保持されたままになります。 これにより、<xref:System.Threading.Thread.Suspend%2A> を実行するスレッドなど、別のスレッドがプリミティブのロックを取得しようとすると、デッドロックが発生することがあります。 この場合、問題自体がデッドロックとして現れます。  
  
## <a name="resolution"></a>解決策  
 <xref:System.Threading.Thread.Suspend%2A> および <xref:System.Threading.Thread.Resume%2A> を使用する必要のある設計を避けます。 スレッド間の協調では、<xref:System.Threading.Monitor>、<xref:System.Threading.ReaderWriterLock>、<xref:System.Threading.Mutex> などの同期プリミティブや、C# の `lock` ステートメントを使用します。 これらのメソッドを使用する必要がある場合は、時間を短くし、スレッドが中断状態にある間に実行されるコードの量を最小限に留めます。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。 危険なスレッド処理操作に関するデータを報告するだけです。  
  
## <a name="output"></a>出力  
 MDA は、アクティブになった原因である危険なスレッド処理メソッドを示します。  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dangerousThreadingAPI />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 <xref:System.Threading.Thread.Suspend%2A> のアクティブ化の原因となる `dangerousThreadingAPI` メソッド呼び出しのコード例を次に示します。  
  
```csharp
using System.Threading;  
void FireMda()  
{  
Thread t = new Thread(delegate() { Thread.Sleep(1000); });  
    t.Start();  
    // The following line activates the MDA.  
    t.Suspend();   
    t.Resume();  
    t.Join();  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Thread>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
- [lock ステートメント](../../csharp/language-reference/keywords/lock-statement.md)
