---
title: Windows でのマネージド スレッド処理とアンマネージド スレッド処理
ms.date: 10/24/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], unmanaged
- threading [.NET Framework], managed
- threading [.NET], managed
- threads and fibers [.NET]
- managed threading
ms.assetid: 4fb6452f-c071-420d-9e71-da16dee7a1eb
ms.openlocfilehash: 6ab0cc7c1ec2f7bbc633ac966dd18ab3ea7a395b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127543"
---
# <a name="managed-and-unmanaged-threading-in-windows"></a>Windows でのマネージド スレッド処理とアンマネージド スレッド処理

共通言語ランタイムにより作成されたスレッド、マネージド環境に入ってコードを実行するランタイム外部で作成されたスレッドなど、すべてのスレッドの管理は、 <xref:System.Threading.Thread> クラスを使用して行われます。 ランタイムは、プロセス内のスレッドのうち、マネージド実行環境内でコードを実行したすべてのスレッドを監視します。 その他のスレッドは追跡しません。 ランタイムがマネージド オブジェクトを COM オブジェクトとしてアンマネージド環境に公開するため、スレッドは COM 相互運用を使用してマネージド実行環境に入ることができます。また、COM [DllGetClassObject](/windows/desktop/api/combaseapi/nf-combaseapi-dllgetclassobject) 関数やプラットフォーム呼び出しを介してマネージド実行環境に入ることもできます。  
  
 ただしアンマネージド スレッドが COM 呼び出し可能ラッパーなどを介してランタイムに入ると、システムがそのスレッド ローカル ストアで内部マネージド <xref:System.Threading.Thread> オブジェクトを検索します。 このオブジェクトが見つかった場合、ランタイムは既にこのスレッドを認識しています。 見つからない場合、ランタイムは新しい <xref:System.Threading.Thread> オブジェクトを作成し、そのスレッドのスレッド ローカル ストアにインストールします。  
  
 マネージド スレッド処理では、 <xref:System.Threading.Thread.GetHashCode%2A?displayProperty=nameWithType> は安定したマネージド スレッド ID です。 この値は、取得されたアプリケーション ドメインに関係なく、スレッドの有効期間にわたって他のスレッドの値と競合することはありません。  
  
> [!NOTE]
> オペレーティング システム **ThreadId** とマネージド スレッドの間には固定的な関係はありません。これは、アンマネージド ホストがマネージド スレッドとアンマネージド スレッドの間の関係を制御できるためです。 特に、高度なホストはファイバー API を使用して、多数のマネージド スレッドを同一オペレーティング システム スレッドに対してスケジュールしたり、マネージド スレッドを異なるオペレーティング システム スレッド間で移動したりできます。  
  
## <a name="mapping-from-win32-threading-to-managed-threading"></a>Win32 スレッド処理とマネージド スレッド処理の対応付け

 Win32 スレッド処理要素とほぼそれに対応するランタイムの対応付けを次の表に示します。 この対応付けは、同一の機能性を示すものではありません。 たとえば **TerminateThread** は **finally** 句の実行やリソースの解放は行わず、また防止することはできません。 ただし <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> はすべてのロールバック コードを実行し、すべてのリソースを解放します。また、 <xref:System.Threading.Thread.ResetAbort%2A>を使用して拒否することができます。 機能について推測する前に、このドキュメントを詳しくお読みください。  
  
|Win32|共通言語ランタイム|  
|--------------|------------------------------------|  
|**CreateThread**|**Thread** と <xref:System.Threading.ThreadStart>の組み合わせ|  
|**TerminateThread**|<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>|  
|**SuspendThread**|<xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType>|  
|**ResumeThread**|<xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType>|  
|**Sleep**|<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>|  
|スレッド ハンドルの**WaitForSingleObject**|<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>|  
|**ExitThread**|同等の機能がありません|  
|**GetCurrentThread**|<xref:System.Threading.Thread.CurrentThread%2A?displayProperty=nameWithType>|  
|**SetThreadPriority**|<xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType>|  
|同等の機能がありません|<xref:System.Threading.Thread.Name%2A?displayProperty=nameWithType>|  
|同等の機能がありません|<xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>|  
|**CoInitializeEx** (OLE32.DLL) に類似|<xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>|  
  
## <a name="managed-threads-and-com-apartments"></a>マネージド スレッドと COM アパートメント

