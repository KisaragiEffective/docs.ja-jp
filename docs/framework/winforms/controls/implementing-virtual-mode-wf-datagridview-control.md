---
title: 'チュートリアル: DataGridView コントロールでの仮想モードの実装'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data [Windows Forms], managing large data sets
- DataGridView control [Windows Forms], virtual mode
- virtual mode [Windows Forms], walkthroughs
- DataGridView control [Windows Forms], large data sets
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: 74eb5276-5ab8-4ce0-8005-dae751d85f7c
ms.openlocfilehash: 5db97b321238bc371c94e627a387bd83ca31b58a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746516"
---
# <a name="walkthrough-implementing-virtual-mode-in-the-windows-forms-datagridview-control"></a>チュートリアル : Windows フォーム DataGridView コントロールでの仮想モードの実装
非常に大量の表形式データを <xref:System.Windows.Forms.DataGridView> コントロールに表示する場合は、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティを `true` に設定し、コントロールとそのデータストアとの対話を明示的に管理できます。 これにより、このような場合にコントロールのパフォーマンスを微調整できます。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールには、カスタムデータストアと対話するために処理できるいくつかのイベントが用意されています。 このチュートリアルでは、これらのイベントハンドラーを実装する手順について説明します。 このトピックのコード例では、簡単に説明するために、非常に単純なデータソースを使用します。 運用環境の設定では、通常はキャッシュに表示する必要のある行だけを読み込み、キャッシュとの対話および更新のために <xref:System.Windows.Forms.DataGridView> イベントを処理します。 詳細については、「 [Windows フォーム DataGridView コントロールでの Just-in-time データ読み込みによる仮想モードの実装](implementing-virtual-mode-jit-data-loading-in-the-datagrid.md)」を参照してください。  
  
 このトピックのコードを単一のリストとしてコピーする方法については、「[方法: Windows フォーム DataGridView コントロールで仮想モードを実装](how-to-implement-virtual-mode-in-the-windows-forms-datagridview-control.md)する」を参照してください。  
  
## <a name="creating-the-form"></a>フォームの作成  
  
#### <a name="to-implement-virtual-mode"></a>仮想モードを実装するには  
  
