---
title: 信頼性に関するベスト プラクティス
ms.date: 03/30/2017
helpviewer_keywords:
- marking locks
- rebooting databases
- denial of service attacks
- back-out code
- SQL Server [.NET Framework], reliability
- synchronization, reliability
- single-threaded COM components
- slow leaks
- suspending threads
- asynchronous exception handling
- leaked resources [.NET Framework]
- unmanaged memory
- memory, reliability
- threading [.NET Framework], reliability
- process-wide domain shared states
- shared states
- SafeHandle class, reliability
- reliability contracts [.NET Framework]
- cleanup operations
- constrained execution regions
- CERs
- finalizers, reliability
- reliability [.NET Framework]
- blocks, reliability
- finally clauses
- cross-application domain shared states
- catch blocks
- identifying locks
- writing reliable code
- impersonation
- GC.KeepAlive method
- managed threading
- locks, reliability
- STA-dependent features
- fibers
ms.assetid: cf624c1f-c160-46a1-bb2b-213587688da7
ms.openlocfilehash: bd51ea1b79ac1dbd89a862f3961cc8508a87f301
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715984"
---
# <a name="reliability-best-practices"></a>信頼性に関するベスト プラクティス

以下の信頼性ルールは SQL Server を対象としたものですが、他のホスト ベースのサーバー アプリケーションにも当てはまります。 SQL Server などのサーバーがリソースをリークせず、停止しないことが非常に重要です。  ただし、オブジェクトの状態を変更するすべてのメソッドに対してバックアウト コードを記述することでは、それを実現できません。  目標は、バックアウト コードによりすべての場所ですべてのエラーから復旧する 100% 信頼できるマネージド コードを記述することではありません。  それは、成功する可能性がほとんどない面倒な作業です。  共通言語ランタイム (CLR) では、完全なマネージド コードを作成できるという十分に強力な保証は簡単には得られません。  ASP.NET とは異なり、SQL Server で使用されているプロセスは 1 つだけであり、受け入れられないほど長い時間データベースを停止させない限りリサイクルできません。

このように強力な保証がなく、単一プロセスで実行されている場合の信頼性は、必要なときにスレッドを終了するか、アプリケーション ドメインをリサイクルすること、および予防策を設けてハンドルやメモリなどのオペレーティング システム リソースがリークしないようにすることに基づきます。  このような単純な信頼性の制約であっても、大きな信頼性の要件があります。

- オペレーティング システムのリソースがリークしないこと。

- CLR に対するすべてのフォームにおいてすべてのマネージド ロックを識別すること。

- アプリケーション間のドメイン共有状態を壊すことなく、<xref:System.AppDomain> のリサイクルが円滑に機能すること。

<xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException>、<xref:System.OutOfMemoryException> の各例外を処理するマネージド コードを記述することは理論的には可能ですが、アプリケーション全体でそのような堅牢なコードを記述することを開発者に期待するのは無謀です。  そのため、帯域外の例外では実行中のスレッドが終了します。また、終了したスレッドが共有の状態を編集していた場合は (これは、スレッドがロックを保持しているかどうかで判断できます)、<xref:System.AppDomain> がアンロードされます。  共有状態を編集しているメソッドが終了された場合、共有状態の更新に対する信頼性の高いバックアウト コードを記述することはできないため、状態が破損します。

.NET Framework バージョン 2.0 では、信頼性が必要なホストは SQL Server だけです。  アセンブリが SQL Server で実行される場合は、データベースでの実行時には無効にされる特定の機能がある場合でも、そのアセンブリのすべての部分について信頼性の作業を行う必要があります。  これが必要になるのは、コード分析エンジンはアセンブリ レベルでコードを調べるため、無効にされるコードを区別できないためです。 SQL Server のプログラミングに関するもう 1 つの考慮事項は、SQL Server はすべての処理を 1 つのプロセスで実行し、メモリやオペレーティング システム ハンドルなどのすべてのリソースをクリーンアップするには <xref:System.AppDomain> のリサイクルが使われるということです。

バックアウト コードでファイナライザー、デストラクター、または `try/finally` ブロックに依存することはできません。 これらは、中断されたり呼び出されない可能性があります。

<xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException>、<xref:System.OutOfMemoryException> などの非同期例外が、予期しない場所 (すべてのマシン命令) でスローされる可能性があります。

マネージド スレッドは必ずしも SQL 内の Win32 スレッドではありません。ファイバーである可能性があります。

プロセス全体またはアプリケーション間のドメイン変更可能な共有状態は、安全に変更することが特に困難であり、可能な限り避ける必要があります。

メモリ不足の状況は SQL Server では珍しくありません。

SQL Server でホストされているライブラリが共有状態を正しく更新しない場合、データベースを再起動しないかぎりコードを復旧できない可能性が高くなります。  さらに、極端なケースでは、これにより SQL Server プロセスが失敗し、データベースが再起動する可能性があります。  データベースが再起動すると、Web サイトが停止したり、会社の運用に影響して、可用性が低下します。  メモリやハンドルなどのオペレーティング システムのリソースがゆっくりリークすると、最終的にサーバーでのハンドルの割り当てが失敗して復旧できなかったり、サーバーのパフォーマンスが徐々に悪化して顧客のアプリケーションの可用性が低下する可能性があります。  これらのシナリオを回避する必要があるのは明らかです。

