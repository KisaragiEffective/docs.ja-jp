---
title: callbackOnCollectedDelegate MDA
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- MDAs (managed debugging assistants), garbage collection
- managed debugging assistants (MDAs), callback on collected delegates
- MDAs (managed debugging assistants), callback on collected delegates
- callback on delegate after garbage collection
- callbackOnCollectedDelegate MDA
- managed debugging assistants (MDAs), garbage collection
- call back collected delegates
- garbage collection, run-time errors
- delegates [.NET Framework], garbage collection
ms.assetid: 398b0ce0-5cc9-4518-978d-b8263aa21e5b
ms.openlocfilehash: eb14e0df5396d92eb223dde2e562684c4c318295
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217566"
---
# <a name="callbackoncollecteddelegate-mda"></a>callbackOnCollectedDelegate MDA
`callbackOnCollectedDelegate` マネージド デバッグ アシスタント (MDA) は、デリゲートがマネージド コードからアンマネージド コードに関数ポインターとしてマーシャリングされ、デリゲートがガベージ コレクトされた後にその関数ポインター上にコールバックが配置された場合にアクティブ化されます。  
  
## <a name="symptoms"></a>現象  
 マネージド デリゲートから取得した関数ポインターを通じてマネージド コードへの呼び出しをしようとすると、アクセス違反が発生します。 このエラーは、共通言語ランタイム (CLR) コード内でアクセス違反が発生するため、CLR のバグのように見えますが、実際には違います。  
  
 このエラーには一貫性がなく、関数ポインターでの呼び出しが成功することもあれば、失敗することもあります。 高負荷のときのみエラーが発生することがあり、ランダムな試行回数ごとに発生することもあります。  
  
## <a name="cause"></a>原因  
 関数ポインターが作成されてアンマネージ コードに公開されたデリゲートが、ガベージ コレクトされました。 アンマネージ コンポーネントからその関数ポインターで呼び出しを行おうとすると、アクセス違反が発生します。  
  
 このエラーの発生するタイミングはガベージ コレクションが行われるタイミングに依存するため、ランダムに発生するように見えます。 デリゲートがガベージ コレクションの対象になった場合でも、コールバックが行われて成功した後にガベージ コレクションが行われることがあります。 別の時には、コールバックの前にガベージ コレクションが実行されたため、コールバックがアクセス違反を生成し、プログラムが停止します。  
  
 エラーの発生確率は、デリゲートのマーシャリングから関数ポインターの呼び出しまでの時間と、ガベージ コレクションの頻度に応じて変わります。 デリゲートのマーシャリングから次のコールバックまでの時間が短い場合、エラーは散発的です。 これは、関数ポインターを受け取るアンマネージ メソッドが後で使用するために関数ポインターを保存することはせず、代わりに関数ポインターをすぐにコールバックし、返る前に操作が完了する場合に当てはまります。 同様に、システムの負荷が高いと頻繁にガベージ コレクションが発生するため、コールバックの前にガベージ コレクションが発生する可能性が高くなります。  
  
## <a name="resolution"></a>解決策  
 デリゲートがアンマネージ関数ポインターとしてマーシャリングされた後、ガベージ コレクターはその有効期間を追跡できません。 代わりに、アンマネージ関数ポインターの有効期間にわたってデリゲートへの参照をコードで保持する必要があります。 しかし、それを行うには、まずどのデリゲートがガベージ コレクトされたかを識別する必要があります。 MDA がアクティブになると、デリゲートの型名が提供されます。 この名前を使用してコードを検索し、アンマネージ コードにデリゲートを渡すプラットフォーム呼び出しまたは COM シグネチャを見つけます。 問題が発生したデリゲートは、これらの呼び出しサイトのいずれかを通じて渡されます。 また、`gcUnmanagedToManaged` MDA を有効にして、ランタイムへのコールバックの前にガベージ コレクションを強制することもできます。 これにより、コールバックが行われる前に必ずガベージ コレクションが行われるため、ガベージ コレクションによって持ち込まれる不確実性が排除されます。 ガベージ コレクトされたデリゲートがわかったら、マーシャリングしたアンマネージド 関数ポインターの有効期間にわたってデリゲートへの参照をマネージド側に保持するようにコードを変更します。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 デリゲートを関数ポインターとしてマーシャリングすると、ランタイムはアンマネージドからマネージドへの遷移を行うサンクを割り当てます。 このサンクが、マネージド デリゲートが最終的に呼び出される前に、アンマネージド コードによって実際に呼び出されるものです。 `callbackOnCollectedDelegate` MDA を有効にしない場合、アンマネージのマーシャリング コードはデリゲートがガベージ コレクトされたときに削除されます。 `callbackOnCollectedDelegate` MDA を有効にした場合、アンマネージのマーシャリング コードはデリゲートがガベージ コレクトされたとき、すぐには削除されません。 代わりに、既定では最新の 1,000 個のインスタンスが有効のまま保持され、呼び出されたときに MDA をアクティブ化するように変更されます。 サンクはやがて、さらに 1,001 個のマーシャリングされたデリゲートがガベージ コレクトされた後に削除されます。  
  
## <a name="output"></a>出力  
 MDA は、デリゲートのアンマネージ関数ポインターへのコールバックが試みられる前に、ガベージ コレクトされたデリゲートの型名を報告します。  
  
## <a name="configuration"></a>構成  
 次の例は、アプリケーションの構成オプションを示します。 MDA が有効に保持するサンクの数を 1,500 に設定しています。 `listSize` の既定値は 1,000、最小値は 50、最大値は 2,000 です。  
  
```xml  
<mdaConfig>  
  <assistants>  
    <callbackOnCollectedDelegate listSize="1500" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 次の例は、この MDA がアクティブ化されることのある状況を示しています。  
  
```cpp
// Library.cpp : Defines the unmanaged entry point for the DLL application.  
#include "windows.h"  
#include "stdio.h"  
  
void (__stdcall *g_pfTarget)();  
  
void __stdcall Initialize(void __stdcall pfTarget())  
{  
    g_pfTarget = pfTarget;  
}  
  
void __stdcall Callback()  
{  
    g_pfTarget();  
}
```

```csharp
// C# Client  
using System;  
using System.Runtime.InteropServices;  
  
public class Entry  
{  
    public delegate void DCallback();  
  
    public static void Main()  
    {  
        new Entry();  
        Initialize(Target);  
        GC.Collect();  
        GC.WaitForPendingFinalizers();  
        Callback();  
    }  
  
    public static void Target()  
    {          
    }  
  
    [DllImport("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Initialize(DCallback pfDelegate);  
  
    [DllImport ("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Callback();  
  
    ~Entry() { Console.Error.WriteLine("Entry Collected"); }  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
- [相互運用マーシャリング](../interop/interop-marshaling.md)
- [gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)
