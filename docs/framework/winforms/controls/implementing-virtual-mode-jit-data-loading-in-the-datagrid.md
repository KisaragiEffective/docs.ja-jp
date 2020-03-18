---
title: DataGridView コントロールでの Just-in-time データ読み込みによる仮想モードの実装
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- examples [Windows Forms], just-in-time data loading
- data [Windows Forms], managing large data sets
- DataGridView control [Windows Forms], virtual mode
- just-in-time data loading
- DataGridView control [Windows Forms], large data sets
- virtual mode [Windows Forms], just-in-time data loading
ms.assetid: c2a052b9-423c-4ff7-91dc-d8c7c79345f6
ms.openlocfilehash: 85c6877a9fc6a92752eb039da9d36e34811f8b77
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745059"
---
# <a name="implementing-virtual-mode-with-just-in-time-data-loading-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでの Just-In-Time データ読み込みによる仮想モードの実装
<xref:System.Windows.Forms.DataGridView> コントロールに仮想モードを実装する理由の1つは、必要なときにのみデータを取得することです。 これは*just-in-time データ読み込み*と呼ばれます。  
  
 たとえば、リモートデータベースで非常に大きなテーブルを使用している場合は、ユーザーが新しい行をビューにスクロールしたときにのみ、表示および追加データを取得するために必要なデータのみを取得することで、起動の遅延を回避できます。 アプリケーションを実行しているクライアントコンピューターで、データの格納に使用できるメモリの量が限られている場合は、データベースから新しい値を取得するときに、未使用のデータを破棄することもできます。  
  
 次のセクションでは、ジャストインタイムキャッシュで <xref:System.Windows.Forms.DataGridView> コントロールを使用する方法について説明します。  
  
 このトピックのコードを単一のリストとしてコピーする方法については、「[方法: Windows フォーム DataGridView コントロールで Just-in-time データ読み込みを使用して仮想モードを実装](virtual-mode-with-just-in-time-data-loading-in-the-datagrid.md)する」を参照してください。  
  
## <a name="the-form"></a>フォーム  
 次のコード例では、<xref:System.Windows.Forms.DataGridView.CellValueNeeded> イベントハンドラーを介して `Cache` オブジェクトと対話する読み取り専用の <xref:System.Windows.Forms.DataGridView> コントロールを含むフォームを定義します。 `Cache` オブジェクトは、ローカルに格納されている値を管理し、`DataRetriever` オブジェクトを使用して、サンプル Northwind データベースの Orders テーブルから値を取得します。 `Cache` クラスに必要な `IDataPageRetriever` インターフェイスを実装する `DataRetriever` オブジェクトは、<xref:System.Windows.Forms.DataGridView> コントロールの行と列を初期化するためにも使用されます。  
  
 `IDataPageRetriever`、`DataRetriever`、および `Cache` の種類については、このトピックの後半で説明します。  
  