## <a name="best-practice-rules"></a>ベストプラクティスルール

概要では、フレームワークの安定性と信頼性を向上させるために、サーバーで実行されるマネージド コードのコード レビューで把握する必要があることに注目しました。 これらのチェックはすべて、一般的によいことであり、サーバーでは絶対に必要なことです。

SQL Server は、デッド ロックやリソースの制約が発生すると、スレッドを中止するか、<xref:System.AppDomain> を破棄します。  その場合は、制約された実行領域 (CER) 内のバックアウト コードのみが実行を保証されます。

### <a name="use-safehandle-to-avoid-resource-leaks"></a>SafeHandle を使用してリソースリークを回避する

<xref:System.AppDomain> がアンロードされる状況では、`finally` ブロックまたはファイナライザーが実行されることに依存できないので、<xref:System.IntPtr>、<xref:System.Runtime.InteropServices.HandleRef>、または同様のクラスではなく、<xref:System.Runtime.InteropServices.SafeHandle> クラスを使用して、オペレーティング システムのすべてのリソース アクセスを抽象化することが重要です。 これにより、<xref:System.AppDomain> が破棄されても、CLR は使われたハンドルを追跡して閉じることができます。  <xref:System.Runtime.InteropServices.SafeHandle> は、CLR が常に実行するクリティカル ファイナライザーを使います。

オペレーティング システム ハンドルは、作成されてから解放されるまで、セーフ ハンドルに格納されます。  <xref:System.Threading.ThreadAbortException> が発生してハンドルをリークする可能性のある時間範囲はありません。  さらに、プラットフォームの呼び出しはハンドルを参照カウントするので、ハンドルの有効期間を詳細に追跡でき、`Dispose` と現在、ハンドルを使っているメソッドの間での競合状態によるセキュリティの問題を防ぐことができます。

現在、ファイナライザーを使ってオペレーティング システム ハンドルを単にクリーンアップしているほとんどのクラスは、ファイナライザーを使う必要がなくなります。 代わりに、ファイナライザーは <xref:System.Runtime.InteropServices.SafeHandle> の派生クラスで呼び出されるようになります。

<xref:System.Runtime.InteropServices.SafeHandle> は <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の代わりに使う機能ではないことに注意してください。  依然としてリソース競合の可能性はあり、オペレーティング システムのリソースを明示的に破棄するとパフォーマンス上の利点があります。  リソースの明示的な破棄を行っている `finally` ブロックが最後まで実行されない可能性があることだけは理解しておいてください。

<xref:System.Runtime.InteropServices.SafeHandle> を使うと、ハンドルを解放する処理 (オペレーティング システム ハンドル解放ルーチンに状態を渡す、ハンドルのセットをループで解放する、など) を実行する独自の <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> メソッドを実装できます。  CLR はこのメソッドが実行されることを保証します。  <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> の実装の作成者には、あらゆる状況においてハンドルが解放されることを保証する責任があります。 解放できないとハンドルがリークされ、多くの場合、ハンドルに関連付けられているネイティブ リソースがリークすることになります。 したがって、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> の実装が呼び出し時に使用できない可能性があるリソースの割り当てを必要としないように、<xref:System.Runtime.InteropServices.SafeHandle> 派生クラスを構成することが不可欠です。 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> の実装内で失敗する可能性があるメソッドの呼び出しは、コードがそのようなエラーを処理し、コントラクトを完了してネイティブ ハンドルを解放できる場合に限り、許容されることに注意してください。 デバッグのため、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> には、致命的なエラーが発生してリソースを解放できない場合に `false` に設定できる戻り値 <xref:System.Boolean> があります。 このようにすると、[releaseHandleFailed](../debug-trace-profile/releasehandlefailed-mda.md) MDA がアクティブ化されて (有効になっている場合)、問題を特定するのに役立ちます。 他にはどのような影響もランタイムに与えません。<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> は同じリソースに対して再び呼び出されることはなく、結果としてハンドルはリークされます。

<xref:System.Runtime.InteropServices.SafeHandle> が適さない特定の状況があります。  <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> メソッドは <xref:System.GC> ファイナライザー スレッドで実行できるので、特定のスレッドで解放する必要があるすべてのハンドルは、<xref:System.Runtime.InteropServices.SafeHandle> にラップされていてはなりません。

ランタイム呼び出し可能ラッパー (RCW) は、コードを追加せずに CLR でクリーンアップできます。  プラットフォーム呼び出しを使い、COM オブジェクトを `IUnknown*` または <xref:System.IntPtr> として扱うコードの場合は、RCW を使うようにコードを書き直す必要があります。  アンマネージド リリース メソッドがマネージド コードをコールバックする可能性があるため、このシナリオには <xref:System.Runtime.InteropServices.SafeHandle> は適していない場合があります。

#### <a name="code-analysis-rule"></a>コード分析規則

