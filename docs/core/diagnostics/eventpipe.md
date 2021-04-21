---
title: EventPipe の概要
description: EventPipe について、およびそれを使用して .NET アプリケーションをトレースし、パフォーマンスの問題を診断する方法について説明します。
ms.date: 11/09/2020
ms.topic: overview
ms.openlocfilehash: de00f448dea9752c3dff2af35977d97c07aec6f3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874161"
---
# <a name="eventpipe"></a>EventPipe

EventPipe は、ETW や LTTng と同様に、トレース データを収集するために使用できるランタイム コンポーネントです。 EventPipe の目的は、.NET 開発者が、ETW や LTTng などのプラットフォーム固有の OS ネイティブ コンポーネントに依存することなく、簡単に .NET アプリケーションをトレースできるようにすることです。

EventPipe は多くの診断ツールの背後にあるメカニズムです。これを使用することで、ランタイムによって生成されたイベントや、[EventSource](xref:System.Diagnostics.Tracing.EventSource) で記述されたカスタム イベントを利用できます。

この記事では、EventPipe の概要を示します。EventPipe をどのような場合にどのような方法で使用するか、および実際のニーズに合わせて最適に構成する方法について説明します。

## <a name="eventpipe-basics"></a>EventPipe の基本

EventPipe により、ランタイム コンポーネント (たとえば、Just-In-Time コンパイラまたはガベージ コレクター) によって生成されたイベントと、ライブラリおよびユーザー コード内の [EventSource](xref:System.Diagnostics.Tracing.EventSource) インスタンスから書き込まれたイベントが集約されます。

その後、イベントはシリアル化され、ファイルに直接書き込むか、診断ポートを介してアウトプロセスから利用できるようになります。 Windows では、診断ポートは `NamedPipe` として実装されます。 Linux や macOS など、Windows 以外のプラットフォームでは、Unix ドメイン ソケットを使用して実装されます。 診断ポートと、カスタムのプロセス間通信プロトコルを使用してそれを操作する方法の詳細については、[診断 IPC プロトコルのドキュメント](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/ipc-protocol.md)を参照してください。

