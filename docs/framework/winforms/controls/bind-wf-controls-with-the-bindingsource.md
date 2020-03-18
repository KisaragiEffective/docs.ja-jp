---
title: デザイナーを使用してコントロールを BindingSource コンポーネントにバインドする
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], binding
- BindingSource component [Windows Forms], binding controls
- data binding [Windows Forms], BindingSource component
ms.assetid: 391ae170-de5c-40f8-8233-91cb2ee4683a
ms.openlocfilehash: 35b3fb7b9884f07dd6e2aef311a01d3090c44227
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744389"
---
# <a name="how-to-bind-windows-forms-controls-with-the-bindingsource-component-using-the-designer"></a>方法 : デザイナーを使用して Windows フォーム コントロールを BindingSource コンポーネントにバインドする
フォームにコントロールを追加し、アプリケーションのユーザーインターフェイスを決定したら、コントロールをデータソースにバインドできます。これにより、ユーザーがアプリケーションに関連するデータを変更したり保存したりできるようになります。

 Windows フォームのコントロールまたは一連のコントロールのバインドは、フォーム上のコントロールとデータソースの間のブリッジとして <xref:System.Windows.Forms.BindingSource> コントロールを使用して、最も簡単に実現できます。

 フォーム上の1つ以上のコントロールをデータにバインドできます。次の手順では、<xref:System.Windows.Forms.TextBox> コントロールがデータソースにバインドされます。

 この手順を完了するには、データベースから派生したデータソースにバインドすることを前提としています。 他のデータストアからデータソースを作成する方法の詳細については、「[新しいデータソースの追加](/visualstudio/data-tools/add-new-data-sources)」を参照してください。

## <a name="to-bind-a-control-at-design-time"></a>デザイン時にコントロールをバインドするには

1. <xref:System.Windows.Forms.TextBox> コントロールをフォームにドラッグします。

2. **[プロパティ]** ウィンドウで、次のようにします。

    1. **[(連結)]** ノードを展開します。

    2. <xref:System.Windows.Forms.TextBox.Text%2A> プロパティの横にある矢印をクリックします。

         **DataSource** UI 型エディターが開きます。

         データソースが既にプロジェクトまたはフォーム用に構成されている場合は、そのデータソースが表示されます。

3. **[プロジェクト データ ソースの追加]** をクリックしてデータに接続し、データ ソースを作成します。

4. **データ ソース構成ウィザード**の開始ページで **[次へ]** をクリックします。

5. **[データソースの種類を選択]** ページで、 **[データベース]** を選択します。

6. **[データ接続の選択]** ページで、使用可能な接続の一覧からデータ接続を選択します。 目的のデータ接続が使用できない場合は、 **[新しい接続]** を選択して新しいデータ接続を作成します。

7. [**はい、接続を保存**します] を選択して、アプリケーション構成ファイルに接続文字列を保存します。

8. アプリケーションで使用するデータベース オブジェクトを選択します。 この場合は、<xref:System.Windows.Forms.TextBox> を表示するテーブルのフィールドを選択します。

9. 必要な場合は、既定のデータセット名を変更します。

10. **[完了]** をクリックします。

11. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.TextBox.Text%2A>] プロパティの横にある矢印をもう一度クリックします。 **DataSource** UI 型エディターで、<xref:System.Windows.Forms.TextBox> をバインドするフィールドの名前を選択します。

     **DataSource** UI 型エディターが閉じ、そのデータ接続に固有のデータセット、<xref:System.Windows.Forms.BindingSource> およびテーブルアダプターがフォームに追加されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingSource>
- <xref:System.Windows.Forms.BindingNavigator>
- [新しいデータ ソースの追加](/visualstudio/data-tools/add-new-data-sources)
- [データソースウィンドウ](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/6ckyxa83(v=vs.120))
