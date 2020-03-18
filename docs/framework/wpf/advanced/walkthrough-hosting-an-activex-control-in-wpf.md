---
title: WPF で ActiveX コントロールをホストする
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ActiveX controls [WPF interoperability]
- hosting ActiveX controls [WPF]
ms.assetid: 1931d292-0dd1-434f-963c-dcda7638d75a
ms.openlocfilehash: 4ca40c0f6e62fd413e7f305649c5c01ddc152b2a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794147"
---
# <a name="walkthrough-hosting-an-activex-control-in-wpf"></a>チュートリアル: WPF での ActiveX コントロールのホスト
ブラウザーとの対話を強化するために、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションで Microsoft ActiveX コントロールを使用できます。 このチュートリアルでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ページのコントロールとして Microsoft Windows Media Player をホストする方法について説明します。

 このチュートリアルでは、以下のタスクを行います。

- プロジェクトの作成。

- ActiveX コントロールを作成しています。

- WPF ページで ActiveX コントロールをホストする。

 このチュートリアルを完了すると、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションで Microsoft ActiveX コントロールを使用する方法を理解できるようになります。

## <a name="prerequisites"></a>Prerequisites
 このチュートリアルを実行するには、次のコンポーネントが必要です。

- Microsoft Windows Media Player、Visual Studio がインストールされているコンピューターにインストールされます。

- Visual Studio 2010。

## <a name="creating-the-project"></a>プロジェクトの作成

### <a name="to-create-and-set-up-the-project"></a>プロジェクトを作成し、設定するには

1. `HostingAxInWpf`という名前の WPF アプリケーションプロジェクトを作成します。

2. ソリューションに Windows フォームコントロールライブラリプロジェクトを追加し、プロジェクトに `WmpAxLib`という名前を指定します。

3. WmpAxLib プロジェクトで、Windows Media Player アセンブリへの参照を追加します。このアセンブリには、wmp という名前が付けられています。

4. **ツールボックス**を開きます。

5. **ツールボックス**内を右クリックし、 **[項目の選択]** をクリックします。

6. **[COM コンポーネント]** タブをクリックし、 **[Windows Media Player]** コントロールを選択して、 **[OK]** をクリックします。

     Windows Media Player コントロールが **[ツールボックス]** に追加されます。

7. ソリューションエクスプローラーで、 **UserControl1**ファイルを右クリックし、[名前の**変更**] をクリックします。

8. 言語に応じて、名前を `WmpAxControl.vb` または `WmpAxControl.cs`に変更します。

9. すべての参照の名前を変更するように求めるメッセージが表示されたら、 **[はい]** をクリックします。

## <a name="creating-the-activex-control"></a>ActiveX コントロールの作成
コントロールがデザインサーフェイスに追加されると、Visual Studio は Microsoft ActiveX コントロールの <xref:System.Windows.Forms.AxHost> ラッパークラスを自動的に生成します。 次の手順では、WMPLib という名前のマネージアセンブリを作成します。

### <a name="to-create-the-activex-control"></a>ActiveX コントロールを作成するには

1. Windows フォームデザイナーで WmpAxControl .vb または WmpAxControl.cs を開きます。

2. **[ツールボックス]** から、Windows Media Player コントロールをデザイン画面に追加します。

3. プロパティウィンドウで、Windows Media Player コントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill>に設定します。

4. WmpAxLib コントロールライブラリプロジェクトをビルドします。

## <a name="hosting-the-activex-control-on-a-wpf-page"></a>WPF ページで ActiveX コントロールをホストする

### <a name="to-host-the-activex-control"></a>ActiveX コントロールをホストするには

1. HostingAxInWpf プロジェクトで、生成された ActiveX 相互運用性アセンブリへの参照を追加します。

     このアセンブリは WMPLib という名前で、Windows Media Player コントロールをインポートしたときに、WmpAxLib プロジェクトの Debug フォルダーに追加されました。

2. Windowsフォーム統合アセンブリへの参照を追加します。このアセンブリには、Windowsフォーム統合 dll という名前が付けられています。

3. Windows フォームアセンブリへの参照を追加します。このアセンブリには、「system.string」という名前が付けられています。

4. WPF デザイナーで Mainwindow.xaml を開きます。

5. <xref:System.Windows.Controls.Grid> 要素に `grid1`という名前を指定します。

     [!code-xaml[HostingAxInWpf#1](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingAxInWpf/CSharp/HostingAxInWpf/window1.xaml#1)]

6. デザインビューまたは XAML ビューで、<xref:System.Windows.Window> 要素を選択します。

7. プロパティウィンドウで、 **[イベント]** タブをクリックします。

8. <xref:System.Windows.FrameworkElement.Loaded> イベントをダブルクリックします。

9. <xref:System.Windows.FrameworkElement.Loaded> イベントを処理する次のコードを挿入します。

     このコードでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールのインスタンスを作成し、その子として `AxWindowsMediaPlayer` コントロールのインスタンスを追加します。

     [!code-csharp[HostingAxInWpf#11](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingAxInWpf/CSharp/HostingAxInWpf/window1.xaml.cs#11)]
     [!code-vb[HostingAxInWpf#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HostingAxInWpf/VisualBasic/HostingAxInWpf/window1.xaml.vb#11)]  
  
10. F5 キーを押してアプリケーションをビルドし、実行します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