1. <xref:System.Windows.Forms.Form> から派生し、<xref:System.Windows.Forms.DataGridView> コントロールを含むクラスを作成します。  
  
     次のコードには、いくつかの基本的な初期化が含まれています。 これは、後の手順で使用されるいくつかの変数を宣言し、`Main` メソッドを提供し、クラスコンストラクターに単純なフォームレイアウトを提供します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#001](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#001)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#001](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#001)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#001](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#001)]  
    [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#002](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#002)]
    [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#002](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#002)]
    [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#002](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#002)]  
  
2. フォームの <xref:System.Windows.Forms.Form.Load> イベントのハンドラーを実装します。このハンドラーは、<xref:System.Windows.Forms.DataGridView> コントロールを初期化し、データストアにサンプル値を設定します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#110](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#110)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#110](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#110)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#110](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#110)]  
  
3. 現在編集中のデータストアまたは `Customer` オブジェクトから要求されたセル値を取得する <xref:System.Windows.Forms.DataGridView.CellValueNeeded> イベントのハンドラーを実装します。  
  
     このイベントは、<xref:System.Windows.Forms.DataGridView> コントロールがセルを描画する必要があるときに発生します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#120](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#120)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#120](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#120)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#120)]  
  
4. 編集された行を表す `Customer` オブジェクトに、編集されたセル値を格納する <xref:System.Windows.Forms.DataGridView.CellValuePushed> イベントのハンドラーを実装します。 このイベントは、ユーザーがセル値の変更をコミットするたびに発生します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#130](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#130)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#130](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#130)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#130](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#130)]  
  
5. 新しく作成された行を表す新しい `Customer` オブジェクトを作成する <xref:System.Windows.Forms.DataGridView.NewRowNeeded> イベントのハンドラーを実装します。  
  
     このイベントは、ユーザーが新しいレコードの行を入力するたびに発生します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#140](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#140)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#140](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#140)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#140](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#140)]  
  
6. 新しい行または変更された行をデータストアに保存する <xref:System.Windows.Forms.DataGridView.RowValidated> イベントのハンドラーを実装します。  
  
     このイベントは、ユーザーが現在の行を変更するたびに発生します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#150](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#150)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#150](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#150)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#150](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#150)]  
  
7. 編集モードで ESC キーを2回押すか、または編集モードの外で1回押すことによって、ユーザーが行再設定を通知するときに、<xref:System.Windows.Forms.DataGridView.CancelRowEdit> イベントが発生するかどうかを示すハンドラーを <xref:System.Windows.Forms.DataGridView.RowDirtyStateNeeded> イベントに実装します。  
  
     既定では、<xref:System.Windows.Forms.DataGridView.RowDirtyStateNeeded> イベントハンドラーで <xref:System.Windows.Forms.QuestionEventArgs.Response%2A?displayProperty=nameWithType> プロパティが `true` に設定されていない限り、現在の行のセルが変更されている場合、行再設定に対して <xref:System.Windows.Forms.DataGridView.CancelRowEdit> が発生します。 このイベントは、実行時にコミットスコープが決定される場合に役立ちます。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#160](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#160)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#160](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#160)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#160](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#160)]  
  
8. 現在の行を表す `Customer` オブジェクトの値を破棄する <xref:System.Windows.Forms.DataGridView.CancelRowEdit> イベントのハンドラーを実装します。  
  
     このイベントは、編集モードで ESC キーを2回押すか、編集モードの外側で1回押すことによって、ユーザーが行再設定を通知するときに発生します。 現在の行にセルが変更されていない場合、または <xref:System.Windows.Forms.QuestionEventArgs.Response%2A?displayProperty=nameWithType> プロパティの値が <xref:System.Windows.Forms.DataGridView.RowDirtyStateNeeded> イベントハンドラーで `false` に設定されている場合、このイベントは発生しません。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#170](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#170)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#170](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#170)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#170](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#170)]  
  
9. 既存の `Customer` オブジェクトをデータストアから削除するか、新しく作成された行を表す未保存の `Customer` オブジェクトを破棄する <xref:System.Windows.Forms.DataGridView.UserDeletingRow> イベントのハンドラーを実装します。  
  
     このイベントは、ユーザーが行ヘッダーをクリックし、DEL キーを押して行を削除するたびに発生します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#180](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#180)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#180](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#180)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#180](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#180)]  
  
10. このコード例で使用されるデータ項目を表す単純な `Customers` クラスを実装します。  
  
     [!code-cpp[System.Windows.Forms.DataGridView.VirtualMode#200](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CPP/virtualmode.cpp#200)]
     [!code-csharp[System.Windows.Forms.DataGridView.VirtualMode#200](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/CS/virtualmode.cs#200)]
     [!code-vb[System.Windows.Forms.DataGridView.VirtualMode#200](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.VirtualMode/VB/virtualmode.vb#200)]  
  
## <a name="testing-the-application"></a>アプリケーションのテスト  
 フォームをテストして、期待どおりに動作することを確認します。  
  
#### <a name="to-test-the-form"></a>フォームをテストするには  
  
- アプリケーションをコンパイルして実行します。  
  
     3つの顧客レコードが設定された <xref:System.Windows.Forms.DataGridView> コントロールが表示されます。 行の複数のセルの値を変更して、編集モードで ESC キーを2回押すと、編集モードの外に1回押して、行全体を元の値に戻すことができます。 コントロールの行を変更、追加、または削除すると、データストア内の `Customer` オブジェクトも変更、追加、または削除されます。  
  
## <a name="next-steps"></a>次の手順  
 このアプリケーションでは、<xref:System.Windows.Forms.DataGridView> コントロールで仮想モードを実装するために処理する必要があるイベントについての基本的な知識が得られます。 この基本的なアプリケーションは、さまざまな方法で改善できます。  
  
- 外部データベースからの値をキャッシュするデータストアを実装します。 キャッシュは必要に応じて値を取得および破棄する必要があります。これにより、クライアントコンピューターで少量のメモリを消費しているときに、表示に必要なものだけが格納されます。  
  
- 要件に応じて、データストアのパフォーマンスを微調整します。 たとえば、より大きなキャッシュサイズを使用してデータベースクエリの数を最小限に抑えることで、クライアントコンピューターのメモリ制限ではなく、低速のネットワーク接続を補正することができます。  
  
 外部データベースからの値のキャッシュの詳細については、「[方法: Windows フォーム DataGridView コントロールで Just-in-time データ読み込みを使用して仮想モードを実装する](virtual-mode-with-just-in-time-data-loading-in-the-datagrid.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.VirtualMode%2A>
- <xref:System.Windows.Forms.DataGridView.CellValueNeeded>
- <xref:System.Windows.Forms.DataGridView.CellValuePushed>
- <xref:System.Windows.Forms.DataGridView.NewRowNeeded>
- <xref:System.Windows.Forms.DataGridView.RowValidated>
- <xref:System.Windows.Forms.DataGridView.RowDirtyStateNeeded>
- <xref:System.Windows.Forms.DataGridView.CancelRowEdit>
- <xref:System.Windows.Forms.DataGridView.UserDeletingRow>
- [Windows フォーム DataGridView コントロールでのパフォーマンス チューニング](performance-tuning-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでの Just-In-Time データ読み込みによる仮想モードの実装](implementing-virtual-mode-jit-data-loading-in-the-datagrid.md)
- [方法: Windows フォーム DataGridView コントロールで仮想モードを実装する](how-to-implement-virtual-mode-in-the-windows-forms-datagridview-control.md)
