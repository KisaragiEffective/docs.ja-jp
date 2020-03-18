---
title: アプリケーションのトレースとインストルメント
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework]
- debugging [.NET Framework], instrumentation
- performance monitoring, instrumentation
- instrumentation, about instrumentation
- tracing [.NET Framework], about tracing
- performance monitoring, tracing code
- Trace class, instrumentation for .NET applications
ms.assetid: 773b6fc4-9013-4322-b728-5dec7a72e743
ms.openlocfilehash: 2dcdbaf50ed053d43fc2df2c80fe7688e7b3e51f
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77542612"
---
# <a name="tracing-and-instrumenting-applications"></a>アプリケーションのトレースとインストルメント
トレースは、アプリケーションの稼働中にアプリケーションの実行を監視する手段です。 .NET Framework アプリケーションの開発時に、トレースとデバッグのインストルメンテーションをアプリケーションに追加できます。このインストルメンテーションは、アプリケーションの開発中でも開発したアプリケーションの配置後でも使用できます。 <xref:System.Diagnostics.Trace?displayProperty=nameWithType>、<xref:System.Diagnostics.Debug?displayProperty=nameWithType>、および <xref:System.Diagnostics.TraceSource?displayProperty=nameWithType> の各クラスを使用すると、エラーおよびアプリケーションの実行についての情報を後で分析するために、ログ、テキスト ファイル、またはその他のデバイスに記録できます。  
  
 ここでの*インストルメンテーション*という用語は、製品のパフォーマンスのレベルを監視または測定し、エラーを診断する具体的な機能を意味しています。 プログラミングでは、組み込むアプリケーションの機能を指します。  
  
- **コードのトレース** - 実行時に、アプリケーションの実行状態について示すメッセージを受け取ります。  
  
- **デバッグ** - 開発中のアプリケーションのプログラミング エラーを追跡して修正します。 詳細については、「[デバッグ](/visualstudio/debugger/debugger-feature-tour)」を参照してください。  
  
- **パフォーマンス カウンター** - アプリケーションのパフォーマンスを追跡するためのコンポーネントです。 これらのパフォーマンス カウンターの詳細については、「 [パフォーマンス カウンター](performance-counters.md)」を参照してください。  
  
- **イベント ログ** - アプリケーションの実行中に発生した重要なイベントを受け取って追跡するためのコンポーネントです。 詳細については、<xref:System.Diagnostics.EventLog> クラスを参照してください。  
  
 トレース ステートメントをコード内に計画的に配置することによってアプリケーションをインストルメントすることは、分散アプリケーションでは特に便利です。 トレース ステートメントを使用してアプリケーションをインストルメントすると、不適切な動作についての情報を表示できるだけでなく、アプリケーションの実行状態を監視するための情報も表示できます。  
  
 <xref:System.Diagnostics.TraceSource> クラスは拡張されたトレース機能を備えており、以前の <xref:System.Diagnostics.Trace> トレース クラスと <xref:System.Diagnostics.Debug> トレース クラスの静的メソッドの代わりに使用できます。 従来の <xref:System.Diagnostics.Trace> クラスと <xref:System.Diagnostics.Debug> クラスも引き続き広く使用されますが、<xref:System.Diagnostics.TraceSource> や <xref:System.Diagnostics.TraceSource.TraceEvent%2A> などの新しいトレース コマンドには <xref:System.Diagnostics.TraceSource.TraceData%2A> クラスが推奨されます。  
  
 <xref:System.Diagnostics.Trace> と <xref:System.Diagnostics.Debug> はほぼ同一のクラスですが、<xref:System.Diagnostics.Trace> クラスのプロシージャおよび関数は既定でリリース ビルドにコンパイルされる点が <xref:System.Diagnostics.Debug> クラスのプロシージャおよび関数と異なります。  
  
 <xref:System.Diagnostics.Trace> クラスおよび <xref:System.Diagnostics.Debug> クラスは、アプリケーションの開発中または配置後に、アプリケーションのパフォーマンスをモニターおよび検査する手段を提供します。 たとえば、(新しいデータベース接続の作成など) が配置されたアプリケーションの操作の特定の種類を追跡するために <xref:System.Diagnostics.Trace> クラスを使用すると、アプリケーションの効率を監視できるようになります。  
  