オペレーティング システムのリソースをカプセル化するには、<xref:System.Runtime.InteropServices.SafeHandle> を使います。 <xref:System.Runtime.InteropServices.HandleRef> または <xref:System.IntPtr> 型のフィールドは使わないでください。

### <a name="ensure-finalizers-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>オペレーティングシステムリソースのリークを防ぐために、ファイナライザーを実行する必要がないことを確認します。

ファイナライザーを慎重に検討し、ファイナライザーが実行しない場合でも重要なオペレーティング システムのリソースがリークされないことを確認します。  アプリケーションが安定した状態で実行しているとき、または SQL Server などのサーバーがシャットダウンするときの、通常の <xref:System.AppDomain> のアンロードとは異なり、<xref:System.AppDomain> の突然のアンロードでは、オブジェクトの終了処理は行われません。  アンロードが突然行われる場合は、アプリケーションの正しさは保証できませんが、リソースをリークしないことでサーバーの整合性を保持する必要があるため、リソースがリークされないことを確認します。  オペレーティング システムのリソースを解放するには、<xref:System.Runtime.InteropServices.SafeHandle> を使います。

### <a name="ensure-that-finally-clauses-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>オペレーティングシステムリソースのリークを防ぐために、finally 句を実行する必要がないことを確認します。

`finally` 句が CER の外部で実行される保証はないので、ライブラリ開発者はアンマネージ リソースを解放するために `finally` ブロック内のコードに依存しないようにする必要があります。  <xref:System.Runtime.InteropServices.SafeHandle> を使うのが推奨される解決策です。

#### <a name="code-analysis-rule"></a>コード分析規則

`Finalize` の代わりに、<xref:System.Runtime.InteropServices.SafeHandle> を使ってオペレーティング システムのリソースをクリーンアップします。 <xref:System.IntPtr> を使わないでください。リソースをカプセル化するには <xref:System.Runtime.InteropServices.SafeHandle> を使います。 finally 句を実行する必要がある場合は、CER 内に配置します。

### <a name="all-locks-should-go-through-existing-managed-locking-code"></a>すべてのロックは、既存のマネージロックコードを通過する必要があります

CLR は、スレッドの単なる中止ではなく、<xref:System.AppDomain> のティアダウンが必要な場合を知るため、コードがロック状態であることを認識する必要があります。  スレッドの中止は、スレッドで使われているデータが不整合な状態のままになる可能性があるため、危険な場合があります。 したがって、<xref:System.AppDomain> 全体をリサイクルする必要があります。  ロックを識別できないと、デッドロックまたは不適切な結果になる可能性があります。 ロック領域を識別するには、<xref:System.Threading.Thread.BeginCriticalRegion%2A> および <xref:System.Threading.Thread.EndCriticalRegion%2A> メソッドを使います。  これらは <xref:System.Threading.Thread> クラスの静的メソッドであり、現在のスレッドにのみ適用され、あるスレッドのロック カウントを別のスレッドが編集するのを防ぐのに役立ちます。

これらのメソッドを使う [lock ステートメント](../../csharp/language-reference/keywords/lock-statement.md)を使うだけでなく、この CLR 通知が組み込まれている <xref:System.Threading.Monitor.Enter%2A> および <xref:System.Threading.Monitor.Exit%2A> を使うこともお勧めします。

スピン ロックや <xref:System.Threading.AutoResetEvent> などの他のロック メカニズムは、これらのメソッドを呼び出して、クリティカルなセクションに入ったことを CLR に通知する必要があります。  これらのメソッドはロックを取得しません。コードがクリティカル セクションで実行していて、スレッドを中止すると共有状態の一貫性がなくなることを、CLR に通知します。  カスタム <xref:System.Threading.ReaderWriterLock> クラスなどの独自のロックの種類を定義している場合は、これらのロック カウント メソッドを使います。

#### <a name="code-analysis-rule"></a>コード分析規則

<xref:System.Threading.Thread.BeginCriticalRegion%2A> および <xref:System.Threading.Thread.EndCriticalRegion%2A> を使ってすべてのロックをマークして識別します。 ループでは <xref:System.Threading.Interlocked.CompareExchange%2A>、<xref:System.Threading.Interlocked.Increment%2A>、および <xref:System.Threading.Interlocked.Decrement%2A> を使わないでください。  これらのメソッドの Win32 バリエーションのプラットフォーム呼び出しは行わないでください。  ループでは <xref:System.Threading.Thread.Sleep%2A> を使わないでください。  volatile フィールドを使わないでください。

### <a name="cleanup-code-must-be-in-a-finally-or-a-catch-block-not-following-a-catch"></a>クリーンアップコードは、catch の後ではなく、finally ブロックまたは catch ブロック内になければなりません

クリーンアップ コードは、`catch` ブロックの後ではなく、`finally` または `catch` ブロック自体の中に置く必要があります。 これは普通に推奨される方法です。 `finally` ブロックは、例外がスローされたときと、`try` ブロックが正常に終了したときの両方で同じコードが実行されるため、一般に優先される方法です。  <xref:System.Threading.ThreadAbortException> などの予期しない例外がスローされた場合は、クリーンアップ コードは実行されません。  `finally` でクリーンアップするアンマネージ リソースをは、リークを防ぐため、できれば <xref:System.Runtime.InteropServices.SafeHandle> にラップする必要があります。  C# の `using` キーワードを効果的に使って、ハンドルなどのオブジェクトを破棄できることに注意してください。

