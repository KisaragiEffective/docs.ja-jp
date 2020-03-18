---
title: NotifyIcon コンポーネントを使用してアプリケーションアイコンをタスクバーに追加する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- TrayIcon
helpviewer_keywords:
- status area icons
- icons [Windows Forms], adding to taskbar
- NotifyIcon component
- taskbar [Windows Forms], adding icons
ms.assetid: d28c0fe6-aaf2-4df7-ad74-928d861a8510
ms.openlocfilehash: 46b50ecaabe75dba08fea922d7b5639031692269
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746250"
---
# <a name="how-to-add-application-icons-to-the-taskbar-with-the-windows-forms-notifyicon-component"></a>方法 : Windows フォームの NotifyIcon コンポーネントによってタスクバーにアプリケーション アイコンを追加する

Windows フォーム <xref:System.Windows.Forms.NotifyIcon> コンポーネントでは、タスクバーの状態通知領域に1つのアイコンが表示されます。 状態領域に複数のアイコンを表示するには、フォームに複数の <xref:System.Windows.Forms.NotifyIcon> コンポーネントが必要です。 コントロールに対して表示されるアイコンを設定するには、<xref:System.Windows.Forms.NotifyIcon.Icon%2A> プロパティを使用します。 また、ユーザーがアイコンをダブルクリックしたときに何かが発生するように、<xref:System.Windows.Forms.NotifyIcon.DoubleClick> イベントハンドラーにコードを記述することもできます。 たとえば、アイコンによって表されるバックグラウンドプロセスをユーザーが構成するためのダイアログボックスを表示することができます。

> [!NOTE]
> <xref:System.Windows.Forms.NotifyIcon> コンポーネントは通知のみを目的として使用され、アクションまたはイベントが発生したこと、または何らかの種類の状態が変更されたことをユーザーに警告します。 アプリケーションとの標準的な対話には、メニュー、ツールバー、およびその他のユーザーインターフェイス要素を使用する必要があります。

### <a name="to-set-the-icon"></a>アイコンを設定するには

1. <xref:System.Windows.Forms.NotifyIcon.Icon%2A> プロパティに値を割り当てます。 値は `System.Drawing.Icon` 型である必要があり、.ico ファイルから読み込むことができます。 アイコンファイルは、コードで指定することも、 **[プロパティ]** ウィンドウの <xref:System.Windows.Forms.NotifyIcon.Icon%2A> プロパティの横にある省略記号ボタン ([.. プロパティウィンドウ.]) を![クリックして指定することもできます。その後、表示される **[開く]** ダイアログボックスでファイルを選択します](./media/visual-studio-ellipsis-button.png)。

2. <xref:System.Windows.Forms.NotifyIcon.Visible%2A> プロパティを `true` に設定します。

3. <xref:System.Windows.Forms.NotifyIcon.Text%2A> プロパティを適切なツールヒント文字列に設定します。

     次のコード例では、アイコンの場所に設定されているパスが **[マイドキュメント**] フォルダーです。 この場所は、Windows オペレーティングシステムを実行しているほとんどのコンピューターにこのフォルダーを含めることを前提としているために使用されます。 また、この場所を選択すると、最小限のシステムアクセスレベルのユーザーがアプリケーションを安全に実行できるようになります。 次の例では、<xref:System.Windows.Forms.NotifyIcon> コントロールが既に追加されているフォームが必要です。 また、`Icon.ico`という名前のアイコンファイルも必要です。

    ```vb
    ' You should replace the bold icon in the sample below
    ' with an icon of your own choosing.
    NotifyIcon1.Icon = New _
       System.Drawing.Icon(System.Environment.GetFolderPath _
       (System.Environment.SpecialFolder.Personal) _
       & "\Icon.ico")
    NotifyIcon1.Visible = True
    NotifyIcon1.Text = "Antivirus program"
    ```

    ```csharp
    // You should replace the bold icon in the sample below
    // with an icon of your own choosing.
    // Note the escape character used (@) when specifying the path.
    notifyIcon1.Icon =
       new System.Drawing.Icon (System.Environment.GetFolderPath
       (System.Environment.SpecialFolder.Personal)
       + @"\Icon.ico");
    notifyIcon1.Visible = true;
    notifyIcon1.Text = "Antivirus program";
    ```

    ```cpp
    // You should replace the bold icon in the sample below
    // with an icon of your own choosing.
    notifyIcon1->Icon = gcnew
       System::Drawing::Icon(String::Concat
       (System::Environment::GetFolderPath
       (System::Environment::SpecialFolder::Personal),
       "\\Icon.ico"));
    notifyIcon1->Visible = true;
    notifyIcon1->Text = "Antivirus program";
    ```

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.NotifyIcon>
- <xref:System.Windows.Forms.NotifyIcon.Icon%2A>
- [方法: ショートカット メニューを Windows フォーム NotifyIcon コンポーネントに関連付ける](how-to-associate-a-shortcut-menu-with-a-windows-forms-notifyicon-component.md)
- [NotifyIcon コンポーネント](notifyicon-component-windows-forms.md)
- [NotifyIcon コンポーネントの概要](notifyicon-component-overview-windows-forms.md)