## <a name="code-tracing-and-debugging"></a>コードのトレースとデバッグ  
 開発時に、Visual Studio 統合開発環境 (IDE) の出力ウィンドウにメッセージを表示するために <xref:System.Diagnostics.Debug> クラスの出力メソッドを使用できます。 例 :  
  
```vb  
Trace.WriteLine("Hello World!")  
Debug.WriteLine("Hello World!")  
```  
  
```csharp  
System.Diagnostics.Trace.WriteLine("Hello World!");  
System.Diagnostics.Debug.WriteLine("Hello World!");  
```  
  
 いずれの例でも、デバッガーで アプリケーションを実行したとき、出力ウィンドウに "Hello World!" が表示されます。  
  
 これにより、アプリケーションをデバッグし、テスト環境での動作に基づいてアプリケーションのパフォーマンスを最適化できます。 すべてのデバッグ出力を受け取ることができるよう、<xref:System.Diagnostics.Debug> 条件属性をオンにしてデバッグ ビルドでアプリケーションをデバッグできます。 アプリケーションのリリースの準備が整ったら、コンパイラが最終的な実行可能ファイルにデバッグ コードを含めないように <xref:System.Diagnostics.Debug> の条件属性をオンにしないでリリース ビルドをコンパイルできます。 詳細については、「[方法 : トレースとデバッグを指定して条件付きコンパイルを実行する](how-to-compile-conditionally-with-trace-and-debug.md)」を参照してください。 アプリケーションのさまざまなビルド構成の詳細については、「[Compiling and Building](/visualstudio/ide/compiling-and-building-in-visual-studio)」(コンパイルとビルド) を参照してください。  
  
 <xref:System.Diagnostics.Trace> クラスのメソッドを使用してインストールされたアプリケーションのコード実行をトレースすることもできます。 [トレース スイッチ](trace-switches.md) をコード内に配置すると、トレースを行うかどうか、対象範囲をどうするかを制御することができます。 これにより、運用環境でアプリケーションの状態を監視できます。 このことは、複数のコンピューターで実行される複数のコンポーネントを使用するビジネス アプリケーションでは特に重要です。 配置後のスイッチの機能は、構成ファイルで制御できます。 詳細については、「[方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)」を参照してください。  
  
 トレースを使用するアプリケーションの開発時には、通常、アプリケーション コードにトレース メッセージとデバッグ メッセージの両方を組み込みます。 アプリケーションを配置する準備が整ったら、**Debug** 条件属性をオンにせずに、リリース ビルドをコンパイルできます。 ただし、**Trace** 条件属性をオンにして、コンパイラがトレース コードを実行可能ファイルに組み込むようにすることもできます。 詳細については、「[方法 : トレースとデバッグを指定して条件付きコンパイルを実行する](how-to-compile-conditionally-with-trace-and-debug.md)」を参照してください。  
  
### <a name="phases-of-code-tracing"></a>コードのトレースの段階  
 コードのトレースには、次の 3 つの段階があります。  
  
1. **インストルメンテーション** - トレース コードをアプリケーションに追加します。  
  
2. **トレース** - トレース コードが、指定された場所に情報を書き込みます。  
  
3. **分析** - トレース情報を分析して、アプリケーションの問題を識別および理解します。  
  
 既定の設定では、開発中は、すべてのデバッグ出力メソッドおよびトレース出力メソッドが、Visual Studio の出力ウィンドウに情報を書き込みます。 配置後のアプリケーションでは、メソッドは指定された場所にトレース情報を書き込みます。 トレースまたはデバッグの出力対象を指定する詳細については、「[トレース リスナー](trace-listeners.md)」を参照してください。  
  
 配置されたアプリケーションで起こりうる問題を、トレースを使用して分析したり修正したりするためにしばしば使用される主要な手順を次に示します。 これらの手順を実行する方法の詳細については、適切なリンクを参照してください。  
  
##### <a name="to-use-tracing-in-an-application"></a>アプリケーションでトレースを使用するには  
  
1. アプリケーションを配置した後で受け取るトレース出力の種類を検討します。  
  
2. スイッチのセットを作成します。 詳細については、「 [トレース スイッチの構成方法](how-to-create-initialize-and-configure-trace-switches.md)」を参照してください。  
  
3. アプリケーションのコードにトレース ステートメントを追加します。  
  
