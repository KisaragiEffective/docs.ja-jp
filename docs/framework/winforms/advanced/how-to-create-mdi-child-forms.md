---
title: '方法 : MDI 子フォームを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- MDI [Windows Forms], creating forms
- child forms
ms.assetid: 164b69bb-2eca-4339-ada3-0679eb2c6dda
ms.openlocfilehash: b49d43e0e1123921cb3800f0d60193d0ea7b3924
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75338592"
---
# <a name="how-to-create-mdi-child-forms"></a>方法: MDI 子フォームを作成する

MDI 子フォームは、ユーザー操作の中心であるため、[マルチドキュメントインターフェイス (mdi) アプリケーション](multiple-document-interface-mdi-applications.md)の重要な要素です。

次の手順では、Visual Studio を使用して、ほとんどのワードプロセッシングアプリケーションと同様に <xref:System.Windows.Forms.RichTextBox> コントロールを表示する MDI 子フォームを作成します。 <xref:System.Windows.Forms> コントロールを、<xref:System.Windows.Forms.DataGridView> コントロールなどの他のコントロール、またはコントロールの組み合わせに置き換えることにより、さまざまな可能性を持つ MDI 子ウィンドウ (および拡張機能、MDI アプリケーション) を作成できます。

## <a name="create-mdi-child-forms"></a>MDI 子フォームを作成する

1. Visual Studio で新しい Windows フォームアプリケーションプロジェクトを作成します。 フォームの **プロパティ** ウィンドウで、<xref:System.Windows.Forms.Form.IsMdiContainer%2A> プロパティを `true` に設定し、その `WindowsState` プロパティを `Maximized` に設定します。

   これによって、フォームが子ウィンドウの MDI コンテナーとして指定されます。

2. `Toolbox` から、<xref:System.Windows.Forms.MenuStrip> コントロールをフォームにドラッグします。 `Text` プロパティを**File**に設定します。

3. **Items**プロパティの横にある省略記号 (...) をクリックし、**追加** をクリックして、2つの子ツールストリップメニュー項目を追加します。 これらの項目の `Text` プロパティを**新規**および**ウィンドウ**に設定します。

4. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、 **[追加]**  >  **[新しい項目]** を選択します。

