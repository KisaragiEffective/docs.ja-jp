---
title: 'チュートリアル: DataGridView コントロールでのデータ入力中に発生したエラーの処理'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- error handling [Windows Forms], dataGridView control
- data grids [Windows Forms], error handling
- DataGridView control [Windows Forms], error handling
- data entry [Windows Forms], error handling
- error handling [Windows Forms], data entry
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: 30a68b85-d3af-4946-83c1-1e2d010d0511
ms.openlocfilehash: 77a4dcd9cf069ed8bbee6c49aa41db619c8e12ff
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738739"
---
# <a name="walkthrough-handling-errors-that-occur-during-data-entry-in-the-windows-forms-datagridview-control"></a>チュートリアル : Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーの処理

データ入力アプリケーションでは、基になるデータストアからのエラーを処理する必要があります。 Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールを使用すると、データストアが制約違反やビジネスルールの破損を検出したときに発生する <xref:System.Windows.Forms.DataGridView.DataError> イベントを公開することで、この操作を簡単に行うことができます。

このチュートリアルでは、Northwind サンプルデータベースの `Customers` テーブルから行を取得し、<xref:System.Windows.Forms.DataGridView> コントロールに表示します。 新しい行または編集済みの既存の行で重複する `CustomerID` 値が検出されると、<xref:System.Windows.Forms.DataGridView.DataError> イベントが発生します。これは、例外を説明する <xref:System.Windows.Forms.MessageBox> を表示することによって処理されます。

このトピックのコードを単一のリストとしてコピーする方法については、「[方法: Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーを処理](handle-errors-that-occur-during-data-entry-in-the-datagrid.md)する」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、以下が必要です。

- Northwind SQL Server サンプルデータベースを持つサーバーへのアクセス。

## <a name="creating-the-form"></a>フォームの作成

#### <a name="to-handle-data-entry-errors-in-the-datagridview-control"></a>DataGridView コントロールでのデータ入力エラーを処理するには

1. <xref:System.Windows.Forms.Form> から派生し、<xref:System.Windows.Forms.DataGridView> コントロールと <xref:System.Windows.Forms.BindingSource> コンポーネントを含むクラスを作成します。

    次のコード例では、基本的な初期化を行い、`Main` メソッドを含めています。

    [!code-csharp[System.Windows.Forms.DataGridView.DataError#01](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/CS/errorhandling.cs#01)]
    [!code-vb[System.Windows.Forms.DataGridView.DataError#01](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/VB/errorhandling.vb#01)]
    [!code-csharp[System.Windows.Forms.DataGridView.DataError#02](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/CS/errorhandling.cs#02)]
    [!code-vb[System.Windows.Forms.DataGridView.DataError#02](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/VB/errorhandling.vb#02)]

2. データベースへの接続の詳細を処理するために、フォームのクラス定義にメソッドを実装します。

    このコード例では、設定された <xref:System.Data.DataTable> オブジェクトを返す `GetData` メソッドを使用します。 `connectionString` 変数は、データベースに適した値に設定してください。

    > [!IMPORTANT]
    > 接続文字列内に機密情報 (パスワードなど) を格納すると、アプリケーションのセキュリティに影響を及ぼすことがあります。 データベースへのアクセスを制御する方法としては、Windows 認証 (統合セキュリティとも呼ばれます) を使用する方が安全です。 詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。

    [!code-csharp[System.Windows.Forms.DataGridView.DataError#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/CS/errorhandling.cs#30)]
    [!code-vb[System.Windows.Forms.DataGridView.DataError#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/VB/errorhandling.vb#30)]

3. フォームの <xref:System.Windows.Forms.Form.Load> イベントのハンドラーを実装します。このハンドラーは、<xref:System.Windows.Forms.DataGridView> を初期化し <xref:System.Windows.Forms.BindingSource>、データバインディングを設定します。

    [!code-csharp[System.Windows.Forms.DataGridView.DataError#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/CS/errorhandling.cs#10)]
    [!code-vb[System.Windows.Forms.DataGridView.DataError#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/VB/errorhandling.vb#10)]

4. <xref:System.Windows.Forms.DataGridView>で <xref:System.Windows.Forms.DataGridView.DataError> イベントを処理します。

    エラーのコンテキストがコミット操作の場合は、<xref:System.Windows.Forms.MessageBox>にエラーを表示します。

    [!code-csharp[System.Windows.Forms.DataGridView.DataError#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/CS/errorhandling.cs#20)]
    [!code-vb[System.Windows.Forms.DataGridView.DataError#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.DataError/VB/errorhandling.vb#20)]

## <a name="testing-the-application"></a>アプリケーションのテスト

フォームをテストして、期待どおりに動作することを確認します。

#### <a name="to-test-the-form"></a>フォームをテストするには

- F5 キーを押してアプリケーションを実行します。

  Customers テーブルのデータが入力された <xref:System.Windows.Forms.DataGridView> コントロールが表示されます。 `CustomerID` に重複する値を入力して編集をコミットすると、セルの値は自動的に元に戻り、データエントリエラーを表示する <xref:System.Windows.Forms.MessageBox> が表示されます。

## <a name="next-steps"></a>次の手順

このアプリケーションでは、<xref:System.Windows.Forms.DataGridView> コントロールの機能についての基本的な理解を得ることができます。 <xref:System.Windows.Forms.DataGridView> コントロールの外観と動作は、次のいくつかの方法でカスタマイズできます。

- 罫線とヘッダーのスタイルを変更します。 詳細については、「[方法: Windows フォーム DataGridView コントロールの境界線とグリッド線のスタイルを変更する](change-the-border-and-gridline-styles-in-the-datagrid.md)」を参照してください。

- <xref:System.Windows.Forms.DataGridView> コントロールへのユーザー入力を有効または制限します。 詳細については、「[方法: Windows フォーム Datagridview コントロールで行の追加と削除を回避](prevent-row-addition-and-deletion-datagridview.md)する」および「[方法: Windows フォーム Datagridview コントロールで列を読み取り](how-to-make-columns-read-only-in-the-windows-forms-datagridview-control.md)専用にする」を参照してください。

- <xref:System.Windows.Forms.DataGridView> コントロールへのユーザー入力を検証します。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでのデータの検証](walkthrough-validating-data-in-the-windows-forms-datagridview-control.md)」を参照してください。

- 仮想モードを使用して非常に大きなデータセットを処理します。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。

- セルの外観をカスタマイズします。 詳細については、「[方法: Windows フォーム Datagridview コントロールのセルの外観をカスタマイズ](customize-the-appearance-of-cells-in-the-datagrid.md)する」および「[方法: Windows フォーム Datagridview コントロールの既定のセルスタイルを設定する](how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.BindingSource>
- [Windows フォーム DataGridView コントロールでのデータ入力](data-entry-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーを処理する](handle-errors-that-occur-during-data-entry-in-the-datagrid.md)
- [チュートリアル: Windows フォーム DataGridView コントロールのデータの妥当性検査](walkthrough-validating-data-in-the-windows-forms-datagridview-control.md)
- [接続情報の保護](../../data/adonet/protecting-connection-information.md)
