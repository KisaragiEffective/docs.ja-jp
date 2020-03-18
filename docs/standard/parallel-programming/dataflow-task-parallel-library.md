---
title: データフロー (タスク並列ライブラリ)
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library
ms.assetid: 643575d0-d26d-4c35-8de7-a9c403e97dd6
ms.openlocfilehash: 6c589e85a0bbfb3f0b5858698ffb2a294ff88cf2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73973777"
---
# <a name="dataflow-task-parallel-library"></a>データフロー (タスク並列ライブラリ)
タスク並列ライブラリ (TPL) で提供されるデータ フロー コンポーネントは、コンカレンシー対応アプリケーションの堅牢性の強化に役立てることができます。 これらのデータ フロー コンポーネントは *TPL データ フロー ライブラリ*と総称されます。 データ フロー モデルは、粒度の粗いデータ フローおよびパイプライン処理タスクのためのインプロセス メッセージ パッシングを提供し、アクター ベースのプログラミング モデルを推進します。 データ フロー コンポーネントは、TPL の種類とスケジュール インフラストラクチャの上でビルドされ、非同期プログラミングをサポートするために C#、Visual Basic、および F# 言語と統合されています。 相互に非同期通信を行う必要がある複数の操作を行う場合、またはデータが使用可能になったときにデータを処理する場合に、これらのデータ フロー コンポーネントは役立ちます。 たとえば、Web カメラからのイメージ データを処理するアプリケーションを考えてみます。 データ フロー モデルを使用すると、イメージ フレームが使用可能になったときに、それをアプリケーションで処理できます。 たとえば、アプリケーションが輝度修正や赤目補正などを実行してイメージ フレームを向上させる場合、データ フロー コンポーネントの*パイプライン*を作成できます。 パイプラインの各ステージは、イメージを変換するために、TPL が提供する機能のような、粒度の粗い並列機能を使用する場合があります。  
  
 ここでは、TPL データ フロー ライブラリの概要を示します。 プログラミング モデル、定義済みのデータ フロー ブロックの型、およびアプリケーションの特定の要件を満たすためのデータ フロー ブロックの構成方法を説明します。  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="programming-model"></a>プログラミング モデル
 TPL データ フロー ライブラリはメッセージ パッシングおよび、並列化され、CPU 負荷が高く、I/O 負荷が高く、スループットが高く、待機時間が短いアプリケーションのための基盤を提供します。 また、システム上でのデータのバッファリング方法や移動について、明示的に制御できます。 データ フロー プログラミング モデルの理解を深めるため、非同期的にディスクからイメージを読み込み、それらの合成イメージを作成するアプリケーションを考えてみます。 従来のプログラミング モデルでは、通常、タスクを協調させ、共有データにアクセスするには、コールバックおよびロックなどの同期オブジェクトを使用する必要があります。 データ フロー プログラミング モデルを使用すると、イメージがディスクから読み込まれたときにそれを処理する、データ フロー オブジェクトを作成できます。 データ フロー モデルでは、データが使用可能になったときの処理方法と、データ間の依存関係を宣言します。 ランタイムがデータ間の依存関係を管理するため、通常は共有データへのアクセスの同期要件を回避できます。 さらに、ランタイムのスケジュールは非同期のデータの到着に基づいて動作するため、基になるスレッドを効率的に管理することによって、データ フローの応答性とスループットが向上します。 Windows フォーム アプリケーションでイメージ処理を実行するためにデータ フロー プログラミング モデルを使用する例については、「[チュートリアル: Windows フォーム アプリケーションでのデータ フローの使用](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」を参照してください。  
  
### <a name="sources-and-targets"></a>ソースとターゲット  
 TPL データ フロー ライブラリは、データのバッファリングと処理を行うデータ構造体である*データ フロー ブロック*で構成されます。 TPL は*ソース ブロック*、*ターゲット ブロック*、および*伝達子ブロック*の 3 種類のデータ フロー ブロックを定義します。 ソース ブロックはデータのソースとして機能し、それを読み取ることができます。 ターゲット ブロックはデータのレシーバーとして機能し、それに書き込むことができます。 伝達子ブロックは、ソース ブロックとターゲット ブロックのどちらとしても機能し、読み取りも書き込みもできます。 TPL は <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601?displayProperty=nameWithType> インターフェイスを定義してソースを表し、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601?displayProperty=nameWithType> によってターゲットを表し、<xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602?displayProperty=nameWithType> によって伝達子を表します。 <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> は、<xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> と <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> の両方から継承します。  
  
 TPL データ フロー ライブラリは、<xref:System.Threading.Tasks.Dataflow.ISourceBlock%601>、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>、および <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> のインターフェイスを実装する、複数の定義済みのデータ フロー ブロックの型を提供します。 これらのデータ フロー ブロックの型については、このドキュメントの「[定義済みのデータ フロー ブロックの型](#predefined-dataflow-block-types)」のセクションで説明します。  
  
### <a name="connecting-blocks"></a>ブロックの接続  
 データ フロー ブロックを接続して、データ フロー ブロックのリニア シーケンスである*パイプライン*を作成するか、またはデータ フロー ブロックのグラフである*ネットワーク*を作成できます。 パイプラインは、ネットワークの 1 つの形態です。 パイプラインまたはネットワークでは、データが使用可能になると、ソースはターゲットに非同期的にデータを伝達します。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A?displayProperty=nameWithType> メソッドは、ターゲット ブロックにソースのデータ フロー ブロックをリンクします。 ソースは、ターゲットにリンクしないか、または複数のターゲットにリンクできます。ターゲットはソースからリンクされないか、または複数のソースからリンクできます。 パイプラインまたはネットワークとの間でデータ フロー ブロックを同時に追加または削除できます。 定義済みのデータ フロー ブロックの型は、リンクとリンク解除のすべてのスレッド セーフな側面を処理します。  
  
 データ フロー ブロックを接続して基本的なパイプラインを作成する例については、「[チュートリアル: データ フロー パイプラインの作成](../../../docs/standard/parallel-programming/walkthrough-creating-a-dataflow-pipeline.md)」を参照してください。 データ フロー ブロックを接続してより複雑なネットワークを作成する例については、「[チュートリアル: Windows フォーム アプリケーションでのデータ フローの使用](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」を参照してください。 ソースがターゲットにメッセージを提供した後でターゲットをソースからリンク解除する例については、「[方法: データ フロー ブロックのリンクを解除する](../../../docs/standard/parallel-programming/how-to-unlink-dataflow-blocks.md)」を参照してください。  
  
#### <a name="filtering"></a>Filtering  
 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A?displayProperty=nameWithType> メソッドを呼び出してソースをターゲットにリンクする場合、デリゲートを指定して、ターゲット ブロックがメッセージの値に基づいて、メッセージを受け入れるか拒否するかを決めることができます。 このフィルター機構は、データ フロー ブロックが特定の値のみを確実に受信するために便利な方法です。 定義済みのデータ フロー ブロックの型のほとんどの場合に、ソース ブロックが複数のターゲット ブロックに接続されている場合は、ターゲット ブロックがメッセージを拒否すると、ソースはそのメッセージを次のターゲットに提供します。 ソースがターゲットにメッセージを提供する順序はソースによって定義され、ソースの種類によって異なる場合があります。 1 つのターゲットがメッセージを受け入れると、ほとんどのソース ブロックの型がメッセージの提供を停止します。 この規則の 1 つの例外は、<xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> クラスです。いくつかのターゲットがメッセージを拒否した場合でも、各メッセージをすべてのターゲットに提供します。 フィルター処理を使用して特定のメッセージのみを処理する例については、「[チュートリアル: Windows フォーム アプリケーションでのデータ フローの使用](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」を参照してください。  
  
> [!IMPORTANT]
> それぞれの定義済みのソースのデータ フロー ブロックの型は、メッセージを受信した順にそれが伝達されることを保証するため、ソース ブロックは各メッセージを読み取ってから、次のメッセージを処理する必要があります。 したがって、フィルター処理を使用して複数のターゲットをソースに接続する場合、各メッセージを少なくとも 1 つのターゲット ブロックが受信するようにします。 そうしない場合、アプリケーションでデッドロックが発生する可能性があります。  
  
### <a name="message-passing"></a>メッセージ パッシング  
 データ フロー プログラミング モデルは、プログラムの独立したコンポーネントがメッセージの送信によって相互に通信する、*メッセージ パッシング*の概念に関連しています。 アプリケーション コンポーネントの間でメッセージを伝達する方法の 1 つは、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> および <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A?displayProperty=nameWithType> メソッドを呼び出して、ターゲット データ フロー ブロック ポストにメッセージを送信し (<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> は同期的に動作し、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> は非同期的に動作します)、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A>、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A>、および <xref:System.Threading.Tasks.Dataflow.DataflowBlock.TryReceive%2A> メソッドを呼び出して、ソース ブロックからメッセージを受け取ることです。 入力データをヘッド ノード (ターゲット ブロック) に送信し、出力データをパイプラインのターミナル ノードまたはネットワーク (1 つ以上のソース ブロック) のターミナル ノードから受信することにより、これらのメソッドとデータ フロー パイプラインまたはネットワークを結合することができます。 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Choose%2A> メソッドを使用して、データが使用可能な、指定されたソースの先頭から読み取り、そのデータにアクションを実行することもできます。  
  
 ソース ブロックは、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A?displayProperty=nameWithType> メソッドを呼び出して、ターゲット ブロックにデータを提供します。 ターゲット ブロックは、提供されたメッセージに、3 つの方法のうちの 1 つで応答します。メッセージを受け入れるか、メッセージを拒否するか、またはメッセージを延期できます。 ターゲットがメッセージを受け入れると、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> メソッドは <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Accepted> を返します。 ターゲットがメッセージを拒否すると、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> メソッドは <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Declined> を返します。 ターゲットがソースからメッセージを受け取らないことを要求すると、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> は <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.DecliningPermanently> を返します。 その戻り値を受信した後は、定義済みのソース ブロックの型は、リンクされたターゲットにメッセージを提供せず、自動的にそのターゲットからリンク解除します。  
  
 後で使用するために、ターゲット ブロックがメッセージを延期すると、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> メソッドは <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Postponed> を返します。 メッセージを延期したターゲット ブロックは、後から <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ReserveMessage%2A?displayProperty=nameWithType> メソッドを呼び出して、提供されたメッセージの予約を試みることができます。 この時点で、メッセージはまだ使用できてターゲット ブロックから使用できるか、または他のターゲットに取得されています。 ターゲット ブロックがメッセージを後から必要とする場合、またはメッセージを必要としない場合には、それぞれ <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ConsumeMessage%2A?displayProperty=nameWithType> または <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ReleaseReservation%2A> メソッドを呼び出します。 通常は、メッセージの予約は、最短一致のモードで動作するデータ フロー ブロックの型で使用されます。 最短一致のモードについては、このドキュメントの後で説明します。 延期されたメッセージを予約せずに、ターゲット ブロックは <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ConsumeMessage%2A?displayProperty=nameWithType> メソッドを使用して、直接延期されたメッセージを使用しようとすることもできます。  
  
### <a name="dataflow-block-completion"></a>データ フロー ブロックの完了  
 データ フロー ブロックは、*完了*の概念をサポートしています。 完了した状態にあるデータ フロー ブロックは、処理を実行しません。 各データ フロー ブロックには、ブロックの完了ステータスを表す "<xref:System.Threading.Tasks.Task?displayProperty=nameWithType>完了タスク *" と呼ばれる*  オブジェクトが関連付けられています。 <xref:System.Threading.Tasks.Task> オブジェクトの終了を待機できるため、完了タスクを使用して、データ フロー ネットワークの 1 つ以上のターミナル ノードが終了するまで待機できます。 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock> インターフェイスは、データ フロー ブロックに完了の要求を通知する <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> メソッドを定義し、またデータ フロー ブロックの完了タスクを返す <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> プロパティを定義します。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> と <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> はどちらも <xref:System.Threading.Tasks.Dataflow.IDataflowBlock> インターフェイスを継承します。  
  
 データ フロー ブロックがエラーなしで完了したか、1 つのエラーが発生したか、複数のエラーが発生したか、取り消されたかを知る 2 つの方法があります。 最初の方法は、<xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>`try`- ブロック (Visual Basic では `catch``Try`-) の完了タスクで `Catch` メソッドを呼び出すことです。 次の例では、入力値が 0 未満の場合に <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> をスローする <xref:System.ArgumentOutOfRangeException> オブジェクトを作成します。 この例では、完了タスクで <xref:System.AggregateException> を呼び出すと、<xref:System.Threading.Tasks.Task.Wait%2A> がスローされます。 <xref:System.ArgumentOutOfRangeException> には <xref:System.AggregateException.InnerExceptions%2A> オブジェクトの <xref:System.AggregateException> プロパティを使用してアクセスします。  
  
 [!code-csharp[TPLDataflow_Overview#10](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#10)]
 [!code-vb[TPLDataflow_Overview#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#10)]  
  
 この例は、例外が実行データ フロー ブロックのデリゲートで処理されない場合を示します。 そのようなブロックの本体で例外を処理することをお勧めします。 そのようにできない場合は、ブロックはそれが取り消されたように動作し、受信メッセージを処理しません。  
  
 データ フロー ブロックが明示的に取り消されると、<xref:System.AggregateException> オブジェクトには <xref:System.OperationCanceledException> プロパティの <xref:System.AggregateException.InnerExceptions%2A> が含まれます。 データ フローの取り消しの詳細については、「[取り消しの有効化](#enabling-cancellation)」セクションを参照してください。  
  
 データ フロー ブロックの完了ステータスを判定する 2 番目の方法は、完了タスクの継続を使用するか、または C# と Visual Basic の非同期言語機能を使用して、完了したタスクを非同期的に待つことです。 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> メソッドに提供するデリゲートは、継続元タスクを表す <xref:System.Threading.Tasks.Task> オブジェクトを受け取ります。 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> プロパティの場合は、継続のデリゲートは完了タスクを受け取ります。 次の例は前の例に似ていますが、<xref:System.Threading.Tasks.Task.ContinueWith%2A> メソッドを使用してデータ フロー操作全体のステータスを出力する継続タスクも作成している点が異なります。  
  
 [!code-csharp[TPLDataflow_Overview#11](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#11)]
 [!code-vb[TPLDataflow_Overview#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#11)]  
  
 また、継続タスクの本体で <xref:System.Threading.Tasks.Task.IsCanceled%2A> などのプロパティを使用して、データ フロー ブロックの完了ステータスに関する追加情報を決定することもできます。 継続タスク、および継続タスクと取り消し処理やエラー処理との関連の詳細については、「[継続タスクを使用したタスクの連結](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md)」、「[タスクのキャンセル](../../../docs/standard/parallel-programming/task-cancellation.md)」、および[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)に関するページを参照してください。  

## <a name="predefined-dataflow-block-types"></a>定義済みのデータ フロー ブロックの型  
 TPL データ フロー ライブラリは、いくつかの定義済みのデータ フロー ブロックの型を提供します。 これらの型は、*バッファリング ブロック*、*実行ブロック*、*グループ化ブロック*の 3 種類に分けられます。 次のセクションでは、これらの種類を構成するブロックの型について説明します。  
  
### <a name="buffering-blocks"></a>バッファリング ブロック  
 バッファリング ブロックはデータ コンシューマーが使用するデータを保持します。 TPL データ フロー ライブラリは <xref:System.Threading.Tasks.Dataflow.BufferBlock%601?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601?displayProperty=nameWithType> と <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601?displayProperty=nameWithType> の 3 つのバッファリング ブロックの型を提供します。  
  
#### <a name="bufferblockt"></a>BufferBlock(T)  
 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> クラスは、汎用的な非同期メッセージング構造体を表します。 このクラスでは、複数のソースが書き込むことができるメッセージ、または複数のターゲットが読み取ることができるメッセージの先入れ先出し (FIFO) のキューを格納します。 ターゲットが <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトからメッセージを受信すると、そのメッセージはメッセージ キューから削除されます。 そのため、<xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトには複数のターゲットを設定できますが、各メッセージを受信するターゲットは 1 つだけです。 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> クラスは、複数のメッセージを別のコンポーネントに渡し、そのコンポーネントで各メッセージを受信する必要がある場合に便利です。  
  
 次の基本的な例は、<xref:System.Int32> オブジェクトに複数の <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> の値をポストし、その値をそのオブジェクトから読み込みます。  
  
 [!code-csharp[TPLDataflow_Overview#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#1)]
 [!code-vb[TPLDataflow_Overview#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#1)]  
  
 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトへのメッセージの書き込み方法や、オブジェクトからのメッセージの読み取り方法の完全な例については、「[方法: データフロー ブロックに対してメッセージの読み取りと書き込みを行う](../../../docs/standard/parallel-programming/how-to-write-messages-to-and-read-messages-from-a-dataflow-block.md)」をご覧ください。  
  
#### <a name="broadcastblockt"></a>BroadcastBlock(T)  
 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> クラスは、複数のメッセージを別のコンポーネントに渡すときに、そのコンポーネントで必要になるのが最新の値のみである場合に便利です。 また、このクラスは、メッセージを複数のコンポーネントにブロードキャストする場合にも便利です。  
  
 次の基本的な例は、<xref:System.Double> の値を <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> オブジェクトにポストし、その値をそのオブジェクトから複数回読み込みます。 値は読み取られた後も <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> オブジェクトから削除されないため、毎回同じ値を使用できます。  
  
 [!code-csharp[TPLDataflow_Overview#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#2)]
 [!code-vb[TPLDataflow_Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#2)]  
  
 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> を使って複数のターゲット ブロックにメッセージをブロードキャストする方法を示す例について詳しくは、「[方法: データフロー ブロックのタスク スケジューラを指定する](../../../docs/standard/parallel-programming/how-to-specify-a-task-scheduler-in-a-dataflow-block.md)」をご覧ください。  
  
#### <a name="writeonceblockt"></a>WriteOnceBlock(T)  
 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> クラスは <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> クラスに似ていますが、<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> オブジェクトに 1 回しか書き込むことができない点が異なります。 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> は C# の [readonly](../../csharp/language-reference/keywords/readonly.md) (Visual Basic では [ReadOnly](../../visual-basic/language-reference/modifiers/readonly.md)) キーワードと似ていると考えることができますが、構築時でなく、値を読み取った後は、<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> オブジェクトを変更できなくなる点が異なります。 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> クラスと同様に、ターゲットが <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> オブジェクトからメッセージを受信しても、そのメッセージはそのオブジェクトから削除されません。 そのため、複数のターゲットがメッセージのコピーを受信します。 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> のクラスは、複数のメッセージの最初のメッセージだけを伝達する場合に便利です。  
  
 次の基本的な例は、<xref:System.String> オブジェクトに複数の <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> の値をポストし、その値をそのオブジェクトから読み込みます。 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> オブジェクトには 1 回だけ書き込むことができるため、<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> オブジェクトは 1 つのメッセージを受信した後は、それ以降のメッセージを破棄します。  
  
 [!code-csharp[TPLDataflow_Overview#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#3)]
 [!code-vb[TPLDataflow_Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#3)]  
  
 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> を使って終了した最初の操作の値を受け取る方法の例について詳しくは、「[方法: データフロー ブロックのリンクを解除する](../../../docs/standard/parallel-programming/how-to-unlink-dataflow-blocks.md)」をご覧ください。  
  
### <a name="execution-blocks"></a>実行ブロック  
 実行ブロックは、受け取ったデータのそれぞれに、ユーザーが指定したデリゲートを呼び出します。 TPL データ フロー ライブラリは <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602?displayProperty=nameWithType> と <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602?displayProperty=nameWithType> の 3 つの実行ブロックの型を提供します。  
  
#### <a name="actionblockt"></a>ActionBlock(T)  
 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> クラスは、データを受け取るとデリゲートを呼び出すターゲット ブロックです。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトは、データが使用可能になったときに非同期的に実行できるデリゲートと考えることができます。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトに提供するデリゲートは、<xref:System.Action%601> 型または`System.Func<TInput, Task>` 型を使用できます。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトを <xref:System.Action%601> と共に使用すると、各入力要素の処理はデリゲートが返されたときに完了したと見なされます。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトを `System.Func<TInput, Task>` と共に使用すると、各入力要素の処理は返された <xref:System.Threading.Tasks.Task> オブジェクトが終了した場合にのみ、完了したと見なされます。 この 2 つの方法を使って、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601> を使用して各入力要素を同期的にも非同期的にも処理することができます。  
  
 次の基本的な例では、<xref:System.Int32> オブジェクトに複数の <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> の値をポストします。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトは、コンソールにそれらの値を出力します。 次にこの例では、ブロックを完了した状態に設定し、すべてのデータ フロー タスクの終了を待機します。  
  
 [!code-csharp[TPLDataflow_Overview#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#4)]
 [!code-vb[TPLDataflow_Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#4)]  
  
 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> クラスでデリゲートを使う方法を示す完全な例については、「[方法: データフロー ブロックでデータを受信したときにアクションを実行する](../../../docs/standard/parallel-programming/how-to-perform-action-when-a-dataflow-block-receives-data.md)」をご覧ください。  
  
#### <a name="transformblocktinput-toutput"></a>TransformBlock(TInput, TOutput)  
 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> クラスは <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> クラスに似ていますが、ソースとしてもターゲットとしても動作する点が異なります。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトに渡すデリゲートは `TOutput` 型の値を返します。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトに提供するデリゲートは、`System.Func<TInput, TOutput>` 型または`System.Func<TInput, Task<TOutput>>` 型を使用できます。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトを `System.Func<TInput, TOutput>` と共に使用すると、各入力要素の処理はデリゲートが返されたときに完了したと見なされます。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトを `System.Func<TInput, Task<TOutput>>` と共に使用すると、各入力要素の処理は返された <xref:System.Threading.Tasks.Task%601> オブジェクトが終了した場合にのみ、完了したと見なされます。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> と同様に、この 2 つの方法を使って、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> を使用して各入力要素を同期的にも非同期的にも処理することができます。  
  
 次の基本的な例では、入力の平方根を計算する <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトを作成します。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトは、入力として <xref:System.Int32> の値を受け取り、出力として <xref:System.Double> の値を生成します。  
  
 [!code-csharp[TPLDataflow_Overview#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#5)]
 [!code-vb[TPLDataflow_Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#5)]  
  
 Windows フォーム アプリケーションでイメージ処理を実行するデータフロー ブロックのネットワークで <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> を使う例について詳しくは、「[チュートリアル: Windows フォーム アプリケーションでのデータフローの使用](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)」をご覧ください。  
  
#### <a name="transformmanyblocktinput-toutput"></a>TransformManyBlock(TInput, TOutput)  
 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> クラスは <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> クラスに似ていますが、各入力値に 1 つの出力値を生成するのでなく、<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> は各入力値に出力値を生成しないか、または 1 つ以上の出力値を生成する点が異なります。 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトに提供するデリゲートは、`System.Func<TInput, IEnumerable<TOutput>>` 型または`System.Func<TInput, Task<IEnumerable<TOutput>>>` 型を使用できます。 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトを `System.Func<TInput, IEnumerable<TOutput>>` と共に使用すると、各入力要素の処理はデリゲートが返されたときに完了したと見なされます。 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトを `System.Func<TInput, Task<IEnumerable<TOutput>>>` と共に使用すると、各入力要素の処理は返された `System.Threading.Tasks.Task<IEnumerable<TOutput>>` オブジェクトが終了した場合にのみ、完了したと見なされます。  
  
 次の基本的な例は、文字列を個別の文字のシーケンスに分割する <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトを作成します。 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトは、入力として <xref:System.String> の値を受け取り、出力として <xref:System.Char> の値を生成します。  
  
 [!code-csharp[TPLDataflow_Overview#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#6)]
 [!code-vb[TPLDataflow_Overview#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#6)]  
  
 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> を使ってデータフロー パイプラインの各入力に複数の独立した出力を生成する方法について詳しくは、「[チュートリアル: データフロー パイプラインの作成](../../../docs/standard/parallel-programming/walkthrough-creating-a-dataflow-pipeline.md)」をご覧ください。  
  
#### <a name="degree-of-parallelism"></a>並列化の次数  
 すべての <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>, <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> と <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトは、ブロックが入力メッセージを処理できるようになるまで、入力メッセージをバッファリングします。 既定では、これらのクラスはメッセージを受信した順序で、一度に 1 つずつ処理します。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> と <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> のオブジェクトを有効化し、並列化の次数を指定して、複数のメッセージを同時に処理することもできます。 同時実行に関する詳細については、このドキュメントの後の「並列化の次数の指定」のセクションを参照してください。 並列化の次数を設定して実行データ フロー ブロックを有効化し、同時に複数のメッセージを処理する例については、「[方法: データ フロー ブロックで並列処理の範囲を指定する](../../../docs/standard/parallel-programming/how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)」を参照してください。  
  
#### <a name="summary-of-delegate-types"></a>デリゲート型の概要  
 次の表に、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>、および <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトに指定できるデリゲート型の概要を示します。 このテーブルでは、デリゲート型が同期的または非同期的に動作するかどうかについても示しています。  
  
|[種類]|同期的なデリゲート型|非同期的なデリゲート型|  
|----------|-------------------------------|--------------------------------|  
|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|`System.Action`|`System.Func<TInput, Task>`|  
|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|`System.Func<TInput, TOutput>`|`System.Func<TInput, Task<TOutput>>`|  
|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|`System.Func<TInput, IEnumerable<TOutput>>`|`System.Func<TInput, Task<IEnumerable<TOutput>>>`|  
  
 実行ブロックの型を使用する場合にラムダ式を使用することもできます。 実行ブロックでラムダ式を使用する方法の例については、「[方法: データフロー ブロックでデータを受信したときにアクションを実行する](../../../docs/standard/parallel-programming/how-to-perform-action-when-a-dataflow-block-receives-data.md)」を参照してください。  
  
### <a name="grouping-blocks"></a>グループ化ブロック  
 グループ化ブロックは、さまざまな制約の下で 1 つ以上のソースからデータをまとめます。 TPL データ フロー ライブラリは <xref:System.Threading.Tasks.Dataflow.BatchBlock%601>、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> と <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> の 3 つの結合ブロックの型を提供します。  
  
#### <a name="batchblockt"></a>BatchBlock(T)  
 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> クラスは、バッチと呼ばれる入力データのセットを、出力データの配列に結合します。 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトを作成するときに、各バッチのサイズを指定します。 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトは、指定した数の入力要素を受け取ると、その要素を含む配列を非同期的に伝達します。 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトが完了状態に設定されているが、バッチを形成するために十分な要素を含んでいない場合には、残りの入力要素を含む最終的な配列を伝達します。  
  
 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> クラスは、"*最長一致*" モードまたは "*最短一致*" モードのどちらかで動作します。 既定では最長一致モードで、<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトは提供されたすべてのメッセージを受け取り、指定された数の要素を受け取った後に、配列を伝達します。 最短一致モードでは、<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトは、バッチを作成するために十分なソースがブロックへのメッセージを提供するまで、すべての受信メッセージを延期します。 最長一致モードでは通常、最短一致モードよりも処理のオーバーヘッドが低いため、パフォーマンスがよくなります。 ただし、アトミックな方法で複数のソースの使用量を調整する必要がある場合、最短一致モードを使用できます。 最短一致モードを指定するには、<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> コンストラクターの `False` パラメーターの `dataflowBlockOptions` へ <xref:System.Threading.Tasks.Dataflow.BatchBlock%601.%23ctor%2A> を設定します。  
  
 次の基本的な例では、複数の <xref:System.Int32> の値を、バッチに 10 個の要素を持つ <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトにポストします。 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> からすべての値が伝達されることを保証するために、この例では <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> メソッドを呼び出します。 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> メソッドは <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトを完了状態に設定するため、<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトは残りのすべての要素を最終バッチとして伝達します。  
  
 [!code-csharp[TPLDataflow_Overview#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#7)]
 [!code-vb[TPLDataflow_Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#7)]  
  
 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> を使ってデータベース挿入操作の効率を向上させる完全な例については、「[チュートリアル: BatchBlock および BatchedJoinBlock を使用した効率の向上](../../../docs/standard/parallel-programming/walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)」をご覧ください。  
  
#### <a name="joinblockt1-t2-"></a>JoinBlock(T1, T2, ...)  
 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> と <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> クラスは、入力要素を収集して、それらの要素を含む <xref:System.Tuple%602?displayProperty=nameWithType> または <xref:System.Tuple%603?displayProperty=nameWithType> オブジェクトを伝達します。 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> クラスと <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> クラスは、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> を継承しません。 代わりに、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602.Target1%2A> を実装する <xref:System.Threading.Tasks.Dataflow.JoinBlock%602.Target2%2A>、<xref:System.Threading.Tasks.Dataflow.JoinBlock%603.Target3%2A>、および <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> のプロパティを提供します。  
  
 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> と同様に、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> および <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> は、最長一致モードまたは最短一致モードのどちらかで動作します。 既定では最長一致モードで、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> または <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> オブジェクトは提供されたすべてのメッセージを受け取り、各ターゲットが少なくとも 1 つのメッセージを受け取った後で、タプルを伝達します。 最短一致モードでは、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> または <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> オブジェクトは、すべてのターゲットがタプルを作成するために必要なデータを提供されるまで、すべての受信メッセージを延期します。 この時点で、ブロックは 2 フェーズ コミット プロトコルによって、ソースからすべての必要な項目をアトミックに取得します。 この延期により、他のエンティティは当面の間データを使用でき、システム全体が進行できます。  
  
 次の基本的な例は、値を計算するために <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> オブジェクトが複数のデータを必要とするケースを示します。 この例では、算術演算を実行するために 2 つの <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> の値と 1 つの <xref:System.Int32> の値を必要とする <xref:System.Char> オブジェクトを作成します。  
  
 [!code-csharp[TPLDataflow_Overview#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#8)]
 [!code-vb[TPLDataflow_Overview#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#8)]  
  
 最短一致モードで <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> オブジェクトを使って協調的にリソースを共有する例について詳しくは、「[方法: JoinBlock を使用して複数のソースからデータを読み込む](../../../docs/standard/parallel-programming/how-to-use-joinblock-to-read-data-from-multiple-sources.md)」をご覧ください。  
  
#### <a name="batchedjoinblockt1-t2-"></a>BatchedJoinBlock(T1, T2, ...)  
 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> と <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%603> クラスは、入力要素のバッチを収集して、それらの要素を含む `System.Tuple(IList(T1), IList(T2))` または `System.Tuple(IList(T1), IList(T2), IList(T3))` オブジェクトを伝達します。 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> は <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> と <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> の組み合わせであると考えることができます。 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> オブジェクトを作成するときに、各バッチのサイズを指定します。 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> は、<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target1%2A> を実装する <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target2%2A> および <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> のプロパティを提供します。 すべてのターゲットから指定した数の入力要素を受け取ると、<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> オブジェクトは、それらの要素を含む `System.Tuple(IList(T1), IList(T2))` オブジェクトを非同期的に伝達します。  
  
 次の基本的な例では、結果を保持する <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> オブジェクト、<xref:System.Int32> の値、エラーである <xref:System.Exception> オブジェクトを作成します。 この例では複数の操作を実行し、結果を <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target1%2A> オブジェクトの <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target2%2A> プロパティに書き込み、エラーを <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> プロパティに書き込みます。 操作の成功と失敗の数はあらかじめ不明であるため、<xref:System.Collections.Generic.IList%601> オブジェクトは、各ターゲット値を受け取らないことも、複数の値を受け取ることも可能です。  
  
 [!code-csharp[TPLDataflow_Overview#9](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#9)]
 [!code-vb[TPLDataflow_Overview#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#9)]  
  
 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> を使用して、プログラムがデータベースを読み取る間にその結果と発生する例外の両方をキャプチャする例について詳しくは、「[チュートリアル: BatchBlock および BatchedJoinBlock を使用した効率の向上](../../../docs/standard/parallel-programming/walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)」をご覧ください。  

## <a name="configuring-dataflow--block-behavior"></a>データフロー ブロックの動作を構成する  
 データ フロー ブロックの型のコンストラクターに <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions?displayProperty=nameWithType> のオブジェクトを提供することによって、追加のオプションを有効にできます。 これらのオプションは、基になるタスクと並列化の次数を管理するスケジューラなどの動作を制御します。 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions> には、特定のデータ フロー ブロックの型に固有の動作を指定する派生型があります。 各データ フロー ブロックの型に関連付けられているオプション型の概要を次の表に示します。  
  
|データ フロー ブロックの型|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions> 型|  
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Threading.Tasks.Dataflow.BufferBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BatchBlock%601>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.JoinBlock%602>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
  
 以下のセクションでは、<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions?displayProperty=nameWithType> と <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions?displayProperty=nameWithType> クラスを通じて利用できる、重要なデータ フロー ブロックのオプションに関する追加情報を提供します。  
  
### <a name="specifying-the-task-scheduler"></a>タスク スケジューラの指定  
 すべての定義済みのデータ フロー ブロックは TPL タスク スケジューリング メカニズムを使って、ターゲットへのデータの伝達、ソースからのデータの受け取り、データが使用可能になったときのユーザー定義のデリゲートの実行、などのアクティビティを実行します。 <xref:System.Threading.Tasks.TaskScheduler> は、タスクをスレッドのキューに置くタスク スケジューラを表す抽象クラスです。 既定のタスク スケジューラである <xref:System.Threading.Tasks.TaskScheduler.Default%2A> は、<xref:System.Threading.ThreadPool> クラスを使用して作業をキューに置き、実行します。 データ フロー ブロック オブジェクトを構成する場合、<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> プロパティを設定して、既定のタスク スケジューラをオーバーライドできます。  
  
 同じタスク スケジューラが複数のデータ フロー ブロックを管理する場合、それらにポリシーを強制的に適用できます。 たとえば、複数のデータ フロー ブロックが、それぞれ同じ <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> オブジェクトの排他スケジューラを対象とするように構成されている場合、これらのブロックにわたって実行されるすべての操作がシリアル化されます。 同様に、これらのブロックが同じ <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> オブジェクトのコンカレント スケジューラをターゲットとして構成され、そのスケジューラが最大コンカレンシー レベルに構成されている場合、これらのブロックのすべての操作は、そのコンカレント操作の数に制限されます。 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> クラスを使って、読み取り操作は並列で実行可能とするものの、書き込み操作はすべての操作で排他的に行う例について詳しくは、「[方法: データフロー ブロックのタスク スケジューラを指定する](../../../docs/standard/parallel-programming/how-to-specify-a-task-scheduler-in-a-dataflow-block.md)」をご覧ください。 TPL のタスク スケジューラの詳細については、<xref:System.Threading.Tasks.TaskScheduler> クラスに関するトピックを参照してください。  
  
### <a name="specifying-the-degree-of-parallelism"></a>並列化の次数の指定  
 既定では、TPL データ フロー ライブラリが提供する 3 つの実行ブロックの型、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> および <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> は、一度に 1 つのメッセージを処理します。 これらのデータ フロー ブロックの型は、メッセージを受け取った順番でそれを処理します。 データ フロー ブロックのオブジェクトを構築するときに、これらのデータ フロー ブロックがメッセージを同時に操作できるようにするには、<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A?displayProperty=nameWithType> のプロパティを設定します。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> の既定値は 1 で、これはデータ フロー ブロックが一度に 1 つのメッセージを処理することを保証します。 このプロパティに 1 を超える値に設定すると、データ フロー ブロックは複数のメッセージを同時に処理できます。 このプロパティを <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded?displayProperty=nameWithType> に設定すると、基になるタスク スケジューラは最大のコンカレンシーの程度を管理することができます。  
  
> [!IMPORTANT]
> 並列処理の最大範囲に 1 を超える数を指定すると、複数のメッセージを同時に処理するため、メッセージが受信した順序で処理されない場合があります。 ただし、ブロックからのメッセージが出力される順序は、メッセージが受信された順序と同じです。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> プロパティは並列処理の最大範囲を表すため、データ フロー ブロックは、指定より低い並列化の度合いで実行される場合があります。 機能要件を満たすため、または使用可能なシステム リソースの不足のため、データ フロー ブロックは、より低い並列化の度合いを使う場合があります。 データ フロー ブロックは、指定より大きな並列化を選択することはありません。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> プロパティの値は、各データ フロー ブロックのオブジェクトに排他的です。 たとえば、4 つのデータ フロー ブロックのオブジェクトが並列処理の最大範囲にそれぞれ 1 を指定した場合、4 つすべてのデータ フロー ブロック オブジェクトを並列に実行できる場合もあります。  
  
 並列処理の最大範囲を設定して時間のかかる操作を並列に行う例については、「[方法: データ フロー ブロックで並列処理の範囲を指定する](../../../docs/standard/parallel-programming/how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)」を参照してください。  
  
### <a name="specifying-the-number-of-messages-per-task"></a>タスクごとのメッセージ数の指定  
 定義済みのデータ フロー ブロックの型は、複数の入力要素を処理するタスクを使用します。 これによって、データを処理するために必要なタスクのオブジェクトの数を最小限に抑えることができ、アプリケーションを効率的に実行できます。 ただし、一連のデータ フロー ブロックのタスクがデータを処理する場合、他のデータ フロー ブロックのタスクはメッセージをキューに置いて処理の時間を待機する必要がある場合があります。 データ フロー タスク間での高い公平性を有効にするには、<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> プロパティを設定します。 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> が既定値である <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded?displayProperty=nameWithType> に設定されている場合、データ フロー ブロックが使用するタスクは、可能な限り多くのメッセージを処理します。 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> が <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded> 以外の値に設定されている場合、データ フロー ブロックは、<xref:System.Threading.Tasks.Task> オブジェクトごとに最大でこの数のメッセージを処理します。 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> プロパティを設定すると、タスク間の公平性を向上できますが、システムが必要以上のタスクを作成する場合があり、その場合にはパフォーマンスが低下する場合があります。  
  
### <a name="enabling-cancellation"></a>取り消しの有効化  
 TPL はタスクが協調的な方法によって取り消しの調整をできる機構を提供します。 データ フロー ブロックがこの取り消し機構に参加するためには、<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> プロパティを設定します。 <xref:System.Threading.CancellationToken> オブジェクトを取り消し状態に設定すると、このトークンを監視するすべてのデータ フロー ブロックは現在の項目の実行を終了しますが、後続の項目を開始しません。 また、これらのデータ フロー ブロックはすべてのバッファリングされたメッセージをクリアし、すべてのソースとターゲット ブロックへの接続を解放し、取り消し状態に遷移します。 取り消し状態に遷移すると、処理中に例外が発生しない限り、<xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> プロパティは <xref:System.Threading.Tasks.Task.Status%2A> プロパティを <xref:System.Threading.Tasks.TaskStatus.Canceled> に設定します。 この場合、<xref:System.Threading.Tasks.Task.Status%2A> は <xref:System.Threading.Tasks.TaskStatus.Faulted> に設定されます。  
  
 Windows フォーム アプリケーションで取り消しを使用する例については、「[方法: データ フロー ブロックをキャンセルする](../../../docs/standard/parallel-programming/how-to-cancel-a-dataflow-block.md)」を参照してください。 TPL での取り消し処理の詳細については、「[タスクのキャンセル](../../../docs/standard/parallel-programming/task-cancellation.md)」を参照してください。  
  
### <a name="specifying-greedy-versus-non-greedy-behavior"></a>最長一致と最短一致の動作の指定  
 複数のグループ化データ フロー ブロックの型は*最長一致*モードまたは*最短一致*モードのどちらかで動作します。 既定では、定義済みのデータ フロー ブロックの型は、最長一致モードで動作します。  
  
 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> などの結合ブロックでは、最長一致モードは、対応する結合データが使用できない場合でも、ブロックが直ちにデータを受け入れることを意味します。 最短一致モードは、ターゲットのそれぞれで結合が完了できるように使用可能になるまで、ブロックがすべての受信メッセージを延期することを意味します。 延期されたメッセージのいずれかが使用できなくなった場合、結合ブロックは延期されたすべてのメッセージを解放し、プロセスを再起動します。 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> クラスにおいては、最長一致と最短一致の動作は似ていますが、最短一致の場合には、バッチを完了するために十分なメッセージを別のソースから使用できるようになるまで <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> オブジェクトがすべての受信メッセージを延期する点が異なります。  
  
 データ フロー ブロックに最短一致モードを指定するには、<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> を `False` に設定します。 最短一致モードを使用し、複数の結合ブロックを有効にして、データ ソースをより効率的に共有する例については、「[方法: JoinBlock を使用して複数のソースからデータを読み込む](../../../docs/standard/parallel-programming/how-to-use-joinblock-to-read-data-from-multiple-sources.md)」を参照してください。

## <a name="custom-dataflow-blocks"></a>カスタム データ フロー ブロック  
 TPL データ フロー ライブラリは多くの定義済みブロックの型を提供しますが、カスタム動作を実行する追加のブロックの型を作成できます。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> または <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> インターフェイスを直接実装するか、または <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> メソッドを使用して、既存のブロックの型の動作をカプセル化する複雑なブロックをビルドします。 カスタム データ フロー ブロックの機能を実装する例については、「[チュートリアル: カスタム データ フロー ブロックの型の作成](../../../docs/standard/parallel-programming/walkthrough-creating-a-custom-dataflow-block-type.md)」を参照してください。

## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[方法: データ フロー ブロックに対してメッセージの読み取りと書き込みを行う](../../../docs/standard/parallel-programming/how-to-write-messages-to-and-read-messages-from-a-dataflow-block.md)|<xref:System.Threading.Tasks.Dataflow.BufferBlock%601> オブジェクトにメッセージを書き込む方法とメッセージを読み取る方法を示します。|  
|[方法: プロデューサー/コンシューマーのデータ フロー パターンを実装する](../../../docs/standard/parallel-programming/how-to-implement-a-producer-consumer-dataflow-pattern.md)|データ フロー モデルを使ってプロデューサー/コンシューマー パターンを実装し、プロデューサーがデータ フロー ブロックにメッセージを送信して、コンシューマーがそのブロックからメッセージを読み取る方法を示します。|  
|[方法: データ フロー ブロックでデータを受信したときにアクションを実行する](../../../docs/standard/parallel-programming/how-to-perform-action-when-a-dataflow-block-receives-data.md)|デリゲートを実行データ フロー ブロックの型である、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> と <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> に提供する方法について説明します。|  
|[チュートリアル: データ フロー パイプラインの作成](../../../docs/standard/parallel-programming/walkthrough-creating-a-dataflow-pipeline.md)|Web からテキストをダウンロードし、そのテキストの操作を実行する、データ フロー パイプラインを作成する方法について説明します。|  
|[方法: データ フロー ブロックのリンクを解除する](../../../docs/standard/parallel-programming/how-to-unlink-dataflow-blocks.md)|<xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A> メソッドを使って、ソースがターゲットにメッセージを提供した後に、ターゲット ブロックをソースからリンク解除する方法を示します。|  
|[チュートリアル: Windows フォーム アプリケーションでのデータ フローの使用](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md)|Windows フォーム アプリケーションでイメージ処理を実行する、データ フロー ブロックのネットワークを作成する方法を示します。|  
|[方法: データ フロー ブロックをキャンセルする](../../../docs/standard/parallel-programming/how-to-cancel-a-dataflow-block.md)|Windows フォーム アプリケーションで取り消しを使う方法を説明します。|  
|[方法: JoinBlock を使用して複数のソースからデータを読み込む](../../../docs/standard/parallel-programming/how-to-use-joinblock-to-read-data-from-multiple-sources.md)|データが複数のソースから使用できるようになった場合に <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> クラスを使って操作を実行する方法、および最短一致モードを使って、複数の結合ブロックがデータ ソースをより効率的に共有できる方法について説明します。|  
|[方法: データ フロー ブロックで並列処理の範囲を指定する](../../../docs/standard/parallel-programming/how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> プロパティを設定して、実行データ フロー ブロックが一度に 1 つ以上のメッセージを処理できる方法について説明します。|  
|[方法: データ フロー ブロックのタスク スケジューラを指定する](../../../docs/standard/parallel-programming/how-to-specify-a-task-scheduler-in-a-dataflow-block.md)|アプリケーションでデータ フローを使用する場合に特定のタスク スケジューラを関連付ける方法を示します。|  
|[チュートリアル: BatchBlock および BatchedJoinBlock を使用した効率の向上](../../../docs/standard/parallel-programming/walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)|<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> クラスを使用してデータベースの挿入操作の効率を向上する方法、および <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> クラスを使用して、プログラムがデータベースを読み取る間にその結果と発生する例外の両方をキャプチャする方法について説明します。|  
|[チュートリアル: カスタム データ フロー ブロックの型の作成](../../../docs/standard/parallel-programming/walkthrough-creating-a-custom-dataflow-block-type.md)|カスタム動作を実装するデータ フロー ブロックの型を作成する 2 とおりの方法を示します。|  
|[タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)|.NET Framework アプリケーションの並列処理と並列プログラミングを簡略化するライブラリである TPL を紹介します。|
