---
title: ショートカットメニューを NotifyIcon コンポーネントに関連付ける
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- context menus [Windows Forms], for background processes
- NotifyIcon component [Windows Forms], associating shortcut menus
- shortcut menus [Windows Forms], for background processes
ms.assetid: d68f3926-08d3-4f7d-949f-1981b29cf188
ms.openlocfilehash: 392c04f73feaec201033ad76f9419a0e070bec70
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742049"
---
# <a name="how-to-associate-a-shortcut-menu-with-a-windows-forms-notifyicon-component"></a>方法 : ショートカット メニューを Windows フォーム NotifyIcon コンポーネントに関連付ける
> [!NOTE]
> <xref:System.Windows.Forms.MenuStrip> と <xref:System.Windows.Forms.ContextMenuStrip> によって、以前のバージョンの <xref:System.Windows.Forms.MainMenu> および <xref:System.Windows.Forms.ContextMenu> のコントロールに置き換えられ、機能が追加されますが、<xref:System.Windows.Forms.MainMenu> と <xref:System.Windows.Forms.ContextMenu> は下位互換性と将来の使用の両方のために保持されます。  
  
 <xref:System.Windows.Forms.NotifyIcon> コンポーネントでは、タスクバーの状態通知領域にアイコンが表示されます。 一般的に、アプリケーションでは、このアイコンを右クリックして、表示されるアプリケーションにコマンドを送信することができます。 <xref:System.Windows.Forms.ContextMenu> コンポーネントを <xref:System.Windows.Forms.NotifyIcon> コンポーネントに関連付けることにより、この機能をアプリケーションに追加できます。  
  
> [!NOTE]
> タスクバーに <xref:System.Windows.Forms.NotifyIcon> コンポーネントのインスタンスを表示しているときに、起動時にアプリケーションを最小化する場合は、メインフォームの <xref:System.Windows.Forms.Form.WindowState%2A> プロパティを <xref:System.Windows.Forms.FormWindowState.Minimized> に設定し、<xref:System.Windows.Forms.NotifyIcon> コンポーネントの <xref:System.Windows.Forms.NotifyIcon.Visible%2A> プロパティが `true`に設定されていることを確認します。  
  
### <a name="to-associate-a-shortcut-menu-with-the-notifyicon-component-at-design-time"></a>デザイン時にショートカットメニューを NotifyIcon コンポーネントに関連付けるには  
  
1. フォームに <xref:System.Windows.Forms.NotifyIcon> コンポーネントを追加し、<xref:System.Windows.Forms.NotifyIcon.Icon%2A> や <xref:System.Windows.Forms.NotifyIcon.Visible%2A> プロパティなどの重要なプロパティを設定します。  
  
     詳細については、「[方法: Windows フォーム NotifyIcon コンポーネントを使用してアプリケーションアイコンをタスクバーに追加する](app-icons-to-the-taskbar-with-wf-notifyicon.md)」を参照してください。  
  
2. Windows フォームに <xref:System.Windows.Forms.ContextMenu> コンポーネントを追加します。  
  
     実行時に使用できるようにするコマンドを表すメニュー項目をショートカットメニューに追加します。 これは、アクセスキーなどのメニュー項目にメニューの機能強化を追加する場合にも適しています。  
  
3. <xref:System.Windows.Forms.NotifyIcon> コンポーネントの [<xref:System.Windows.Forms.NotifyIcon.ContextMenu%2A>] プロパティを、追加したショートカットメニューに設定します。  
  
     このプロパティを設定すると、タスクバーのアイコンがクリックされたときにショートカットメニューが表示されます。  
  
### <a name="to-associate-a-shortcut-menu-with-the-notifyicon-component-programmatically"></a>プログラムによってショートカットメニューを NotifyIcon コンポーネントに関連付けるには  
  
1. <xref:System.Windows.Forms.NotifyIcon> クラスのインスタンスと <xref:System.Windows.Forms.ContextMenu> クラスを作成します。このクラスには、アプリケーションに必要なプロパティ設定があります (<xref:System.Windows.Forms.NotifyIcon.Icon%2A> および <xref:System.Windows.Forms.NotifyIcon.Visible%2A> プロパティの <xref:System.Windows.Forms.NotifyIcon> コンポーネントのメニュー項目、<xref:System.Windows.Forms.ContextMenu> コンポーネントのメニュー項目)。  
  
