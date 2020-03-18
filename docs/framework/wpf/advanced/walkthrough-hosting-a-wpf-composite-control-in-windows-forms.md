---
title: Windows フォームでの WPF 複合コントロールのホスト
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- hosting WPF content in Windows Forms [WPF]
ms.assetid: 0ac41286-4c1b-4b17-9196-d985cb844ce1
ms.openlocfilehash: 05ba8120c90175801aa2cb61499c48133853e8f7
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794165"
---
# <a name="walkthrough-hosting-a-wpf-composite-control-in-windows-forms"></a>チュートリアル: Windows フォームでの WPF 複合コントロールのホスト
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] は、アプリケーションの作成に適した環境を提供します。 ただし、Windows フォームコードに多大な投資をしている場合は、最初から書き換えるのではなく、既存の Windows フォームアプリケーションを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で拡張する方が効果的です。 一般的なシナリオは、Windows フォームアプリケーション内の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] で実装された1つ以上のコントロールを埋め込む場合です。 WPF コントロールのカスタマイズの詳細については、「[コントロールのカスタマイズ](../controls/control-customization.md)」を参照してください。  
  
 このチュートリアルでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールをホストし、Windows フォームアプリケーションでデータ入力を実行するアプリケーションについて説明します。 複合コントロールは DLL にパッケージ化されています。 この一般的な手順は、より複雑なアプリケーションやコントロールに拡張することができます。 このチュートリアルは、 [「チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)」という外観と機能にほぼ同じように設計されています。 主な違いは、ホストする側とされる側が逆であることです。  
  
 このチュートリアルは、2 つのセクションに分かれています。 最初のセクションでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールの実装について簡単に説明します。 2番目のセクションでは、Windows フォームアプリケーションで複合コントロールをホストする方法、コントロールからイベントを受信する方法、およびコントロールのプロパティの一部にアクセスする方法の詳細について説明します。  
  
 このチュートリアルでは、以下のタスクを行います。  
  
- WPF 複合コントロールを実装する。  
  