<xref:System.AppDomain> のリサイクルによってファイナライザー スレッドでリソースをクリーンアップできますが、それでもクリーンアップ コードを適切な場所に配置することが重要です。 ロックを保持していないときにスレッドが非同期例外を受け取った場合、CLR は <xref:System.AppDomain> をリサイクルしないでスレッド自体を終了しようとすることに注意してください。  リソースが早期にクリーンアップされるようにすると、リソースの可用性が高くなり、有効期間が適切に管理されるようになる利点があります。 エラー コード パスでファイルへのハンドルを明示的に閉じず、<xref:System.Runtime.InteropServices.SafeHandle> ファイナライザーがクリーンアップするのを待った場合、次にコードがそれを実行したときに、ファイナライザーがまだ実行していないと、まったく同じファイルへのアクセスが失敗する可能性があります。  このため、確実にクリーンアップ コードが存在して正しく機能するようにすることは、絶対に必要なことではありませんが、障害からよりクリーンかつ迅速に復旧するのに役立ちます。

#### <a name="code-analysis-rule"></a>コード分析規則

`catch` の後のクリーンアップ コードは、`finally` ブロック内に配置する必要があります。 dispose の呼び出しは finally ブロック内に置きます。 `catch` ブロックは、スローまたは再スローで終了する必要があります。 例外はありますが (多数の例外のいずれかを取得する可能性があるときにネットワーク接続を確立できるかどうかを検出するコードなど)、通常の状況で複数の例外をキャッチする必要があるコードでは、コードをテストしてそれが成功するかどうかを確認する必要があることを示すようにします。

### <a name="process-wide-mutable-shared-state-between-application-domains-should-be-eliminated-or-use-a-constrained-execution-region"></a>アプリケーションドメイン間でプロセス全体の変更可能な共有状態を削除するか、制約された実行領域を使用する必要があります

概要で説明したように、アプリケーション ドメイン間でプロセス全体の共有状態を確実な方法で監視するマネージド コードを記述するのは非常に困難な場合があります。  プロセス全体の共有状態は、Win32 コード、CLR 内、またはリモート処理を使うマネージド コードにおいて、アプリケーション ドメイン間で共有される何らかの種類のデータ構造です。  変更可能な共有状態をマネージド コードで正確に記述するのは非常に困難であり、静的な共有は細心の注意を払うことによってのみ実現できる場合があります。  プロセス全体またはコンピューター全体の共有状態がある場合は、それを使わないで済む方法を探すか、制約された実行領域 (CER) を使って共有状態を保護するようにします。  共有状態の識別と修正が行われていないライブラリでは、<xref:System.AppDomain> のクリーンなアンロードを必要とする SQL Server などのホストがクラッシュする可能性があることに注意してください。

コードが COM オブジェクトを使っている場合は、アプリケーション ドメイン間でその COM オブジェクトを共有しないでください。

### <a name="locks-do-not-work-process-wide-or-between-application-domains"></a>ロックは、プロセス全体またはアプリケーションドメイン間で機能しません。

以前は、<xref:System.Threading.Monitor.Enter%2A> および [lock ステートメント](../../csharp/language-reference/keywords/lock-statement.md)は、グローバルなプロセス ロックの作成に使われていました。  たとえば、これは、非共有アセンブリからの <xref:System.Type> インスタンスなどの <xref:System.AppDomain> のアジャイル クラス、<xref:System.Threading.Thread> オブジェクト、インターン処理された文字列、およびリモート処理を使ってアプリケーション ドメイン間で共有される文字列でのロック時に発生します。  これらのロックはプロセス全体ではなくなりました。  プロセス全体にわたるアプリケーション間ドメイン ロックの存在を識別するには、ロック内のコードが、ディスク上のファイルやデータベースなどの、外部の永続リソースを使っているかどうかを確認します。

保護されたコードが外部リソースを使っている場合、そのコードが複数のアプリケーション ドメインで同時に実行することがあるため、<xref:System.AppDomain> 内でロックを取得すると問題が発生する可能性があることに注意してください。  これは、1 つのログ ファイルへの書き込み、またはプロセス全体のソケットへのバインドで、問題になる場合があります。  これらの変更は、名前付きの <xref:System.Threading.Mutex> または <xref:System.Threading.Semaphore> インスタンスを使う以外に、マネージド コードを使ってプロセスのグローバルなロックを取得する簡単な方法はないことを意味します。  2 つのアプリケーション ドメインで同時に実行しないコードを作成するか、<xref:System.Threading.Mutex> または <xref:System.Threading.Semaphore> クラスを使ってください。  既存のコードを変更できない場合は、この同期を実現するために Win32 名前付きミューテックスを使わないでください。なぜなら、ファイバー モードで実行するということは、同じオペレーティング システム スレッドでミューテックスを取得して解放することが保証されないことを意味します。  アンマネージド コードを使ってロックを同期するのではなく、マネージド <xref:System.Threading.Mutex> クラス、または名前付きの <xref:System.Threading.ManualResetEvent>、<xref:System.Threading.AutoResetEvent>、または <xref:System.Threading.Semaphore> を使って、CLR が認識する方法でコード ロックを同期する必要があります。

