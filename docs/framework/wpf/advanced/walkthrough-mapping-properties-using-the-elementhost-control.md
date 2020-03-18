---
title: 'チュートリアル : ElementHost コントロールを使用したプロパティの割り当て'
ms.date: 08/18/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- mapping properties [WPF]
- ElementHost control [WPF], mapping properties
ms.assetid: bccd6e0d-2272-4924-9107-ff8ed58b88aa
ms.openlocfilehash: 7ff4ff607ab70b55cda1e2c4736ff773d4907a22
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794114"
---
# <a name="walkthrough-mapping-properties-using-the-elementhost-control"></a>チュートリアル : ElementHost コントロールを使用したプロパティの割り当て

このチュートリアルでは、<xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A> プロパティを使用して、ホストされる [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 要素の対応するプロパティに Windows フォームプロパティをマップする方法について説明します。

このチュートリアルでは、以下のタスクを行います。

- プロジェクトの作成。

- 新しいプロパティマッピングを定義します。

- 既定のプロパティマッピングを削除しています。

- 既定のプロパティマッピングの拡張。

このチュートリアルで説明されているタスクの完全なコード一覧については、「[コントロールのプロパティを使用したプロパティのマッピング](https://go.microsoft.com/fwlink/?LinkID=160018)」を参照してください。

終了すると、ホストされている要素の対応する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティに Windows フォームプロパティをマップできるようになります。

## <a name="prerequisites"></a>Prerequisites

このチュートリアルを実行するには、次のコンポーネントが必要です。

- Visual Studio 2017

## <a name="creating-the-project"></a>プロジェクトの作成

### <a name="to-create-the-project"></a>プロジェクトを作成するには

1. `PropertyMappingWithElementHost`という名前の**Windows フォームアプリ**プロジェクトを作成します。

2. **ソリューションエクスプローラー**で、次の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アセンブリへの参照を追加します。

    - PresentationCore

    - PresentationFramework

    - WindowsBase

    - WindowsFormsIntegration

3. `Form1` コードファイルの先頭に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#10](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#10)]
     [!code-vb[PropertyMappingWithElementHost#10](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#10)]

4. Windows フォーム デザイナーで `Form1` を開きます。 フォームをダブルクリックして、<xref:System.Windows.Forms.Form.Load> イベントのイベントハンドラーを追加します。

5. Windows フォームデザイナーに戻り、フォームの <xref:System.Windows.Forms.Control.Resize> イベントのイベントハンドラーを追加します。 詳細については、「[方法: デザイナーを使用してイベントハンドラーを作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/zwwsdtbk(v=vs.100))する」を参照してください。

6. `Form1` クラスの <xref:System.Windows.Forms.Integration.ElementHost> フィールドを宣言します。

     [!code-csharp[PropertyMappingWithElementHost#16](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#16)]
     [!code-vb[PropertyMappingWithElementHost#16](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#16)]

## <a name="defining-new-property-mappings"></a>新しいプロパティマッピングの定義

<xref:System.Windows.Forms.Integration.ElementHost> コントロールには、いくつかの既定のプロパティマッピングが用意されています。 新しいプロパティマッピングを追加するには、<xref:System.Windows.Forms.Integration.ElementHost> コントロールの <xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A>の <xref:System.Windows.Forms.Integration.PropertyMap.Add%2A> メソッドを呼び出します。

### <a name="to-define-new-property-mappings"></a>新しいプロパティマッピングを定義するには

1. `Form1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#12](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#12)]
     [!code-vb[PropertyMappingWithElementHost#12](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#12)]

     `AddMarginMapping` メソッドは、<xref:System.Windows.Forms.Control.Margin%2A> プロパティの新しいマッピングを追加します。

     `OnMarginChange` メソッドは、<xref:System.Windows.Forms.Control.Margin%2A> プロパティを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.FrameworkElement.Margin%2A> プロパティに変換します。

2. `Form1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#14](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#14)]
     [!code-vb[PropertyMappingWithElementHost#14](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#14)]

     `AddRegionMapping` メソッドは、<xref:System.Windows.Forms.Control.Region%2A> プロパティの新しいマッピングを追加します。

     `OnRegionChange` メソッドは、<xref:System.Windows.Forms.Control.Region%2A> プロパティを [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.UIElement.Clip%2A> プロパティに変換します。

     `Form1_Resize` メソッドは、フォームの <xref:System.Windows.Forms.Control.Resize> イベントを処理し、ホストされている要素に合うようにクリッピング領域のサイズを調整します。

## <a name="removing-a-default-property-mapping"></a>既定のプロパティマッピングの削除

<xref:System.Windows.Forms.Integration.ElementHost> コントロールの <xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A>の <xref:System.Windows.Forms.Integration.PropertyMap.Remove%2A> メソッドを呼び出すことによって、既定のプロパティマッピングを削除します。

### <a name="to-remove-a-default-property-mapping"></a>既定のプロパティマッピングを削除するには

- `Form1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#13](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#13)]
     [!code-vb[PropertyMappingWithElementHost#13](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#13)]

     `RemoveCursorMapping` メソッドは、<xref:System.Windows.Forms.Control.Cursor%2A> プロパティの既定のマッピングを削除します。

## <a name="extending-a-default-property-mapping"></a>既定のプロパティマッピングの拡張

既定のプロパティマッピングを使用し、独自のマッピングで拡張することもできます。

### <a name="to-extend-a-default-property-mapping"></a>既定のプロパティマッピングを拡張するには

- `Form1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#15](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#15)]
     [!code-vb[PropertyMappingWithElementHost#15](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#15)]

     `ExtendBackColorMapping` メソッドは、既存の <xref:System.Windows.Forms.Control.BackColor%2A> プロパティのマッピングにカスタムプロパティトランスレーターを追加します。

     `OnBackColorChange` メソッドは、ホストされているコントロールの <xref:System.Windows.Controls.Control.Background%2A> プロパティに特定のイメージを割り当てます。 `OnBackColorChange` メソッドは、既定のプロパティマッピングが適用された後に呼び出されます。

## <a name="initialize-your-property-mappings"></a>プロパティマッピングの初期化

1. `Form1` クラスの定義に次のコードをコピーします。

     [!code-csharp[PropertyMappingWithElementHost#11](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithElementHost/CSharp/PropertyMappingWithElementHost/Form1.cs#11)]
     [!code-vb[PropertyMappingWithElementHost#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithElementHost/VisualBasic/PropertyMappingWithElementHost/Form1.vb#11)]

     `Form1_Load` メソッドは、<xref:System.Windows.Forms.Form.Load> イベントを処理し、次の初期化を実行します。

    - <xref:System.Windows.Controls.Button> 要素 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] を作成します。

    - チュートリアルの前の手順で定義したメソッドを呼び出して、プロパティマッピングを設定します。

    - マップされたプロパティに初期値を割り当てます。

2. F5 キーを押してアプリケーションをビルドし、実行します。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Windows フォームと WPF プロパティの割り当て](windows-forms-and-wpf-property-mapping.md)
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