5. **[新しい項目の追加]** ダイアログボックスで、 **[Windows フォーム]** (Visual Basic またC#は visual では) を選択するか、 C++ **[テンプレート]** ペインの **[アプリケーション (.net)]** (visual の場合) を Windows フォームます。 **[名前]** ボックスに、「 **Form2**」という形式で名前を指定します。 **[開く]** を選択して、フォームをプロジェクトに追加します。

    > [!NOTE]
    > この手順で作成した、MDI 子フォームは、標準の Windows フォームです。 そのため、フォームの透明度を制御できる <xref:System.Windows.Forms.Form.Opacity%2A> プロパティを持っています。 ただし、<xref:System.Windows.Forms.Form.Opacity%2A> プロパティは最上位レベルのウィンドウ用に設計されています。 描画に関する問題が発生する可能性があるため、MDI 子フォームと共に使用しないでください。

     このフォームは、MDI 子フォーム用のテンプレートになります。

     **Windows フォームデザイナー**が開き、 **Form2**が表示されます。

6. **ツールボックス**から、 **RichTextBox**コントロールをフォームにドラッグします。

7. **[プロパティ]** ウィンドウで、[`Anchor`] プロパティを [**上]、[左**]、[`Dock`] プロパティを **[塗りつぶし]** に設定します。

   これにより、フォームがサイズ変更された場合でも、<xref:System.Windows.Forms.RichTextBox> コントロールが MDI 子フォームの領域を完全に塗りつぶします。

8. **[新規]** メニュー項目をダブルクリックして、その <xref:System.Windows.Forms.Control.Click> イベントハンドラーを作成します。

9. 次のようなコードを挿入して、ユーザーが **[新規]** メニュー項目をクリックしたときに新しい MDI 子フォームを作成します。

   > [!NOTE]
   > 次の例では、イベント ハンドラーが `MenuItem2` の <xref:System.Windows.Forms.Control.Click> イベントを処理します。 アプリケーションアーキテクチャの詳細によっては、**新しい**メニュー項目が `MenuItem2`ない場合があることに注意してください。

    ```vb
    Protected Sub MDIChildNew_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MenuItem2.Click
       Dim NewMDIChild As New Form2()
       'Set the Parent Form of the Child window.
       NewMDIChild.MdiParent = Me
       'Display the new form.
       NewMDIChild.Show()
    End Sub
    ```

    ```csharp
    protected void MDIChildNew_Click(object sender, System.EventArgs e){
       Form2 newMDIChild = new Form2();
       // Set the Parent Form of the Child window.
       newMDIChild.MdiParent = this;
       // Display the new form.
       newMDIChild.Show();
    }
    ```

    ```cpp
    private:
       void menuItem2_Click(System::Object ^ sender,
          System::EventArgs ^ e)
       {
          Form2^ newMDIChild = gcnew Form2();
          // Set the Parent Form of the Child window.
          newMDIChild->MdiParent = this;
          // Display the new form.
          newMDIChild->Show();
       }
    ```

   でC++、Form1 の先頭に次の `#include` ディレクティブを追加します。

   ```cpp
   #include "Form2.h"
   ```

10. **[プロパティ]** ウィンドウの上部にあるドロップダウンリストで、 **[ファイル]** メニューストリップに対応するメニューストリップを選択し、[<xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A>] プロパティをウィンドウ <xref:System.Windows.Forms.ToolStripMenuItem>に設定します。

    これにより、**ウィンドウ**メニューで、アクティブな子ウィンドウの横にチェックマークが付いた、開いている MDI 子ウィンドウの一覧を保持できます。

11. **F5** キーを押してアプリケーションを実行します。 **[ファイル]** メニューの **[新規]** 作成 を選択すると、新しい MDI 子フォームを作成できます。この子フォームは、 **[ウィンドウ]** メニュー項目に記録されます。

    > [!NOTE]
    > MDI 子フォームが (通常はメニュー項目のメニュー構造を持つ) <xref:System.Windows.Forms.MainMenu> コンポーネントを持っていて、(通常はメニュー項目のメニュー構造を持つ) <xref:System.Windows.Forms.MainMenu> コンポーネントを持つ MDI 親フォーム内で開いている場合、<xref:System.Windows.Forms.MenuItem.MergeType%2A> プロパティ (およびオプションで <xref:System.Windows.Forms.MenuItem.MergeOrder%2A> プロパティ) を設定した場合に、メニュー項目が自動的にマージされます。 両方の <xref:System.Windows.Forms.MainMenu> コンポーネント、および子フォームのすべてのメニュー項目の <xref:System.Windows.Forms.MenuItem.MergeType%2A> プロパティを <xref:System.Windows.Forms.MenuMerge.MergeItems> に設定します。 また、<xref:System.Windows.Forms.MenuItem.MergeOrder%2A> プロパティを設定し、両方のメニューのメニュー項目が指定した順序で表示されるようにします。 さらに、MDI 親フォームを閉じた時に、MDI 親の <xref:System.Windows.Forms.Form.Closing> イベントが発生する前に、各 MDI 子フォームが <xref:System.Windows.Forms.Form.Closing> イベントを発生させます。 MDI 子の <xref:System.Windows.Forms.Form.Closing> イベントをキャンセルしても、MDI 親の <xref:System.Windows.Forms.Form.Closing> イベントの発生を防ぐことはできません。ただし、MDI 親の <xref:System.Windows.Forms.Form.Closing> イベントの <xref:System.ComponentModel.CancelEventArgs> 引数は `true` に設定されます。 <xref:System.ComponentModel.CancelEventArgs> 引数を `false` に設定することで、MDI 親レポートとすべての MDI 子フォームを強制的に閉じることができます。

## <a name="see-also"></a>関連項目

- [マルチ ドキュメント インターフェイス (MDI) アプリケーション](multiple-document-interface-mdi-applications.md)
- [方法: MDI 親フォームを作成する](how-to-create-mdi-parent-forms.md)
- [方法: アクティブな MDI 子フォームを特定する](how-to-determine-the-active-mdi-child.md)
- [方法: アクティブな MDI 子フォームにデータを送信する](how-to-send-data-to-the-active-mdi-child.md)
- [方法: MDI 子フォームを配置する](how-to-arrange-mdi-child-forms.md)