マネージド スレッドには、[シングル スレッド](/windows/desktop/com/single-threaded-apartments) アパートメントをホストするか、[マルチ スレッド](/windows/desktop/com/multithreaded-apartments) アパートメントをホストするかを示すようマークすることができます (COM スレッド アーキテクチャの詳細については、「[Processes, Threads, and Apartments](/windows/desktop/com/processes--threads--and-apartments)」(プロセス、スレッド、アパートメント) を参照してください。)<xref:System.Threading.Thread.GetApartmentState%2A> クラスの <xref:System.Threading.Thread.SetApartmentState%2A>、<xref:System.Threading.Thread.TrySetApartmentState%2A>、および <xref:System.Threading.Thread> の各スレッドは、スレッドのアパートメント状態を返して割り当てます。 状態が設定されていない場合、<xref:System.Threading.Thread.GetApartmentState%2A> は <xref:System.Threading.ApartmentState.Unknown?displayProperty=nameWithType> を返します。  
  
 プロパティは、スレッドが <xref:System.Threading.ThreadState.Unstarted?displayProperty=nameWithType> 状態の場合にのみ設定することができます。設定できるのは、1 つのスレッドにつき 1 回だけです。  
  
 スレッド開始前にアパートメントの状態が設定されていない場合、このスレッドはマルチスレッド アパートメント (MTA) として初期化されます。 ファイナライザー スレッドと、 <xref:System.Threading.ThreadPool> により制御されるすべてのスレッドは MTA です。  
  
> [!IMPORTANT]
> アプリケーションのスタートアップ コードでは、アパートメントの状態を制御する方法は、 <xref:System.MTAThreadAttribute> または <xref:System.STAThreadAttribute> をエントリ ポイント プロシージャに適用する方法だけです。 .NET Framework 1.0 と 1.1 では、 <xref:System.Threading.Thread.ApartmentState%2A> プロパティを最初のコード行として設定できます。 .NET Framework 2.0 ではこの設定は許可されていません。  
  
 COM に対して公開されるマネージド オブジェクトは、フリー スレッド マーシャラーを集約した場合と同様に動作します。 つまり、フリースレッドな方法ですべての COM アパートメントから呼び出すことができます。 このフリー スレッドな動作を示さないマネージド オブジェクトは、<xref:System.EnterpriseServices.ServicedComponent> または <xref:System.Runtime.InteropServices.StandardOleMarshalObject> から派生したオブジェクトだけです。  
  
 マネージ環境では、コンテキストおよびコンテキストにバインディングされたマネージド インスタンスを使用しない場合には <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> はサポートされません。 Enterprise Services を使う場合は、オブジェクトを <xref:System.EnterpriseServices.ServicedComponent> (<xref:System.ContextBoundObject> から派生したオブジェクト) から派生する必要があります。  
  
 マネージド コードは、COM オブジェクトを呼び出すときには常に COM 規則に従います。 つまり、OLE32 によって示される COM アパートメント プロキシと COM+ 1.0 コンテキスト ラッパーを介して呼び出します。  
  
## <a name="blocking-issues"></a>障害となっている問題点  

アンマネージ コードでスレッドをブロックしているオペレーティング システムに対し、そのスレッドがアンマネージ呼び出しを実行する場合、ランタイムは <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> または <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>に対してその呼び出しを制御しません。 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>では、スレッドが再びマネージド コードに入ると、ランタイムはスレッドを **Abort** 対象としてマークし、スレッドを制御します。 アンマネージド ブロックではなくマネージド ブロックを使用することをお勧めします。 <xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>、<xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType>, <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType>, <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>, <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType>などはすべて <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> と <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>に応答します。 また、スレッドがシングルスレッド アパート内にある場合、これらのマネージド ブロック操作はすべて、スレッドがブロックされている間でもアパートメント内で正しくメッセージ ポンプを行います。  

## <a name="threads-and-fibers"></a>スレッドとファイバー

.NET スレッド モデルでは、[ファイバー](/windows/desktop/procthread/fibers)がサポートされていません。 ファイバーの使用により実装されるアンマネージド関数を呼び出さないでください。 呼び出すと、.NET ランタイムがクラッシュする可能性があります。

## <a name="see-also"></a>関連項目

- <xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>
- <xref:System.Threading.ThreadState>
- <xref:System.EnterpriseServices.ServicedComponent>
- <xref:System.Threading.Thread>
- <xref:System.Threading.Monitor>