- Windows フォーム ホスト アプリケーションを実装する。  
  
 このチュートリアルで示すタスクの完全なコード一覧については、「 [Windows フォームサンプルでの WPF 複合コントロールのホスト](https://github.com/microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WindowsFormsHostingWpfControl)」を参照してください。  
  
## <a name="prerequisites"></a>Prerequisites  

このチュートリアルを完了するには Visual Studio が必要です。  
  
## <a name="implementing-the-wpf-composite-control"></a>WPF 複合コントロールの実装  
 この例で使用される複合コントロール [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、ユーザーの名前と住所を取得する単純なデータ入力フォームです。 ユーザーが 2 つのボタンのいずれかをクリックして入力操作が終了したことを示すと、コントロールはカスタム イベントを発生させて入力情報をホストに返します。 次の図はレンダリングされたコントロールを示しています。 

 次の図は、WPF 複合コントロールを示しています。 

 ![単純な WPF コントロールを示すスクリーンショット。](./media/walkthrough-hosting-a-wpf-composite-control-in-windows-forms/windows-presentation-foundation-composite-control.png)  
  
### <a name="creating-the-project"></a>プロジェクトの作成  
 プロジェクトを開始するには  
  
1. Visual Studio を起動し、 **[新しいプロジェクト]** ダイアログボックスを開きます。  
  
2. ビジュアルC#  と Windows カテゴリで、 **WPF ユーザーコントロールライブラリ** テンプレートを選択します。  
  
3. 新しいプロジェクトに `MyControls` という名前を付けます。  
  
4. [場所] には、`WindowsFormsHostingWpfControl`など、便利な名前の付いたトップレベルフォルダーを指定します。 このフォルダーには後でホスト アプリケーションも配置します。  
  
5. **[OK]** をクリックして、プロジェクトを作成します。 既定のプロジェクトには、`UserControl1`という名前のコントロールが1つ含まれています。  
  
6. ソリューションエクスプローラーで、`UserControl1` の名前を `MyControl1`に変更します。  
  
 プロジェクトは、以下のシステム DLL を参照している必要があります。 これらの DLL のいずれかが既定で含まれていない場合は、プロジェクトに追加します。  
  
- PresentationCore  
  
- PresentationFramework  
  
- System  
  
- WindowsBase  
  
### <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成  
 複合コントロールの [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] は [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]と共に実装されます。 複合コントロール [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] は、5つの <xref:System.Windows.Controls.TextBox> 要素で構成されます。 各 <xref:System.Windows.Controls.TextBox> 要素には、ラベルとして機能する <xref:System.Windows.Controls.TextBlock> 要素が関連付けられています。 下部には、 **[OK]** と **[キャンセル**] の2つの <xref:System.Windows.Controls.Button> 要素があります。 ユーザーがいずれかのボタンをクリックすると、コントロールは情報をホストに返すカスタム イベントを発生させます。  
  
#### <a name="basic-layout"></a>基本的なレイアウト  
 さまざまな [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素は、<xref:System.Windows.Controls.Grid> 要素に含まれています。 <xref:System.Windows.Controls.Grid> を使用すると、HTML で `Table` 要素を使用する場合とほぼ同じ方法で複合コントロールの内容を配置できます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には <xref:System.Windows.Documents.Table> 要素もありますが、<xref:System.Windows.Controls.Grid> は軽量で、単純なレイアウトタスクに適しています。  
  
 次の XAML に基本的なレイアウトを示します。 この XAML は、<xref:System.Windows.Controls.Grid> 要素内の列と行の数を指定することによって、コントロールの全体的な構造を定義します。  
  
 MyControl1.xaml で、で、既存の XAML を次の XAML で置き換えます。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#101](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#101)]  
[!code-xaml[WindowsFormsHostingWpfControl#102](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#102)]  
  
#### <a name="adding-textblock-and-textbox-elements-to-the-grid"></a>TextBlock および TextBox 要素のグリッドへの追加  
 グリッドに [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 要素を配置するには、要素の <xref:System.Windows.Controls.Grid.RowProperty> 属性と <xref:System.Windows.Controls.Grid.ColumnProperty> 属性を適切な行と列の番号に設定します。 行と列の番号付けは 0 から始まることに注意してください。 要素は、<xref:System.Windows.Controls.Grid.ColumnSpanProperty> 属性を設定することにより、複数の列にまたがることができます。 <xref:System.Windows.Controls.Grid> 要素の詳細については、「 [Grid 要素の作成](../controls/how-to-create-a-grid-element.md)」を参照してください。  
  
 次の XAML は、複合コントロールの <xref:System.Windows.Controls.TextBox> と <xref:System.Windows.Controls.TextBlock> 要素とその <xref:System.Windows.Controls.Grid.RowProperty> および <xref:System.Windows.Controls.Grid.ColumnProperty> 属性を示しています。これらの要素は、グリッドに要素を適切に配置するように設定されています。  
  
 Mycontrol1.xaml で、<xref:System.Windows.Controls.Grid> 要素内に次の XAML を追加します。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#103](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#103)]  
  
#### <a name="styling-the-ui-elements"></a>UI 要素のスタイル設定  
 データ入力フォームの多くの要素は外観が似ていますが、それは、そのいくつかのプロパティの設定が同一であることを意味しています。 各要素の属性を個別に設定するのではなく、前の XAML では <xref:System.Windows.Style> 要素を使用して要素のクラスの標準プロパティ設定を定義します。 この方法だと、コントロールの複雑さが軽減され、1 つのスタイル属性を使用して複数要素の外観を変更することができます。  
  
 <xref:System.Windows.Style> 要素は、<xref:System.Windows.Controls.Grid> 要素の <xref:System.Windows.FrameworkElement.Resources%2A> プロパティに含まれているので、コントロール内のすべての要素で使用できます。 スタイルにという名前が付けられている場合は、スタイル名に <xref:System.Windows.Style> 要素セットを追加することによって、要素に適用します。 名前が付いていないスタイルは、その要素の既定のスタイルになります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] スタイルの詳細については、「スタイル[とテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。  
  
 次の XAML は、複合コントロールの <xref:System.Windows.Style> 要素を示しています。 スタイルがどのように要素に適用されるかについては、前の XAML を参照してください。 たとえば、最後の <xref:System.Windows.Controls.TextBlock> 要素は `inlineText` スタイルを持ち、最後の <xref:System.Windows.Controls.TextBox> 要素は既定のスタイルを使用します。  
  
 Mycontrol1.xaml で、<xref:System.Windows.Controls.Grid> 開始要素の直後に次の XAML を追加します。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#104](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#104)]  
  
#### <a name="adding-the-ok-and-cancel-buttons"></a>[OK] ボタンと [キャンセル] ボタンの追加  
 複合コントロールの最後の要素は、<xref:System.Windows.Controls.Grid>の最後の行の最初の2つの列を占有する、 **OK**および**Cancel**<xref:System.Windows.Controls.Button> 要素です。 これらの要素は、前の XAML で定義されている共通のイベントハンドラー、`ButtonClicked`、および既定の <xref:System.Windows.Controls.Button> スタイルを使用します。  
  
 Mycontrol1.xaml で、最後の <xref:System.Windows.Controls.TextBox> 要素の後に次の XAML を追加します。 複合コントロールの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の部分が完成しました。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#105](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#105)]  
  
### <a name="implementing-the-code-behind-file"></a>分離コード ファイルの実装  
 分離コードファイル MyControl1.xaml.cs には、次の3つの重要なタスクが実装されています。
  
1. ユーザーがいずれかのボタンをクリックしたときに発生するイベントを処理します。  
  
2. <xref:System.Windows.Controls.TextBox> 要素からデータを取得し、カスタムイベント引数オブジェクトでパッケージ化します。  
  
3. カスタム `OnButtonClick` イベントを発生させます。これにより、ユーザーが終了したことがホストに通知され、データがホストに戻されます。  
  
 コントロールは、外観を変更するための色とフォントの多くのプロパティも公開します。 Windows フォームコントロールをホストするために使用される <xref:System.Windows.Forms.Integration.WindowsFormsHost> クラスとは異なり、<xref:System.Windows.Forms.Integration.ElementHost> クラスでは、コントロールの <xref:System.Windows.Controls.Panel.Background%2A> プロパティのみが公開されます。 このコード例と「[チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)」で説明されている例との類似性を維持するために、コントロールは残りのプロパティを直接公開します。  
  
#### <a name="the-basic-structure-of-the-code-behind-file"></a>分離コード ファイルの基本構造  
 分離コードファイルは、1つの名前空間 `MyControls`で構成されます。この名前空間には、`MyControl1` と `MyControlEventArgs`の2つのクラスが含まれます。  
  
```csharp  
namespace MyControls  
{  
  public partial class MyControl1 : Grid  
  {  
    //...  
  }  
  public class MyControlEventArgs : EventArgs  
  {  
    //...  
  }  
}  
```  
  
 最初のクラス `MyControl1`は、Mycontrol1.xaml で定義された [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の機能を実装するコードを含む部分クラスです。 Mycontrol1.xaml が解析されると、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] が同じ部分クラスに変換され、2つの部分クラスがマージされ、コンパイルされたコントロールが形成されます。 このため、分離コード ファイル内のクラス名は MyControl1.xaml に割り当てられたクラス名と一致する必要があり、コントロールのルート要素から継承する必要があります。 2番目のクラス `MyControlEventArgs`は、データをホストに送信するために使用されるイベント引数クラスです。  
  
 MyControl1.xaml.cs を開きます。 次の名前を持つように既存のクラス宣言を変更し、<xref:System.Windows.Controls.Grid>から継承するようにします。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#21](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#21)]  
  
#### <a name="initializing-the-control"></a>コントロールの初期化  
 このコードでは、次の基本タスクを実装します。  
  
- プライベートイベント、`OnButtonClick`、およびそれに関連付けられているデリゲート、`MyControlEventHandler`を宣言します。  
  
- ユーザーのデータを格納するいくつかのプライベート グローバル変数を作成します。 このデータは、対応するプロパティを通じて公開されます。  
  
- コントロールの <xref:System.Windows.FrameworkElement.Loaded> イベントのハンドラー `Init`を実装します。 このハンドラーは、MyControl1.xaml で定義された値をグローバル変数に割り当てることで、グローバル変数を初期化します。 これを行うには、通常の <xref:System.Windows.Controls.TextBlock> 要素 `nameLabel`に割り当てられた <xref:System.Windows.FrameworkElement.Name%2A> を使用して、その要素のプロパティ設定にアクセスします。  
  
 既存のコンストラクターを削除し、次のコードを `MyControl1` クラスに追加します。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#11](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#11)]  
  
#### <a name="handling-the-buttons-click-events"></a>ボタンのクリック イベントの処理  
 ユーザーは、 **[OK** ] ボタンまたは **[キャンセル**] ボタンをクリックして、データ入力タスクが完了したことを示します。 どちらのボタンも、同じ <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントハンドラー、`ButtonClicked`を使用します。 どちらのボタンにも名前、`btnOK`、または `btnCancel`があり、ハンドラーは `sender` 引数の値を調べて、どのボタンがクリックされたかを判断できます。 ハンドラーは次の処理を行います。  
  
- <xref:System.Windows.Controls.TextBox> 要素のデータを格納する `MyControlEventArgs` オブジェクトを作成します。  
  
- ユーザーが **[キャンセル**] ボタンをクリックした場合、`MyControlEventArgs` オブジェクトの `IsOK` プロパティを `false`に設定します。  
  
- `OnButtonClick` イベントを発生させて、ユーザーが終了したことをホストに示すと共に、収集されたデータを返します。  
  
 `Init` メソッドの後に、`MyControl1` クラスに次のコードを追加します。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#12](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#12)]  
  
#### <a name="creating-properties"></a>プロパティの作成  
 クラスの残りの部分では単に、前に説明したグローバル変数に対応するプロパティを公開します。 プロパティが変更されると、set アクセサーは、対応する要素のプロパティを変更し、基になるグローバル変数を更新することによって、コントロールの外観を変更します。  
  
 `MyControl1` クラスに次のコードを追加します。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#13](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#13)]  
  
#### <a name="sending-the-data-back-to-the-host"></a>ホストへのデータの返送  
 ファイルの最後のコンポーネントは `MyControlEventArgs` クラスです。このクラスは、収集されたデータをホストに送り返すために使用されます。  
  
 `MyControls` 名前空間に次のコードを追加します。 この実装は簡単なので、これ以上の説明はしません。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#14](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#14)]  
  
 ソリューションをビルドします。 ビルドでは、MyControls.dll という名前の DLL が生成されます。  
  
<a name="winforms_host_section"></a>   
## <a name="implementing-the-windows-forms-host-application"></a>Windows フォーム ホスト アプリケーションの実装  
 Windows フォームホストアプリケーションは、<xref:System.Windows.Forms.Integration.ElementHost> オブジェクトを使用して、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールをホストします。 アプリケーションは、複合コントロールからデータを受信するために `OnButtonClick` イベントを処理します。 アプリケーションにはオプションのボタンのセットもあり、それを使用してコントロールの外観を変更することができます。 次の図は、アプリケーションを示しています。  

次の図は、Windows フォームアプリケーションでホストされる WPF 複合コントロールを示しています。  

 ![Scteenshot は、Avalon コントロールをホストする Windows フォームを示しています。](./media/walkthrough-hosting-a-wpf-composite-control-in-windows-forms/windows-form-hosting-avalon-control.png)  
  
### <a name="creating-the-project"></a>プロジェクトの作成  
 プロジェクトを開始するには  
  
1. Visual Studio を起動し、 **[新しいプロジェクト]** ダイアログボックスを開きます。  
  
2. ビジュアルC#  と Windows カテゴリで、 **Windows フォームアプリケーション** テンプレートを選択します。  
  
3. 新しいプロジェクトに `WFHost` という名前を付けます。  
  
4. 配置場所として、MyControls の配置先と同じ最上位フォルダーを指定します。  
  
5. **[OK]** をクリックして、プロジェクトを作成します。  
  
 また、`MyControl1` およびその他のアセンブリを含む DLL への参照を追加する必要もあります。  
  
1. ソリューションエクスプローラーでプロジェクト名を右クリックし、 **[参照の追加]** を選択します。  
  
2. **[参照]** タブをクリックし、mycontrols.dll が含まれているフォルダーを参照します。 このチュートリアルの場合は、MyControls\bin\Debug フォルダーです。  
  
3. Mycontrols.dll を選択し、 **OK** をクリックします。  
  
4. 次のアセンブリへの参照を追加します。  
  
    - PresentationCore  
  
    - PresentationFramework  
  
    - System.Xaml  
  
    - WindowsBase  
  
    - WindowsFormsIntegration  
  
### <a name="implementing-the-user-interface-for-the-application"></a>アプリケーションのユーザー インターフェイスの実装  
 Windows フォーム アプリケーションの UI には、WPF 複合コントロールを操作するいくつかのコントロールが含まれています。  
  
1. Windows フォーム デザイナーで、Form1 を開きます。  
  
2. コントロールを配置するためにフォームを拡大します。  
  
3. フォームの右上隅に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールを保持する <xref:System.Windows.Forms.Panel?displayProperty=nameWithType> コントロールを追加します。  
  
4. 次の <xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType> コントロールをフォームに追加します。  
  
    |[名前]|テキスト|  
    |----------|----------|  
    |groupBox1|背景色|  
    |groupBox2|前景色|  
    |groupBox3|フォント サイズ|  
    |groupBox4|フォント ファミリ|  
    |groupBox5|フォント スタイル|  
    |groupBox6|フォントの太さ|  
    |groupBox7|コントロールからのデータ|  
  
5. 次の <xref:System.Windows.Forms.RadioButton?displayProperty=nameWithType> コントロールを <xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType> コントロールに追加します。  
  
    |GroupBox|[名前]|テキスト|  
    |--------------|----------|----------|  
    |groupBox1|radioBackgroundOriginal|元|  
    |groupBox1|radioBackgroundLightGreen|ライトグリーン|  
    |groupBox1|radioBackgroundLightSalmon|ライトサーモン|  
    |groupBox2|radioForegroundOriginal|元|  
    |groupBox2|radioForegroundRed|赤|  
    |groupBox2|radioForegroundYellow|黄|  
    |groupBox3|radioSizeOriginal|元|  
    |groupBox3|radioSizeTen|10|  
    |groupBox3|radioSizeTwelve|12|  
    |groupBox4|radioFamilyOriginal|元|  
    |groupBox4|radioFamilyTimes|ＭＳ Ｐゴシック|  
    |groupBox4|radioFamilyWingDings|WingDings|  
    |groupBox5|radioStyleOriginal|標準|  
    |groupBox5|radioStyleItalic|[斜体]|  
    |groupBox6|radioWeightOriginal|元|  
    |groupBox6|radioWeightBold|太字|  
  
6. 次の <xref:System.Windows.Forms.Label?displayProperty=nameWithType> コントロールを最後の <xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType>に追加します。 これらのコントロールは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールによって返されるデータを表示します。  
  
    |GroupBox|[名前]|テキスト|  
    |--------------|----------|----------|  
    |groupBox7|lblName|名前:|  
    |groupBox7|lblAddress|番地:|  
    |groupBox7|lblCity|都市:|  
    |groupBox7|lblState|状態:|  
    |groupBox7|lblZip|郵便番号:|  
  
### <a name="initializing-the-form"></a>フォームの初期化  
 通常は、フォームの <xref:System.Windows.Forms.Form.Load> イベントハンドラーにホストコードを実装します。 次のコードは、<xref:System.Windows.Forms.Form.Load> イベントハンドラー、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールの <xref:System.Windows.FrameworkElement.Loaded> イベントのハンドラー、および後で使用されるいくつかのグローバル変数の宣言を示しています。  
  
 Windows フォームデザイナーで、フォームをダブルクリックして、<xref:System.Windows.Forms.Form.Load> イベントハンドラーを作成します。 Form1.cs の先頭に、次の `using` ステートメントを追加します。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#10](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#10)]  
  
 既存の `Form1` クラスの内容を次のコードに置き換えます。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#2](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#2)]  
  
 前のコードの `Form1_Load` メソッドは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールをホストするための一般的な手順を示しています。  
  
1. 新しい <xref:System.Windows.Forms.Integration.ElementHost> オブジェクトを作成します。  
  
2. コントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティを <xref:System.Windows.Forms.DockStyle.Fill?displayProperty=nameWithType>に設定します。  
  
3. <xref:System.Windows.Forms.Integration.ElementHost> コントロールを <xref:System.Windows.Forms.Panel> コントロールの <xref:System.Windows.Forms.Control.Controls%2A> コレクションに追加します。  
  
4. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールのインスタンスを作成します。  
  
5. コントロールを <xref:System.Windows.Forms.Integration.ElementHost> コントロールの <xref:System.Windows.Forms.Integration.ElementHost.Child%2A> プロパティに割り当てることによって、フォーム上で複合コントロールをホストします。  
  
 `Form1_Load` メソッドの残りの2行は、次の2つのコントロールイベントにハンドラーをアタッチします。  
  
- `OnButtonClick` は、ユーザーが **[OK]** または **[キャンセル**] ボタンをクリックしたときに、複合コントロールによって起動されるカスタムイベントです。 このイベントを処理してユーザーの応答を取得し、ユーザーが指定したデータをすべて収集します。  
  
- <xref:System.Windows.FrameworkElement.Loaded> は、完全に読み込まれたときに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロールによって発生する標準イベントです。 ここでこのイベントを使用するのは、この例ではこのコントロールからのプロパティを使用して複数のグローバル変数を初期化する必要があるからです。 フォームの <xref:System.Windows.Forms.Form.Load> イベントの時点では、コントロールは完全には読み込まれず、これらの値はまだ `null`に設定されています。 これらのプロパティにアクセスするには、コントロールの <xref:System.Windows.FrameworkElement.Loaded> イベントが発生するまで待機する必要があります。  
  
 前のコードでは、<xref:System.Windows.FrameworkElement.Loaded> イベントハンドラーを示しています。 `OnButtonClick` ハンドラーについては、次のセクションで説明します。  
  
### <a name="handling-onbuttonclick"></a>OnButtonClick の処理  
 `OnButtonClick` イベントは、ユーザーが **[OK]** または **[キャンセル**] ボタンをクリックしたときに発生します。  
  
 イベントハンドラーは、イベント引数の `IsOK` フィールドをチェックして、どのボタンがクリックされたかを判別します。 `lbl`の*データ*変数は、前に説明した <xref:System.Windows.Forms.Label> コントロールに対応します。 ユーザーが **[OK** ] ボタンをクリックすると、コントロールの <xref:System.Windows.Controls.TextBox> コントロールのデータが、対応する <xref:System.Windows.Forms.Label> コントロールに割り当てられます。 ユーザーが **[キャンセル**] をクリックすると、<xref:System.Windows.Forms.Label.Text%2A> の値が既定の文字列に設定されます。  
  
 次のボタンクリックイベントハンドラーコードを `Form1` クラスに追加します。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#3)]  
  
 アプリケーションをビルドして実行します。 WPF 複合コントロールにテキストを追加し、[ **OK]** をクリックします。 そのテキストがラベルに表示されます。 この時点で、オプション ボタンを処理するコードは追加されていません。  
  
### <a name="modifying-the-appearance-of-the-control"></a>コントロールの外観の変更  
 フォーム上の <xref:System.Windows.Forms.RadioButton> コントロールを使用すると、ユーザーは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 複合コントロールの前景色と背景色、およびいくつかのフォントプロパティを変更できます。 背景色は、<xref:System.Windows.Forms.Integration.ElementHost> オブジェクトによって公開されます。 残りのプロパティは、コントロールのカスタム プロパティとして公開されます。  
  
 フォーム上の各 <xref:System.Windows.Forms.RadioButton> コントロールをダブルクリックして、<xref:System.Windows.Forms.RadioButton.CheckedChanged> イベントハンドラーを作成します。 <xref:System.Windows.Forms.RadioButton.CheckedChanged> イベントハンドラーを次のコードに置き換えます。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#4](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#4)]  
  
 アプリケーションをビルドして実行します。 別のオプション ボタンをクリックして、WPF 複合コントロール上の影響を確認します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの 3D WPF 複合コントロールのホスト](walkthrough-hosting-a-3-d-wpf-composite-control-in-windows-forms.md)
