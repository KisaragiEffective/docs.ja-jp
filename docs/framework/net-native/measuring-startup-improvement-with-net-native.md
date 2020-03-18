---
title: .NET ネイティブによる起動時間の改善の測定
ms.date: 03/30/2017
ms.assetid: c4d25b24-9c1a-4b3e-9705-97ba0d6c0289
ms.openlocfilehash: 453159c3fd0590a1ed549bb7e6f8c171aac7d064
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937748"
---
# <a name="measuring-startup-improvement-with-net-native"></a>.NET ネイティブによる起動時間の改善の測定
.NET ネイティブすると、アプリの起動時間が大幅に短縮されます。 この改善は、ポータブルの低電力デバイスや複雑なアプリで特に顕著です。 このトピックでは、この起動時間の改善を測定するために必要となる基本的なインストルメンテーションの概要を示します。  
  
 パフォーマンスの調査を容易にするために、.NET Framework と Windows では、イベントが発生したときにアプリからツールに通知できるようにする Windows イベント トレーシング (ETW) という名前のイベント フレームワークを使用しています。 PerfView というツールを使用して、ETW イベントを簡単に表示および分析できます。 このトピックでは、次の方法を説明します。  
  
- <xref:System.Diagnostics.Tracing.EventSource> クラスを使用したイベントの生成。  
  
- PerfView を使用したこれらのイベントの収集。  
  
- PerfView を使用したこれらのイベントの表示。  
  
## <a name="using-eventsource-to-emit-events"></a>EventSource を使用したイベントの生成  
 <xref:System.Diagnostics.Tracing.EventSource> は、カスタム イベント プロバイダーの作成元となる基底クラスを提供します。 一般的に、<xref:System.Diagnostics.Tracing.EventSource> のサブクラスを作成して、独自のイベント メソッドで `Write*` メソッドをラップします。 通常、シングルトン パターンが各 <xref:System.Diagnostics.Tracing.EventSource> に使用されます。  
  
 たとえば、次の例のクラスを使用して、2 つのパフォーマンス特性を測定できます。  
  
- `App` クラス コンストラクターが呼び出されるまでの時間。  
  
