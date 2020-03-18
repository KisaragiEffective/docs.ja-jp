---
title: '方法 : ハイブリッド アプリケーションで視覚スタイルを有効にする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hybrid applications [WPF interoperability]
- visual styles [Windows Forms]
ms.assetid: 95de9b9c-d804-405c-b2d1-49a88c1e0fe1
ms.openlocfilehash: dd52313e9100f9c6a1141b53ccc5a23a4b54410a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789920"
---
# <a name="how-to-enable-visual-styles-in-a-hybrid-application"></a>方法 : ハイブリッド アプリケーションで視覚スタイルを有効にする
このトピックでは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションでホストされている Windows フォームコントロールで visual スタイルを有効にする方法について説明します。  
  
 アプリケーションが <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドを呼び出すと、ほとんどの Windows フォームコントロールで自動的に visual スタイルが使用されます。 詳細については、「[visual スタイルが使用されているコントロールのレンダリング](../../winforms/controls/rendering-controls-with-visual-styles.md)」を参照してください。  
  
 このトピックで説明されているタスクの完全なコード一覧については、「[ハイブリッドアプリケーションでの Visual スタイルの有効化](https://go.microsoft.com/fwlink/?LinkID=159986)」のサンプルを参照してください。  
  
## <a name="enabling-windows-forms-visual-styles"></a>Windows フォーム視覚スタイルの有効化  
  
#### <a name="to-enable-windows-forms-visual-styles"></a>Windows フォーム視覚スタイルを有効にするには  
  
1. `HostingWfWithVisualStyles`という名前の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションプロジェクトを作成します。  
  
2. ソリューション エクスプローラーで、次のアセンブリへの参照を追加します。  
  
    - WindowsFormsIntegration  
  
    - System.Windows.Forms  
  
3. ツールボックスで <xref:System.Windows.Controls.Grid> アイコンをダブルクリックして、デザインサーフェイスに <xref:System.Windows.Controls.Grid> 要素を配置します。  
  
4. プロパティウィンドウで、<xref:System.Windows.FrameworkElement.Height%2A> の値と <xref:System.Windows.FrameworkElement.Width%2A> プロパティを**Auto**に設定します。  
  
5. デザインビューまたは XAML ビューで、<xref:System.Windows.Window>を選択します。  
  
6. プロパティウィンドウで、 **[イベント]** タブをクリックします。  
  
7. <xref:System.Windows.FrameworkElement.Loaded> イベントをダブルクリックします。
  
8. Mainwindow.xaml または MainWindow.xaml.cs で、<xref:System.Windows.FrameworkElement.Loaded> イベントを処理する次のコードを挿入します。  
  
     [!code-csharp[HostingWfWithVisualStyles#11](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWfWithVisualStyles/CSharp/HostingWfWithVisualStyles/Window1.xaml.cs#11)]
     [!code-vb[HostingWfWithVisualStyles#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HostingWfWithVisualStyles/VisualBasic/HostingWfWithVisualStyles/Window1.xaml.vb#11)]  
  
9. F5 キーを押してアプリケーションをビルドし、実行します。  
  
     Windows フォームコントロールは、visual スタイルを使用して描画されます。  
  
## <a name="disabling-windows-forms-visual-styles"></a>Windows フォーム視覚スタイルの無効化  
 視覚スタイルを無効にするには、単に <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドの呼び出しを削除します。  
  
#### <a name="to-disable-windows-forms-visual-styles"></a>Windows フォーム視覚スタイルを無効にするには  
  
1. コード エディターで MainWindow.xaml.vb または MainWindow.xaml.cs を開きます。  
  
2. <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドへの呼び出しをコメントアウトします。  
  
3. F5 キーを押してアプリケーションをビルドし、実行します。  
  
     Windows フォームコントロールは、既定のシステムスタイルで描画されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Application.EnableVisualStyles%2A>
- <xref:System.Windows.Forms.VisualStyles>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [visual スタイルが使用されているコントロールのレンダリング](../../winforms/controls/rendering-controls-with-visual-styles.md)
- [チュートリアル: WPF での Windows フォーム コントロールのホスト](walkthrough-hosting-a-windows-forms-control-in-wpf.md)
