---
title: BindingNavigator コントロールに [Load]、[Save]、[Cancel] の各ボタンを追加する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [Windows Forms], manipulating
- BindingNavigator control [Windows Forms], adding buttons
ms.assetid: faa33042-186e-4bb2-8798-17ceb987ec62
ms.openlocfilehash: 0305df5e7566f9441ce09fa3346a8b2dc67c8943
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77627969"
---
# <a name="how-to-add-load-save-and-cancel-buttons-to-the-windows-forms-bindingnavigator-control"></a>方法 : Windows フォーム BindingNavigator コントロールに [Load]、[Save]、[Cancel] の各ボタンを追加する

<xref:System.Windows.Forms.BindingNavigator> コントロールは、データにバインドされているフォーム上のコントロールの移動や操作を目的とした特殊な用途の <xref:System.Windows.Forms.ToolStrip> コントロールです。

これは <xref:System.Windows.Forms.ToolStrip> コントロールであるため、<xref:System.Windows.Forms.BindingNavigator> コンポーネントを簡単に変更して、ユーザーの追加または代替コマンドを含めることができます。

次の手順では、<xref:System.Windows.Forms.TextBox> コントロールがデータにバインドされ、フォームに追加された <xref:System.Windows.Forms.ToolStrip> コントロールが、[読み込み]、[保存]、および [キャンセル] の各ボタンを含むように変更されます。

## <a name="add-load-save-and-cancel-buttons-to-the-bindingnavigator-component"></a>[Load]、[save]、[cancel] の各ボタンを BindingNavigator コンポーネントに追加します。

1. Visual Studio で、フォームに <xref:System.Windows.Forms.TextBox> コントロールを追加します。

2. データソースにバインドされている <xref:System.Windows.Forms.BindingSource>にバインドします。 この例では、<xref:System.Windows.Forms.BindingSource> はデータベースにバインドされています。

3. データセットとテーブルアダプターが生成されたら、<xref:System.Windows.Forms.BindingNavigator> コントロールをフォームにドラッグします。

4. コントロールにバインドされているフォームの <xref:System.Windows.Forms.BindingSource> に <xref:System.Windows.Forms.BindingNavigator> コントロールの <xref:System.Windows.Forms.BindingNavigator.BindingSource%2A> プロパティを設定します。

5. <xref:System.Windows.Forms.BindingNavigator> コントロールを選択します。

6. [デザイナーアクション] グリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックして **[BindingNavigator タスク]** ダイアログボックスを表示し、 **[アイテムの編集]** を選択します。

     **項目コレクションエディター**が表示されます。

7. **Items コレクションエディター**で、次の手順を実行します。

    1. 適切な種類の <xref:System.Windows.Forms.ToolStripItem> を選択し、 **[追加]** ボタンをクリックして、<xref:System.Windows.Forms.ToolStripSeparator> と3つの <xref:System.Windows.Forms.ToolStripButton> 項目を追加します。

    2. ボタンの <xref:System.Windows.Forms.ToolStripItem.Name%2A> プロパティを**Loadbutton**、 **savebutton**、および**cancelbutton**にそれぞれ設定します。

    3. **[読み込み]** 、 **[保存]** 、 **[キャンセル]** の各ボタンの <xref:System.Windows.Forms.ToolStripItem.Text%2A> プロパティを設定します。

    4. 各ボタンの [<xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A>] プロパティを **[テキスト]** に設定します。 または、このプロパティを**image**または**imageandtext**に設定し、<xref:System.Windows.Forms.ToolStripItem.Image%2A> プロパティに表示されるようにイメージを設定することもできます。

    5. **[OK]** をクリックして、ダイアログ ボックスを閉じます。 ボタンが <xref:System.Windows.Forms.ToolStrip>に追加されます。

8. フォームを右クリックし、 **[コードの表示]** を選択します。

9. コードエディターで、テーブルアダプターにデータを読み込むコード行を見つけます。 このコードは、手順 2. でデータバインディングを設定したときに生成されました。 コードは次のようになります。 `TableAdapterName.Fill(DataSetName.TableName)`。 通常、フォームの <xref:System.Windows.Forms.Form.Load> イベントに含まれます。

10. 前に作成した**読み込み**<xref:System.Windows.Forms.ToolStripButton> の <xref:System.Windows.Forms.ToolStripItem.Click> イベントのイベントハンドラーを作成し、このデータ読み込みコードをそこに移動します。

     これで、コードは次のようになります。

    ```vb
    Private Sub LoadButton_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles LoadButton.Click
        TableAdapterName.Fill(DataSetName.TableName)
    End Sub
    ```

    ```csharp
    private void LoadButton_Click(System.Object sender,
        System.EventArgs e)
    {
        TableAdapterName.Fill(DataSetName.TableName);
    }
    ```

11. 前に作成した**保存**<xref:System.Windows.Forms.ToolStripButton> の <xref:System.Windows.Forms.ToolStripItem.Click> イベントのイベントハンドラーを作成し、コントロールがバインドされているテーブル内のデータを更新するコードを記述します。

    ```vb
    Private Sub SaveButton_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles SaveButton.Click
        TableAdapterName.Update(DataSetName.TableName)
    End Sub
    ```

    ```csharp
    private void SaveButton_Click(System.Object sender,
        System.EventArgs e)
    {
        TableAdapterName.Update(DataSetName.TableName);
    }
    ```

    > [!NOTE]
    > <xref:System.Windows.Forms.BindingNavigator> コンポーネントには既に **[保存]** ボタンがありますが、Windows フォームデザイナーによってコードが生成されていない場合もあります。 この場合、<xref:System.Windows.Forms.ToolStrip>にまったく新しいボタンを作成するのではなく、そのボタンの <xref:System.Windows.Forms.ToolStripItem.Click> イベントハンドラーに上記のコードを配置できます。 ただし、ボタンは既定では無効になっているため、ボタンが正しく機能するようにするには、ボタンの <xref:System.Windows.Forms.ToolBarButton.Enabled%2A> プロパティを `true` に設定する必要があります。

12. 前に作成した**cancel** <xref:System.Windows.Forms.ToolStripButton> の <xref:System.Windows.Forms.ToolStripItem.Click> イベントのイベントハンドラーを作成し、表示されるデータレコードへの変更を取り消すコードを記述します。

    ```vb
    Private Sub CancelButton_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles CancelButton.Click
        BindingSourceName.CancelEdit()
    End Sub
    ```

    ```csharp
    private void CancelButton_Click(System.Object sender, System.EventArgs e)
    {
        BindingSourceName.CancelEdit();
    }
    ```

    > [!NOTE]
    > <xref:System.Windows.Forms.BindingSource.CancelEdit%2A> メソッドは、データの行にスコープが設定されています。 次のレコードに移動する前に、個々のレコードを表示しているときに行ったすべての変更を保存します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingNavigator>
- <xref:System.Windows.Forms.BindingSource>
- <xref:System.Windows.Forms.ToolStrip>
- [BindingNavigator コントロール](bindingnavigator-control-windows-forms.md)
- [BindingSource コンポーネントの概要](bindingsource-component-overview.md)