2. <xref:System.Windows.Forms.NotifyIcon> コンポーネントの [<xref:System.Windows.Forms.NotifyIcon.ContextMenu%2A>] プロパティを、追加したショートカットメニューに設定します。  
  
     このプロパティを設定すると、タスクバーのアイコンがクリックされたときにショートカットメニューが表示されます。  
  
    > [!NOTE]
    > 次のコード例では、基本的なメニュー構造を作成します。 メニューの選択肢は、開発中のアプリケーションに適したものにカスタマイズする必要があります。 また、これらのメニュー項目の <xref:System.Windows.Forms.MenuItem.Click> イベントを処理するコードを記述することもできます。  
  
    ```vb  
    Public ContextMenu1 As New ContextMenu  
    Public NotifyIcon1 As New NotifyIcon  
  
    Public Sub CreateIconMenuStructure()  
       ' Add menu items to shortcut menu.  
       ContextMenu1.MenuItems.Add("&Open Application")  
       ContextMenu1.MenuItems.Add("S&uspend Application")  
       ContextMenu1.MenuItems.Add("E&xit")  
  
       ' Set properties of NotifyIcon component.  
       NotifyIcon1.Icon = New System.Drawing.Icon _   
          (System.Environment.GetFolderPath _   
          (System.Environment.SpecialFolder.Personal)  _   
          & "\Icon.ico")  
       NotifyIcon1.Text = "Right-click me!"  
       NotifyIcon1.Visible = True  
       NotifyIcon1.ContextMenu = ContextMenu1  
    End Sub  
    ```  
  
```csharp  
public NotifyIcon notifyIcon1 = new NotifyIcon();  
public ContextMenu contextMenu1 = new ContextMenu();  
  
public void createIconMenuStructure()  
{  
   // Add menu items to shortcut menu.  
   contextMenu1.MenuItems.Add("&Open Application");  
   contextMenu1.MenuItems.Add("S&uspend Application");  
   contextMenu1.MenuItems.Add("E&xit");  
  
   // Set properties of NotifyIcon component.  
   notifyIcon1.Icon = new System.Drawing.Icon  
      (System.Environment.GetFolderPath  
      (System.Environment.SpecialFolder.Personal)  
      + @"\Icon.ico");  
   notifyIcon1.Visible = true;  
   notifyIcon1.Text = "Right-click me!";  
   notifyIcon1.Visible = true;  
   notifyIcon1.ContextMenu = contextMenu1;  
}  
```  
  
```cpp  
public:  
   System::Windows::Forms::NotifyIcon ^ notifyIcon1;  
   System::Windows::Forms::ContextMenu ^ contextMenu1;  
  
   void createIconMenuStructure()  
   {  
      // Add menu items to shortcut menu.  
      contextMenu1->MenuItems->Add("&Open Application");  
      contextMenu1->MenuItems->Add("S&uspend Application");  
      contextMenu1->MenuItems->Add("E&xit");  
  
      // Set properties of NotifyIcon component.  
      notifyIcon1->Icon = gcnew System::Drawing::Icon  
          (String::Concat(System::Environment::GetFolderPath  
          (System::Environment::SpecialFolder::Personal),  
          "\\Icon.ico"));  
      notifyIcon1->Text = "Right-click me!";  
      notifyIcon1->Visible = true;  
      notifyIcon1->ContextMenu = contextMenu1;  
   }  
```  
  
> [!NOTE]
> フォームのコンストラクターに次のステートメントを含めることにより、`notifyIcon1` と `contextMenu1,` を初期化する必要があります。  
  
```cpp  
notifyIcon1 = gcnew System::Windows::Forms::NotifyIcon();  
contextMenu1 = gcnew System::Windows::Forms::ContextMenu();  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.NotifyIcon>
- <xref:System.Windows.Forms.NotifyIcon.Icon%2A>
- [方法: Windows フォームの NotifyIcon コンポーネントによってタスクバーにアプリケーション アイコンを追加する](app-icons-to-the-taskbar-with-wf-notifyicon.md)
- [NotifyIcon コンポーネント](notifyicon-component-windows-forms.md)
- [NotifyIcon コンポーネントの概要](notifyicon-component-overview-windows-forms.md)