#### <a name="avoid-locktypeofmytype"></a>lock(typeof(MyType)) を使わない

すべてのアプリケーション ドメイン間でコードのただ 1 つのコピーが共有される共有アセンブリのプライベートおよびパブリックの <xref:System.Type> オブジェクトでも、問題が発生します。  共有アセンブリの場合、プロセスごとに <xref:System.Type> のインスタンスが 1 つだけ存在し、これは複数のアプリケーション ドメインがまったく同じ <xref:System.Type> インスタンスを共有することを意味します。  <xref:System.Type> のインスタンスでロックを取得すると、その <xref:System.AppDomain> だけでなく、プロセス全体に影響するロックが取得されます。  ある <xref:System.AppDomain> が <xref:System.Type> オブジェクトでロックを取得した後、そのスレッドが突然中止されると、ロックは解放されません。  その後、このロックにより、他のアプリケーション ドメインでデッドロックが発生する可能性があります。

静的メソッドでロックを取得するよい方法は、静的な内部同期オブジェクトをコードに追加することです。  これは、クラス コンストラクターが存在する場合はそれで初期化できますが、存在しない場合は次のようにして初期化できます。

```csharp
private static Object s_InternalSyncObject;
private static Object InternalSyncObject
{
    get
    {
        if (s_InternalSyncObject == null)
        {
            Object o = new Object();
            Interlocked.CompareExchange(
                ref s_InternalSyncObject, o, null);
        }
        return s_InternalSyncObject;
    }
}
```

その後、ロックを取得するときは、`InternalSyncObject` プロパティを使ってロックするオブジェクトを取得します。  クラス コンストラクターで内部同期オブジェクトを初期化した場合は、プロパティを使う必要はありません。  二重チェックを行うロック初期化コードの例を次に示します。

```csharp
public static MyClass SingletonProperty
{
    get
    {
        if (s_SingletonProperty == null)
        {
            lock(InternalSyncObject)
            {
                // Do not use lock(typeof(MyClass))
                if (s_SingletonProperty == null)
                {
                    MyClass tmp = new MyClass(…);
                    // Do all initialization before publishing
                    s_SingletonProperty = tmp;
                }
            }
        }
        return s_SingletonProperty;
    }
}
```

#### <a name="a-note-about-lockthis"></a>Lock (this) に関する注意事項

一般に、パブリックにアクセスできる個々のオブジェクトでロックを取得することは認められます。  しかし、オブジェクトがサブシステム全体のデッドロックを引き起こす可能性のあるシングルトン オブジェクトである場合は、前述のデザイン パターンの使用も検討する必要があります。  たとえば、1 つの <xref:System.Security.SecurityManager> オブジェクトでのロックにより、<xref:System.AppDomain> 内でデッドロックが発生し、<xref:System.AppDomain> 全体が使用できなくなる可能性があります。 この種のパブリックにアクセスできるオブジェクトではロックを取得しないことをお勧めします。  ただし、個別のコレクションまたは配列でのロックの場合は、一般に問題になりません。

#### <a name="code-analysis-rule"></a>コード分析規則

アプリケーション ドメイン間で使われる可能性がある型では、ロックを取得しないでください。または、ID を強く意識しないでください。 <xref:System.Type>、<xref:System.Reflection.MethodInfo>、<xref:System.Reflection.PropertyInfo>、<xref:System.String>、<xref:System.ValueType>、<xref:System.Threading.Thread>、または <xref:System.MarshalByRefObject> から派生するすべてのオブジェクトでは、<xref:System.Threading.Monitor.Enter%2A> を呼び出さないでください。

### <a name="remove-gckeepalive-calls"></a>GC を削除します。KeepAlive 呼び出し

非常に多くの既存のコードが、使うべき時に <xref:System.GC.KeepAlive%2A> を使っていないか、または適切ではないときにそれを使っています。  <xref:System.Runtime.InteropServices.SafeHandle> に変換した後、ファイナライザーを使わずに、<xref:System.Runtime.InteropServices.SafeHandle> を使ってオペレーティング システム ハンドルの終了処理を行っている場合は、クラスで <xref:System.GC.KeepAlive%2A> を呼び出す必要はありません。  <xref:System.GC.KeepAlive%2A> の呼び出しを残しておくことによるパフォーマンス コストはほんのわずかかもしれませんが、<xref:System.GC.KeepAlive%2A> の呼び出しが、もう存在していないかもしれない有効期間の問題を解決するために必要または十分なものであると意識することは、コードの保守を困難にします。  ただし、COM 相互運用機能の CLR 呼び出し可能ラッパー (RCW) を使うコードでは、<xref:System.GC.KeepAlive%2A> がまだ必要です。

#### <a name="code-analysis-rule"></a>コード分析規則

<xref:System.GC.KeepAlive%2A> を削除します。

### <a name="use-the-hostprotection-attribute"></a>HostProtection 属性を使用する