> [!NOTE]
> 接続文字列内に機密情報 (パスワードなど) を格納すると、アプリケーションのセキュリティに影響を及ぼすことがあります。 データベースへのアクセスを制御する方法としては、Windows 認証 (統合セキュリティとも呼ばれます) を使用する方が安全です。 詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。  
  
 [!code-csharp[System.Windows.Forms.DataGridView.Virtual_lazyloading#100](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/CS/lazyloading.cs#100)]
 [!code-vb[System.Windows.Forms.DataGridView.Virtual_lazyloading#100](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/VB/lazyloading.vb#100)]  
  
## <a name="the-idatapageretriever-interface"></a>IDataPageRetriever インターフェイス  
 次のコード例では、`DataRetriever` クラスによって実装される `IDataPageRetriever` インターフェイスを定義します。 このインターフェイスで宣言されている唯一のメソッドは `SupplyPageOfData` メソッドです。このメソッドには、最初の行インデックスと1つのデータページの行数のカウントが必要です。 これらの値は、データソースからデータのサブセットを取得するために実装側によって使用されます。  
  
 `Cache` オブジェクトは、構築時にこのインターフェイスの実装を使用して、2つの初期ページのデータを読み込みます。 キャッシュされていない値が必要な場合、キャッシュはこれらのページのいずれかを破棄し、`IDataPageRetriever`の値を含む新しいページを要求します。  
  
 [!code-csharp[System.Windows.Forms.DataGridView.Virtual_lazyloading#201](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/CS/lazyloading.cs#201)]
 [!code-vb[System.Windows.Forms.DataGridView.Virtual_lazyloading#201](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/VB/lazyloading.vb#201)]  
  
## <a name="the-dataretriever-class"></a>DataRetriever コンポーネントクラス  
 次のコード例では、サーバーからデータページを取得するための `IDataPageRetriever` インターフェイスを実装する `DataRetriever` クラスを定義します。 `DataRetriever` クラスには、`Columns` および `RowCount` プロパティも用意されています。これらのプロパティは、<xref:System.Windows.Forms.DataGridView> コントロールが必要な列を作成し、適切な数の空の行を <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションに追加するために使用します。 コントロールがテーブル内のすべてのデータを含んでいるかのように動作するように、空の行を追加する必要があります。 これは、スクロールバーのスクロールボックスが適切なサイズになり、ユーザーがテーブル内の任意の行にアクセスできるようになることを意味します。 行が <xref:System.Windows.Forms.DataGridView.CellValueNeeded> イベントハンドラーによって表示されるのは、スクロールして表示されるときだけです。  
  
 [!code-csharp[System.Windows.Forms.DataGridView.Virtual_lazyloading#200](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/CS/lazyloading.cs#200)]
 [!code-vb[System.Windows.Forms.DataGridView.Virtual_lazyloading#200](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/VB/lazyloading.vb#200)]  
  
## <a name="the-cache-class"></a>Cache クラス  
 次のコード例では `Cache` クラスを定義しています。このクラスは、`IDataPageRetriever` の実装によって入力された2つのデータページを管理します。 `Cache` クラスは、1つのキャッシュページに値を格納するための <xref:System.Data.DataTable> を含む内部 `DataPage` 構造体を定義し、ページの上限と下限の境界を表す行インデックスを計算します。  
  
 `Cache` クラスは、構築時に2ページのデータを読み込みます。 <xref:System.Windows.Forms.DataGridView.CellValueNeeded> イベントが値を要求するたびに、`Cache` オブジェクトは、その値が2つのページのいずれかで使用できるかどうかを判断し、存在する場合はそれを返します。 値がローカルで使用できない場合、`Cache` オブジェクトは、現在表示されている行から最も遠い2つのページを特定し、そのページを要求された値を含む新しいページに置き換えます。  
  
 データページ内の行の数が、一度に画面に表示できる行数と同じであると仮定すると、このモデルでは、ユーザーがテーブルをページングして、最も最近表示されたページに効率的に戻ることができます。  
  
 [!code-csharp[System.Windows.Forms.DataGridView.Virtual_lazyloading#300](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/CS/lazyloading.cs#300)]
 [!code-vb[System.Windows.Forms.DataGridView.Virtual_lazyloading#300](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.Virtual_lazyloading/VB/lazyloading.vb#300)]  
  
## <a name="additional-considerations"></a>その他の注意点  
 前のコード例は、just-in-time データ読み込みのデモとして提供されています。 最大限の効率を実現するには、独自のニーズに合わせてコードを変更する必要があります。 少なくとも、キャッシュ内のデータの1ページあたりの行数に適切な値を選択する必要があります。 この値は `Cache` コンストラクターに渡されます。 1ページあたりの行数は、<xref:System.Windows.Forms.DataGridView> コントロールに同時に表示できる行数より少なくする必要があります。  
  
 最良の結果を得るには、パフォーマンステストとユーザビリティテストを実施して、システムとユーザーの要件を決定する必要があります。 考慮する必要がある要素には、アプリケーションを実行しているクライアントコンピューターのメモリの量、使用されるネットワーク接続の使用可能な帯域幅、および使用されるサーバーの待機時間などがあります。 帯域幅と待機時間は、ピーク時の使用時に決定する必要があります。  
  
 アプリケーションのスクロールパフォーマンスを向上させるために、ローカルに保存されるデータの量を増やすことができます。 ただし、起動時間を短縮するには、最初に大量のデータが読み込まれないようにする必要があります。 `Cache` クラスを変更して、格納できるデータページの数を増やすことができます。 より多くのデータページを使用すると、スクロール効率が向上しますが、使用可能な帯域幅とサーバーの待機時間によっては、データページ内の最適な行数を確認する必要があります。 ページ数を小さくすると、サーバーにはより頻繁にアクセスされますが、要求されたデータが返されるまでにかかる時間は短くなります。 待機時間が帯域幅よりも大きな問題である場合は、より大きなデータページを使用することをお勧めします。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.VirtualMode%2A>
- [Windows フォーム DataGridView コントロールでのパフォーマンス チューニング](performance-tuning-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでの仮想モード](virtual-mode-in-the-windows-forms-datagridview-control.md)
- [チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールで Just-In-Time データ読み込みを使用して仮想モードを実装する](virtual-mode-with-just-in-time-data-loading-in-the-datagrid.md)
