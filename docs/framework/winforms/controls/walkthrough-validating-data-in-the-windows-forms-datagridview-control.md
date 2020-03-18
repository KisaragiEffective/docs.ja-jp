---
title: 'チュートリアル: DataGridView コントロールでのデータの検証'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- validating data [Windows Forms], Windows Forms
- data [Windows Forms], validation
- DataGridView control [Windows Forms], data validation
- data grids [Windows Forms], validating data
- data validation [Windows Forms], Windows Forms
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: a4f1d015-2969-430c-8ea2-b612d179c290
ms.openlocfilehash: 45753fb206ad58616c9a727bd81bd2458db6053e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740095"
---
# <a name="walkthrough-validating-data-in-the-windows-forms-datagridview-control"></a>チュートリアル : Windows フォーム DataGridView コントロールのデータの妥当性検査

データ入力機能をユーザーに表示する場合は、フォームに入力されたデータを頻繁に検証する必要があります。 <xref:System.Windows.Forms.DataGridView> クラスは、データがデータストアにコミットされる前に検証を実行する便利な方法を提供します。 現在のセルが変更されたときに <xref:System.Windows.Forms.DataGridView> によって発生する <xref:System.Windows.Forms.DataGridView.CellValidating> イベントを処理することで、データを検証できます。

このチュートリアルでは、Northwind サンプルデータベースの `Customers` テーブルから行を取得し、<xref:System.Windows.Forms.DataGridView> コントロールに表示します。 ユーザーが `CompanyName` 列のセルを編集し、そのセルから離れようとすると、<xref:System.Windows.Forms.DataGridView.CellValidating> イベントハンドラーは新しい会社名の文字列を調べて、空でないことを確認します。新しい値が空の文字列の場合、<xref:System.Windows.Forms.DataGridView> によって、空でない文字列が入力されるまで、ユーザーのカーソルがセルから離れるのを防ぐことができます。

このトピックのコードを単一のリストとしてコピーする方法については、「[方法: Windows フォーム DataGridView コントロールでデータを検証](how-to-validate-data-in-the-windows-forms-datagridview-control.md)する」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、以下が必要です。

- Northwind SQL Server サンプルデータベースを持つサーバーへのアクセス。

## <a name="creating-the-form"></a>フォームの作成

#### <a name="to-validate-data-entered-in-a-datagridview"></a>DataGridView に入力されたデータを検証するには

