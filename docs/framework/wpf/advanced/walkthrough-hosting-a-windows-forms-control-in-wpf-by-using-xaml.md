---
title: XAML を使用した WPF での Windows フォームコントロールのホスト
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- hosting Windows Forms control in WPF [WPF]
ms.assetid: 1aef42cb-4cfb-44b4-9a7a-c02632d3d9c7
ms.openlocfilehash: 99c077801a26043e17e0d51ecc0a97b9608784c0
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095061"
---
# <a name="walkthrough-hosting-a-windows-forms-control-in-wpf-by-using-xaml"></a>チュートリアル: WPF での、XAML を使用した Windows フォーム コントロールのホスト
[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、豊富な機能セットを備えたさまざまなコントロールが用意されています。 ただし、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のページで Windows フォームコントロールを使用することが必要になる場合があります。 たとえば、既存の Windows フォームコントロールに多大な投資を行っている場合や、独自の機能を提供する Windows フォームコントロールがある場合があります。  
  
 このチュートリアルでは、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を使用して [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ページで Windows フォーム <xref:System.Windows.Forms.MaskedTextBox?displayProperty=nameWithType> コントロールをホストする方法について説明します。  
  
 このチュートリアルで示されているタスクの完全なコード一覧については、「 [XAML を使用した WPF での Windows フォームコントロールのホスト](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/HostingWfInWpfWithXaml)」を参照してください。
  
## <a name="prerequisites"></a>前提条件  

このチュートリアルを完了するには Visual Studio が必要です。  
  
## <a name="hosting-the-windows-forms-control"></a>Windows フォームコントロールのホスト  
  
#### <a name="to-host-the-maskedtextbox-control"></a>MaskedTextBox コントロールをホストするには  
  
1. `HostingWfInWpfWithXaml`という名前の WPF アプリケーションプロジェクトを作成します。  
  
2. 次のアセンブリへの参照を追加します。  
  
    - WindowsFormsIntegration  
  
    - System.Windows.Forms  
  
3. WPF デザイナーで Mainwindow.xaml を開きます。  
  
4. <xref:System.Windows.Window> 要素に、次の名前空間マッピングを追加します。 `wf` 名前空間マッピングは、Windows フォームコントロールを含むアセンブリへの参照を確立します。  
  
    ```xaml  
    xmlns:wf="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"  
    ```  
  
5. <xref:System.Windows.Controls.Grid> 要素で、次の XAML を追加します。  
  
     <xref:System.Windows.Forms.MaskedTextBox> コントロールは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールの子として作成されます。  
  
     [!code-xaml[HostingWfInWpfWithXaml#3](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWfInWpfWithXaml/CSharp/HostingWfInWpf/Window1.xaml#3)]  
  
6. F5 キーを押して、アプリケーションをビルドして実行します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: WPF での Windows フォーム コントロールのホスト](walkthrough-hosting-a-windows-forms-control-in-wpf.md)
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
- [Windows フォーム コントロールおよび同等の WPF コントロール](windows-forms-controls-and-equivalent-wpf-controls.md)
- [XAML サンプルを使用した WPF での Windows フォームコントロールのホスト](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/HostingWfInWpfWithXaml)
