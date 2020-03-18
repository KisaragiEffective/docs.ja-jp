---
title: WPF で Windows フォーム複合コントロールをホストする
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hosting Windows Forms control in WPF [WPF]
- composite controls [WPF], hosting in WPF
ms.assetid: 96fcd78d-1c77-4206-8928-3a0579476ef4
ms.openlocfilehash: 22eb323d1c1921832630d1d1b30463b4ecb7d1fd
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794228"
---
# <a name="walkthrough-hosting-a-windows-forms-composite-control-in-wpf"></a>チュートリアル: WPF での Windows フォーム複合コントロールのホスト
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、アプリケーションの作成に適した環境を提供します。 ただし Windows フォームコードに多大な投資をしている場合は、最初から書き換えるのではなく、少なくともそのコードの一部を [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションで再利用する方が効率的な場合があります。 最も一般的なシナリオは、既存の Windows フォームコントロールがある場合です。 場合によっては、これらのコントロールのソースコードにアクセスできないこともあります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、このようなコントロールを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでホストするための簡単な手順が用意されています。 たとえば、特殊な <xref:System.Windows.Forms.DataGridView> コントロールをホストしているときに、ほとんどのプログラミングで [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を使用できます。  
  
 このチュートリアルでは、Windows フォーム複合コントロールをホストし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションでデータ入力を実行するアプリケーションについて説明します。 複合コントロールは DLL にパッケージ化されています。 この一般的な手順は、より複雑なアプリケーションやコントロールに拡張することができます。 このチュートリアルは、 [「チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)」と同様の外観と機能を持つように設計されています。 主な違いは、ホストする側とされる側が逆であることです。  
  
 このチュートリアルは、2 つのセクションに分かれています。 最初のセクションでは、Windows フォーム複合コントロールの実装について簡単に説明します。 2番目のセクションでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションで複合コントロールをホストする方法、コントロールからイベントを受信する方法、およびコントロールのプロパティの一部にアクセスする方法の詳細について説明します。  
  
 このチュートリアルでは、以下のタスクを行います。  
  
- Windows フォーム複合コントロールの実装。  
  
- WPF ホストアプリケーションの実装。  
  
 このチュートリアルで示すタスクの完全なコード一覧については、「 [WPF での Windows フォーム複合コントロールのホスト](https://go.microsoft.com/fwlink/?LinkID=159999)」のサンプルを参照してください。  
  
## <a name="prerequisites"></a>Prerequisites  

このチュートリアルを完了するには Visual Studio が必要です。
  
## <a name="implementing-the-windows-forms-composite-control"></a>Windows フォーム複合コントロールの実装  
 この例で使用される複合コントロール Windows フォームは、単純なデータ入力フォームです。 このフォームは、ユーザーの名前とアドレスを受け取り、カスタムイベントを使用してその情報をホストに返します。 次の図はレンダリングされたコントロールを示しています。  

 次の図は Windows フォーム複合コントロールを示しています。  

 ![単純な Windows フォームコントロールを示すスクリーンショット。](./media/walkthrough-hosting-a-windows-forms-composite-control-in-wpf/windows-forms-control.gif)  
  
### <a name="creating-the-project"></a>プロジェクトの作成  
 プロジェクトを開始するには  
  
1. Visual Studio を起動し、 **[新しいプロジェクト]** ダイアログボックスを開きます。  
  
2. ウィンドウ カテゴリで、 **Windows フォームコントロールライブラリ** テンプレートを選択します。  
  
3. 新しいプロジェクトに `MyControls` という名前を付けます。  
  
4. [場所] には、`WpfHostingWindowsFormsControl`など、便利な名前の付いたトップレベルフォルダーを指定します。 このフォルダーには後でホスト アプリケーションも配置します。  
  
5. **[OK]** をクリックして、プロジェクトを作成します。 既定のプロジェクトには、`UserControl1`という名前のコントロールが1つ含まれています。  
  
6. ソリューションエクスプローラーで、`UserControl1` の名前を `MyControl1`に変更します。  
  
 プロジェクトは、以下のシステム DLL を参照している必要があります。 これらの Dll のいずれかが既定で含まれていない場合は、プロジェクトに追加します。  
  
- System  
  
- System.Data  
  
- System.Drawing  
  
- System.Windows.Forms  
  
- System.Xml  
  
### <a name="adding-controls-to-the-form"></a>フォームへのコントロールの追加  
 フォームにコントロールを追加するには、次のようにします。  
  
- デザイナーで `MyControl1` を開きます。  
  
 上の図に示すように、5つの <xref:System.Windows.Forms.Label> コントロールとそれに対応する <xref:System.Windows.Forms.TextBox> コントロールを、フォームに追加します。 この例では、<xref:System.Windows.Forms.TextBox> コントロールにという名前が付けられています。  
  
- `txtName`  
  
- `txtAddress`  
  
- `txtCity`  
  
- `txtState`  
  
- `txtZip`  
  
 **[OK]** と **[キャンセル**] というラベルの付いた <xref:System.Windows.Forms.Button> コントロールを2つ追加します。 この例では、ボタン名はそれぞれ `btnOK`、`btnCancel`ます。  
  
### <a name="implementing-the-supporting-code"></a>サポートコードの実装  
 コードビューでフォームを開きます。 コントロールは、カスタム `OnButtonClick` イベントを発生させることによって、収集されたデータをホストに返します。 データは、イベント引数オブジェクトに格納されます。 次のコードは、イベントとデリゲート宣言を示しています。  
  
 `MyControl1` クラスに次のコードを追加します。  
  
 [!code-csharp[WpfHostingWindowsFormsControl#2](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/MyControls/MyControl1.cs#2)]
 [!code-vb[WpfHostingWindowsFormsControl#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/MyControls/MyControl1.vb#2)]

 `MyControlEventArgs` クラスには、ホストに返される情報が含まれています。

 次のクラスをフォームに追加します。

 [!code-csharp[WpfHostingWindowsFormsControl#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/MyControls/MyControl1.cs#3)]
 [!code-vb[WpfHostingWindowsFormsControl#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/MyControls/MyControl1.vb#3)]

 ユーザーが **[OK]** または **[キャンセル**] ボタンをクリックすると、<xref:System.Windows.Forms.Control.Click> イベントハンドラーによって、データを含む `MyControlEventArgs` オブジェクトが作成され、`OnButtonClick` イベントが発生します。 2つのハンドラーの唯一の違いは、イベント引数の `IsOK` プロパティです。 このプロパティを使用すると、ホストはどのボタンがクリックされたかを判断できます。 **[OK** ] ボタンの `true` に設定され、 **[キャンセル**] ボタンの `false` ます。 次のコードは、2つのボタンハンドラーを示しています。

 `MyControl1` クラスに次のコードを追加します。

 [!code-csharp[WpfHostingWindowsFormsControl#4](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/MyControls/MyControl1.cs#4)]
 [!code-vb[WpfHostingWindowsFormsControl#4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/MyControls/MyControl1.vb#4)]

### <a name="giving-the-assembly-a-strong-name-and-building-the-assembly"></a>アセンブリに厳密な名前を付け、アセンブリをビルドする
 このアセンブリを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションで参照するには、厳密な名前を持つ必要があります。 厳密な名前を作成するには、Sn.exe でキーファイルを作成し、プロジェクトに追加します。

1. Visual Studio コマンド プロンプトを開きます。 これを行うには、 **[スタート]** メニューをクリックし、すべてのプログラム、Microsoft Visual Studio 2010、Visual Studio Tools、 **[Visual Studio コマンドプロンプト]** の順に選択します。 これにより、環境変数がカスタマイズされたコンソールウィンドウが起動します。

2. コマンドプロンプトで、`cd` コマンドを使用してプロジェクトフォルダーにアクセスします。

3. 次のコマンドを実行して、Mycontrols.dll という名前のキーファイルを生成します。

    ```console
    Sn.exe -k MyControls.snk
    ```

4. キーファイルをプロジェクトに含めるには、ソリューションエクスプローラーでプロジェクト名を右クリックし、 **[プロパティ]** をクリックします。 プロジェクトデザイナーで、 **[署名]** タブをクリックし、 **[アセンブリの署名]** チェックボックスをオンにして、キーファイルを参照します。

5. ソリューションをビルドします。 ビルドでは、MyControls.dll という名前の DLL が生成されます。

## <a name="implementing-the-wpf-host-application"></a>WPF ホストアプリケーションの実装
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ホストアプリケーションは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールを使用して `MyControl1`をホストします。 アプリケーションは、コントロールからデータを受信するために `OnButtonClick` イベントを処理します。 また、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションからコントロールのプロパティの一部を変更できるようにするオプションボタンのコレクションも用意されています。 完成したアプリケーションを次の図に示します。

次の図は、WPF アプリケーションに埋め込まれたコントロールを含む、完全なアプリケーションを示しています。

 ![WPF ページに埋め込まれたコントロールを示すスクリーンショット。](./media/walkthrough-hosting-a-windows-forms-composite-control-in-wpf/windows-presentation-foundation-page-control.gif)

### <a name="creating-the-project"></a>プロジェクトの作成
 プロジェクトを開始するには

1. Visual Studio を開き、 **[新しいプロジェクト]** を選択します。

2. ウィンドウ カテゴリで、 **WPF アプリケーション** テンプレートを選択します。

3. 新しいプロジェクトに `WpfHost` という名前を付けます。

4. 配置場所として、MyControls の配置先と同じ最上位フォルダーを指定します。

5. **[OK]** をクリックして、プロジェクトを作成します。

 また、`MyControl1` およびその他のアセンブリを含む DLL への参照を追加する必要もあります。

1. ソリューションエクスプローラーでプロジェクト名を右クリックし、 **[参照の追加]** を選択します。

2. **[参照]** タブをクリックし、mycontrols.dll が含まれているフォルダーを参照します。 このチュートリアルの場合は、MyControls\bin\Debug フォルダーです。

3. Mycontrols.dll を選択し、 **OK** をクリックします。

4. Windowsフォーム統合アセンブリへの参照を追加します。このアセンブリには、Windowsフォーム統合 dll という名前が付けられています。

### <a name="implementing-the-basic-layout"></a>基本レイアウトの実装
 ホストアプリケーションの [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] は、Mainwindow.xaml に実装されています。 このファイルには、レイアウトを定義し、Windows フォームコントロールをホストする [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] マークアップが含まれています。 アプリケーションは、次の3つのリージョンに分割されます。

- **[コントロールのプロパティ]** パネル。このパネルには、ホストされているコントロールのさまざまなプロパティを変更するために使用できるオプションボタンのコレクションが含まれています。

- コントロールパネルの**データ**。ホストされるコントロールから返されたデータを表示する <xref:System.Windows.Controls.TextBlock> 要素がいくつか含まれています。

- ホストされるコントロール自体。

 基本的なレイアウトを次の XAML に示します。 `MyControl1` をホストするために必要なマークアップは、この例では省略されていますが、後で説明します。

 Mainwindow.xaml の XAML を次のように置き換えます。 Visual Basic を使用している場合は、クラスを `x:Class="MainWindow"`に変更します。

 [!code-xaml[WpfHostingWindowsFormsControl#100](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml#100)]

 最初の <xref:System.Windows.Controls.StackPanel> 要素には、ホストされるコントロールのさまざまな既定のプロパティを変更できるようにする、<xref:System.Windows.Controls.RadioButton> コントロールのセットがいくつか含まれています。 その後に、`MyControl1`をホストする <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素が続きます。 最後の <xref:System.Windows.Controls.StackPanel> 要素には、ホストされるコントロールによって返されるデータを表示するいくつかの <xref:System.Windows.Controls.TextBlock> 要素が含まれています。 要素の順序、および <xref:System.Windows.Controls.DockPanel.Dock%2A> および <xref:System.Windows.FrameworkElement.Height%2A> の属性の設定によって、ホストされるコントロールがギャップやひずみなしでウィンドウに埋め込まれます。

#### <a name="hosting-the-control"></a>コントロールのホスト
 前の XAML の次の編集されたバージョンは、`MyControl1`をホストするために必要な要素に焦点を当てています。

 [!code-xaml[WpfHostingWindowsFormsControl#101](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml#101)]
[!code-xaml[WpfHostingWindowsFormsControl#102](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml#102)]

 `xmlns` 名前空間マッピング属性は、ホストされているコントロールを含む `MyControls` 名前空間への参照を作成します。 このマッピングにより、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の `MyControl1` を `<mcl:MyControl1>`として表すことができます。

 XAML の2つの要素は、ホストを処理します。

- `WindowsFormsHost` は、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションで Windows フォームコントロールをホストできるようにする <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素を表します。

- `MyControl1`を表す `mcl:MyControl1`は、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の子コレクションに追加されます。 その結果、この Windows フォームコントロールは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ウィンドウの一部としてレンダリングされ、アプリケーションからコントロールと通信できます。

### <a name="implementing-the-code-behind-file"></a>分離コード ファイルの実装
 分離コードファイル Mainwindow.xaml または MainWindow.xaml.cs には、前のセクションで説明した [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の機能を実装する手続き型のコードが含まれています。 主なタスクは次のとおりです。

- イベントハンドラーを `MyControl1`の `OnButtonClick` イベントにアタッチしています。

- オプションボタンのコレクションの設定方法に基づいて、`MyControl1`のさまざまなプロパティを変更します。

- コントロールによって収集されたデータを表示します。

#### <a name="initializing-the-application"></a>アプリケーションの初期化
 初期化コードは、ウィンドウの <xref:System.Windows.FrameworkElement.Loaded> イベントのイベントハンドラーに含まれており、コントロールの `OnButtonClick` イベントにイベントハンドラーをアタッチします。

 Mainwindow.xaml または MainWindow.xaml.cs で、次のコードを `MainWindow` クラスに追加します。

 [!code-csharp[WpfHostingWindowsFormsControl#11](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml.cs#11)]
 [!code-vb[WpfHostingWindowsFormsControl#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/WpfHost/Page1.xaml.vb#11)]

 前に説明した [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] は <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の子要素コレクションに `MyControl1` 追加されているため、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.Forms.Integration.WindowsFormsHost.Child%2A> をキャストして `MyControl1`への参照を取得できます。 その後、その参照を使用して、`OnButtonClick`にイベントハンドラーをアタッチできます。

 コントロール自体への参照を提供するだけでなく、<xref:System.Windows.Forms.Integration.WindowsFormsHost> は、アプリケーションから操作できるコントロールのプロパティをいくつか公開します。 初期化コードは、これらの値をプライベートグローバル変数に割り当てて、後でアプリケーションで使用できるようにします。

 `MyControls` DLL の型に簡単にアクセスできるようにするには、ファイルの先頭に次の `Imports` または `using` ステートメントを追加します。

```vb
Imports MyControls
```

```csharp
using MyControls;
```

#### <a name="handling-the-onbuttonclick-event"></a>OnButtonClick イベントの処理
 ユーザーがコントロールのボタンのいずれかをクリックすると、`MyControl1` によって `OnButtonClick` イベントが発生します。

 `MainWindow` クラスに次のコードを追加します。

 [!code-csharp[WpfHostingWindowsFormsControl#12](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml.cs#12)]
 [!code-vb[WpfHostingWindowsFormsControl#12](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/WpfHost/Page1.xaml.vb#12)]

 テキストボックス内のデータは、`MyControlEventArgs` オブジェクトにパックされます。 ユーザーが **[OK** ] ボタンをクリックすると、イベントハンドラーによってデータが抽出され、`MyControl1`下のパネルに表示されます。

#### <a name="modifying-the-controls-properties"></a>コントロールのプロパティの変更
 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素は、ホストされているコントロールの既定のプロパティのいくつかを公開します。 その結果、アプリケーションのスタイルに合わせてコントロールの外観を変更することができます。 左側のパネルにあるオプションボタンのセットを使用すると、ユーザーはいくつかの色とフォントのプロパティを変更できます。 各ボタンのセットには、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのハンドラーがあります。このハンドラーによって、ユーザーのオプションボタンの選択が検出され、コントロールの対応するプロパティが変更されます。

 `MainWindow` クラスに次のコードを追加します。

 [!code-csharp[WpfHostingWindowsFormsControl#13](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/CSharp/WpfHost/Page1.xaml.cs#13)]
 [!code-vb[WpfHostingWindowsFormsControl#13](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfHostingWindowsFormsControl/VisualBasic/WpfHost/Page1.xaml.vb#13)]  
  
 アプリケーションをビルドして実行します。 Windows フォーム複合コントロールにテキストを追加し、[ **OK]** をクリックします。 そのテキストがラベルに表示されます。 さまざまなオプションボタンをクリックして、コントロールの効果を確認します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: WPF での Windows フォーム コントロールのホスト](walkthrough-hosting-a-windows-forms-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