1. <xref:System.Windows.Forms.Form> から派生し、<xref:System.Windows.Forms.DataGridView> コントロールと <xref:System.Windows.Forms.BindingSource> コンポーネントを含むクラスを作成します。

    次のコード例では、基本的な初期化を行い、`Main` メソッドを含めています。

    [!code-csharp[System.Windows.Forms.DataGridViewDataValidation#01](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/CS/datavalidation.cs#01)]
    [!code-vb[System.Windows.Forms.DataGridViewDataValidation#01](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/VB/datavalidation.vb#01)]
    [!code-csharp[System.Windows.Forms.DataGridViewDataValidation#02](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/CS/datavalidation.cs#02)]
    [!code-vb[System.Windows.Forms.DataGridViewDataValidation#02](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/VB/datavalidation.vb#02)]

2. データベースへの接続の詳細を処理するために、フォームのクラス定義にメソッドを実装します。

    このコード例では、設定された <xref:System.Data.DataTable> オブジェクトを返す `GetData` メソッドを使用します。 `connectionString` 変数は、データベースに適した値に設定してください。

    > [!IMPORTANT]
    > 接続文字列内に機密情報 (パスワードなど) を格納すると、アプリケーションのセキュリティに影響を及ぼすことがあります。 Windows 認証 (統合セキュリティとも呼ばれます) を使用すると、データベースへのアクセスをより安全に制御することができます。 詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。

    [!code-csharp[System.Windows.Forms.DataGridViewDataValidation#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/CS/datavalidation.cs#30)]
    [!code-vb[System.Windows.Forms.DataGridViewDataValidation#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/VB/datavalidation.vb#30)]

3. フォームの <xref:System.Windows.Forms.Form.Load> イベントのハンドラーを実装します。このハンドラーは、<xref:System.Windows.Forms.DataGridView> を初期化し <xref:System.Windows.Forms.BindingSource>、データバインディングを設定します。

    [!code-csharp[System.Windows.Forms.DataGridViewDataValidation#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/CS/datavalidation.cs#10)]
    [!code-vb[System.Windows.Forms.DataGridViewDataValidation#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/VB/datavalidation.vb#10)]

4. <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CellValidating> イベントと <xref:System.Windows.Forms.DataGridView.CellEndEdit> イベントのハンドラーを実装します。

    <xref:System.Windows.Forms.DataGridView.CellValidating> イベントハンドラーでは、`CompanyName` 列のセルの値が空であるかどうかを判断します。 セル値が検証に失敗した場合は、<xref:System.Windows.Forms.DataGridViewCellValidatingEventArgs?displayProperty=nameWithType> クラスの <xref:System.ComponentModel.CancelEventArgs.Cancel%2A> プロパティを `true`に設定します。 これにより、<xref:System.Windows.Forms.DataGridView> コントロールによって、カーソルがセルから離れるのを防ぐことができます。 行の <xref:System.Windows.Forms.DataGridViewRow.ErrorText%2A> プロパティを説明の文字列に設定します。 エラーアイコンと、エラーテキストを含むツールヒントが表示されます。 <xref:System.Windows.Forms.DataGridView.CellEndEdit> イベントハンドラーで、行の <xref:System.Windows.Forms.DataGridViewRow.ErrorText%2A> プロパティを空の文字列に設定します。 <xref:System.Windows.Forms.DataGridView.CellEndEdit> イベントは、セルが編集モードを終了したときにのみ発生します。これは、検証に失敗した場合には実行できません。

    [!code-csharp[System.Windows.Forms.DataGridViewDataValidation#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/CS/datavalidation.cs#20)]
    [!code-vb[System.Windows.Forms.DataGridViewDataValidation#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewDataValidation/VB/datavalidation.vb#20)]

## <a name="testing-the-application"></a>アプリケーションのテスト

フォームをテストして、期待どおりに動作することを確認します。

#### <a name="to-test-the-form"></a>フォームをテストするには

- アプリケーションをコンパイルして実行します。

  `Customers` テーブルのデータが入力された <xref:System.Windows.Forms.DataGridView> が表示されます。 [`CompanyName`] 列のセルをダブルクリックすると、値を編集できます。 すべての文字を削除し、TAB キーを押してセルを終了すると、<xref:System.Windows.Forms.DataGridView> によって終了できなくなります。 空でない文字列をセルに入力すると、<xref:System.Windows.Forms.DataGridView> コントロールによってセルを終了できます。

## <a name="next-steps"></a>次の手順

このアプリケーションでは、<xref:System.Windows.Forms.DataGridView> コントロールの機能についての基本的な理解を得ることができます。 <xref:System.Windows.Forms.DataGridView> コントロールの外観と動作は、次のいくつかの方法でカスタマイズできます。

- 罫線とヘッダーのスタイルを変更します。 詳細については、「[方法: Windows フォーム DataGridView コントロールの境界線とグリッド線のスタイルを変更する](change-the-border-and-gridline-styles-in-the-datagrid.md)」を参照してください。

- <xref:System.Windows.Forms.DataGridView> コントロールへのユーザー入力を有効または制限します。 詳細については、「[方法: Windows フォーム Datagridview コントロールで行の追加と削除を回避](prevent-row-addition-and-deletion-datagridview.md)する」および「[方法: Windows フォーム Datagridview コントロールで列を読み取り](how-to-make-columns-read-only-in-the-windows-forms-datagridview-control.md)専用にする」を参照してください。

- データベース関連のエラーについては、ユーザー入力を確認してください。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーの処理](handling-errors-that-occur-during-data-entry-in-the-datagrid.md)」を参照してください。

- 仮想モードを使用して非常に大きなデータセットを処理します。 詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。

- セルの外観をカスタマイズします。 詳細については、「[方法: Windows フォーム Datagridview コントロールのセルの外観をカスタマイズ](customize-the-appearance-of-cells-in-the-datagrid.md)する」および「[方法: Windows フォーム Datagridview コントロールのフォントと色のスタイルを設定する](how-to-set-font-and-color-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.BindingSource>
- [Windows フォーム DataGridView コントロールでのデータ入力](data-entry-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのデータを検証する](how-to-validate-data-in-the-windows-forms-datagridview-control.md)
- [チュートリアル: Windows フォーム DataGridView コントロールでのデータ入力中に発生したエラーの処理](handling-errors-that-occur-during-data-entry-in-the-datagrid.md)
- [接続情報の保護](../../data/adonet/protecting-connection-information.md)
