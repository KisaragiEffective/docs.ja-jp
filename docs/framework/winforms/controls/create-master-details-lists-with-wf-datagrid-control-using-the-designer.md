---
title: デザイナーを使用して DataGrid コントロールでマスター/詳細リストを作成する
ms.date: 03/30/2017
helpviewer_keywords:
- master-details lists
- DataGrid control [Windows Forms], master-details lists
- related tables [Windows Forms], displaying in DataGrid control
ms.assetid: 19438ba2-f687-4417-a2fb-ab1cd69d4ded
ms.openlocfilehash: 0dea3dd88a8c6c2aceb894789ace8ef3b5c83632
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743389"
---
# <a name="how-to-create-master-details-lists-with-the-windows-forms-datagrid-control-using-the-designer"></a>方法 : デザイナーで Windows フォーム DataGrid コントロールを使用してマスター/詳細リストを作成する

> [!NOTE]
> <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.DataGrid> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。

 <xref:System.Data.DataSet> に一連の関連テーブルが含まれている場合は、2つの <xref:System.Windows.Forms.DataGrid> コントロールを使用して、マスター/詳細形式でデータを表示できます。 1つの <xref:System.Windows.Forms.DataGrid> はマスターグリッドとして指定され、2つ目は詳細グリッドとして指定されます。 マスターリストでエントリを選択すると、関連するすべての子エントリが詳細一覧に表示されます。 たとえば、<xref:System.Data.DataSet> に Customers テーブルと関連する Orders テーブルが含まれている場合は、Customers テーブルをマスターグリッドに指定し、Orders テーブルを詳細グリッドとして指定します。 マスターグリッドから顧客を選択すると、Orders テーブル内のその顧客に関連付けられているすべての注文が詳細グリッドに表示されます。

 次の手順では、 **Windows アプリケーション**プロジェクト (**ファイル** > **新しい** > **プロジェクト** >  **C# Visual**または**Visual Basic** > **クラシックデスクトップ** > **Windows フォームアプリケーション**) が必要です。

## <a name="to-create-a-master-details-list-in-the-designer"></a>デザイナーでマスター/詳細リストを作成するには

1. フォームに2つの <xref:System.Windows.Forms.DataGrid> コントロールを追加します。 詳細については、「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。 Visual Studio 2005 では、<xref:System.Windows.Forms.DataGrid> コントロールは、既定では**ツールボックス**に含まれていません。 詳細については、「[方法: ツールボックスに項目を追加](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms165355(v=vs.100))する」を参照してください。

    > [!NOTE]
    > 次の手順は、デザイン時のデータバインディングに **[データソース]** ウィンドウを使用する Visual Studio 2005 には適用されません。 詳細については、「 [Visual Studio でのデータへのコントロールのバインド](/visualstudio/data-tools/bind-controls-to-data-in-visual-studio)」および「[方法: Windows フォームアプリケーションに関連データを表示する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/57tx3hhe(v=vs.120))」を参照してください。

2. **サーバーエクスプローラー**からフォームに2つ以上のテーブルをドラッグします。

3. **[データ]** メニューの データ **[セットの生成]** を選択します。

4. XML デザイナーを使用して、テーブル間のリレーションシップを設定します。 詳細については、MSDN の「XML スキーマおよびデータセットで一対多のリレーションシップを作成する方法」を参照してください。

5. **[ファイル]** メニューの **[すべてを保存]** を選択して、リレーションシップを保存します。

6. 次のように、マスターグリッドを指定する <xref:System.Windows.Forms.DataGrid> コントロールを構成します。

    1. <xref:System.Windows.Forms.DataGrid.DataSource%2A> プロパティのドロップダウンリストから <xref:System.Data.DataSet> を選択します。

    2. [<xref:System.Windows.Forms.DataGrid.DataMember%2A>] プロパティのドロップダウンリストからマスターテーブル (たとえば、"Customers") を選択します。

7. 詳細グリッドを指定する <xref:System.Windows.Forms.DataGrid> コントロールを次のように構成します。

    1. <xref:System.Windows.Forms.DataGrid.DataSource%2A> プロパティのドロップダウンリストから <xref:System.Data.DataSet> を選択します。

    2. <xref:System.Windows.Forms.DataGrid.DataMember%2A> プロパティのドロップダウンリストから、マスターテーブルと詳細テーブルの間のリレーションシップ (例: "CustOrd") を選択します。 リレーションシップを表示するには、ドロップダウンリストでマスターテーブルの横にあるプラス記号 ( **+** ) をクリックして、ノードを展開します。

## <a name="see-also"></a>参照

- [DataGrid コントロール](datagrid-control-windows-forms.md)
- [DataGrid コントロールの概要](datagrid-control-overview-windows-forms.md)
- [方法: データ ソースに Windows フォーム DataGrid コントロールをバインドする](how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)
- [Visual Studio でのデータへのコントロールのバインド](/visualstudio/data-tools/bind-controls-to-data-in-visual-studio)
