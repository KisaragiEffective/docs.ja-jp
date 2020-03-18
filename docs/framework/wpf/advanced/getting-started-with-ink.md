---
title: Visual Studio での System.windows.controls.inkcanvas> の作成
ms.date: 08/15/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- procedural code in lieu of XAML [WPF]
- XAML [WPF], procedural code in lieu of
- InkCanvas (WPF)
ms.assetid: 760332dd-594a-475d-865b-01659db8cab7
ms.openlocfilehash: b8087d6db04f7024b9ee48f28002bee04045a14b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737888"
---
# <a name="get-started-with-ink-in-wpf"></a>WPF でインクを使ってみる

Windows Presentation Foundation (WPF) には、デジタルインクをアプリに簡単に組み込むことができるインク機能があります。

## <a name="prerequisites"></a>前提条件

次の例を使用するには、最初に[Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)をインストールします。 また、基本的な WPF アプリの記述方法を理解するのにも役立ちます。 WPF の概要については、「[チュートリアル: 初めての wpf デスクトップアプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)」を参照してください。

## <a name="quick-start"></a>クイック スタート

このセクションでは、インクを収集する単純な WPF アプリケーションを作成する方法について説明します。

### <a name="got-ink"></a>インクを持っていますか?

インクをサポートする WPF アプリを作成するには:

1. Visual Studio を開きます。

2. 新しい**WPF アプリ**を作成します。

   **[新しいプロジェクト]** ダイアログで、[**インストールされている** > **ビジュアルC#**  ] または [ **Visual Basic** > **Windows デスクトップ**] カテゴリを展開します。 次に、 **[WPF アプリ (.NET Framework)]** アプリテンプレートを選択します。 名前を入力し、[ **OK]** を選択します。

   Visual Studio によってプロジェクトが作成され、 *mainwindow.xaml*がデザイナーに表示されます。

3. `<Grid>` タグ間の `<InkCanvas/>` を入力します。

   ![System.windows.controls.inkcanvas> タグを持つ XAML デザイナー](./media/getting-started-with-ink/inkcanvas-xaml.png)

4. **F5**キーを押して、デバッガーでアプリケーションを起動します。

5. スタイラスまたはマウスを使用して、ウィンドウに**hello world**と記述します。

"Hello world" アプリケーションに相当するインクを12個のキーストロークだけで記述しました。

### <a name="spice-up-your-app"></a>アプリをスパイスする

WPF の一部の機能を活用してみましょう。 \<ウィンドウの開始と終了の間にあるすべての要素 > タグを次のマークアップに置き換えます。

```xaml
<Page>
  <InkCanvas Name="myInkCanvas" MouseRightButtonUp="RightMouseUpHandler">
    <InkCanvas.Background>
      <LinearGradientBrush>
        <GradientStop Color="Yellow" Offset="0.0" />
          <GradientStop Color="Blue" Offset="0.5" />
            <GradientStop Color="HotPink" Offset="1.0" />
              </LinearGradientBrush>
    </InkCanvas.Background>
  </InkCanvas>
</Page>
```

この XAML は、インク描画サーフェイスにグラデーションブラシの背景を作成します。

![WPF アプリでのインク描画画面のグラデーションの色](./media/getting-started-with-ink/gradient-colors.png)

### <a name="add-some-code-behind-the-xaml"></a>XAML の背後にコードを追加する

XAML を使用すると、ユーザーインターフェイスを簡単にデザインできますが、実際のアプリケーションでは、イベントを処理するコードを追加する必要があります。 マウスからの右クリックに応じてインクを拡大する簡単な例を次に示します。

1. XAML で `MouseRightButtonUp` ハンドラーを設定します。

   [!code-xaml[DigitalInkTopics#3](~/samples/snippets/csharp/VS_Snippets_Wpf/DigitalInkTopics/CSharp/Window2.xaml#3)]

1. **ソリューションエクスプローラー**で、mainwindow.xaml を展開し、分離コードファイル (MainWindow.xaml.cs または mainwindow.xaml) を開きます。 次のイベントハンドラーコードを追加します。

   [!code-csharp[DigitalInkTopics#4](~/samples/snippets/csharp/VS_Snippets_Wpf/DigitalInkTopics/CSharp/Window2.xaml.cs#4)]
   [!code-vb[DigitalInkTopics#4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DigitalInkTopics/VisualBasic/Window2.xaml.vb#4)]

1. アプリケーションを実行します。 インクをいくつか追加し、マウスで右クリックするか、またはスタイラスを使用してプレスアンドホールドと同等の操作を実行します。

   マウスの右ボタンをクリックするたびに、画面がズームされます。

### <a name="use-procedural-code-instead-of-xaml"></a>XAML ではなく手続き型コードを使用する

すべての WPF 機能には、手続き型コードからアクセスできます。 次の手順に従って、XAML をまったく使用しない "Hello Ink World" アプリケーションを WPF に作成します。

1. Visual Studio で新しいコンソールアプリケーションプロジェクトを作成します。

   **[新しいプロジェクト]** ダイアログで、[**インストールされている** > **ビジュアルC#**  ] または [ **Visual Basic** > **Windows デスクトップ**] カテゴリを展開します。 次に、 **[コンソールアプリ (.NET Framework)]** アプリテンプレートを選択します。 名前を入力し、[ **OK]** を選択します。

1. Program.cs ファイルまたは .vb ファイルに次のコードを貼り付けます。

   [!code-csharp[InkCanvasConsoleApp#1](~/samples/snippets/csharp/VS_Snippets_Wpf/InkCanvasConsoleApp/CSharp/Program.cs#1)]
   [!code-vb[InkCanvasConsoleApp#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InkCanvasConsoleApp/VisualBasic/Module1.vb#1)]

1. **ソリューションエクスプローラー**の**参照**を右クリックし、 **[参照の追加]** を選択して、プレゼンテーションコア、プレゼンテーションフレームワーク、および windowsbase アセンブリへの参照を追加します。

   ![プレゼンテーションコアとプレゼンテーションフレームワークを示すリファレンスマネージャー](./media/getting-started-with-ink/reference-manager-presentationcore-presentationframework.png)

1. **F5**キーを押してアプリケーションをビルドします。

## <a name="see-also"></a>参照

- [デジタル インク](digital-ink.md)
- [インクの収集](collecting-ink.md)
- [手書き認識](handwriting-recognition.md)
- [インクの格納](storing-ink.md)