<xref:System.Security.Permissions.HostProtectionAttribute> (HPA) を使うと、宣言型のセキュリティ アクションを使ってホストの保護要件を決定でき、ホストは完全に信頼されたコードが特定のホストに対して適切ではない特定のメソッド (SQL Server に対する <xref:System.Environment.Exit%2A> や <xref:System.Windows.Forms.MessageBox.Show%2A> など) を呼び出すのを防ぐことができます。

HPA は、共通言語ランタイムをホストし、ホスト保護を実装している SQL Server などのアンマネージ アプリケーションにのみ影響します。 HPA を適用すると、セキュリティ アクションはクラスまたはメソッドが公開するホスト リソースに基づいてリンク確認要求を作成します。 コードがホスト保護されていないクライアント アプリケーションまたはサーバーで実行される場合、この属性は "消滅" します。つまり、検出されないため適用されません。

> [!IMPORTANT]
> この属性の目的は、セキュリティ動作ではなく、ホスト固有のプログラミング モデルのガイドラインを強制することです。  リンク確認要求はプログラミング モデルの要件への準拠を確認するために使われますが、<xref:System.Security.Permissions.HostProtectionAttribute> はセキュリティ アクセス許可ではありません。

ホストにプログラミング モデルの要件がない場合、リンク確認要求は発生しません。

この属性は次のものを識別します。

- ホスト プログラミング モデルには適合しないが、それ以外の問題はないメソッドまたはクラス。

- ホスト プログラミング モデルに適合せず、サーバーが管理するユーザー コードが不安定になる可能性があるメソッドまたはクラス。

- ホスト プログラミング モデルに適合せず、サーバー プロセス自体が不安定になる可能性があるメソッドまたはクラス。

> [!NOTE]
> ホストで保護された環境で実行する可能性のあるアプリケーションによって呼び出されるクラス ライブラリを作成する場合は、<xref:System.Security.Permissions.HostProtectionResource> リソース カテゴリを公開するメンバーにこの属性を適用する必要があります。 この属性を持つ .NET Framework クラス ライブラリのメンバーについては、直前の呼び出し元だけがチェックされます。  カスタムのライブラリ メンバーについても、同じように直前の呼び出し元がチェックされるようにする必要があります。

HPA の詳細については、「<xref:System.Security.Permissions.HostProtectionAttribute>」を参照してください。

#### <a name="code-analysis-rule"></a>コード分析規則

SQL Server の場合、同期またはスレッド化を導入するために使われるすべてのメソッドを、HPA で識別する必要があります。 これには、状態を共有するメソッド、同期されるメソッド、または外部プロセスを管理するメソッドが含まれます。 SQL Server に影響を与える <xref:System.Security.Permissions.HostProtectionResource> の値は、<xref:System.Security.Permissions.HostProtectionResource.SharedState>、<xref:System.Security.Permissions.HostProtectionResource.Synchronization>、および <xref:System.Security.Permissions.HostProtectionResource.ExternalProcessMgmt> です。 ただし、SQL に影響を与えるリソースを使うものだけでなく、いずれかの <xref:System.Security.Permissions.HostProtectionResource> を公開するすべてのメソッドを HPA によって識別する必要があります。

### <a name="do-not-block-indefinitely-in-unmanaged-code"></a>アンマネージコードで無制限にブロックしない

マネージド コード内ではなくアンマネージド コード内でブロックすると、CLR がスレッドを中止できないため、サービス拒否攻撃を受ける可能性があります。  ブロックされたスレッドは、少なくとも一部の非常に安全でない操作を実行せずに、CLR が <xref:System.AppDomain> をアンロードするのを妨げます。  Windows 同期プリミティブを使用したブロックは、許可できないことを明確に示したものです。  ソケットでの `ReadFile` の呼び出しでのブロックは、可能であれば回避する必要があります。理想的には、Windows API は、このような操作のためのメカニズムを提供することをお勧めします。

ネイティブを呼び出すメソッドでは、合理的な有限のタイムアウトで Win32 呼び出しを使うのが理想的です。  ユーザーがタイムアウトを指定できる場合は、何らかの特定のセキュリティ アクセス許可なしでは、ユーザーが無限のタイムアウトを指定できないようにする必要があります。  ガイドラインとしては、メソッドが 10 秒以上ブロックする場合は、タイムアウトをサポートするバージョンを使うか、CLR のサポートを追加する必要があります。

問題のある Api の例をいくつか次に示します。  パイプ (匿名と名前付きのどちらも) はタイムアウトを指定して作成できます。ただし、コードでは、NMPWAIT_WAIT_FOREVER を指定して `CreateNamedPipe` と `WaitNamedPipe` を呼び出さないようにする必要があります。  さらに、タイムアウトを指定した場合でも、予期しないブロックが発生する可能性があります。  匿名パイプで `WriteFile` を呼び出すと、すべてのバイトが書き込まれるまでブロックします。つまり、バッファーに読み取られていないデータがある場合、リーダーがパイプのバッファー内の領域を解放するまで、`WriteFile` の呼び出しをブロックします。  ソケットでは、タイムアウト メカニズムを優先する API を常に使う必要があります。