- `MainPage` コンストラクターが呼び出されるまでの時間。  
  
 [!code-csharp[ProjectN_ETW#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw1.cs#1)]  
  
 ここではいくつか注意する点があります。 1 つ目は、シングルトンが `AppEventSource.Log` で作成されることです。 そのインスタンスが、すべてのログに使用されます。 2 つ目は、各イベント メソッドに <xref:System.Diagnostics.Tracing.EventAttribute> があることです。 これは、ツールが <xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A> メソッドのインデックスを、`AppEventSource` で呼び出されたメソッドに関連付けるのに役立ちます。  
  
 これらのイベントは単に説明を目的としていることに注意してください。 ほとんどのアプリ コードは、これらのイベントの後に実行されます。 ユーザー操作に対して、コード内のどのイベントが対応し、測定し、ベンチマークを改善するかを理解する必要があります。 また、イベント自体は時間内に 1 つのインスタンスのみをログに記録します。 一般的に、操作ごとに開始イベントと停止イベントを対にしておくと便利です。 アプリの起動を調べる場合、開始イベントは通常、オペレーティング システムが生成する "Process/Start" イベントです。  
  
 たとえば、RSS リーダーを作成するとします。 次のような場合にイベントをログに記録すると役立ちます。  
  
- メイン ページが初めて表示された場合。  
  
- ローカル ストレージから古い RSS ストーリーが逆シリアル化された場合。  
  
- アプリが新しいストーリーの同期を開始した場合。  
  
- アプリが新しいストーリーの同期を終了した場合。  
  
 アプリのインストルメント化は簡単です。派生クラスで適切なメソッドを呼び出すのみです。 前の例の `AppEventSource` を使用して、次のようにアプリをインストルメント化できます。  
  
 [!code-csharp[ProjectN_ETW#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw2.cs#2)]  
  
 アプリがインストルメント化されたら、イベントを収集できるようになります。  
  
## <a name="gathering-events-with-perfview"></a>PerfView でのイベントの収集  
 PerfView は ETW イベントを使用して、アプリでさまざまなパフォーマンスを調査できるようにします。 また、さまざまな種類のイベントのログ記録をオンまたはオフにするために使用できる構成 GUI も含まれています。 PerfView は無料ツールであり、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=28567)からダウンロードできます。 詳細については、[PerfView のチュートリアル ビデオ](https://channel9.msdn.com/Series/PerfView-Tutorial)をご覧ください。  
  
> [!NOTE]
> PerfView を使用して ARM システムでイベントを収集することはできません。 ARM システムでイベントを収集する場合は、Windows Performance Recorder (WPR) を使用します。 詳細については、[Vance Morrison のブログ投稿](https://docs.microsoft.com/archive/blogs/vancem/collecting-etwperfview-data-on-an-windows-rt-winrt-arm-surface-device)をご覧ください。  
  
 コマンド ラインから PerfView を呼び出すこともできます。 プロバイダーからのイベントのみをログに記録する場合は、コマンドプロンプト ウィンドウを開き、次のコマンドを入力します。  
  
```console
perfview -KernelEvents:Process -OnlyProviders:*MyCompany-MyApp collect outputFile   
```  
  
 それぞれの文字について以下に説明します。  
  
 `-KernelEvents:Process`  
 プロセスが開始および停止したときに通知するよう指定します。 他のイベント時間から減算できるよう、アプリに Process/Start イベントが必要です。  
  
 `-OnlyProviders:*MyCompany-MyApp`  
 PerfView が既定でオンにするその他のプロバイダーをオフにし、MyCompany-MyApp プロバイダーをオンにします。  (アスタリスクは、<xref:System.Diagnostics.Tracing.EventSource> であることを示します。)  
  
 `collect outputFile`  
 収集を開始して、データを outputFile.etl.zip に保存するよう指定します。  
  
 PerfView の開始後にアプリを実行します。 アプリを実行するときには、次のことに注意してください。  
  
- デバッグ ビルドではなく、リリース ビルドを使用します。 デバッグ ビルドには、多くの場合、アプリの実行に予想よりも時間がかかる原因となる、余分なエラー チェックおよびエラー処理コードが含まれています。  
  
- デバッガーをアタッチしてアプリを実行すると、アプリのパフォーマンスに影響します。  
  
- Windows では、アプリの起動時間を短縮するために複数のキャッシュ戦略を使用しています。 アプリが現在メモリにキャッシュされており、ディスクから読み込む必要がない場合、起動時間は短縮されます。 一貫性を保つために、測定前にアプリを数回起動して停止してください。  
  
 生成されたイベントを PerfView can が収集できるようにアプリを実行した場合は、 **[コレクションの停止]** を選択します。 通常は、アプリを停止する前にコレクションを停止して、不要なイベントを取得しないようにする必要があります。 ただし、シャットダウンまたは中断のパフォーマンスを測定する場合は、コレクションを続行します。  
  
## <a name="displaying-the-events"></a>イベントの表示  
 既に収集されたイベントを表示するには、PerfView を使用して作成した .etl ファイルまたは .etl.zip ファイルを開き、 **[イベント]** を選択します。 ETW によって、他のプロセスのイベントを含む、多数のイベントに関する情報が収集されています。 調査の対象を絞り込むために、イベント ビューで次のテキスト ボックスに入力します。  
  
- **[Process Filter]\(プロセス フィルター\)** ボックスで、アプリの名前を指定します (".exe" は含めません)。  
  
- **[Event Types Filter]\(イベント タイプ フィルター\)** ボックスに、「`Process/Start | MyCompany-MyApp`」と入力します。 これにより、MyCompany-MyApp のイベントと Windows Kernel/Process/Start イベントのフィルターが設定されます。  
  
 左ペインに示されているイベントをすべて選択し (Ctrl + A)、**Enter** キーを押します。 これで、各イベントのタイムスタンプを表示できるようになります。 これらのタイムスタンプは、トレースの開始時間を基準としています。そのため、起動時からの経過時間を調べるには、プロセスの開始時間から各イベントの時間を減算する必要があります。 Ctrl キーを押しながらクリックして 2 つのタイムスタンプを選択すると、ページ下部にあるステータス バーにそれらのタイムスタンプの差が表示されます。 これにより、表示されている 2 つのイベント間の経過時間が簡単にわかるようになります (プロセスの開始を含む)。 ビューのショートカット メニューを開いて、CSV ファイルにエクスポートしたり、Microsoft Excel を開いてデータを保存または処理したりするなど、便利なオプションを選択できます。  
  
 元のアプリと .NET ネイティブツールチェーンを使用して作成したバージョンの両方に対して手順を繰り返すことで、パフォーマンスの違いを比較できます。   .NET ネイティブアプリは、一般に non-.NET ネイティブアプリよりも高速に起動します。 より詳しく調べる場合は、最も時間がかかっているコードの部分を PerfView で特定することもできます。 詳細については、[PerfView のチュートリアル](https://channel9.msdn.com/Series/PerfView-Tutorial)または [Vance Morrison のブログ エントリ](https://docs.microsoft.com/archive/blogs/vancem/publication-of-the-perfview-performance-analysis-tool)をご覧ください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.Tracing.EventSource>
