---
title: スレッド モデル
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text on buttons [WPF], updating
- message processing [WPF], nested
- blocking operations [WPF]
- Common Language Runtime (CLR), locking mechanism
- locking mechanism of Common Language Runtime (CLR)
- threading model [WPF]
- Word [WPF], spelling checking
- button text [WPF], updating
- spelling checking in Word [WPF]
- asynchronous behavior [WPF], exposing
- nested message processing [WPF]
- reentrancy [WPF]
ms.assetid: 02d8fd00-8d7c-4604-874c-58e40786770b
ms.openlocfilehash: 87dcfa22bcce730c5a9b61721c3a846a08146475
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094502"
---
# <a name="threading-model"></a>スレッド モデル
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、スレッド処理の難しさから開発者を節約するように設計されています。 その結果、多くの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 開発者は、複数のスレッドを使用するインターフェイスを作成する必要がなくなります。 マルチスレッドプログラムは複雑でデバッグが困難なため、シングルスレッドソリューションが存在する場合は回避する必要があります。

 ただし、どのように設計されていても、どのような種類の問題に対しても、シングルスレッドソリューションを提供できる [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] フレームワークはありません。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が終了しますが、複数のスレッドが [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] の応答性やアプリケーションのパフォーマンスを向上させる状況もあります。 いくつかの背景情報について説明した後、この記事ではこのような状況をいくつか紹介した後、いくつかの下位レベルの詳細について説明します。

> [!NOTE]
> このトピックでは、非同期呼び出しに <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> メソッドを使用したスレッド処理について説明します。 また、パラメーターとして <xref:System.Action> または <xref:System.Func%601> を受け取る <xref:System.Windows.Threading.Dispatcher.InvokeAsync%2A> メソッドを呼び出すことによって、非同期呼び出しを行うこともできます。  <xref:System.Windows.Threading.Dispatcher.InvokeAsync%2A> メソッドは、<xref:System.Windows.Threading.DispatcherOperation.Task%2A> プロパティを持つ <xref:System.Windows.Threading.DispatcherOperation> または <xref:System.Windows.Threading.DispatcherOperation%601>を返します。 `await` キーワードは、<xref:System.Windows.Threading.DispatcherOperation> または関連付けられた <xref:System.Threading.Tasks.Task>のいずれかと共に使用できます。 <xref:System.Threading.Tasks.Task> または <xref:System.Windows.Threading.DispatcherOperation> によって返される <xref:System.Windows.Threading.DispatcherOperation%601> を同期的に待機する必要がある場合、<xref:System.Windows.Threading.TaskExtensions.DispatcherOperationWait%2A> 拡張メソッドを呼び出します。  <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> を呼び出すと、デッドロックが発生します。 <xref:System.Threading.Tasks.Task> を使用して非同期操作を実行する方法の詳細については、「タスクの並列化」を参照してください。  <xref:System.Windows.Threading.Dispatcher.Invoke%2A> メソッドには、パラメーターとして <xref:System.Action> または <xref:System.Func%601> を受け取るオーバーロードもあります。  <xref:System.Windows.Threading.Dispatcher.Invoke%2A> メソッドを使用して、デリゲート、<xref:System.Action> または <xref:System.Func%601>に渡すことによって同期呼び出しを行うことができます。

