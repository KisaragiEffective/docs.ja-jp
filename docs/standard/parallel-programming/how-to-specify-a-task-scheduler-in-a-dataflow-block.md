---
title: '方法: データフロー ブロックのタスク スケジューラを指定する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- TPL dataflow library, linking to task scheduler in TPL
- Task Parallel Library, dataflows
- task scheduler, linking from TPL
ms.assetid: 27ece374-ed5b-49ef-9cec-b20db34a65e8
ms.openlocfilehash: 2abac1ccf45fc9c9c28e27c132e72fe483a24d75
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73122219"
---
# <a name="how-to-specify-a-task-scheduler-in-a-dataflow-block"></a>方法: データフロー ブロックのタスク スケジューラを指定する
このドキュメントでは、アプリケーションでデータ フローを使用する場合に特定のタスク スケジューラを関連付ける方法を示します。 この例では、Windows フォーム アプリケーションの <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair?displayProperty=nameWithType> クラスを使用して、リーダー タスクがアクティブである場合と、ライター タスクがアクティブである場合を示します。 また、<xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> メソッドを使用してデータ フロー ブロックを有効にし、ユーザー インターフェイス スレッドで実行できるようにします。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="to-create-the-windows-forms-application"></a>Windows フォーム アプリケーションを作成するには  
  
1. Visual C# または Visual Basic の **Windows フォーム アプリケーション** プロジェクトを作成します。 以降の手順では、プロジェクトの名前は `WriterReadersWinForms` とします。  
  
2. メイン フォーム Form1.cs (Visual Basic の Form1.vb) のフォーム デザイナーで、4 つの <xref:System.Windows.Forms.CheckBox> コントロールを追加します。 <xref:System.Windows.Forms.Control.Text%2A> プロパティを、 **に対しては「** リーダー 1`checkBox1`」に、 **に対しては「** リーダー 2`checkBox2`」に、 **に対しては「** リーダー 3`checkBox3`」に、そして  **に対しては「** ライター`checkBox4`」に設定します。 コントロールごとに、<xref:System.Windows.Forms.Control.Enabled%2A> プロパティを `False` に設定します。  
  
3. フォームに <xref:System.Windows.Forms.Timer> コントロールを追加します。 <xref:System.Windows.Forms.Timer.Interval%2A> プロパティを `2500` に設定します。  
  
## <a name="adding-dataflow-functionality"></a>データ フロー機能の追加  
 このセクションでは、アプリケーションに参加するデータ フロー ブロックを作成する方法と、各データ フロー ブロックをタスク スケジューラとを関連付ける方法について説明します。  
  
### <a name="to-add-dataflow-functionality-to-the-application"></a>アプリケーションにデータ フロー機能を追加するには  
  
1. プロジェクトで、System.Threading.Tasks.Dataflow.dll への参照を追加します。  
  
2. Form1.cs (Visual Basic の Form1.vb) に次の `using` ステートメント (Visual Basic では `Imports`) が含まれていることを確認します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#1)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#1)]  
  
3. <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> データ メンバーを `Form1` クラスに追加します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#2)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#2)]  
  
4. `Form1` コンストラクターで、`InitializeComponent` の呼び出しの後に、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトの状態を切り替える <xref:System.Windows.Forms.CheckBox> オブジェクトを作成します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#3)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#3)]  
  
5. `Form1` コンストラクターで、1 つの <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> オブジェクトと 4 つの <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクト (各 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトに 1 つの <xref:System.Windows.Forms.CheckBox> オブジェクト) を作成します。 それぞれの <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトに対して <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> オブジェクトを指定します。このオブジェクトの <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> プロパティは、リーダーの場合は <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ConcurrentScheduler%2A> プロパティに設定し、ライターの場合は <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ExclusiveScheduler%2A> プロパティに設定します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#4)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#4)]  
  
6. `Form1` コンストラクターで、<xref:System.Windows.Forms.Timer> オブジェクトを開始します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#5)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#5)]  
  
7. メイン フォームのフォーム デザイナーで、タイマーの <xref:System.Windows.Forms.Timer.Tick> イベントのイベント ハンドラーを作成します。  
  
8. タイマーに <xref:System.Windows.Forms.Timer.Tick> イベントを実装します。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#6)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#6)]  
  
 `toggleCheckBox` データ フロー ブロックはユーザー インターフェイスで機能するので、この操作をユーザー インターフェイス スレッドで実行することが重要です。 これを実現するため、構築時にこのオブジェクトは <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> プロパティが <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> に設定された <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> オブジェクトを提供します。 <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A> メソッドは、現行の同期コンテキストで作業を実行する <xref:System.Threading.Tasks.TaskScheduler> オブジェクトを作成します。 `Form1` コンストラクターはユーザー インターフェイス スレッドから呼び出されるので、`toggleCheckBox` データ フロー ブロックに対するアクションも、ユーザー インターフェイス スレッドで実行されます。  
  
 この例では、<xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> クラスを使用していくつかのデータ フロー ブロックが同時に実行できるようにし、別の 1 つのデータ フロー ブロックが、同じ <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> オブジェクトで実行するその他すべてのデータ フロー ブロックに対して排他的に実行するようにします。 この手法は、複数のデータ フロー ブロックが 1 つのリソースを共有し、一部のデータ フロー ブロックがそのリソースへの排他アクセスを必要とする場合に、手動でそのリソースへのアクセスを同期する必要がなくなるため、便利です。 手動で同期する必要がなくなることで、コードの効率性が向上する可能性があります。  
  
## <a name="example"></a>例  
 次の例は、Form1.cs (Visual Basic の Form1.vb) のコード全体を示しています。  
  
 [!code-csharp[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#100)]
 [!code-vb[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#100)]  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
