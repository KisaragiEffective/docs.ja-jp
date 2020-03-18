---
title: 'チュートリアル: 2 つの DataGridView コントロールを使用してマスター/詳細フォームを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], master/detail form
- parent-child tables [Windows Forms], displaying on Windows Forms
- master-details lists [Windows Forms], displaying on Windows Forms
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: c5fa29e8-47f7-4691-829b-0e697a691f36
ms.openlocfilehash: bb3da36430c7493b941f3711cb584b4517d5bd54
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740585"
---
# <a name="walkthrough-creating-a-masterdetail-form-using-two-windows-forms-datagridview-controls"></a>チュートリアル : Windows フォームの 2 つの DataGridView コントロールを使用したマスター/詳細形式のフォームの作成

<xref:System.Windows.Forms.DataGridView> コントロールを使用する最も一般的なシナリオの1つとして、*マスター/詳細*フォームがあります。このフォームでは、2つのデータベーステーブル間の親子リレーションシップが表示されます。 マスターテーブル内の行を選択すると、対応する子データで詳細テーブルが更新されます。

マスター/詳細フォームの実装は、<xref:System.Windows.Forms.DataGridView> コントロールと <xref:System.Windows.Forms.BindingSource> コンポーネントの間の相互作用を使用して簡単に行うことができます。 このチュートリアルでは、2つの <xref:System.Windows.Forms.DataGridView> コントロールと2つの <xref:System.Windows.Forms.BindingSource> コンポーネントを使用してフォームをビルドします。 フォームでは、Northwind SQL Server サンプルデータベースに2つの関連テーブルが表示されます。 `Customers` と `Orders`です。 完了すると、マスター <xref:System.Windows.Forms.DataGridView> 内のデータベース内のすべての顧客と、詳細 <xref:System.Windows.Forms.DataGridView>で選択した顧客のすべての注文を表示するフォームが作成されます。

このトピックのコードを単一のリストとしてコピーする方法については、「[方法: 2 つの Windows フォーム DataGridView コントロールを使用してマスター/詳細フォームを作成](create-a-master-detail-form-using-two-datagridviews.md)する」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、以下が必要です。

- Northwind SQL Server サンプルデータベースを持つサーバーへのアクセス。

## <a name="creating-the-form"></a>フォームの作成

#### <a name="to-create-a-masterdetail-form"></a>マスター/詳細フォームを作成するには

1. <xref:System.Windows.Forms.Form> から派生し、2つの <xref:System.Windows.Forms.DataGridView> コントロールと2つの <xref:System.Windows.Forms.BindingSource> コンポーネントを含むクラスを作成します。 次のコードは、基本的なフォーム初期化を提供し、`Main` メソッドを含んでいます。 Visual Studio デザイナーを使用してフォームを作成する場合は、このコードの代わりにデザイナーで生成されたコードを使用できますが、ここでは変数宣言に示されている名前を使用してください。

    [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#01](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#01)]
    [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#01](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#01)]
    [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#02](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#02)]
    [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#02](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#02)]

2. データベースへの接続の詳細を処理するために、フォームのクラス定義にメソッドを実装します。 この例では、`GetData` メソッドを使用して、<xref:System.Data.DataSet> オブジェクトを設定し、<xref:System.Data.DataRelation> オブジェクトをデータセットに追加して、<xref:System.Windows.Forms.BindingSource> コンポーネントをバインドします。 `connectionString` 変数は、使用しているデータベースに合った値に設定してください。

    > [!IMPORTANT]
    > 接続文字列内に機密情報 (パスワードなど) を格納すると、アプリケーションのセキュリティに影響を及ぼすことがあります。 データベースへのアクセスを制御する方法としては、Windows 認証 (統合セキュリティとも呼ばれます) を使用する方が安全です。 詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。

    [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#20)]
    [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#20)]

3. フォームの <xref:System.Windows.Forms.Form.Load> イベントのハンドラーを実装します。このハンドラーは、<xref:System.Windows.Forms.DataGridView> コントロールを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドし、`GetData` メソッドを呼び出します。 次の例には、表示データに合わせて列の <xref:System.Windows.Forms.DataGridView> サイズを変更するコードが含まれています。

    [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#10)]
    [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#10)]

## <a name="testing-the-application"></a>アプリケーションのテスト

フォームをテストして、期待どおりに動作することを確認します。

#### <a name="to-test-the-form"></a>フォームをテストするには

- アプリケーションをコンパイルして実行します。

  2つの <xref:System.Windows.Forms.DataGridView> コントロールが表示されます。 [上位] は Northwind `Customers` テーブルの顧客であり、下部は選択した顧客に対応する `Orders` です。 上部 <xref:System.Windows.Forms.DataGridView>で異なる行を選択すると、それに応じて低い <xref:System.Windows.Forms.DataGridView> の内容が変更されます。

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
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォームの 2 つの DataGridView コントロールを使用してマスター/詳細形式のフォームを作成する](create-a-master-detail-form-using-two-datagridviews.md)
- [接続情報の保護](../../data/adonet/protecting-connection-information.md)