<a name="threading_overview"></a>
## <a name="overview-and-the-dispatcher"></a>概要とディスパッチャー
 通常、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションは2つのスレッドで開始されます。1つはレンダリングを処理し、もう1つは [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を管理します。 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドが入力を受け取り、イベントを処理し、画面を描画し、アプリケーションコードを実行している間、レンダリングスレッドがバックグラウンドで効果的に実行されます。 ほとんどのアプリケーションは1つの [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドを使用しますが、状況によっては複数のを使用するのが最適です。 これについては、後で例を使って説明します。

 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドは、<xref:System.Windows.Threading.Dispatcher>と呼ばれるオブジェクト内の作業項目をキューに配置します。 <xref:System.Windows.Threading.Dispatcher> は作業項目を優先順位に従って選択し、それぞれを最後まで実行します。  すべての [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドには少なくとも1つの <xref:System.Windows.Threading.Dispatcher>が必要であり、各 <xref:System.Windows.Threading.Dispatcher> は1つのスレッドで作業項目を実行できます。

 応答性の高いユーザーフレンドリなアプリケーションを構築するには、作業項目を小さくして、<xref:System.Windows.Threading.Dispatcher> のスループットを最大化します。 このようにすると、項目が処理を待機している <xref:System.Windows.Threading.Dispatcher> キューに古くなってしまうことはありません。 入力と応答の間の perceivable 遅延によって、ユーザーの不満が生じる可能性があります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションは、ビッグ操作を処理することが想定されていますか。 コードに大きな計算が含まれている場合、またはリモートサーバー上のデータベースに対してクエリを実行する必要がある場合は、どうすればよいでしょうか。 通常、この応答は、大きな操作を別のスレッドで処理することで、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドは <xref:System.Windows.Threading.Dispatcher> キュー内の項目になる傾向があります。 大規模な操作が完了すると、結果を表示するために [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドに報告できます。

 従来、Windows では、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素には、それを作成したスレッドだけがアクセスできるようになっています。 これは、長時間実行されているタスクを実行するバックグラウンドスレッドが、完了時にテキストボックスを更新できないことを意味します。 これは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] コンポーネントの整合性を確保するために Windows によって行われます。 コンテンツが描画中にバックグラウンドスレッドによって更新された場合、リストボックスは奇妙に見える可能性があります。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、この調整を適用する組み込みの相互排他機構があります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のほとんどのクラスは <xref:System.Windows.Threading.DispatcherObject>から派生します。 構築時、<xref:System.Windows.Threading.DispatcherObject> は、現在実行中のスレッドにリンクされている <xref:System.Windows.Threading.Dispatcher> への参照を格納します。 実際には、<xref:System.Windows.Threading.DispatcherObject> は、それを作成するスレッドに関連付けられます。 プログラムの実行中に、<xref:System.Windows.Threading.DispatcherObject> はそのパブリック <xref:System.Windows.Threading.DispatcherObject.VerifyAccess%2A> メソッドを呼び出すことができます。 <xref:System.Windows.Threading.DispatcherObject.VerifyAccess%2A> は、現在のスレッドに関連付けられている <xref:System.Windows.Threading.Dispatcher> を調べ、構築中に格納されている <xref:System.Windows.Threading.Dispatcher> 参照と比較します。 一致しない場合、<xref:System.Windows.Threading.DispatcherObject.VerifyAccess%2A> は例外をスローします。 <xref:System.Windows.Threading.DispatcherObject.VerifyAccess%2A> は、<xref:System.Windows.Threading.DispatcherObject>に属するすべてのメソッドの先頭で呼び出すことを目的としています。

 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を変更できるのが1つのスレッドだけの場合、バックグラウンドスレッドがユーザーとどのように対話しますか。 バックグラウンドスレッドは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドに代わって操作を実行するように要求できます。 これを行うには、作業項目を [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドの <xref:System.Windows.Threading.Dispatcher> に登録します。 <xref:System.Windows.Threading.Dispatcher> クラスには、作業項目を登録するための2つの方法 (<xref:System.Windows.Threading.Dispatcher.Invoke%2A> と <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A>) が用意されています。 どちらのメソッドも、デリゲートの実行をスケジュールします。 <xref:System.Windows.Threading.Dispatcher.Invoke%2A> は同期呼び出しです。つまり、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドがデリゲートの実行を実際に終了するまでは返されません。 <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> は非同期であり、すぐに制御を戻します。

 <xref:System.Windows.Threading.Dispatcher> は、優先順位によってキュー内の要素を並べ替えます。 <xref:System.Windows.Threading.Dispatcher> キューに要素を追加するときに指定できるレベルは10個あります。 これらの優先順位は、<xref:System.Windows.Threading.DispatcherPriority> 列挙体に保持されます。 <xref:System.Windows.Threading.DispatcherPriority> レベルの詳細については、Windows SDK のドキュメントを参照してください。

<a name="samples"></a>
## <a name="threads-in-action-the-samples"></a>動作中のスレッド: サンプル

<a name="prime_number"></a>
### <a name="a-single-threaded-application-with-a-long-running-calculation"></a>実行時間の長い計算を含むシングルスレッドアプリケーション
 ほとんどのグラフィカルユーザーインターフェイス (Gui) は、ユーザーの操作に応じて生成されたイベントを待機している間、時間の長い部分を消費します。 プログラムを注意深くプログラミングすると、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]の応答性に影響を与えることなく、このアイドル時間を constructively に使用できます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のスレッドモデルでは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドで発生する操作を中断するための入力を許可していません。 これは、保留中の入力イベントが古くなる前に処理するために、定期的に <xref:System.Windows.Threading.Dispatcher> に戻る必要があることを意味します。

 次の例を確認してください。

 ![素数のスレッド処理を示すスクリーンショット。](./media/threading-model/threading-prime-numbers.png)

 この単純なアプリケーションは、3から昇順にカウントし、素数を検索します。 ユーザーが **[スタート]** ボタンをクリックすると、検索が開始されます。 プログラムが素数を検出すると、ユーザーインターフェイスが検出されて更新されます。 どの時点でも、ユーザーは検索を停止できます。

 非常に単純ですが、素数の検索は永久に発生し、いくつかの問題が発生する可能性があります。  ボタンの click イベントハンドラー内で検索全体を処理した場合、他のイベントを処理する機会が [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドに与えられることはありません。 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] は、入力メッセージまたは処理メッセージに応答できません。 これは再描画せず、ボタンのクリックに応答することもありません。

 素数の検索は別のスレッドで実行できますが、その場合は同期の問題に対処する必要があります。 シングルスレッド方式では、検出された最大素数を一覧表示するラベルを直接更新できます。

 計算作業を管理しやすいチャンクに分割すると、定期的に <xref:System.Windows.Threading.Dispatcher> に戻り、イベントを処理できます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]、入力を再描画して処理する機会を与えることができます。

 計算とイベント処理の間で処理時間を分割する最善の方法は、<xref:System.Windows.Threading.Dispatcher>から計算を管理することです。 <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> メソッドを使用すると、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] イベントが描画されるのと同じキュー内の素数のチェックをスケジュールできます。 この例では、一度に1つの素数チェックのみをスケジュールします。 素数の確認が完了したら、すぐに次のチェックをスケジュールします。 このチェックは、保留中の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] イベントが処理された後にのみ実行されます。

 ![ディスパッチャーキューを示すスクリーンショット。](./media/threading-model/threading-dispatcher-queue.png)

 Microsoft Word は、このメカニズムを使用してスペルチェックを行います。 スペルチェックは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドのアイドル時間を使用してバックグラウンドで実行されます。 コードを見てみましょう。

 次の例は、ユーザーインターフェイスを作成する XAML を示しています。

 [!code-xaml[ThreadingPrimeNumbers#ThreadingPrimeNumberXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingPrimeNumbers/CSharp/Window1.xaml#threadingprimenumberxaml)]

 次の例は、分離コードを示しています。

 [!code-csharp[ThreadingPrimeNumbers#ThreadingPrimeNumberCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingPrimeNumbers/CSharp/Window1.xaml.cs#threadingprimenumbercodebehind)]
 [!code-vb[ThreadingPrimeNumbers#ThreadingPrimeNumberCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingPrimeNumbers/visualbasic/mainwindow.xaml.vb#threadingprimenumbercodebehind)]

 次の例は、<xref:System.Windows.Controls.Button>のイベントハンドラーを示しています。

 [!code-csharp[ThreadingPrimeNumbers#ThreadingPrimeNumberStartOrStop](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingPrimeNumbers/CSharp/Window1.xaml.cs#threadingprimenumberstartorstop)]
 [!code-vb[ThreadingPrimeNumbers#ThreadingPrimeNumberStartOrStop](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingPrimeNumbers/visualbasic/mainwindow.xaml.vb#threadingprimenumberstartorstop)]

 このハンドラーは、<xref:System.Windows.Controls.Button>のテキストを更新するだけでなく、デリゲートを <xref:System.Windows.Threading.Dispatcher> キューに追加することによって、最初の素数チェックをスケジュールします。 このイベントハンドラーが処理を完了した後、<xref:System.Windows.Threading.Dispatcher> はこのデリゲートを選択して実行します。

 既に説明したように、<xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> は、実行のためにデリゲートをスケジュールするために使用される <xref:System.Windows.Threading.Dispatcher> メンバーです。 この場合、<xref:System.Windows.Threading.DispatcherPriority.SystemIdle> の優先順位を選択します。 <xref:System.Windows.Threading.Dispatcher> は、処理する重要なイベントがない場合にのみ、このデリゲートを実行します。 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の応答性は、数値チェックよりも重要です。 また、数値チェックルーチンを表す新しいデリゲートも渡します。

 [!code-csharp[ThreadingPrimeNumbers#ThreadingPrimeNumberCheckNextNumber](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingPrimeNumbers/CSharp/Window1.xaml.cs#threadingprimenumberchecknextnumber)]
 [!code-vb[ThreadingPrimeNumbers#ThreadingPrimeNumberCheckNextNumber](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingPrimeNumbers/visualbasic/mainwindow.xaml.vb#threadingprimenumberchecknextnumber)]

 このメソッドは、次の奇数の値が素数かどうかを確認します。 素数の場合、メソッドは、検出を反映するために `bigPrime`<xref:System.Windows.Controls.TextBlock> を直接更新します。 これは、コンポーネントの作成に使用されたのと同じスレッドで計算が行われているためです。 計算に別のスレッドを使用することを選択した場合は、より複雑な同期機構を使用して、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドで更新を実行する必要があります。 この状況を次に示します。

 このサンプルの完全なソースコードについては、「[実行時間の長い計算サンプルを使用したシングルスレッドアプリケーション](https://github.com/Microsoft/WPF-Samples/tree/master/Threading/SingleThreadedApplication)」を参照してください。

<a name="weather_sim"></a>
### <a name="handling-a-blocking-operation-with-a-background-thread"></a>バックグラウンドスレッドを使用したブロッキング操作の処理
 グラフィカルアプリケーションでのブロック操作の処理は困難な場合があります。 アプリケーションがフリーズしているように見えるため、イベントハンドラーからブロックメソッドを呼び出す必要はありません。 これらの操作を処理するために別のスレッドを使用できますが、完了したら、ワーカースレッドから GUI を直接変更することはできないため、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドと同期する必要があります。 <xref:System.Windows.Threading.Dispatcher.Invoke%2A> または <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> を使用して、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドの <xref:System.Windows.Threading.Dispatcher> にデリゲートを挿入できます。 最終的には、これらのデリゲートは [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素を変更するためのアクセス許可を使用して実行されます。

 この例では、天気予報を取得するリモートプロシージャ呼び出しを模倣しています。 この呼び出しを実行するために別のワーカースレッドを使用し、終了時に [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドの <xref:System.Windows.Threading.Dispatcher> で更新メソッドをスケジュールします。

 ![気象 UI を示すスクリーンショット。](./media/threading-model/threading-weather-ui.png)

 [!code-csharp[ThreadingWeatherForecast#ThreadingWeatherCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingWeatherForecast/CSharp/Window1.xaml.cs#threadingweathercodebehind)]
 [!code-vb[ThreadingWeatherForecast#ThreadingWeatherCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingWeatherForecast/visualbasic/window1.xaml.vb#threadingweathercodebehind)]

 注意する必要がある詳細の一部を次に示します。

- ボタンハンドラーの作成

     [!code-csharp[ThreadingWeatherForecast#ThreadingWeatherButtonHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingWeatherForecast/CSharp/Window1.xaml.cs#threadingweatherbuttonhandler)]
     [!code-vb[ThreadingWeatherForecast#ThreadingWeatherButtonHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingWeatherForecast/visualbasic/window1.xaml.vb#threadingweatherbuttonhandler)]

 ボタンがクリックされると、時計の描画が表示され、アニメーション化が開始されます。 このボタンは無効になっています。 新しいスレッドで `FetchWeatherFromServer` メソッドを呼び出した後、を返します。これにより、天気予報の収集を待機している間に <xref:System.Windows.Threading.Dispatcher> がイベントを処理できるようになります。

- 天気予報

     [!code-csharp[ThreadingWeatherForecast#ThreadingWeatherFetchWeather](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingWeatherForecast/CSharp/Window1.xaml.cs#threadingweatherfetchweather)]
     [!code-vb[ThreadingWeatherForecast#ThreadingWeatherFetchWeather](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingWeatherForecast/visualbasic/window1.xaml.vb#threadingweatherfetchweather)]

 単純にするために、この例ではネットワークコードを実際に使用していません。 代わりに、新しいスレッドを4秒間スリープ状態にすることで、ネットワークアクセスの遅延をシミュレートします。 この時点で、元の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドはまだ実行中で、イベントに応答しています。 これを示すために、アニメーションを実行したままにしておき、[最小化] ボタンと [最大化] ボタンも引き続き機能します。

 遅延が終了し、ランダムに天気予報を選択した場合は、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドに報告します。 これを行うには、そのスレッドの <xref:System.Windows.Threading.Dispatcher>を使用して、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドで `UpdateUserInterface` の呼び出しをスケジュールします。 このスケジュールされたメソッドの呼び出しに、天気を説明する文字列を渡します。

- [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の更新

     [!code-csharp[ThreadingWeatherForecast#ThreadingWeatherUpdateUI](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingWeatherForecast/CSharp/Window1.xaml.cs#threadingweatherupdateui)]
     [!code-vb[ThreadingWeatherForecast#ThreadingWeatherUpdateUI](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingWeatherForecast/visualbasic/window1.xaml.vb#threadingweatherupdateui)]

 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッド内の <xref:System.Windows.Threading.Dispatcher> に時間がある場合、スケジュールされている `UpdateUserInterface`の呼び出しが実行されます。 このメソッドは、クロックアニメーションを停止し、天気を説明する画像を選択します。 このイメージが表示され、"フェッチ予測" ボタンが復元されます。

<a name="multi_browser"></a>
### <a name="multiple-windows-multiple-threads"></a>複数のウィンドウ、複数のスレッド
 一部の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでは、複数のトップレベルウィンドウが必要です。 1つのスレッドと<xref:System.Windows.Threading.Dispatcher> の組み合わせで複数のウィンドウを管理することは完全に許容されますが、複数のスレッドがより適切なジョブを実行する場合もあります。 これは特に、いずれかのウィンドウがスレッドを独占する可能性がある場合に当てはまります。

 Windows Explorer はこの方法で動作します。 新しいエクスプローラーウィンドウはそれぞれ元のプロセスに属していますが、独立したスレッドの制御下に作成されます。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Controls.Frame> コントロールを使用して、Web ページを表示できます。 簡単な Internet Explorer の代替を簡単に作成できます。 まず、重要な機能である、新しいエクスプローラーウィンドウを開く機能について説明します。 ユーザーが [新しいウィンドウ] ボタンをクリックすると、ウィンドウのコピーが別のスレッドで起動されます。 これにより、windows の1つで実行時間の長い操作またはブロック操作によって、他のウィンドウがすべてロックされることはありません。

 実際には、Web ブラウザーモデルには独自の複雑なスレッドモデルがあります。 これは、ほとんどの閲覧者にとって理解しておく必要があるため、選択しました。

 次の例は、コードを示しています。

 [!code-xaml[ThreadingMultipleBrowsers#ThreadingMultiBrowserXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingMultipleBrowsers/CSharp/Window1.xaml#threadingmultibrowserxaml)]

 [!code-csharp[ThreadingMultipleBrowsers#ThreadingMultiBrowserCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingMultipleBrowsers/CSharp/Window1.xaml.cs#threadingmultibrowsercodebehind)]
 [!code-vb[ThreadingMultipleBrowsers#ThreadingMultiBrowserCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingMultipleBrowsers/VisualBasic/Window1.xaml.vb#threadingmultibrowsercodebehind)]

 このコードの次のスレッド処理セグメントは、このコンテキストで最も興味深いものです。

 [!code-csharp[ThreadingMultipleBrowsers#ThreadingMultiBrowserNewWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingMultipleBrowsers/CSharp/Window1.xaml.cs#threadingmultibrowsernewwindow)]
 [!code-vb[ThreadingMultipleBrowsers#ThreadingMultiBrowserNewWindow](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingMultipleBrowsers/VisualBasic/Window1.xaml.vb#threadingmultibrowsernewwindow)]

 このメソッドは、[新しいウィンドウ] ボタンがクリックされたときに呼び出されます。 新しいスレッドを作成し、非同期的に開始します。

 [!code-csharp[ThreadingMultipleBrowsers#ThreadingMultiBrowserThreadStart](~/samples/snippets/csharp/VS_Snippets_Wpf/ThreadingMultipleBrowsers/CSharp/Window1.xaml.cs#threadingmultibrowserthreadstart)]
 [!code-vb[ThreadingMultipleBrowsers#ThreadingMultiBrowserThreadStart](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ThreadingMultipleBrowsers/VisualBasic/Window1.xaml.vb#threadingmultibrowserthreadstart)]

 このメソッドは、新しいスレッドの開始点です。 このスレッドのコントロールの下に新しいウィンドウを作成します。 新しいスレッドを管理する新しい <xref:System.Windows.Threading.Dispatcher> は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] によって自動的に作成されます。 ウィンドウを機能させるために必要な作業は、<xref:System.Windows.Threading.Dispatcher>を開始することだけです。

<a name="stumbling_points"></a>
## <a name="technical-details-and-stumbling-points"></a>技術的な詳細とむやみポイント

### <a name="writing-components-using-threading"></a>スレッド処理を使用したコンポーネントの記述
 Microsoft .NET Framework 開発者ガイドでは、コンポーネントがクライアントに非同期動作を公開する方法のパターンについて説明します (「[イベントベースの非同期パターンの概要](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)」を参照してください)。 たとえば、`FetchWeatherFromServer` メソッドを再利用可能な非グラフィカルコンポーネントにパッケージ化するとします。 標準の Microsoft .NET Framework パターンに従うと、次のようになります。

 [!code-csharp[CommandingOverviewSnippets#ThreadingArticleWeatherComponent1](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#threadingarticleweathercomponent1)]
 [!code-vb[CommandingOverviewSnippets#ThreadingArticleWeatherComponent1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#threadingarticleweathercomponent1)]

 `GetWeatherAsync` では、バックグラウンドスレッドの作成など、前に説明した手法のいずれかを使用して、呼び出し元のスレッドをブロックするのではなく、非同期に処理します。

 このパターンの最も重要な部分の1つは、 *methodname*`Async` メソッドを呼び出したのと同じスレッドで*methodname*`Completed` メソッドを呼び出すことです。 これは、<xref:System.Windows.Threading.Dispatcher.CurrentDispatcher%2A>を格納することによって [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 非常に簡単に実行できますが、グラフィカルでないコンポーネントは、Windows フォームや ASP.NET プログラムではなく [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでのみ使用できます。

 <xref:System.Windows.Threading.DispatcherSynchronizationContext> クラスはこのニーズに対応しており、他の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] フレームワークとも連携する <xref:System.Windows.Threading.Dispatcher> の簡略化されたバージョンと考えることができます。

 [!code-csharp[CommandingOverviewSnippets#ThreadingArticleWeatherComponent2](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#threadingarticleweathercomponent2)]
 [!code-vb[CommandingOverviewSnippets#ThreadingArticleWeatherComponent2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#threadingarticleweathercomponent2)]

### <a name="nested-pumping"></a>入れ子になったポンプ
 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドを完全にロックすることはできない場合があります。 <xref:System.Windows.MessageBox> クラスの <xref:System.Windows.MessageBox.Show%2A> メソッドについて考えてみましょう。 <xref:System.Windows.MessageBox.Show%2A> は、ユーザーが [OK] ボタンをクリックするまで戻りません。 ただし、対話型にするためにメッセージループが必要なウィンドウを作成します。 ユーザーが [OK] をクリックするのを待機している間、元のアプリケーションウィンドウはユーザー入力に応答しません。 ただし、描画メッセージの処理は続行されます。 元のウィンドウは、カバーされて表示されたときに再描画します。

 ![MessageBox と [OK] ボタンを示すスクリーンショット](./media/threading-model/threading-message-loop.png)

 スレッドによっては、メッセージボックスウィンドウが使用されている必要があります。 メッセージボックスウィンドウだけに新しいスレッドを作成 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ことがありますが、このスレッドでは、元のウィンドウで無効になっている要素を描画することはできません (前述の相互排他の説明を思い出してください)。 代わりに、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は入れ子になったメッセージ処理システムを使用します。 <xref:System.Windows.Threading.Dispatcher> クラスには、<xref:System.Windows.Threading.Dispatcher.PushFrame%2A>と呼ばれる特殊なメソッドが含まれています。このメソッドは、アプリケーションの現在の実行ポイントを格納し、新しいメッセージループを開始します。 入れ子になったメッセージループが終了すると、元の <xref:System.Windows.Threading.Dispatcher.PushFrame%2A> 呼び出しの後に実行が再開されます。

 この場合、<xref:System.Windows.Threading.Dispatcher.PushFrame%2A> は <xref:System.Windows.MessageBox.Show%2A?displayProperty=nameWithType>の呼び出し時にプログラムコンテキストを保持し、新しいメッセージループを開始してバックグラウンドウィンドウを再描画し、メッセージボックスウィンドウへの入力を処理します。 ユーザーが [OK] をクリックしてポップアップウィンドウをクリアすると、入れ子になったループが終了し、<xref:System.Windows.MessageBox.Show%2A>の呼び出しの後で制御が再開されます。

### <a name="stale-routed-events"></a>古いルーティングイベント
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のルーティングイベントシステムは、イベントが発生したときにツリー全体に通知します。

 [!code-xaml[InputOvw#ThreadingArticleStaticRoutedEvent](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#threadingarticlestaticroutedevent)]

 楕円の上にマウスの左ボタンを押すと、`handler2` が実行されます。 `handler2` が完了すると、イベントは <xref:System.Windows.Controls.Canvas> オブジェクトに渡され、`handler1` を使用して処理されます。 これは、`handler2` でイベントオブジェクトが処理済みとして明示的にマークされていない場合にのみ発生します。

 `handler2` がこのイベントを処理するのにかなりの時間がかかる可能性があります。 `handler2` は、時間を返さない入れ子になったメッセージループを開始するために <xref:System.Windows.Threading.Dispatcher.PushFrame%2A> を使用する場合があります。 このメッセージループが完了したときにイベントが処理済みとしてマークされていない `handler2` 場合は、イベントが非常に古い場合でも、ツリーに渡されます。

### <a name="reentrancy-and-locking"></a>再入とロック
 共通言語ランタイム (CLR) のロック機構は、まったく同じように動作しません。ロックを要求するときに、スレッドが操作を完全に停止することが予想される場合があります。 実際には、スレッドは、優先度の高いメッセージを受信して処理し続けます。 これにより、デッドロックを防止し、インターフェイスの応答性を最小限にすることができますが、軽度のバグが生じる可能性があります。  ほとんどの場合、この点について知っておく必要はありませんが、まれな状況 (通常は Win32 ウィンドウメッセージまたは COM STA コンポーネントが関係します) では、このことを理解しておく価値があります。

 多くのインターフェイスは、スレッドの安全性を考慮して構築されていません。開発者は、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] が複数のスレッドによってアクセスされることがないことを前提としています。 この場合、1つのスレッドによって環境の変更が予期しない時間に行われる可能性があるため、<xref:System.Windows.Threading.DispatcherObject> 相互排他機構が解決されるという不適切な影響が生じます。 次の擬似コードを考えてみましょう。

 ![スレッド処理の再入を示す図。](./media/threading-model/threading-reentrancy.png "ThreadingReentrancy 入")

 ほとんどの場合は適切ですが、このような予期しない再入によって問題が発生する可能性がある [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] があります。 そのため、特定のキー時に [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が <xref:System.Windows.Threading.Dispatcher.DisableProcessing%2A>を呼び出します。これにより、通常の CLR ロックではなく、再入可能なロック [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用するように、そのスレッドのロック命令が変更されます。

 では、CLR チームがこの動作を選択したのはなぜですか。 これは、COM STA オブジェクトと終了スレッドで実行する必要がありました。 オブジェクトがガベージコレクションされると、その `Finalize` メソッドは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドではなく、専用のファイナライザースレッドで実行されます。 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドで作成された COM STA オブジェクトは、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のスレッドでのみ破棄できるため、問題が発生しています。 CLR は <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> に相当します (この場合は、Win32's `SendMessage`を使用します)。 ただし、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] スレッドがビジー状態の場合は、ファイナライザースレッドが停止し、COM STA オブジェクトを破棄できないため、深刻なメモリリークが発生します。 そのため、CLR チームは、ロックを機能させるための困難な呼び出しを行っていました。

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のタスクでは、メモリリークを再導入ことなく予期しない再入を避けることができます。これは、どこでも再入をブロックしないためです。

## <a name="see-also"></a>参照

- [実行時間の長い計算サンプルを使用したシングルスレッドアプリケーション](https://github.com/Microsoft/WPF-Samples/tree/master/Threading/SingleThreadedApplication)