次に、シリアル化されたイベントが、EventPipe によって `.nettrace` ファイル形式で (診断ポートを介してストリームとして、またはファイルへの直接書き込みで) 書き込まれます。 EventPipe のシリアル化形式の詳細については、[EventPipe 形式のドキュメント](https://github.com/microsoft/perfview/blob/master/src/TraceEvent/EventPipe/EventPipeFormat.md)を参照してください。

## <a name="eventpipe-vs-etwlttng"></a>EventPipe と、ETW または LTTng

EventPipe は .NET ランタイム (CoreCLR) の一部であり、.NET Core でサポートされるすべてのプラットフォームで同じように動作するよう設計されています。 これにより複数のプラットフォーム間で、`dotnet-counters`、`dotnet-gcdump`、`dotnet-trace` などの EventPipe に基づくトレース ツールのシームレスな動作が実現します。

ただし、EventPipe はランタイムの組み込みコンポーネントであるため、そのスコープはマネージド コードとランタイム自体に限定されます。 EventPipe は、ネイティブ コード スタックの解決やさまざまなカーネル イベントの取得といった下位レベルのイベントの追跡には使用できません。 アプリで C または C++ の interop を使用するか、ランタイム自体 (C++ で記述されたもの) をトレースするか、カーネル イベント (つまり、ネイティブ スレッドのコンテキスト切り替えイベント) を必要とするアプリの動作についてより詳細な診断が必要な場合は、ETW、[perf、LTTng](./trace-perfcollect-lttng.md) のいずれかを使用する必要があります。

EventPipe と、ETW または LTTng とのもう 1 つの大きな違いは、管理者または root の特権の要件です。 ETW または LTTng を使用してアプリケーションをトレースするには、管理者または root である必要があります。 EventPipe を使用すると、トレーサー (`dotnet-trace` など) が、アプリケーションを起動したユーザーと同じユーザーとして実行されている限り、アプリケーションをトレースできます。

次の表に、EventPipe、ETW、LTTng の違いの概要を示します。

|機能|EventPipe|ETW|LTTng|
|-------|---------|---|-----------|
|クロスプラットフォーム|はい|いいえ (Windows のみ)|いいえ (サポートされている Linux ディストリビューションのみ)|
|管理者または root の特権が必要|いいえ|はい|はい|
|OS またはカーネル イベントを取得できる|いいえ|はい|はい|
|ネイティブの呼び出し履歴を解決できる|いいえ|はい|はい|

## <a name="use-eventpipe-to-trace-your-net-application"></a>EventPipe を使用して .NET アプリケーションをトレースする

EventPipe を使用すると、さまざまな方法で .NET アプリケーションをトレースできます。

* EventPipe の上に構築された[診断ツール](#tools-that-use-eventpipe)のどれかを使用します。

* [Microsoft.Diagnostics.NETCore.Client](https://github.com/dotnet/diagnostics/blob/main/documentation/diagnostics-client-library-instructions.md) ライブラリを使用して、EventPipe セッションを構成して開始するための独自のツールを自分で作成します。

* [環境変数](#trace-using-environment-variables)を使用して EventPipe を開始します。

EventPipe イベントを含む `nettrace` ファイルを作成した後、[`PerfView`](https://github.com/Microsoft/perfview#perfview-overview) または Visual Studio でそのファイルを表示できます。 Windows 以外のプラットフォームでは、[`dotnet-trace convert`](./dotnet-trace.md#dotnet-trace-convert) コマンドを使用して `nettrace` ファイルを `speedscope` または `Chromium` トレース形式に変換し、[`speedscope`](https://www.speedscope.app/) または Chrome DevTools を使用して表示できます。

[TraceEvent](https://github.com/Microsoft/perfview/blob/master/documentation/TraceEvent/TraceEventLibrary.md) を使用してプログラムで EventPipe トレースを分析することもできます。

### <a name="tools-that-use-eventpipe"></a>EventPipe を使用するツール

これは EventPipe を使用してアプリケーションをトレースする最も簡単な方法です。 これらの各ツールの使用方法の詳細については、各ツールのドキュメントを参照してください。

* [dotnet-counters](./dotnet-counters.md) を使用すると、.NET ランタイムとコア ライブラリによって生成されるさまざまなメトリックに加えて、自分で作成できるカスタム メトリックを監視および収集することができます。

* [dotnet-gcdump](./dotnet-gcdump.md) を使用すると、アプリケーションのマネージド ヒープを分析するためにライブ プロセスの GC ヒープ ダンプを収集できます。

* [dotnet-trace](./dotnet-trace.md) を使用すると、パフォーマンスを分析するためにアプリケーションのトレースを収集できます。

## <a name="trace-using-environment-variables"></a>環境変数を使用したトレース

EventPipe を使用する場合の好ましい方法は、`dotnet-trace` または `Microsoft.Diagnostics.NETCore.Client` ライブラリを使用することです。

ただし、次の環境変数を使用すると、アプリで EventPipe セッションを設定し、それによってトレースをファイルに直接書き込むことができます。 トレースを停止するには、アプリケーションを終了します。

* `COMPlus_EnableEventPipe`: ファイルに直接書き込む EventPipe セッションを開始するには、これを `1` に設定します。 既定値は `0` です。

* `COMPlus_EventPipeOutputPath`: EventPipe が `COMPlus_EnableEventPipe` を介して実行するように構成されている場合に、その出力となるトレース ファイルへのパス。 既定値は `trace.nettrace` です。これは、アプリが実行されているのと同じディレクトリに作成されます。

* `COMPlus_EventPipeCircularMB`: EventPipe の内部バッファーのサイズをメガバイトで表す 16 進数。 この構成値は、EventPipe が `COMPlus_EnableEventPipe` 経由で実行されるように構成されているときにのみ使用されます。 既定のバッファー サイズは 1024 MB であり、`0x400` == `1024` であるため、この環境変数が `400` に設定されることになります。

  > [!NOTE]
  > ターゲット プロセスによるイベントの書き込みが頻繁すぎる場合、このバッファーがオーバーフローして、一部のイベントが削除される可能性があります。 削除されるイベントが多すぎる場合は、バッファー サイズを増やし、削除されるイベントの数が減少するかどうかを確認します。 バッファー サイズを大きくしても削除されるイベントの数が減らない場合は、リーダーが遅いために、ターゲット プロセスのバッファーのフラッシュが妨げられている可能性があります。

* `COMPlus_EventPipeProcNumbers`: これを `1` に設定すると、EventPipe イベント ヘッダーのプロセッサ番号をキャプチャできるようになります。 既定値は `0` です。

* `COMPlus_EventPipeConfig`: `COMPlus_EnableEventPipe` を使用して EventPipe セッションを開始するときに、EventPipe セッション構成を設定します。
  構文は次のとおりです。

  `<provider>:<keyword>:<level>`

  コンマで連結することによって複数のプロバイダーを指定することもできます。

  `<provider1>:<keyword1>:<level1>,<provider2>:<keyword2>:<level2>`

  この環境変数が設定されていないが、EventPipe が `COMPlus_EnableEventPipe` によって有効になる場合は、次に示すキーワードとレベルを使用して次のプロバイダーを有効にすることでトレースが開始されます。

  - `Microsoft-Windows-DotNETRuntime:4c14fccbd:5`
  - `Microsoft-Windows-DotNETRuntimePrivate:4002000b:5`
  - `Microsoft-DotNETCore-SampleProfiler:0:5`

  .NET でのいくつかの既知のプロバイダーの詳細については、[既知のイベント プロバイダー](./well-known-event-providers.md)に関するページを参照してください。