#### <a name="code-analysis-rule"></a>コード分析規則

アンマネージ コードでのタイムアウトのないブロックは、サービス拒否攻撃です。 `WaitForSingleObject`、`WaitForSingleObjectEx`、`WaitForMultipleObjects`、`MsgWaitForMultipleObjects`、および `MsgWaitForMultipleObjectsEx` のプラットフォーム呼び出しは実行しないでください。  NMPWAIT_WAIT_FOREVER は使わないでください。

### <a name="identify-any-sta-dependent-features"></a>STA に依存する機能を特定する

COM シングルスレッド アパートメント (STA) を使用するコードを明らかにします。  SQL Server プロセスでは STA は無効になります。  パフォーマンス カウンターやクリップボードなどの `CoInitialize` に依存する機能は、SQL Server 内では無効にする必要があります。

### <a name="ensure-finalizers-are-free-of-synchronization-problems"></a>ファイナライザーに同期の問題がないことを確認する

.NET Framework の将来のバージョンでは、複数のファイナライザー スレッドが存在する可能性があります。つまり、同じ型の異なるインスタンスのファイナライザーが同時に実行します。  これらは、完全にスレッド セーフである必要はありません。ガベージ コレクターが、ただ 1 つのスレッドで特定のオブジェクト インスタンスのファイナライザーが実行されることを保証します。  ただし、複数の異なるオブジェクト インスタンスで同時に実行するときの競合状態やデッドロックを回避するように、ファイナライザーをコーディングする必要があります。  ログ ファイルへの書き込みなどの外部状態をファイナライザーで使う場合は、スレッド処理の問題に対処する必要があります。  終了処理に依存してスレッド セーフを提供しないでください。 マネージドまたはネイティブのスレッド ローカル記憶域を使って、ファイナライザー スレッドに状態を保存しないでください。

#### <a name="code-analysis-rule"></a>コード分析規則

ファイナライザーには同期の問題が存在していない必要があります。 静的な変更可能状態をファイナライザーで使用しないでください。

### <a name="avoid-unmanaged-memory-if-possible"></a>可能な場合はアンマネージメモリを使用しない

オペレーティング システム ハンドルと同じように、アンマネージ メモリはリークする可能性があります。 可能であれば、[stackalloc](../../csharp/language-reference/operators/stackalloc.md) を使ってスタック上のメモリを使うか、[fixed ステートメント](../../csharp/language-reference/keywords/fixed-statement.md) や byte[] を使う <xref:System.Runtime.InteropServices.GCHandle> などの固定されたマネージド オブジェクトを使うようにします。 最終的には <xref:System.GC> がこれらをクリーンアップします。 ただし、アンマネージ メモリを割り当てる必要がある場合は、<xref:System.Runtime.InteropServices.SafeHandle> から派生するクラスを使ってメモリの割り当てをラップすることを考えます。

<xref:System.Runtime.InteropServices.SafeHandle> が適切ではないケースが少なくとも 1 つあることに注意してください。 メモリの割り当てや解放を行う COM メソッド呼び出しでは、1 つの DLL が `CoTaskMemAlloc` を使ってメモリを割り当てた後、別の DLL が `CoTaskMemFree` でそのメモリを解放するのが一般的です。  これらの場所で <xref:System.Runtime.InteropServices.SafeHandle> を使うのは、他の DLL がメモリの有効期間を制御できるようにする代わりに、アンマネージ メモリの有効期間を <xref:System.Runtime.InteropServices.SafeHandle> の有効期間に結び付けようとするため不適切です。

### <a name="review-all-uses-of-catchexception"></a>Catch (Exception) のすべての使用を確認する

1 つの特定の例外ではなくすべての例外をキャッチする catch ブロックは、非同期例外もキャッチするようになります。 すべての catch(Exception) ブロックを調べて、スキップされる可能性のある重要ではないリソース解放やバックアウト コード、および <xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException>、<xref:System.OutOfMemoryException> の処理に関する catch ブロック自体の内部での不適切な可能性のある動作を探します。  このコードでは、特定の例外だけを認識できる、または例外が発生したときは常に厳密に特定の 1 つの理由で失敗するということが、ログに記録されたり、想定されたりしている可能性があることに注意してください。  これらの想定は、<xref:System.Threading.ThreadAbortException> を含むように更新することが必要な場合があります。

すべての例外をキャッチするようになっているすべての場所を、スローされると予想される特定の種類の例外だけをキャッチするように変更することを検討してください (文字列書式設定メソッドからの <xref:System.FormatException> など)。  このようにすると、catch ブロックが予期しない例外で実行されることがなくなり、予期しない例外をキャッチすることでコードのバグが非表示にされなくなります。  一般的なルールとして、ライブラリ コードでは例外を処理しないでください (例外をキャッチする必要があるコードが、呼び出しているコード内の設計上の欠陥を示す可能性があります)。  場合によっては、例外をキャッチし、異なる例外の種類をスローすることで、より多くのデータを提供できることがあります。  このような場合は入れ子になった例外を使い、エラーの実際の原因を新しい例外の <xref:System.Exception.InnerException%2A> プロパティに格納します。

