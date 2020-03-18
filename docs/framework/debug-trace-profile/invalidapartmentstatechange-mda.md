---
title: invalidApartmentStateChange MDA
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid apartment state
- managed debugging assistants (MDAs), invalid apartment state
- InvalidApartmentStateChange MDA
- invalid apartment state changes
- threading [.NET Framework], apartment states
- apartment states
- threading [.NET Framework], managed debugging assistants
- COM apartment states
ms.assetid: e56fb9df-5286-4be7-b313-540c4d876cd7
ms.openlocfilehash: 8acafcc2fba9a7d30cc77f25f06adaca7c79db32
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217413"
---
# <a name="invalidapartmentstatechange-mda"></a>invalidApartmentStateChange MDA
`invalidApartmentStateChange` マネージド デバッグ アシスタント (MDS) は、次の 2 つのどちらかの問題によってアクティブ化されます。  
  
- COM によって既に初期化されているスレッドの COM アパートメント状態を異なるアパートメント状態に変更しようとした。  
  
- スレッドの COM アパートメント状態が予期せず変更された。  
  
## <a name="symptoms"></a>現象  
  
- スレッドの COM アパートメント状態が要求されたものと異なります。 これが原因で、現在のモデルと異なるスレッド処理モデルの COM コンポーネントがプロキシで使用される場合があります。 これにより、アパートメント間のマーシャリング用に設定されていないインターフェイスを介して COM オブジェクトが呼び出されるときに <xref:System.InvalidCastException> がスローされる場合があります。  
  
- スレッドの COM アパートメント状態が予想と異なります。 これが原因で、<xref:System.Runtime.InteropServices.COMException>ランタイム呼び出し可能ラッパー<xref:System.InvalidCastException> (RCW) で呼び出しを行うときに、HRESULT が RPC_E_WRONG_THREAD の [、および ](../../standard/native-interop/runtime-callable-wrapper.md) が発生する可能性があります。 これにより、一部のシングル スレッド COM が同時に複数のスレッドによってアクセスされ、そのために破損またはデータの損失につながることがあります。  
  
## <a name="cause"></a>原因  
  
- スレッドは以前に異なる COM アパートメント状態に初期化されました。 スレッドのアパートメント状態を明示的または暗黙的に設定できることに注意してください。 明示的な操作には、<xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType> プロパティ、<xref:System.Threading.Thread.SetApartmentState%2A> メソッド、<xref:System.Threading.Thread.TrySetApartmentState%2A> メソッドが含まれます。 <xref:System.Threading.Thread.Start%2A> メソッドを使用して作成されたスレッドは、スレッドが開始される前に <xref:System.Threading.ApartmentState.MTA> が呼び出されない限り、暗黙的に <xref:System.Threading.Thread.SetApartmentState%2A> に設定されます。 アプリケーションのメイン スレッドも、メイン メソッドで <xref:System.Threading.ApartmentState.MTA> 属性が指定されない限り、暗黙的に <xref:System.STAThreadAttribute> に初期化されます。  
  
- 異なるコンカレンシー モデルを持つ `CoUninitialize` メソッド (または `CoInitializeEx` メソッド) がスレッドで呼び出されます。  
  
## <a name="resolution"></a>解決策  
 スレッドの実行開始前に、スレッドのアパートメント状態を設定するか、<xref:System.STAThreadAttribute> 属性または <xref:System.MTAThreadAttribute> 属性をアプリケーションのメイン メソッドに適用します。  
  
 2 番目の原因の場合、理想的には、`CoUninitialize` メソッドを呼び出すコードを変更し、スレッドの終了間近になり、RCW がなく、基になる COM コンポーネントがまだスレッドによって使用されている状態まで、呼び出しを遅延する必要があります。 ただし、`CoUninitialize` を呼び出すコードを変更できない場合は、この方法で初期化解除されるスレッドから RCW を使用しないようにする必要があります。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。  
  
## <a name="output"></a>出力  
 現在のスレッドの COM アパートメント状態、およびコードが適用しようとした状態。  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidApartmentStateChange />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 次のコードの例は、この MDA がアクティブ化されることのある状況を示しています。  
  
```csharp
using System.Threading;  
namespace ApartmentStateMDA  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            Thread.CurrentThread.SetApartmentState(ApartmentState.STA);  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
- [相互運用マーシャリング](../interop/interop-marshaling.md)
