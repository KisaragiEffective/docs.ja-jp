---
title: 'チュートリアル : WindowsFormsHost 要素を使用したプロパティの割り当て'
ms.date: 08/18/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- mapping properties [WPF]
- WindowsFormsHost element property mapping [WPF]
ms.assetid: 74809167-bf8e-48b7-a2e7-b4ea08bc7d8c
ms.openlocfilehash: c076937d6431adf1750793d47ece88dc82edf95c
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794100"
---
# <a name="walkthrough-mapping-properties-using-the-windowsformshost-element"></a>チュートリアル : WindowsFormsHost 要素を使用したプロパティの割り当て

このチュートリアルでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A> プロパティを使用して、ホストされている Windows フォームコントロールの対応するプロパティに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティをマップする方法について説明します。

このチュートリアルでは、以下のタスクを行います。

- プロジェクトの作成。

- アプリケーションレイアウトを定義します。

- 新しいプロパティマッピングを定義します。

- 既定のプロパティマッピングを削除しています。

- 既定のプロパティマッピングを置き換える。

- 既定のプロパティマッピングの拡張。

このチュートリアルで示すタスクの完全なコード一覧については、「 [WindowsFormsHost 要素のサンプルを使用したプロパティのマッピング](https://go.microsoft.com/fwlink/?LinkID=160019)」を参照してください。

終了すると、ホストされている Windows フォームコントロールの対応するプロパティに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティをマップできるようになります。

## <a name="prerequisites"></a>Prerequisites

このチュートリアルを実行するには、次のコンポーネントが必要です。

- Visual Studio 2017

## <a name="create-and-set-up-the-project"></a>プロジェクトを作成して設定する

1. `PropertyMappingWithWfhSample`という名前の**WPF アプリ**プロジェクトを作成します。

2. **ソリューションエクスプローラー**で、windowsフォーム統合アセンブリへの参照を追加します。このアセンブリには、windowsフォーム integration .dll という名前が付けられています。

3. **ソリューションエクスプローラー**で、Drawing アセンブリおよび system.string アセンブリへの参照を追加します。

## <a name="defining-the-application-layout"></a>アプリケーションレイアウトの定義

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションでは、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素を使用して Windows フォームコントロールをホストします。

### <a name="to-define-the-application-layout"></a>アプリケーションのレイアウトを定義するには

1. WPF デザイナーで Window1.xaml を開きます。

2. 既存のコードを次のコードに置き換えます。

     [!code-xaml[PropertyMappingWithWfhSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml#1)]

3. コードエディターで Window1.xaml.cs を開きます。

4. ファイルの先頭に、次の名前空間をインポートします。

     [!code-csharp[PropertyMappingWithWfhSample#20](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#20)]
     [!code-vb[PropertyMappingWithWfhSample#20](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#20)]

## <a name="defining-a-new-property-mapping"></a>新しいプロパティマッピングの定義

<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素には、いくつかの既定のプロパティマッピングが用意されています。 新しいプロパティマッピングを追加するには、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>の <xref:System.Windows.Forms.Integration.PropertyMap.Add%2A> メソッドを呼び出します。

### <a name="to-define-a-new-property-mapping"></a>新しいプロパティマッピングを定義するには

- `Window1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithWfhSample#14](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#14)]
     [!code-vb[PropertyMappingWithWfhSample#14](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#14)]

     `AddClipMapping` メソッドは、<xref:System.Windows.UIElement.Clip%2A> プロパティの新しいマッピングを追加します。

     `OnClipChange` メソッドは、<xref:System.Windows.UIElement.Clip%2A> プロパティを Windows フォーム<xref:System.Windows.Forms.Control.Region%2A> プロパティに変換します。

     `Window1_SizeChanged` メソッドは、ウィンドウの <xref:System.Windows.FrameworkElement.SizeChanged> イベントを処理し、アプリケーションウィンドウに合うようにクリッピング領域のサイズを調整します。

## <a name="removing-a-default-property-mapping"></a>既定のプロパティマッピングの削除

<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>の <xref:System.Windows.Forms.Integration.PropertyMap.Remove%2A> メソッドを呼び出すことによって、既定のプロパティマッピングを削除します。

### <a name="to-remove-a-default-property-mapping"></a>既定のプロパティマッピングを削除するには

- `Window1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithWfhSample#13](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#13)]
     [!code-vb[PropertyMappingWithWfhSample#13](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#13)]

     `RemoveCursorMapping` メソッドは、<xref:System.Windows.FrameworkElement.Cursor%2A> プロパティの既定のマッピングを削除します。

## <a name="replacing-a-default-property-mapping"></a>既定のプロパティマッピングの置換

既定のマッピングを削除し、<xref:System.Windows.Forms.Integration.WindowsFormsHost> 要素の <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>に対して <xref:System.Windows.Forms.Integration.PropertyMap.Add%2A> メソッドを呼び出すことによって、既定のプロパティマッピングを置き換えます。

### <a name="to-replace-a-default-property-mapping"></a>既定のプロパティマッピングを置き換えるには

- `Window1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithWfhSample#12](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#12)]
     [!code-vb[PropertyMappingWithWfhSample#12](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#12)]

     `ReplaceFlowDirectionMapping` メソッドは、<xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティの既定のマッピングを置き換えます。

     `OnFlowDirectionChange` メソッドは、<xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティを Windows フォーム<xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティに変換します。

     `cb_CheckedChanged` メソッドは、<xref:System.Windows.Forms.CheckBox> コントロールの <xref:System.Windows.Forms.CheckBox.CheckedChanged> イベントを処理します。 <xref:System.Windows.Forms.CheckBox.CheckState%2A> プロパティの値に基づいて <xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティを割り当てます。

## <a name="extending-a-default-property-mapping"></a>既定のプロパティマッピングの拡張

既定のプロパティマッピングを使用し、独自のマッピングで拡張することもできます。

### <a name="to-extend-a-default-property-mapping"></a>既定のプロパティマッピングを拡張するには

- `Window1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithWfhSample#15](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#15)]
     [!code-vb[PropertyMappingWithWfhSample#15](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#15)]

     `ExtendBackgroundMapping` メソッドは、既存の <xref:System.Windows.Controls.Control.Background%2A> プロパティのマッピングにカスタムプロパティトランスレーターを追加します。

     `OnBackgroundChange` メソッドは、ホストされているコントロールの <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティに特定のイメージを割り当てます。 `OnBackgroundChange` メソッドは、既定のプロパティマッピングが適用された後に呼び出されます。

## <a name="initializing-your-property-mappings"></a>プロパティマッピングの初期化

<xref:System.Windows.FrameworkElement.Loaded> イベントハンドラーで前に説明したメソッドを呼び出すことによって、プロパティマッピングを設定します。

### <a name="to-initialize-your-property-mappings"></a>プロパティマッピングを初期化するには

1. `Window1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithWfhSample#11](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#11)]
     [!code-vb[PropertyMappingWithWfhSample#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#11)]

     `WindowLoaded` メソッドは、<xref:System.Windows.FrameworkElement.Loaded> イベントを処理し、次の初期化を実行します。

    - <xref:System.Windows.Forms.CheckBox> コントロール Windows フォームを作成します。

    - チュートリアルの前の手順で定義したメソッドを呼び出して、プロパティマッピングを設定します。

    - マップされたプロパティに初期値を割り当てます。

2. **F5** キーを押してアプリケーションをビルドし、実行します。 <xref:System.Windows.FrameworkElement.FlowDirection%2A> マッピングの効果を確認するには、このチェックボックスをオンにします。 このチェックボックスをオンにすると、レイアウトが左から右方向に反転します。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Windows フォームと WPF プロパティの割り当て](windows-forms-and-wpf-property-mapping.md)
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: WPF での Windows フォーム コントロールのホスト](walkthrough-hosting-a-windows-forms-control-in-wpf.md)
