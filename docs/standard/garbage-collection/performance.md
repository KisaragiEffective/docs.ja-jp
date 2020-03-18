---
title: ガベージ コレクションとパフォーマンス
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, troubleshooting
- garbage collection, performance
ms.assetid: c203467b-e95c-4ccf-b30b-953eb3463134
ms.openlocfilehash: 72cf742aae26f9441229b355dc6e70da7a5fc9cd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75900579"
---
# <a name="garbage-collection-and-performance"></a>ガベージ コレクションとパフォーマンス

ここでは、ガベージ コレクションおよびメモリ使用に関連する問題について説明します。 マネージド ヒープに関する問題について取り上げ、ガベージ コレクションによるアプリケーションに対する影響を最小限に抑える方法を説明します。 問題を調査するために使用できる手順のリンクを問題ごとに示してあります。

## <a name="performance-analysis-tools"></a>パフォーマンス分析ツール

以下のセクションでは、メモリ使用とガベージ コレクションに関する問題を調査するために使用できるツールについて説明します。 このトピックの後半で説明する[手順](#performance-check-procedures)では、これらのツールを使用します。

### <a name="memory-performance-counters"></a>メモリ パフォーマンス カウンター

パフォーマンス カウンターを使用してパフォーマンス データを収集できます。 手順については、「[ランタイム プロファイリング](../../../docs/framework/debug-trace-profile/runtime-profiling.md)」を参照してください。 ガベージ コレクターに関する情報は、.NET CLR Memory カテゴリのパフォーマンス カウンターから提供されます。詳細については、「[.NET Framework のパフォーマンス カウンター](../../../docs/framework/debug-trace-profile/performance-counters.md)」を参照してください。

### <a name="debugging-with-sos"></a>SOS キーを使ったデバッグ

[Windows デバッガー (WinDbg)](/windows-hardware/drivers/debugger/index) を使用して、マネージド ヒープのオブジェクトを検査できます。

WinDbg をインストールするには、「[Download Debugging Tools for Windows](/windows-hardware/drivers/debugger/debugger-download-tools)」 (Windows 用デバッグ ツールのダウンロード) ページから Windows 用デバッグ ツールをインストールします。

### <a name="garbage-collection-etw-events"></a>ガベージ コレクション ETW イベント

Windows イベント トレーシング (ETW) は、.NET Framework のプロファイリングとデバッグのサポートを補足するトレース システムです。 .NET Framework 4 以降では、[ガベージ コレクション ETW イベント](../../../docs/framework/performance/garbage-collection-etw-events.md)により、統計的な観点からマネージド ヒープを分析するために役立つ情報が得られるようになりました。 たとえば、ガベージ コレクションの発生前に発生する `GCStart_V1` イベントでは、次の情報が提供されます。

- 収集されるオブジェクトのジェネレーション

- ガベージ コレクションが発生した理由

- ガベージ コレクションの種類 (同時実行または非同時実行)

ETW イベント ログは効率的であり、ガベージ コレクションに関連するパフォーマンスの問題がマスクされることはありません。 ETW イベントと組み合わせてプロセス独自のイベントを提供できます。 ログを記録すると、アプリケーションのイベントとガベージ コレクションのイベントの両方を相互に関連付けて、ヒープの問題がいつどのようにして発生するかを特定できます。 たとえば、サーバー アプリケーションでは、クライアント要求の開始時と終了時にイベントを提供することができます。

### <a name="the-profiling-api"></a>プロファイル API

共通言語ランタイム (CLR) のプロファイル インターフェイスは、ガベージ コレクションの影響を受けたオブジェクトに関する詳細な情報を提供します。 プロファイラーでは、ガベージ コレクションの開始時と終了時に通知を受け取り、 各ジェネレーションにおけるオブジェクトの識別情報など、マネージド ヒープのオブジェクトに関するレポートを提供することができます。 詳細については、「[プロファイルの概要](../../../docs/framework/unmanaged-api/profiling/profiling-overview.md)」を参照してください。

プロファイラーでは包括的な情報を提供できますが、 複雑なプロファイラーを使用すると、アプリケーションの動作が変更される可能性があります。

### <a name="application-domain-resource-monitoring"></a>アプリケーション ドメインのリソース監視

.NET Framework 4 以降では、アプリケーション ドメインのリソース監視 (ARM) によって、ホストで CPU とメモリのアプリケーション ドメインによる使用状況を監視できるようになります。 詳細については、「[アプリケーション ドメインのリソース監視](../../../docs/standard/garbage-collection/app-domain-resource-monitoring.md)」を参照してください。

## <a name="troubleshooting-performance-issues"></a>パフォーマンスに関する問題のトラブルシューティング

最初に、[本当にガベージ コレクションの問題なのかどうかを確認します](#IsGC)。 ガベージ コレクションの問題であることがわかったら、次の一覧から問題を選択してトラブルシューティングを行います。

- [メモリ不足の例外がスローされる](#Issue_OOM)

- [プロセスによるメモリ使用量が多すぎる](#Issue_TooMuchMemory)

- [ガベージ コレクターによるオブジェクトの解放に時間がかかる](#Issue_NotFastEnough)

- [マネージド ヒープが過度に断片化される](#Issue_Fragmentation)

- [ガベージ コレクションの一時停止が長すぎる](#Issue_LongPauses)

- [ジェネレーション 0 が大きすぎる](#Issue_Gen0)

- [ガベージ コレクションの実行時の CPU 使用率が高すぎる](#Issue_HighCPU)

<a name="Issue_OOM"></a>

### <a name="issue-an-out-of-memory-exception-is-thrown"></a>問題:メモリ不足の例外がスローされる

<xref:System.OutOfMemoryException> マネージド例外がスローされる正当な状況としては、次の 2 つがあります。

- 仮想メモリが不足している。

  ガベージ コレクターは、サイズがあらかじめ決められたセグメント単位でシステムのメモリを割り当てます。 割り当てで追加のセグメントが必要になっとときに、プロセスの仮想メモリ空間に連続する空きブロックが残っていなければ、マネージド ヒープの割り当ては失敗します。

- 十分な物理メモリを確保できない。

|パフォーマンス チェック|
|------------------------|
|[メモリ不足の例外がマネージド例外かどうかを確認する。](#OOMIsManaged)<br /><br /> [予約できる仮想メモリの量を確認する。](#GetVM)<br /><br /> [十分な物理メモリがあるかどうかを確認する。](#Physical)|

例外が正当なものでないと判断した場合は、マイクロソフト カスタマー サポート サービスにお問い合わせください。その際、次の情報をご連絡ください。

- メモリ不足のマネージド例外を含む履歴

- 完全なメモリ ダンプ

- メモリ不足の例外が正当なものでないことを証明するデータ (仮想メモリまたは物理メモリに問題がないことを示すデータなど)

<a name="Issue_TooMuchMemory"></a>

### <a name="issue-the-process-uses-too-much-memory"></a>問題:プロセスによるメモリ使用量が多すぎる

一般的な前提として、メモリ使用量が多すぎる場合については、Windows タスク マネージャーの **[パフォーマンス]** タブのメモリ使用量の表示で確認できます。 ただし、この表示はワーキング セットに関するもので、仮想メモリの使用量に関する情報ではありません。

マネージド ヒープが原因であると判断した場合は、一定の期間にわたってマネージド ヒープを測定し、パターンを確認する必要があります。

マネージド ヒープが原因でないと判断した場合は、ネイティブ デバッグを使用する必要があります。

|パフォーマンス チェック|
|------------------------|
|[予約できる仮想メモリの量を確認する。](#GetVM)<br /><br /> [マネージド ヒープでコミットしているメモリの量を確認する。](#ManagedHeapCommit)<br /><br /> [マネージド ヒープで予約されているメモリの量を確認する。](#ManagedHeapReserve)<br /><br /> [ジェネレーション 2 の大きいオブジェクトを確認する。](#ExamineGen2)<br /><br /> [オブジェクトへの参照を確認する。](#ObjRef)|

<a name="Issue_NotFastEnough"></a>

### <a name="issue-the-garbage-collector-does-not-reclaim-objects-fast-enough"></a>問題:ガベージ コレクターによるオブジェクトの解放に時間がかかる

ガベージ コレクションでオブジェクトが通常どおりに解放されていないように見える場合は、それらのオブジェクトに対する強い参照がないかどうかを確認する必要があります。

この問題は、死んだ状態のオブジェクトを含むジェネレーションに対してガベージ コレクションが行われていない場合にも発生します。死んだ状態のオブジェクトは、そのオブジェクトのファイナライザーが実行されていないことを示します。 たとえば、シングルスレッド アパートメント (STA) のアプリケーションを実行している場合に、ファイナライザー キューを処理するスレッドがファイナライザーの呼び出しに失敗すると、この状態になる可能性があります。

|パフォーマンス チェック|
|------------------------|
|[オブジェクトへの参照を確認する。](#ObjRef)<br /><br /> [ファイナライザーが実行されたかどうかを確認する。](#Induce)<br /><br /> [終了待機中のオブジェクトがないかどうかを確認する。](#Finalize)|

<a name="Issue_Fragmentation"></a>

### <a name="issue-the-managed-heap-is-too-fragmented"></a>問題:マネージド ヒープが過度に断片化される

断片化レベルは、ジェネレーションに割り当てられたメモリの合計に占める空き領域の割合として計算されます。 ジェネレーション 2 の場合、許容される断片化レベルは 20% 以下です。 ジェネレーション 2 は非常に大きくなる可能性があるため、断片化の割合の方が絶対値より重要になります。

ジェネレーション 0 は、新しいオブジェクトが割り当てられるジェネレーションなので、空き領域が多くても問題はありません。

断片化は常に大きなオブジェクト ヒープで発生します。大きなオブジェクト ヒープは圧縮されないからです。 隣接する空きオブジェクトは、大きなオブジェクトの割り当て要求を満たすために必然的に 1 つの領域にまとめられます。

断片化が問題になるのは、ジェネレーション 1 とジェネレーション 2 です。 これらのジェネレーションで、ガベージ コレクションの終了後に大量の空き領域ある場合は、アプリケーションのオブジェクトの使用方法を変更する必要がある可能性があります。長期間のオブジェクトの有効期間を再評価することを検討してください。

オブジェクトの固定が過度に行われていると断片化レベルが高くなることがあります。 断片化レベルが高い場合は、固定されているオブジェクトが多すぎる可能性があります。

仮想メモリの断片化によってガベージ コレクターがセグメントを追加できなくなっている場合、次のような原因が考えられます。

- 多数の小さなアセンブリの読み込みとアンロードが頻繁に行われている。

- アンマネージ コードとの相互運用時に保持される COM オブジェクトへの参照が多すぎる。

- 大きな一時オブジェクトが作成されているために、大きなオブジェクト ヒープでヒープ セグメントの割り当てと解放が頻繁に行われている。

  アプリケーションで CLR をホストする際には、セグメントを保持するようにガベージ コレクターに要求することができます。 これにより、セグメント割り当ての頻度が減少します。 そのためには、[STARTUP_FLAGS 列挙型](../../../docs/framework/unmanaged-api/hosting/startup-flags-enumeration.md)の STARTUP_HOARD_GC_VM フラグを使用します。

|パフォーマンス チェック|
|------------------------|
|[マネージド ヒープの空き領域の容量を確認する。](#Fragmented)<br /><br /> [固定されたオブジェクトの数を確認する。](#Pinned)|

正当な理由もないのに断片化が発生していると思われる場合は、マイクロソフト カスタマー サポート サービスにお問い合わせください。

<a name="Issue_LongPauses"></a>

### <a name="issue-garbage-collection-pauses-are-too-long"></a>問題:ガベージ コレクションの一時停止が長すぎる

ガベージ コレクションはソフト リアルタイムで動作するため、アプリケーションはある程度の一時停止に耐えられなければなりません。 ソフト リアルタイムの基準では、95% の操作が時間どおりに完了する必要があります。

同時実行ガベージ コレクションでは、コレクションの実行中もマネージド スレッドを実行できるため、一時停止は最小限に抑えられます。

短期ガベージ コレクション (ジェネレーション 0 および 1) は数ミリ秒しかかからないため、一般に一時停止を減らすことは不可能です。 一方、ジェネレーション 2 のコレクションでは、アプリケーションによる割り当て要求のパターンを変更することによって一時停止を減らすことができます。

より正確な方法として、[ガベージ コレクション ETW イベント](../../../docs/framework/performance/garbage-collection-etw-events.md)を使用することもできます。 一連のイベントにタイム スタンプを追加して区別することにより、コレクションのタイミングを特定できます。 コレクションのシーケンス全体には、実行エンジンの中断、ガベージ コレクション自体、および実行エンジンの再開が含まれます。

[ガベージ コレクションの通知](../../../docs/standard/garbage-collection/notifications.md)を使用すると、サーバーでジェネレーション 2 のコレクションが発生しそうかどうか、要求を別のサーバーに再ルーティングすることで一時停止の問題を緩和できるかどうかを確認できます。

|パフォーマンス チェック|
|------------------------|
|[ガベージ コレクションの継続時間を確認する。](#TimeInGC)<br /><br /> [ガベージ コレクションが発生した原因を確認する。](#Triggered)|

<a name="Issue_Gen0"></a>

### <a name="issue-generation-0-is-too-big"></a>問題:ジェネレーション 0 が大きすぎる

64 ビット システムでは、ジェネレーション 0 のオブジェクトの数が増える傾向があります。ワークステーションのガベージ コレクションではなくサーバーのガベージ コレクションを使用している場合は特にその傾向が強くなります。 それらの環境では、ジェネレーション 0 のガベージ コレクションをトリガーするしきい値が高いので、ジェネレーション 0 のコレクションが非常に大きくなる可能性があるためです。 アプリケーションで、ガベージ コレクションがトリガーされる前により多くのメモリを割り当てると、パフォーマンスが向上します。

<a name="Issue_HighCPU"></a>

### <a name="issue-cpu-usage-during-a-garbage-collection-is-too-high"></a>問題:ガベージ コレクションの実行時の CPU 使用率が高すぎる

ガベージ コレクションの実行時には CPU 使用率が高くなります。 ガベージ コレクションに大量の処理時間が費やされている場合は、コレクションの発生頻度が高すぎるか、コレクションの継続時間が長すぎます。 マネージド ヒープに対するオブジェクトの割り当ての速度を上げるとガベージ コレクションの発生頻度が高くなります。 割り当ての速度を下げるとガベージ コレクションの発生頻度が低くなります。

割り当ての速度を監視するには、`Allocated Bytes/second` パフォーマンス カウンターを使用します。 詳細については、「[.NET Framework のパフォーマンス カウンター](../../../docs/framework/debug-trace-profile/performance-counters.md)」を参照してください。

コレクションの継続時間は、主に、割り当て後に残ったオブジェクトの数によって決まります。 コレクションの対象となるオブジェクトが数多く残っていると、ガベージ コレクターが大量のメモリを処理しなければならなくなります。 残存オブジェクトの圧縮には時間がかかります。 コレクションの実行中に処理されたオブジェクトの数を確認するには、デバッガーで特定のジェネレーションのガベージ コレクションの終了時にブレークポイントを設定します。

|パフォーマンス チェック|
|------------------------|
|[CPU の使用率が高いのはガベージ コレクションのためかどうかを確認する。](#HighCPU)<br /><br /> [ガベージ コレクションの終了時にブレークポイントを設定する。](#GenBreak)|

## <a name="troubleshooting-guidelines"></a>トラブルシューティングのガイドライン

ここでは、調査を開始するときに考慮する必要があるガイドラインについて説明します。

### <a name="workstation-or-server-garbage-collection"></a>ワークステーションのガベージ コレクションかサーバーのガベージ コレクションか

使用しているガベージ コレクションの種類が正しいかどうかを確認します。 アプリケーションで複数のスレッドおよびオブジェクト インスタンスを使用する場合は、ワークステーションのガベージ コレクションではなくサーバーのガベージ コレクションを使用します。 サーバーのガベージ コレクションは複数のスレッドで動作しますが、ワークステーションのガベージ コレクションでは、アプリケーションの複数のインスタンスでそれぞれ専用のガベージ コレクション スレッドを実行する必要があるため、CPU 時間の競合が発生します。

負荷が低く、バックグラウンドでタスクを実行することの少ないアプリケーション (サービスなど) では、同時実行ガベージ コレクションを無効にしてワークステーションのガベージ コレクションを使用できます。

### <a name="when-to-measure-the-managed-heap-size"></a>いつマネージド ヒープのサイズを測定するか

プロファイラーを使用しない場合、パフォーマンスの問題を効果的に診断するには、一貫した測定パターンを確立する必要があります。 スケジュールを確立する際の考慮事項を以下に示します。

- ジェネレーション 2 のガベージ コレクションの後に測定する場合は、マネージド ヒープ全体からガベージ (死んだ状態のオブジェクト) がなくなっています。

- ジェネレーション 0 のガベージ コレクションの直後に測定する場合は、ジェネレーション 1 と 2 のオブジェクトのコレクションはまだ行われていません。

- ガベージ コレクションの直前に測定する場合は、どのくらいの割り当てが行われるとガベージ コレクションが開始されるのかが測定されます。

- ガベージ コレクションの実行中に測定するのには問題があります。ガベージ コレクターのデータ構造が走査可能な状態になっていないので、完全な結果が得られない可能性があるためです。 これは仕様に基づく制限事項です。

- 同時実行ガベージ コレクションを有効にしてワークステーションのガベージ コレクションを使用している場合は、解放されたオブジェクトが圧縮されないため、ヒープ サイズが変わらなかったり大きくなっていたりすることがあります (断片化のために大きく見えることがあります)。

- ジェネレーション 2 の同時実行ガベージ コレクションは、物理メモリの負荷が高すぎると延期されます。

マネージド ヒープを測定するためのブレークポイントを設定する方法を以下に示します。

<a name="GenBreak"></a>

#### <a name="to-set-a-breakpoint-at-the-end-of-garbage-collection"></a>ガベージ コレクションの終了時にブレークポイントを設定するには

- SOS デバッガー拡張が読み込まれた WinDbg で、次のコマンドを入力します。

  **bp mscorwks!WKS::GCHeap::RestartEE "j (dwo(mscorwks!WKS::GCHeap::GcCondemnedGeneration)==2) 'kb';'g'"**

  **GcCondemnedGeneration** は、目的のジェネレーションに設定します。 このコマンドにはプライベート シンボルが必要です。

  これにより、ジェネレーション 2 のオブジェクトがガベージ コレクションのために解放された後に **RestartEE** が実行されると、実行が中断されます。

  サーバーのガベージ コレクションでは、**RestartEE** を呼び出すスレッドは 1 つだけなので、ジェネレーション 2 のガベージ コレクションの実行中に 1 回だけブレークポイントが発生します。

## <a name="performance-check-procedures"></a>パフォーマンス チェックの手順

ここでは、パフォーマンスの問題の原因を切り分けるための以下の手順について説明します。

- [問題の原因がガベージ コレクションにあるかどうかを確認する。](#IsGC)

- [メモリ不足の例外がマネージド例外かどうかを確認する。](#OOMIsManaged)

- [予約できる仮想メモリの量を確認する。](#GetVM)

- [十分な物理メモリがあるかどうかを確認する。](#Physical)

- [マネージド ヒープでコミットしているメモリの量を確認する。](#ManagedHeapCommit)

- [マネージド ヒープで予約されているメモリの量を確認する。](#ManagedHeapReserve)

- [ジェネレーション 2 の大きいオブジェクトを確認する。](#ExamineGen2)

- [オブジェクトへの参照を確認する。](#ObjRef)

- [ファイナライザーが実行されたかどうかを確認する。](#Induce)

- [終了待機中のオブジェクトがないかどうかを確認する。](#Finalize)

- [マネージド ヒープの空き領域の容量を確認する。](#Fragmented)

- [固定されたオブジェクトの数を確認する。](#Pinned)

- [ガベージ コレクションの継続時間を確認する。](#TimeInGC)

- [ガベージ コレクションが発生した原因を確認する。](#Triggered)

- [CPU の使用率が高いのはガベージ コレクションのためかどうかを確認する。](#HighCPU)

<a name="IsGC"></a>

### <a name="to-determine-whether-the-problem-is-caused-by-garbage-collection"></a>問題の原因がガベージ コレクションにあるかどうかを確認するには

- 次の 2 つのメモリ パフォーマンス カウンターを調べます。

  - **% Time in GC**。 前回のガベージ コレクション サイクルの後にガベージ コレクションの実行に費やされた経過時間の割合を表示します。 このカウンターを使用すると、ガベージ コレクターがマネージド ヒープの領域を確保するために費やしている時間が長すぎないかどうかを確認できます。 ガベージ コレクションに費やされている時間が比較的短い場合は、マネージド ヒープ以外のリソースに問題があると考えられます。 このカウンターは、同時実行ガベージ コレクションやバックグラウンド ガベージ コレクションでは正確な値が得られない可能性があります。

  - **# Total committed Bytes**。 ガベージ コレクターによって現在コミットされている仮想メモリの量を表示します。 このカウンターを使用すると、アプリケーションが使用しているメモリのうちガベージ コレクターによって消費されているメモリが多すぎないかどうかを確認できます。

  ほとんどのメモリ パフォーマンス カウンターは、各ガベージ コレクションの終了時に更新されます。 そのため、情報を得ようとしている時点の状態が反映されていない場合もあります。

<a name="OOMIsManaged"></a>

### <a name="to-determine-whether-the-out-of-memory-exception-is-managed"></a>メモリ不足の例外がマネージド例外かどうかを確認するには

1. SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、**pe** (print exception) コマンドを入力します。

    **!pe**

    マネージド例外の場合は、次の例のように、<xref:System.OutOfMemoryException> が例外の種類として表示されます。

    ```console
    Exception object: 39594518
    Exception type: System.OutOfMemoryException
    Message: <none>
    InnerException: <none>
    StackTrace (generated):
    ```

2. 出力に例外が明記されていない場合は、メモリ不足の例外が発生したスレッドを特定する必要があります。 デバッガーで次のコマンドを入力して、すべてのスレッドとその呼び出し履歴を表示します。

    **~\*kb**

    履歴に例外の呼び出しが含まれているスレッドは、`RaiseTheException` 引数によって示されます。 これはマネージド例外オブジェクトです。

    ```console
    28adfb44 7923918f 5b61f2b4 00000000 5b61f2b4 mscorwks!RaiseTheException+0xa0
    ```

3. 次のコマンドを使用して、入れ子になった例外をダンプします。

    **!pe -nested**

    例外が見つからない場合、そのメモリ不足の例外は、アンマネージ コードで発生した例外です。

<a name="GetVM"></a>

### <a name="to-determine-how-much-virtual-memory-can-be-reserved"></a>予約できる仮想メモリの量を確認するには

- SOS デバッガー拡張が読み込まれた WinDbg で次のコマンドを入力して、最も大きな空き領域を取得します。

  **!address -summary**

  次の例のように、最も大きな空き領域が表示されます。

  ```console
  Largest free region: Base 54000000 - Size 0003A980
  ```

  この例では、最も大きな空き領域のサイズは約 24000 KB (16 進形式では 3A980) です。 これは、ガベージ コレクターのセグメントに必要なサイズよりはるかに小さいサイズです。

  \- または -

- **vmstat** コマンドを使用します。

  **!vmstat**

  MAXIMUM 列の最大値が最も大きな空き領域です。以下に例を示します。

  ```console
  TYPE        MINIMUM   MAXIMUM     AVERAGE   BLK COUNT   TOTAL
  ~~~~        ~~~~~~~   ~~~~~~~     ~~~~~~~   ~~~~~~~~~~  ~~~~
  Free:
  Small       8K        64K         46K       36          1,671K
  Medium      80K       864K        349K      3           1,047K
  Large       1,384K    1,278,848K  151,834K  12          1,822,015K
  Summary     8K        1,278,848K  35,779K   51          1,824,735K
  ```

<a name="Physical"></a>

### <a name="to-determine-whether-there-is-enough-physical-memory"></a>十分な物理メモリがあるかどうかを確認するには

1. Windows タスク マネージャーを起動します。

2. **[パフォーマンス]** タブで、コミットの値を確認します (Windows 7 では **[システム]** の **[コミット (KB)]** )。

    **[合計]** が **[制限値]** に近い場合は、物理メモリが不足しています。

<a name="ManagedHeapCommit"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-is-committing"></a>マネージド ヒープでコミットしているメモリの量を確認するには

- マネージド ヒープでコミットしているバイト数を確認するには、`# Total committed bytes` メモリ パフォーマンス カウンターを使用します。 ガベージ コレクターは、セグメントのチャンクを必要に応じてコミットします。すべてを同時にコミットするのではありません。

  > [!NOTE]
  > `# Bytes in all Heaps` パフォーマンス カウンターは使用しないでください。このパフォーマンス カウンターによって表されるのは、マネージド ヒープの実際のメモリ使用量ではありません。 この値にはジェネレーションのサイズが含まれますが、それは、実質的にはジェネレーションのしきい値 (ジェネレーションがオブジェクトでいっぱいになった場合にガベージ コレクションが発生するサイズ) です。 したがって、この値は、通常は 0 になります。

<a name="ManagedHeapReserve"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-reserves"></a>マネージド ヒープで予約されているメモリの量を確認するには

- `# Total reserved bytes` メモリ パフォーマンス カウンターを使用します。

  ガベージ コレクターはメモリをセグメント単位で予約します。セグメントの開始位置を特定するには **eeheap** コマンドを使用します。

  > [!IMPORTANT]
  > ガベージ コレクターが各セグメントに割り当てるメモリ量を判別することは可能ですが、セグメント サイズは実装に固有であり、定期的な更新プログラムによる場合を含め、いつでも変更されることがあります。 アプリでは、セグメント サイズを推測することや、特定のセグメント サイズに依存することを絶対に避けてください。また、セグメントの割り当てに使用可能なメモリの量を構成しようとしてもなりません。

- SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、次のコマンドを入力します。

  **!eeheap -gc**

  この結果は次のようになります。

  ```console
  Number of GC Heaps: 2
  ------------------------------
  Heap 0 (002db550)
  generation 0 starts at 0x02abe29c
  generation 1 starts at 0x02abdd08
  generation 2 starts at 0x02ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  02ab0000 02ab0038  02aceff4 0x0001efbc(126908)
  Large object heap starts at 0x0aab0038
    segment    begin allocated     size
  0aab0000 0aab0038  0aab2278 0x00002240(8768)
  Heap Size   0x211fc(135676)
  ------------------------------
  Heap 1 (002dc958)
  generation 0 starts at 0x06ab1bd8
  generation 1 starts at 0x06ab1bcc
  generation 2 starts at 0x06ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  06ab0000 06ab0038  06ab3be4 0x00003bac(15276)
  Large object heap starts at 0x0cab0038
    segment    begin allocated     size
  0cab0000 0cab0038  0cab0048 0x00000010(16)
  Heap Size    0x3bbc(15292)
  ------------------------------
  GC Heap Size   0x24db8(150968)
  ```

  "segment" によって示されるアドレスがセグメントの開始アドレスです。

<a name="ExamineGen2"></a>

### <a name="to-determine-large-objects-in-generation-2"></a>ジェネレーション 2 の大きいオブジェクトを確認するには

- SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、次のコマンドを入力します。

  **!dumpheap –stat**

  **dumpheap** は、マネージド ヒープが大きいと完了までにしばらくかかります。

  最も多くの領域を使用しているオブジェクトは出力の最後の数行に表示されるため、そこを分析します。 次に例を示します。

  ```console
  2c6108d4   173712     14591808 DevExpress.XtraGrid.Views.Grid.ViewInfo.GridCellInfo
  00155f80      533     15216804      Free
  7a747c78   791070     15821400 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700930     19626040 System.Collections.Specialized.ListDictionary
  2c64e36c    78644     20762016 DevExpress.XtraEditors.ViewInfo.TextEditViewInfo
  79124228   121143     29064120 System.Object[]
  035f0ee4    81626     35588936 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    40182     90664128 System.Collections.Hashtable+bucket[]
  790fa3e0  3154024    137881448 System.String
  Total 8454945 objects
  ```

  最後の行のオブジェクトは文字列で、最も多くの領域を占有しています。 したがって、アプリケーションで文字列オブジェクトを最適化する方法を調べます。 サイズが 150 ～ 200 バイトの文字列を表示するには、次のコマンドを入力します。

  **!dumpheap -type System.String -min 150 -max 200**

  結果の例を次に示します。

  ```console
  Address  MT           Size  Gen
  1875d2c0 790fa3e0      152    2 System.String HighlightNullStyle_Blotter_PendingOrder-11_Blotter_PendingOrder-11
  …
  ```

  ID に文字列の代わりに整数を使用すると、効率を改善できます。 同じ文字列が何千回も繰り返し使用されている場合は、文字列インターンの使用を検討します。 文字列インターンの詳細については、<xref:System.String.Intern%2A?displayProperty=nameWithType> メソッドのリファレンス トピックを参照してください。

<a name="ObjRef"></a>

### <a name="to-determine-references-to-objects"></a>オブジェクトへの参照を確認するには

- SOS デバッガー拡張が読み込まれた WinDbg で次のコマンドを入力して、オブジェクトへの参照を表示します。

  **!gcroot**

  `-or-`

- 特定のオブジェクトの参照を確認するには、アドレスを含めます。

  **!gcroot 1c37b2ac**

  履歴で見つかるルートは誤検出である可能性があります。 詳細については、コマンド `!help gcroot` を使用してください。

  ```console
  ebx:Root:19011c5c(System.Windows.Forms.Application+ThreadContext)->
  19010b78(DemoApp.FormDemoApp)->
  19011158(System.Windows.Forms.PropertyStore)->
  … [omitted]
  1c3745ec(System.Data.DataTable)->
  1c3747a8(System.Data.DataColumnCollection)->
  1c3747f8(System.Collections.Hashtable)->
  1c376590(System.Collections.Hashtable+bucket[])->
  1c376c98(System.Data.DataColumn)->
  1c37b270(System.Data.Common.DoubleStorage)->
  1c37b2ac(System.Double[])
  Scan Thread 0 OSTHread 99c
  Scan Thread 6 OSTHread 484
  ```

  **gcroot** コマンドは、完了までに時間がかかることがあります。 ガベージ コレクションによって解放されないオブジェクトはライブ オブジェクトです。 したがって、そのオブジェクトを直接的または間接的に参照しているルートがあるため、**gcroot** でそのオブジェクトへのパス情報が返されます。 返されたグラフを調べて、それらのオブジェクトがまだ参照されている理由を確認する必要があります。

<a name="Induce"></a>

### <a name="to-determine-whether-a-finalizer-has-been-run"></a>ファイナライザーが実行されたかどうかを確認するには

- 次のコードを含むテスト プログラムを実行します。

  ```csharp
  GC.Collect();
  GC.WaitForPendingFinalizers();
  GC.Collect();
  ```

  このテストで問題が解決される場合は、ファイナライザーが実行されていないためにガベージ コレクターによって解放されないオブジェクトがあったことになります。 <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType> メソッドを使用すると、ファイナライザーがタスクを完了できるようになるため、問題が解決されます。

<a name="Finalize"></a>

### <a name="to-determine-whether-there-are-objects-waiting-to-be-finalized"></a>終了待機中のオブジェクトがないかどうかを確認するには

1. SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、次のコマンドを入力します。

    **!finalizequeue**

    終了準備が完了しているオブジェクトの数を確認します。 その数が多い場合は、それらのファイナライザーが実行されない理由、または実行が遅れている理由を調べる必要があります。

2. スレッドの出力を取得するには、次のコマンドを入力します。

    **threads -special**

    次のような出力が表示されます。

    ```console
       OSID     Special thread type
    2    cd0    DbgHelper
    3    c18    Finalizer
    4    df0    GC SuspendEE
    ```

    ファイナライザー スレッドは、現在実行されているファイナライザーを示します (存在する場合)。 実行中のファイナライザー スレッドが存在しない場合は、処理の開始を通知するイベントを待機しています。 ファイナライザー スレッドは、ほとんどの場合この状態にあります。THREAD_HIGHEST_PRIORITY で実行されるので、実行するファイナライザーがあればすぐに実行されるためです。

<a name="Fragmented"></a>

### <a name="to-determine-the-amount-of-free-space-in-the-managed-heap"></a>マネージド ヒープの空き領域の容量を確認するには

- SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、次のコマンドを入力します。

  **!dumpheap -type Free -stat**

  このコマンドは、次の例に示すように、マネージド ヒープのすべての空きオブジェクトの合計サイズを表示します。

  ```console
  total 230 objects
  Statistics:
        MT    Count    TotalSize Class Name
  00152b18      230     40958584      Free
  Total 230 objects
  ```

- ジェネレーション 0 の空き領域を確認するには、次のコマンドを入力して、ジェネレーション別のメモリ消費情報を表示します。

  **!eeheap -gc**

  次のような出力が表示されます。 最後の行には短期セグメントが表示されています。

  ```console
  Heap 0 (0015ad08)
  generation 0 starts at 0x49521f8c
  generation 1 starts at 0x494d7f64
  generation 2 starts at 0x007f0038
  ephemeral segment allocation context: none
  segment  begin     allocated  size
  00178250 7a80d84c  7a82f1cc   0x00021980(137600)
  00161918 78c50e40  78c7056c   0x0001f72c(128812)
  007f0000 007f0038  047eed28   0x03ffecf0(67103984)
  3a120000 3a120038  3a3e84f8   0x002c84c0(2917568)
  46120000 46120038  49e05d04   0x03ce5ccc(63855820)
  ```

- ジェネレーション 0 によって使用されている領域を計算します。

  **?49e05d04-0x49521f8c**

  この結果は次のようになります。 ジェネレーション 0 は約 9 MB です。

  ```console
  Evaluate expression: 9321848 = 008e3d78
  ```

- 次のコマンドは、ジェネレーション 0 の範囲にある空き領域をダンプします。

  **!dumpheap -type Free -stat 0x49521f8c 49e05d04**

  この結果は次のようになります。

  ```console
  ------------------------------
  Heap 0
  total 409 objects
  ------------------------------
  Heap 1
  total 0 objects
  ------------------------------
  Heap 2
  total 0 objects
  ------------------------------
  Heap 3
  total 0 objects
  ------------------------------
  total 409 objects
  Statistics:
        MT    Count TotalSize Class Name
  0015a498      409   7296540      Free
  Total 409 objects
  ```

  この出力は、ヒープのジェネレーション 0 の部分で 9 MB の領域がオブジェクトに使用されていて、7 MB が空いていることを示しています。 この分析から、ジェネレーション 0 がどの程度断片化に関与しているのかがわかります。 この分のヒープ使用量は、長期間のオブジェクトによる断片化の原因となる合計容量から除外する必要があります。

<a name="Pinned"></a>

### <a name="to-determine-the-number-of-pinned-objects"></a>固定されたオブジェクトの数を確認するには

- SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで、次のコマンドを入力します。

  **!gchandles**

  次の例に示すように、固定ハンドルの数を含む統計情報が表示されます。

  ```console
  GC Handle Statistics:
  Strong Handles:      29
  Pinned Handles:      10
  ```

<a name="TimeInGC"></a>

### <a name="to-determine-the-length-of-time-in-a-garbage-collection"></a>ガベージ コレクションの継続時間を確認するには

- `% Time in GC` メモリ パフォーマンス カウンターを調べます。

  この値は、サンプル間隔の時間を使用して計算します。 このカウンターは各ガベージ コレクションの終了時に更新されるため、サンプル間隔の間にコレクションが発生しなかった場合は前のサンプルと同じ値になります。

  コレクションの時間は、サンプル間隔の時間にパーセント値を掛けて計算します。

  次のデータには 4 つのサンプリング間隔が示されています。サンプリング間隔の時間は 2 秒で、8 秒間にわたって調査が行われています。 `Gen0`、`Gen1`、`Gen2` の各列には、そのジェネレーションでその間隔の間に発生したガベージ コレクションの番号が表示されています。

  ```console
  Interval    Gen0    Gen1    Gen2    % Time in GC
          1       9       3       1              10
          2      10       3       1               1
          3      11       3       1               3
          4      11       3       1               3
  ```

  この情報からは、ガベージ コレクションが発生した時間はわかりませんが、特定の時間間隔の間に発生したガベージ コレクションの番号を特定できます。 最悪のケースでは、ジェネレーション 0 の 10 番目のガベージ コレクションが間隔 2 の開始時に終了し、ジェネレーション 0 の 11 番目のガベージ コレクションが間隔 5 の終了時に終了したことになります。 10 番目のガベージ コレクションが終了してから 11 番目のガベージ コレクションが終了するまでの時間は約 2 秒で、パフォーマンス カウンターの値は 3% になっているため、ジェネレーション 0 の 11 番目のガベージ コレクションの継続時間は 60 ミリ秒 (2 秒 * 3%) になります。

  次の例には 5 つの間隔があります。

  ```console
  Interval    Gen0    Gen1    Gen2     % Time in GC
          1       9       3       1                3
          2      10       3       1                1
          3      11       4       2                1
          4      11       4       2                1
          5      11       4       2               20
  ```

  ジェネレーション 2 の 2 番目のガベージ コレクションは、間隔 3 の間に開始され、間隔 5 で終了しています。 最悪のケースでは、このガベージ コレクションの前のガベージ コレクションは、間隔 2 の開始時に終了したジェネレーション 0 のコレクションで、このジェネレーション 2 のガベージ コレクション自体は、間隔 5 の終了時に終了したことになります。 したがって、そのジェネレーション 0 のガベージ コレクションが終了してからこのジェネレーション 2 のガベージ コレクションが終了するまでの時間は 4 秒になります。 `% Time in GC` カウンターの値は 20% なので、このジェネレーション 2 のガベージ コレクションの継続時間は最大で 800 ミリ秒 (4 秒 * 20%) になります。

- [ガベージ コレクション ETW イベント](../../../docs/framework/performance/garbage-collection-etw-events.md)を使用してガベージ コレクションの長さを確認し、その情報を分析してガベージ コレクションの継続時間を特定することもできます。

  たとえば、次のデータは、非同時実行ガベージ コレクションの実行中に発生したイベント シーケンスを示しています。

  ```console
  Timestamp    Event name
  513052        GCSuspendEEBegin_V1
  513078        GCSuspendEEEnd
  513090        GCStart_V1
  517890        GCEnd_V1
  517894        GCHeapStats
  517897        GCRestartEEBegin
  517918        GCRestartEEEnd
  ```

  マネージド スレッドの中断に 26 マイクロ秒かかっています (`GCSuspendEEEnd` – `GCSuspendEEBegin_V1`)。

  実際のガベージ コレクションに 4.8 ミリ秒かかっています (`GCEnd_V1` – `GCStart_V1`)。

  マネージド スレッドの再開に 21 マイクロ秒かかっています (`GCRestartEEEnd` – `GCRestartEEBegin`)。

  次の例は、バックグラウンド ガベージ コレクションの出力を示しています。この出力には、process、thread、および event field が含まれています (すべてのデータが示されているわけではありません)。

  ```console
  timestamp(us)    event name            process    thread    event field
  42504385        GCSuspendEEBegin_V1    Test.exe    4372             1
  42504648        GCSuspendEEEnd         Test.exe    4372
  42504816        GCStart_V1             Test.exe    4372        102019
  42504907        GCStart_V1             Test.exe    4372        102020
  42514170        GCEnd_V1               Test.exe    4372
  42514204        GCHeapStats            Test.exe    4372        102020
  42832052        GCRestartEEBegin       Test.exe    4372
  42832136        GCRestartEEEnd         Test.exe    4372
  63685394        GCSuspendEEBegin_V1    Test.exe    4744             6
  63686347        GCSuspendEEEnd         Test.exe    4744
  63784294        GCRestartEEBegin       Test.exe    4744
  63784407        GCRestartEEEnd         Test.exe    4744
  89931423        GCEnd_V1               Test.exe    4372        102019
  89931464        GCHeapStats            Test.exe    4372
  ```

  42504816 の `GCStart_V1` イベントは、前のフィールドが `1` であるため、バックグラウンド ガベージ コレクションを示します。 これは、次の番号のガベージ コレクションになります。 102019.

  バックグラウンド ガベージ コレクションを開始する前に短期ガベージ コレクションを実行する必要があるため、`GCStart` イベントが発生します。 これは、次の番号のガベージ コレクションになります。 102020.

  42514170 の時点で、ガベージ コレクションの番号は 102020 になり、終了します。 この時点でマネージド スレッドが再開されます。 スレッド 4372 で以上の処理が完了すると、バックグラウンド ガベージ コレクションがトリガーされます。

  スレッド 4744 で中断が発生します。 このバックグラウンド ガベージ コレクションによってマネージド スレッドが中断されるのはこのときだけです。 この中断の期間は約 99 ミリ秒です ((63784407-63685394)/1000)。

  バックグラウンド ガベージ コレクションの `GCEnd` イベントは 89931423 で発生しています。 したがって、このバックグラウンド ガベージ コレクションの継続時間は約 47 秒になります ((89931423-42504816)/1000)。

  マネージド スレッドの実行中には短期ガベージ コレクションが何度も発生する可能性があります。

<a name="Triggered"></a>

### <a name="to-determine-what-triggered-a-garbage-collection"></a>ガベージ コレクションが発生した原因を確認するには

- SOS デバッガー拡張が読み込まれた WinDbg または Visual Studio デバッガーで次のコマンドを入力して、すべてのスレッドとその呼び出し履歴を表示します。

  **~\*kb**

  次のような出力が表示されます。

  ```console
  0012f3b0 79ff0bf8 mscorwks!WKS::GCHeap::GarbageCollect
  0012f454 30002894 mscorwks!GCInterface::CollectGeneration+0xa4
  0012f490 79fa22bd fragment_ni!request.Main(System.String[])+0x48
  ```

  オペレーティング システムからのメモリ不足の通知によってガベージ コレクションが発生した場合も同じような呼び出し履歴になりますが、その場合はスレッドがファイナライザー スレッドになります。 ファイナライザー スレッドは、非同期のメモリ不足の通知を受け取って、ガベージ コレクションを発生させます。

  メモリの割り当てによってガベージ コレクションが発生した場合は、次のような履歴になります。

  ```console
  0012f230 7a07c551 mscorwks!WKS::GCHeap::GarbageCollectGeneration
  0012f2b8 7a07cba8 mscorwks!WKS::gc_heap::try_allocate_more_space+0x1a1
  0012f2d4 7a07cefb mscorwks!WKS::gc_heap::allocate_more_space+0x18
  0012f2f4 7a02a51b mscorwks!WKS::GCHeap::Alloc+0x4b
  0012f310 7a02ae4c mscorwks!Alloc+0x60
  0012f364 7a030e46 mscorwks!FastAllocatePrimitiveArray+0xbd
  0012f424 300027f4 mscorwks!JIT_NewArr1+0x148
  000af70f 3000299f fragment_ni!request..ctor(Int32, Single)+0x20c
  0000002a 79fa22bd fragment_ni!request.Main(System.String[])+0x153
  ```

  最終的に Just-In-Time ヘルパー (`JIT_New*`) が `GCHeap::GarbageCollectGeneration` を呼び出します。 ジェネレーション 2 のガベージ コレクションが割り当てによって発生していると判断された場合は、ジェネレーション 2 のガベージ コレクションの対象になっているオブジェクトを確認し、それを回避する方法を特定する必要があります。 そのためには、ジェネレーション 2 のガベージ コレクションの開始時と終了時の違いを確認し、ジェネレーション 2 のコレクションを発生させたオブジェクトを特定します。

  たとえば、ジェネレーション 2 のコレクションの開始時の情報を表示するには、デバッガーで次のコマンドを入力します。

  **!dumpheap –stat**

  出力の例を以下に示します (最も多くの領域を使用しているオブジェクトのみが示されています)。

  ```console
  79124228    31857      9862328 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  00155f80    21248     12256296      Free
  79103b6c   297003     13068132 System.Threading.ReaderWriterLock
  7a747ad4   708732     14174640 System.Collections.Specialized.HybridDictionary
  7a747c78   786498     15729960 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  035f0ee4    89192     38887712 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  7912c444    91616     71887080 System.Double[]
  791242ec    32451     82462728 System.Collections.Hashtable+bucket[]
  790fa3e0  2459154    112128436 System.String
  Total 6471774 objects
  ```

  ジェネレーション 2 の終了時にも同じコマンドを実行します。

  **!dumpheap –stat**

  出力の例を以下に示します (最も多くの領域を使用しているオブジェクトのみが示されています)。

  ```console
  79124228    26648      9314256 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  79103b6c   296770     13057880 System.Threading.ReaderWriterLock
  7a747ad4   708730     14174600 System.Collections.Specialized.HybridDictionary
  7a747c78   786497     15729940 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  00155f80    13806     34007212      Free
  035f0ee4    89187     38885532 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    32370     82359768 System.Collections.Hashtable+bucket[]
  790fa3e0  2440020    111341808 System.String
  Total 6417525 objects
  ```

  出力の末尾から `double[]` オブジェクトがなくなっているため、これらがコレクションの対象になったことがわかります。 これらのオブジェクトは約 70 MB を占めています。 その他のオブジェクトはあまり変更されていません。 したがって、これらの `double[]` オブジェクトが、このジェネレーション 2 のガベージ コレクションが発生した原因です。 ガベージ コレクションが発生した原因がわかったら、今度は、これらの `double[]` オブジェクトが存在する理由と、死んだ状態になった理由を特定します。 コードの開発者にたずねることも、**gcroot** コマンドを使用して確認することもできます。

<a name="HighCPU"></a>

### <a name="to-determine-whether-high-cpu-usage-is-caused-by-garbage-collection"></a>CPU の使用率が高いのはガベージ コレクションのためかどうかを確認するには

- `% Time in GC` メモリ パフォーマンス カウンターの値と処理時間を互いに関連付けます。

  `% Time in GC` の値と処理時間が同時に急激に増加している場合は、CPU の使用率が高いのはガベージ コレクションのためです。 それ以外の場合は、アプリケーションのプロファイリングを実行して、使用率の高い箇所を見つけます。

## <a name="see-also"></a>関連項目

- [ガベージ コレクション](../../../docs/standard/garbage-collection/index.md)