4. トレース出力を表示する場所を決定し、適切なリスナーを追加します。 詳細については「[トレース リスナーの作成と初期化](how-to-create-and-initialize-trace-listeners.md)」を参照してください。  
  
5. アプリケーションおよびそのアプリケーションに含まれるトレース コードをテストおよびデバッグします。  
  
6. 次のいずれかのプロシージャを使用して、アプリケーションを実行可能なコードにコンパイルします。  
  
    - **ソリューション エクスプローラー**の **[プロパティ ページ]** ダイアログ ボックスの **[デバッグ]** ページで **[ビルド]** メニューを使用します。 Visual Studio でコンパイルをする場合は、この方法を使用してください。  
  
         \- - または -  
  
    - コマンド ラインによるコンパイル用の **Trace** コンパイラ ディレクティブおよび **Debug** コンパイラ ディレクティブを使用します。 詳細については、「[トレースとデバッグによる条件付きコンパイル](how-to-compile-conditionally-with-trace-and-debug.md)」を参照してください。 コマンド ラインからコンパイルする場合はこの方法を使用してください。  
  
7. 問題が実行時に発生する場合は、適切なトレース スイッチをオンにしてください。 詳細については、「[トレース スイッチを設定する](how-to-create-initialize-and-configure-trace-switches.md)」を参照してください。  
  
     トレース コードは、指定された場所 (画面、テキスト ファイル、イベント ログなど) にトレース メッセージを書き込みます。 <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> コレクションに含まれるリスナーの種類によって、ターゲットが決まります。  
  
8. トレース メッセージを分析して、アプリケーションの問題を識別および理解します。  
  
## <a name="trace-instrumentation-and-distributed-applications"></a>トレースの実装および分散アプリケーション  
 分散アプリケーションを作成するときに、アプリケーションが使用される環境でテストをすることが難しい場合があります。 オペレーティング システムや Web ブラウザーのすべての組み合わせ (ローカライズ言語オプションのすべてのケースも含む) をテストしたり、アプリケーションに同時にアクセスする最大ユーザー数をシミュレートしたりできる開発チームはほとんどありません。 このため、大量の情報、異なる設定、エンド ユーザー固有の動作などの要因に対して分散アプリケーションがどのように応答するかをテストすることはできません。 また、分散アプリケーションを構成する要素の大半には、直接対話したりこれらの構成要素の利用状況を表示したりするためのユーザー インターフェイスは備わっていません。  
  
 ただし、アプリケーションの*実装* (つまりコード内にトレース ステートメントを計画的に配置すること) によって、分散アプリケーションが特定のイベント、特に不当な動作をシステム管理者に説明できるようにすることにより補うことができます。 これにより、予測不可能な動作 (極度に遅い応答時間など) が実行時に発生した場合でも、原因を判断できます。  
  
 トレース ステートメントを使用すると、オリジナル ソース コードを検査、変更、および再コンパイルしたり、デバッグ環境で実行時エラーを生成したりするなど、困難なタスクを避けることができます。 アプリケーションの実装は、エラーの表示だけでなく、パフォーマンスを監視するためにもすることができます。  
  
## <a name="strategic-placement-of-trace-statements"></a>トレース ステートメントの計画的な配置  
 実行時に使用するトレース ステートメントを配置するときは、特に注意が必要です。 配置済みのアプリケーションで必要と思われるトレース情報を検討し、考えられるすべてのトレース シナリオを適切に網羅する必要があります。 しかし、トレースを使用するアプリケーションは多岐に及ぶため、トレースを戦略的に配置するための一般的なガイドラインはありません。 トレース ステートメントを配置する詳細については、「[方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)」を参照してください。  
  
## <a name="output-from-tracing"></a>トレースの出力  
 トレースの出力は*リスナー*と呼ばれるオブジェクトによって収集されます。 リスナーは、トレースの出力を受け取り、出力デバイス (通常は、ウィンドウ、ログ、またはテキスト ファイル) に書き込むためのオブジェクトです。 トレース リスナーが作成されると、多くの場合、<xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> のコレクションに追加され、リスナーがすべてのトレース出力を受け取ることを許可します。  
  
 トレース情報は、既定の <xref:System.Diagnostics.Trace> の出力対象、<xref:System.Diagnostics.DefaultTraceListener> に少なくとも常に書き込まれます。 なんらかの理由 <xref:System.Diagnostics.DefaultTraceListener> のコレクションに他のリスナーを追加せずに <xref:System.Diagnostics.Trace.Listeners%2A> を削除する場合、トレース メッセージは表示されません。 詳細については、「[トレース リスナー](trace-listeners.md)」を参照してください。  
  
 トレース情報を書き込む <xref:System.Diagnostics.Debug> の <xref:System.Diagnostics.Trace> の 6 種類のメンバーとメソッドを次の表に示します。  
  