#### <a name="code-analysis-rule"></a>コード分析規則

すべてのオブジェクトまたはすべての例外をキャッチしているマネージド コード内のすべての catch ブロックを確認します。  でC#は、これは `catch` {} と `catch(Exception)` {}の両方にフラグをかけることを意味します。  例外の種類を非常に限定的にすることを考えます。または、コードを調べて、予期しない例外の種類をキャッチした場合に不適切に動作しないことを確認します。

### <a name="do-not-assume-a-managed-thread-is-a-win32-thread--it-is-a-fiber"></a>マネージスレッドが Win32 スレッドであることを想定しないでください。

マネージド スレッド ローカル記憶域は動作しますが、アンマネージド スレッド ローカル記憶域を使うこと、またはコードが現在のオペレーティング システム スレッドで再び実行されると想定することはできません。 スレッドのロケールなどの設定を変更しないでください。 プラットフォーム呼び出しでは `InitializeCriticalSection` または `CreateMutex` を呼び出さないでください。これらでは、ロックを開始したオペレーティング システム スレッドがロックを終了する必要があります。 ファイバーを使うとこれは該当しないので、Win32 のクリティカル セクションおよびミューテックスを SQL で直接使うことはできません。  マネージド <xref:System.Threading.Mutex> クラスはこれらのスレッドのアフィニティに関する注意事項を処理しないことに注意してください。

マネージド スレッド ローカル記憶域やスレッドの現在の UI カルチャなど、マネージド <xref:System.Threading.Thread> オブジェクトのほとんどの状態は安全に使うことができます。 また、<xref:System.ThreadStaticAttribute> を使うこともできます。これは、既存の静的変数の値を、現在のマネージド スレッドによってのみアクセスできるようにします (これは、CLR でファイバー ローカル記憶域を行うもう 1 つの方法です)。 プログラミング モデルの理由から、SQL で実行しているときは、スレッドの現在のカルチャを変更できません。

#### <a name="code-analysis-rule"></a>コード分析規則

SQL Server はファイバー モードで実行します。スレッド ローカル記憶域は使わないでください。 `TlsAlloc`、`TlsFree`、`TlsGetValue`、および `TlsSetValue.` のプラットフォーム呼び出しを行わないでください。

### <a name="let-sql-server-handle-impersonation"></a>偽装 SQL Server 処理する

偽装はスレッド レベルで動作し、SQL が実行できるのはファイバー モードなので、マネージド コードはユーザーを偽装してはならず、`RevertToSelf` を呼び出してはなりません。

#### <a name="code-analysis-rule"></a>コード分析規則

SQL Server に偽装を処理させるようにします。 `RevertToSelf`、`ImpersonateAnonymousToken`、`DdeImpersonateClient`、`ImpersonateDdeClientWindow`、`ImpersonateLoggedOnUser`、`ImpersonateNamedPipeClient`、`ImpersonateSelf`、`RpcImpersonateClient`、`RpcRevertToSelf`、`RpcRevertToSelfEx`、`SetThreadToken` は使わないでください。

### <a name="do-not-call-threadsuspend"></a>Thread:: Suspend を呼び出さないでください

スレッドを一時停止させる機能は単純な操作のように見えますが、デッドロックが発生する可能性があります。  ロックを保持しているスレッドが第 2 のスレッドによって一時停止された後、第 2 のスレッドが同じロックを取得しようとすると、デッドロックが発生します。  現在、<xref:System.Threading.Thread.Suspend%2A> はセキュリティ、クラスの読み込み、リモート処理、およびリフレクションを妨げる可能性があります。

#### <a name="code-analysis-rule"></a>コード分析規則

<xref:System.Threading.Thread.Suspend%2A> を呼び出さないでください。 代わりに <xref:System.Threading.Semaphore> や <xref:System.Threading.ManualResetEvent> などの実際の同期プリミティブの使用を検討してください。

### <a name="protect-critical-operations-with-constrained-execution-regions-and-reliability-contracts"></a>制約された実行領域と信頼性のコントラクトで重要な操作を保護する

共有状態を更新する複雑な操作、または完全に成功するか完全に失敗することが確定的に必要な複雑な操作を実行するときは、制約された実行領域 (CER) によって保護されるようにします。 これにより、スレッドの突然の中止や突然の <xref:System.AppDomain> のアンロードなど、すべてのケースでコードが実行されることが保証されます。

CER は、<xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> の呼び出しが直前にある特定の `try/finally` ブロックです。

このようにすると、`try` ブロックを実行する前に finally ブロック内のすべてのコードを準備するよう、Just-In-Time コンパイラに指示されます。 これにより、finally ブロック内のコードがすべてのケースでビルドされて実行されることが保証されます。 CER では空の `try` ブロックを使うことが珍しくありません。 CER を使うと、非同期スレッドの中止およびメモリ不足例外に対して保護されます。 非常に深いコードに対するスタック オーバーフローを追加で処理する CER の形式については、「<xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A>」をご覧ください。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.ConstrainedExecution>
- [SQL Server プログラミングとホスト保護属性](sql-server-programming-and-host-protection-attributes.md)