|方法|出力|  
|------------|------------|  
|`Assert`|指定されたテキスト。何も指定されない場合は、呼び出し履歴。 `Assert` ステートメントの引数として指定された条件が**false**の場合にのみ、出力が書き込まれます。|  
|`Fail`|指定されたテキスト。何も指定されない場合は、呼び出し履歴。|  
|`Write`|指定されたテキスト。|  
|`WriteIf`|`WriteIf` ステートメントの引数として指定された条件が満たされる場合は、指定されたテキスト。|  
|`WriteLine`|指定されたテキストおよびキャリッジ リターン。|  
|`WriteLineIf`|`WriteLineIf` ステートメントの引数として指定された条件が満たされる場合は、指定されたテキストと復帰。|  
  
 <xref:System.Diagnostics.Trace.Listeners%2A> のコレクションのすべてのリスナーは、上の表で説明したメッセージを受け取りますが、取られるアクションは、どのリスナーがメッセージを受け取るかによって異なる場合があります。 たとえば、<xref:System.Diagnostics.DefaultTraceListener> は `Fail` または失敗した `Assert` 通知を受信したときにアサーションダイアログボックスを表示しますが、<xref:System.Diagnostics.TextWriterTraceListener> は単にストリームに出力を書き込みます。  
  
 独自のリスナーを実装することにより、結果をカスタマイズできます。 たとえば、メッセージ ボックスにメッセージを表示するカスタム トレース リスナーや、データベースに接続してメッセージをテーブルに追加するトレース リスナーなどを実装できます。 すべてのカスタム リスナーは、前に示した 6 つのメソッドをサポートする必要があります。 開発者が定義するリスナーの作成については、「.NET Framework リファレンス」の「<xref:System.Diagnostics.TraceListener>」を参照してください。  
  
 `Write` メソッドと `WriteLine` メソッドは、指定したテキストを常に書き込みます。 `Assert`、`WriteIf`、および `WriteLineIf` には、指定されたテキストを書き込むかどうかを制御するブール型の引数が必要です。式が**true**の場合 (`WriteIf` と `WriteLineIf`の場合)、または**false** (`Assert`の場合) の場合にのみ、指定されたテキストを書き込みます。 `Fail` メソッドは、常に指定されたテキストを書き込みます。 詳細については、「[方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)」および「.NET Framework リファレンス」を参照してください。  
  
## <a name="security-concerns"></a>セキュリティに関する注意事項  
 ASP.NET アプリケーションを配置する前にトレースとデバッグを無効にしないと、アプリケーションに関する情報が公開され、悪意を持ったプログラムによって利用される可能性があります。 詳細については、「[方法 : トレースとデバッグを指定して条件付きコンパイルを実行する](how-to-compile-conditionally-with-trace-and-debug.md)」、「[Compiling and Building](/visualstudio/ide/compiling-and-building-in-visual-studio)」(コンパイルとビルド)、および「[方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)」を参照してください。 デバッグは、IIS (Internet Information Services) で設定することもできます。  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.TraceSource>
- [コード コントラクト](code-contracts.md)
- [C#、F#、および Visual Basic のプロジェクト](/visualstudio/debugger/debugging-preparation-csharp-f-hash-and-visual-basic-project-types)
- [方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)
- [方法 : トレースとデバッグを指定して条件付きコンパイルを実行する](how-to-compile-conditionally-with-trace-and-debug.md)
- [方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)
- [方法: トレース ソースを作成し初期化する](how-to-create-and-initialize-trace-sources.md)
- [方法 : TraceSource とフィルターをトレース リスナーと共に使用する](how-to-use-tracesource-and-filters-with-trace-listeners.md)
- [トレース リスナー](trace-listeners.md)
- [トレース スイッチ](trace-switches.md)
