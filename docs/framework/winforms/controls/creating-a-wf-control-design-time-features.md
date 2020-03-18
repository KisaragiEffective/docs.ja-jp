---
title: Visual Studio のデザイン時機能を利用するコントロールを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms controls, creating
- design-time functionality [Windows Forms], Windows Forms
- DocumentDesigner class [Windows Forms]
- walkthroughs [Windows Forms], controls
ms.assetid: 6f487c59-cb38-4afa-ad2e-95edacb1d626
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 7166b4203c54ab31f1d929c85cf1e6481ff120f8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744079"
---
# <a name="walkthrough-create-a-control-that-takes-advantage-of-design-time-features"></a>チュートリアル: デザイン時機能を活用したコントロールの作成

カスタムコントロールのデザイン時のエクスペリエンスは、関連するカスタムデザイナーを作成することによって拡張できます。

この記事では、カスタムコントロールのカスタムデザイナーを作成する方法について説明します。 `MarqueeControl` 型と、それに関連付けられた `MarqueeControlRootDesigner`と呼ばれるデザイナークラスを実装します。

`MarqueeControl` 型は、アニメーション化されたライトと点滅するテキストを含むシアターマーキーに似た表示を実装します。

このコントロールのデザイナーは、デザイン時のカスタムエクスペリエンスを提供するために、デザイン環境と対話します。 カスタムデザイナーを使用すると、さまざまな組み合わせでアニメーション化されたライトと点滅するテキストを使用して、カスタム `MarqueeControl` 実装を組み立てることができます。 アセンブルされたコントロールは、他の Windows フォームコントロールと同様に、フォーム上で使用できます。

このチュートリアルを終了すると、カスタムコントロールは次のようになります。

![テキストと [開始] および [停止] ボタンを示すマーキーを表示するアプリ。](./media/creating-a-wf-control-design-time-features/demo-marquee-control.gif)

完全なコードリストについては、「[方法: デザイン時機能を利用する Windows フォームコントロールを作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/307hck25(v=vs.120))する」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、Visual Studio が必要です。

## <a name="create-the-project"></a>プロジェクトを作成する

最初にアプリケーションのプロジェクトを作成します。 このプロジェクトは、カスタムコントロールをホストするアプリケーションをビルドするために使用します。

Visual Studio で、新しい Windows フォームアプリケーションプロジェクトを作成し、 **MarqueeControlTest**という名前を指定します。

## <a name="create-the-control-library-project"></a>コントロールライブラリプロジェクトを作成する

1. ソリューションに Windows フォームコントロールライブラリプロジェクトを追加します。 プロジェクトに**MarqueeControlLibrary**という名前を指定します。

2. **ソリューションエクスプローラー**を使用して、選択した言語に応じて、"UserControl1.cs" または "UserControl1" という名前のソースファイルを削除して、プロジェクトの既定のコントロールを削除します。

3. 新しい <xref:System.Windows.Forms.UserControl> 項目を `MarqueeControlLibrary` プロジェクトに追加します。 新しいソースファイルに**MarqueeControl**のベース名を指定します。

4. **ソリューションエクスプローラー**を使用して、`MarqueeControlLibrary` プロジェクトに新しいフォルダーを作成します。

5. **Design**フォルダーを右クリックし、新しいクラスを追加します。 **MarqueeControlRootDesigner**という名前を指定します。

6. System.string アセンブリの型を使用する必要があるため、この参照を `MarqueeControlLibrary` プロジェクトに追加します。

## <a name="reference-the-custom-control-project"></a>カスタムコントロールプロジェクトを参照する

カスタムコントロールをテストするには、`MarqueeControlTest` プロジェクトを使用します。 `MarqueeControlLibrary` アセンブリにプロジェクト参照を追加すると、テストプロジェクトはカスタムコントロールを認識するようになります。

`MarqueeControlTest` プロジェクトで、`MarqueeControlLibrary` アセンブリへのプロジェクト参照を追加します。 `MarqueeControlLibrary` アセンブリを直接参照するのではなく、 **[参照の追加]** ダイアログボックスの **[プロジェクト]** タブを使用するようにしてください。

## <a name="define-a-custom-control-and-its-custom-designer"></a>カスタムコントロールとそのカスタムデザイナーを定義する

カスタムコントロールは <xref:System.Windows.Forms.UserControl> クラスから派生します。 これにより、コントロールに他のコントロールを含めることができます。また、コントロールには、既定の機能が多数用意されています。

カスタムコントロールには、関連付けられたカスタムデザイナーがあります。 これにより、カスタムコントロール専用にカスタマイズされた独自のデザインエクスペリエンスを作成できます。

コントロールをデザイナーに関連付けるには、<xref:System.ComponentModel.DesignerAttribute> クラスを使用します。 カスタムコントロールのデザイン時動作全体を開発しているため、カスタムデザイナーは <xref:System.ComponentModel.Design.IRootDesigner> インターフェイスを実装します。

### <a name="to-define-a-custom-control-and-its-custom-designer"></a>カスタムコントロールとそのカスタムデザイナーを定義するには

1. **コードエディター**で `MarqueeControl` ソースファイルを開きます。 ファイルの先頭に、次の名前空間をインポートします。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#220](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#220)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#220](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#220)]

2. <xref:System.ComponentModel.DesignerAttribute> を `MarqueeControl` クラスの宣言に追加します。 これにより、カスタムコントロールがデザイナーに関連付けられます。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#240](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#240)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#240](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#240)]

3. **コードエディター**で `MarqueeControlRootDesigner` ソースファイルを開きます。 ファイルの先頭に、次の名前空間をインポートします。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#520](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#520)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#520](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#520)]

4. <xref:System.Windows.Forms.Design.DocumentDesigner> クラスを継承するように `MarqueeControlRootDesigner` の宣言を変更します。 <xref:System.ComponentModel.ToolboxItemFilterAttribute> を適用して、デザイナーと**ツールボックス**の対話を指定します。

   > [!NOTE]
   > `MarqueeControlRootDesigner` クラスの定義は、MarqueeControlLibrary という名前空間で囲まれています。 この宣言により、デザイン関連の型用に予約された特殊な名前空間にデザイナーが配置されます。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#530](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#530)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#530](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#530)]

5. `MarqueeControlRootDesigner` クラスのコンストラクターを定義します。 <xref:System.Diagnostics.Trace.WriteLine%2A> ステートメントをコンストラクター本体に挿入します。 これはデバッグに役立ちます。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#540](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#540)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#540](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#540)]

## <a name="create-an-instance-of-your-custom-control"></a>カスタムコントロールのインスタンスを作成する

1. 新しい <xref:System.Windows.Forms.UserControl> 項目を `MarqueeControlTest` プロジェクトに追加します。 新しいソースファイルに**DemoMarqueeControl**のベース名を指定します。

2. **コードエディター**で `DemoMarqueeControl` ファイルを開きます。 ファイルの先頭に、`MarqueeControlLibrary` 名前空間をインポートします。

   ```vb
   Imports MarqueeControlLibrary
   ```

   ```csharp
   using MarqueeControlLibrary;
   ```

3. `MarqueeControl` クラスを継承するように `DemoMarqueeControl` の宣言を変更します。

4. プロジェクトをビルドします。

5. Windows フォームデザイナーで Form1 を開きます。

6. **ツールボックス**の **[MarqueeControlTest Components]** タブを見つけて開きます。 **[ツールボックス]** から `DemoMarqueeControl` をフォームにドラッグします。

7. プロジェクトをビルドします。

## <a name="set-up-the-project-for-design-time-debugging"></a>デザイン時デバッグ用にプロジェクトを設定する

カスタムデザイン時のエクスペリエンスを開発している場合は、コントロールとコンポーネントをデバッグする必要があります。 デザイン時にデバッグを許可するようにプロジェクトを設定する簡単な方法があります。 詳細については、「[チュートリアル: デザイン時のカスタム Windows フォームコントロールのデバッグ](walkthrough-debugging-custom-windows-forms-controls-at-design-time.md)」を参照してください。

1. `MarqueeControlLibrary` プロジェクトを右クリックし、 **[プロパティ]** を選択します。

2. **[MarqueeControlLibrary プロパティページ]** ダイアログボックスで、 **[デバッグ]** ページを選択します。

3. **[開始アクション]** セクションで、 **[外部プログラムの開始]** を選択します。 Visual Studio の別のインスタンスをデバッグするには、省略記号![([Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] ボタンをクリックして、Visual Studio IDE を参照します。 実行可能ファイルの名前は devenv.exe で、を既定の場所にインストールした場合、そのパスは *% ProgramFiles (x86)% \ Microsoft Visual Studio\2019\\\<edition > \Common7\IDE\devenv.exe*になります。

4. **[OK]** を選択してダイアログ ボックスを閉じます。

5. MarqueeControlLibrary プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択して、このデバッグ構成を有効にします。

## <a name="checkpoint"></a>Checkpoint

これで、カスタムコントロールのデザイン時動作をデバッグする準備ができました。 デバッグ環境が正しく設定されていることを確認したら、カスタムコントロールとカスタムデザイナーの関連付けをテストします。

### <a name="to-test-the-debugging-environment-and-the-designer-association"></a>デバッグ環境とデザイナーの関連付けをテストするには

1. **コードエディター**で MarqueeControlRootDesigner ソースファイルを開き、<xref:System.Diagnostics.Trace.WriteLine%2A> ステートメントにブレークポイントを設定します。

2. **F5**キーを押してデバッグセッションを開始します。

   Visual Studio の新しいインスタンスが作成されます。

3. Visual Studio の新しいインスタンスで、MarqueeControlTest ソリューションを開きます。 **[ファイル]** メニューから **[最近使ったプロジェクト]** を選択すると、ソリューションを簡単に見つけることができます。 MarqueeControlTest ソリューションファイルは、最近使用したファイルとして一覧表示されます。

4. デザイナーで `DemoMarqueeControl` を開きます。

   Visual Studio のデバッグインスタンスによってフォーカスが取得され、ブレークポイントで実行が停止します。 **F5**キーを押してデバッグセッションを続行します。

この時点で、カスタムコントロールとそれに関連付けられているカスタムデザイナーを開発およびデバッグするために、すべてが準備されています。 この記事の残りの部分では、コントロールとデザイナーの機能を実装する方法の詳細について説明します。

## <a name="implement-the-custom-control"></a>カスタムコントロールを実装する

`MarqueeControl` は、カスタマイズが少ししかない <xref:System.Windows.Forms.UserControl> です。 これは、マーキーアニメーションを開始する `Start`と、アニメーションを停止する `Stop`の2つのメソッドを公開します。 `MarqueeControl` には `IMarqueeWidget` インターフェイスを実装する子コントロールが含まれているため、`StartMarquee` を実装する各子コントロールに対して、それぞれの子コントロールを `Start` および `Stop` 列挙し、それぞれ `StopMarquee` メソッドと `IMarqueeWidget`メソッドを呼び出します。

`MarqueeBorder` コントロールと `MarqueeText` コントロールの外観はレイアウトに依存しているため、`MarqueeControl` は <xref:System.Windows.Forms.Control.OnLayout%2A> メソッドをオーバーライドし、この型の子コントロールに対して <xref:System.Windows.Forms.Control.PerformLayout%2A> を呼び出します。

これは `MarqueeControl` カスタマイズの範囲です。 実行時の機能は `MarqueeBorder` および `MarqueeText` コントロールによって実装され、デザイン時の機能は `MarqueeBorderDesigner` クラスと `MarqueeControlRootDesigner` クラスによって実装されます。

### <a name="to-implement-your-custom-control"></a>カスタムコントロールを実装するには

1. **コードエディター**で `MarqueeControl` ソースファイルを開きます。 `Start` メソッドと `Stop` メソッドを実装します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#260](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#260)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#260](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#260)]

2. <xref:System.Windows.Forms.Control.OnLayout%2A> メソッドをオーバーライドします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#270](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#270)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#270](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#270)]

## <a name="create-a-child-control-for-your-custom-control"></a>カスタムコントロールの子コントロールを作成する

`MarqueeControl` は、`MarqueeBorder` コントロールと `MarqueeText` コントロールの2種類の子コントロールをホストします。

- `MarqueeBorder`: このコントロールは、端の周りに "ライト" の境界線を描画します。 ライトが連続して点滅しているように見えます。 ライトフラッシュが `UpdatePeriod`というプロパティによって制御される速度。 他のいくつかのカスタムプロパティによって、コントロールの外観の他の側面が決まります。 `StartMarquee` と `StopMarquee`と呼ばれる2つのメソッドは、アニメーションの開始時と停止時に制御します。

- `MarqueeText`: このコントロールは、点滅する文字列を描画します。 `MarqueeBorder` コントロールと同様に、テキストが点滅する速度は、`UpdatePeriod` プロパティによって制御されます。 `MarqueeText` コントロールには、`MarqueeBorder` コントロールと共通の `StartMarquee` および `StopMarquee` メソッドもあります。

デザイン時には、この `MarqueeControlRootDesigner` によって、これら2つのコントロール型を任意の組み合わせで `MarqueeControl` に追加できます。

2つのコントロールの共通機能は、`IMarqueeWidget`と呼ばれるインターフェイスに考慮されます。 これにより、`MarqueeControl` は、マーキーに関連する子コントロールを検出し、特別な処理を行うことができます。

定期的なアニメーション機能を実装するには、<xref:System.ComponentModel?displayProperty=nameWithType> 名前空間の <xref:System.ComponentModel.BackgroundWorker> オブジェクトを使用します。 <xref:System.Windows.Forms.Timer> オブジェクトを使用することもできますが、多くの `IMarqueeWidget` オブジェクトが存在すると、1つの UI スレッドがアニメーションを維持できなくなる可能性があります。

### <a name="to-create-a-child-control-for-your-custom-control"></a>カスタムコントロールの子コントロールを作成するには

1. 新しいクラス項目を `MarqueeControlLibrary` プロジェクトに追加します。 新しいソースファイルに "IMarqueeWidget" のベース名を指定します。

2. **コードエディター**で `IMarqueeWidget` ソースファイルを開き、宣言を `class` から `interface`に変更します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/imarqueewidget.cs#2)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/imarqueewidget.vb#2)]

3. 次のコードを `IMarqueeWidget` インターフェイスに追加して、2つのメソッドと、マーキーアニメーションを操作するプロパティを公開します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/imarqueewidget.cs#3)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/imarqueewidget.vb#3)]

4. 新しい**カスタムコントロール**項目を `MarqueeControlLibrary` プロジェクトに追加します。 新しいソースファイルに "MarqueeText" のベース名を指定します。

5. <xref:System.ComponentModel.BackgroundWorker> コンポーネントを **[ツールボックス]** から `MarqueeText` コントロールにドラッグします。 このコンポーネントを使用すると、`MarqueeText` コントロール自体を非同期的に更新できます。

6. **[プロパティ]** ウィンドウで、<xref:System.ComponentModel.BackgroundWorker> コンポーネントの `WorkerReportsProgress` と <xref:System.ComponentModel.BackgroundWorker.WorkerSupportsCancellation%2A> プロパティを**true**に設定します。 これらの設定により、<xref:System.ComponentModel.BackgroundWorker> コンポーネントは定期的に <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントを発生させ、非同期更新を取り消すことができます。

   詳細については、「 [BackgroundWorker Component](backgroundworker-component.md)」を参照してください。

7. **コードエディター**で `MarqueeText` ソースファイルを開きます。 ファイルの先頭に、次の名前空間をインポートします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#120](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#120)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#120)]

8. <xref:System.Windows.Forms.Label> から継承するように `MarqueeText` の宣言を変更し、`IMarqueeWidget` インターフェイスを実装します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#130](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#130)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#130](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#130)]

9. 公開されたプロパティに対応するインスタンス変数を宣言し、コンストラクターで初期化します。 `isLit` フィールドは、`LightColor` プロパティによって指定された色でテキストを描画するかどうかを決定します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#140](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#140)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#140](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#140)]

10. `IMarqueeWidget` インターフェイスを実装します。

    `StartMarquee` メソッドと `StopMarquee` メソッドは、<xref:System.ComponentModel.BackgroundWorker> コンポーネントの <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A> および <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> メソッドを呼び出して、アニメーションを開始および停止します。

    <xref:System.ComponentModel.CategoryAttribute.Category%2A> 属性と <xref:System.ComponentModel.BrowsableAttribute.Browsable%2A> 属性が `UpdatePeriod` プロパティに適用されるため、"Marquee" と呼ばれるプロパティウィンドウのカスタムセクションに表示されます。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#150](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#150)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#150](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#150)]

11. プロパティアクセサーを実装します。 クライアントには、`LightColor` と `DarkColor`の2つのプロパティが公開されます。 これらのプロパティには <xref:System.ComponentModel.CategoryAttribute.Category%2A> 属性と <xref:System.ComponentModel.BrowsableAttribute.Browsable%2A> 属性が適用されるため、"Marquee" と呼ばれるプロパティウィンドウのカスタムセクションにプロパティが表示されます。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#160](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#160)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#160](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#160)]

12. <xref:System.ComponentModel.BackgroundWorker> コンポーネントの <xref:System.ComponentModel.BackgroundWorker.DoWork> イベントと <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントのハンドラーを実装します。

    <xref:System.ComponentModel.BackgroundWorker.DoWork> イベントハンドラーは、`UpdatePeriod` によって指定されたミリ秒数、<xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントを発生させます。これにより、コードは <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A>を呼び出してアニメーションを停止します。

    <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントハンドラーは、テキストを淡色と濃色の状態の間で切り替えて、点滅のようにします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#180](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#180)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#180](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#180)]

13. アニメーションを有効にするには、<xref:System.Windows.Forms.Control.OnPaint%2A> メソッドをオーバーライドします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#170](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#170)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#170](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#170)]

14. **F6** キーを押して、ソリューションをビルドします。

## <a name="create-the-marqueeborder-child-control"></a>MarqueeBorder 子コントロールを作成する

`MarqueeBorder` コントロールは、`MarqueeText` コントロールよりも多少洗練されています。 これには、さらに多くのプロパティがあり、<xref:System.Windows.Forms.Control.OnPaint%2A> メソッドのアニメーションも複雑になります。 原則として、`MarqueeText` コントロールと非常によく似ています。

`MarqueeBorder` コントロールは子コントロールを持つことができるため、<xref:System.Windows.Forms.Control.Layout> イベントに注意する必要があります。

### <a name="to-create-the-marqueeborder-control"></a>MarqueeBorder コントロールを作成するには

1. 新しい**カスタムコントロール**項目を `MarqueeControlLibrary` プロジェクトに追加します。 新しいソースファイルに "MarqueeBorder" のベース名を指定します。

2. <xref:System.ComponentModel.BackgroundWorker> コンポーネントを **[ツールボックス]** から `MarqueeBorder` コントロールにドラッグします。 このコンポーネントを使用すると、`MarqueeBorder` コントロール自体を非同期的に更新できます。

3. **[プロパティ]** ウィンドウで、<xref:System.ComponentModel.BackgroundWorker> コンポーネントの `WorkerReportsProgress` と <xref:System.ComponentModel.BackgroundWorker.WorkerSupportsCancellation%2A> プロパティを**true**に設定します。 これらの設定により、<xref:System.ComponentModel.BackgroundWorker> コンポーネントは定期的に <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントを発生させ、非同期更新を取り消すことができます。 詳細については、「 [BackgroundWorker Component](backgroundworker-component.md)」を参照してください。

4. **[プロパティ]** ウィンドウで、 **[イベント]** ボタンを選択します。 <xref:System.ComponentModel.BackgroundWorker.DoWork> イベントと <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントのハンドラーをアタッチします。

5. **コードエディター**で `MarqueeBorder` ソースファイルを開きます。 ファイルの先頭に、次の名前空間をインポートします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#20)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#20)]

6. <xref:System.Windows.Forms.Panel> から継承するように `MarqueeBorder` の宣言を変更し、`IMarqueeWidget` インターフェイスを実装します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#30)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#30)]

7. `MarqueeBorder` コントロールの状態を管理するための2つの列挙体を宣言します。 `MarqueeSpinDirection`は、光源の周囲を "スピン" する方向を決定し、`MarqueeLightShape`、ライトの形状 (正方形または円) を決定します。 これらの宣言は `MarqueeBorder` クラス宣言の前に配置します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#97](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#97)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#97](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#97)]

8. 公開されたプロパティに対応するインスタンス変数を宣言し、コンストラクターで初期化します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#40](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#40)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#40](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#40)]

9. `IMarqueeWidget` インターフェイスを実装します。

    `StartMarquee` メソッドと `StopMarquee` メソッドは、<xref:System.ComponentModel.BackgroundWorker> コンポーネントの <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A> および <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> メソッドを呼び出して、アニメーションを開始および停止します。

    `MarqueeBorder` コントロールには子コントロールを含めることができるため、`StartMarquee` メソッドは、すべての子コントロールを列挙し、`IMarqueeWidget`を実装するコントロールに対して `StartMarquee` を呼び出します。 `StopMarquee` メソッドには同様の実装があります。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#50](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#50)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#50](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#50)]

10. プロパティアクセサーを実装します。 `MarqueeBorder` コントロールには、外観を制御するためのプロパティがいくつかあります。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#60](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#60)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#60](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#60)]

11. <xref:System.ComponentModel.BackgroundWorker> コンポーネントの <xref:System.ComponentModel.BackgroundWorker.DoWork> イベントと <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントのハンドラーを実装します。

    <xref:System.ComponentModel.BackgroundWorker.DoWork> イベントハンドラーは、`UpdatePeriod` によって指定されたミリ秒数、<xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントを発生させます。これにより、コードは <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A>を呼び出してアニメーションを停止します。

    <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> イベントハンドラーは、他のライトのライト/ダーク状態が決定される "基本" ライトの位置をインクリメントし、<xref:System.Windows.Forms.Control.Refresh%2A> メソッドを呼び出して、コントロール自体を再描画します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#90](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#90)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#90](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#90)]

12. ヘルパーメソッド、`IsLit` および `DrawLight`を実装します。

    `IsLit` メソッドは、指定された位置にあるライトの色を決定します。 "Lit" のライトは、`LightColor` プロパティによって指定された色で描画され、"ダーク" であるライトは、`DarkColor` プロパティによって指定された色で描画されます。

    `DrawLight` メソッドは、適切な色、形状、および位置を使用してライトを描画します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#80](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#80)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#80](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#80)]

13. <xref:System.Windows.Forms.Control.OnLayout%2A> メソッドと <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドをオーバーライドします。

    <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドは、`MarqueeBorder` コントロールの端に沿ってライトを描画します。

    <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドは `MarqueeBorder` コントロールの大きさに依存しているため、レイアウトが変更されるたびに呼び出す必要があります。 これを実現するには、<xref:System.Windows.Forms.Control.OnLayout%2A> をオーバーライドし、<xref:System.Windows.Forms.Control.Refresh%2A>を呼び出します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#70](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#70)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#70](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#70)]

## <a name="create-a-custom-designer-to-shadow-and-filter-properties"></a>カスタムデザイナーを作成してプロパティをシャドウおよびフィルター処理する

`MarqueeControlRootDesigner` クラスは、ルートデザイナーの実装を提供します。 `MarqueeControl`で動作するこのデザイナーに加えて、`MarqueeBorder` コントロールに特に関連付けられたカスタムデザイナーが必要です。 このデザイナーには、カスタムルートデザイナーのコンテキストに適したカスタム動作が用意されています。

具体的には、`MarqueeBorderDesigner` は `MarqueeBorder` コントロールの特定のプロパティを "シャドウ" してフィルター処理し、デザイン環境との相互作用を変更します。

コンポーネントのプロパティアクセサーへの呼び出しのインターセプトは、"シャドウ処理" と呼ばれます。 これにより、デザイナーはユーザーが設定した値を追跡し、必要に応じてその値をデザイン対象のコンポーネントに渡すことができます。

この例では、<xref:System.Windows.Forms.Control.Visible%2A> プロパティと <xref:System.Windows.Forms.Control.Enabled%2A> プロパティが `MarqueeBorderDesigner`によってシャドウされます。これにより、デザイン時にユーザーが `MarqueeBorder` コントロールを非表示または無効にすることができなくなります。

デザイナーでは、プロパティを追加および削除することもできます。 この例では、`MarqueeBorder` コントロールは `LightSize` プロパティによって指定されたライトのサイズに基づいて埋め込みを設定するため、デザイン時に <xref:System.Windows.Forms.Control.Padding%2A> プロパティは削除されます。

`MarqueeBorderDesigner` の基本クラスは <xref:System.ComponentModel.Design.ComponentDesigner>です。このクラスには、デザイン時にコントロールによって公開される属性、プロパティ、およびイベントを変更するためのメソッドがあります。

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterProperties%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterProperties%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterAttributes%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterAttributes%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterEvents%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterEvents%2A>

これらのメソッドを使用してコンポーネントのパブリックインターフェイスを変更する場合は、次の規則に従います。

- `PreFilter` メソッドでのみ項目を追加または削除する

- `PostFilter` メソッドの既存の項目のみを変更する

- 常に基本実装を `PreFilter` メソッドで呼び出す

- `PostFilter` メソッドの最後に基本実装を呼び出します。

これらの規則に従うことで、デザイン時環境のすべてのデザイナーが、デザインされているすべてのコンポーネントの一貫したビューを確実に表示できるようになります。

<xref:System.ComponentModel.Design.ComponentDesigner> クラスは、シャドウプロパティの値を管理するためのディクショナリを提供します。これにより、特定のインスタンス変数を作成する必要がなくなります。

### <a name="to-create-a-custom-designer-to-shadow-and-filter-properties"></a>プロパティをシャドウおよびフィルター処理するためのカスタムデザイナーを作成するには

1. **Design**フォルダーを右クリックし、新しいクラスを追加します。 ソースファイルに**MarqueeBorderDesigner**のベース名を指定します。

2. **コードエディター**で MarqueeBorderDesigner ソースファイルを開きます。 ファイルの先頭に、次の名前空間をインポートします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#420](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#420)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#420](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#420)]

3. <xref:System.Windows.Forms.Design.ParentControlDesigner>から継承するように `MarqueeBorderDesigner` の宣言を変更します。

    `MarqueeBorder` コントロールには子コントロールを含めることができるため、`MarqueeBorderDesigner` は親子の相互作用を処理する <xref:System.Windows.Forms.Design.ParentControlDesigner>から継承されます。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#430](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#430)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#430](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#430)]

4. <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterProperties%2A>の基本実装をオーバーライドします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#450](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#450)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#450](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#450)]

5. <xref:System.Windows.Forms.Control.Enabled%2A> プロパティと <xref:System.Windows.Forms.Control.Visible%2A> プロパティを実装します。 これらの実装は、コントロールのプロパティをシャドウします。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#440](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#440)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#440](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#440)]

## <a name="handle-component-changes"></a>コンポーネントの変更を処理する

`MarqueeControlRootDesigner` クラスは、`MarqueeControl` インスタンスのカスタムデザイン時エクスペリエンスを提供します。 デザイン時の機能のほとんどは、<xref:System.Windows.Forms.Design.DocumentDesigner> クラスから継承されています。 コードには、コンポーネントの変更の処理とデザイナー動詞の追加という2つのカスタマイズのカスタマイズが実装されます。

ユーザーが `MarqueeControl` インスタンスをデザインすると、ルートデザイナーは `MarqueeControl` とその子コントロールに対する変更を追跡します。 デザイン時環境では、コンポーネントの状態への変更を追跡するための便利なサービス <xref:System.ComponentModel.Design.IComponentChangeService>が提供されます。

このサービスへの参照を取得するには、<xref:System.ComponentModel.Design.ComponentDesigner.GetService%2A> メソッドを使用して環境に対してクエリを実行します。 クエリが成功した場合、デザイナーは <xref:System.ComponentModel.Design.IComponentChangeService.ComponentChanged> イベントのハンドラーをアタッチし、デザイン時に一貫性のある状態を維持するために必要なすべてのタスクを実行できます。

`MarqueeControlRootDesigner` クラスの場合は、`MarqueeControl`に含まれる各 `IMarqueeWidget` オブジェクトで <xref:System.Windows.Forms.Control.Refresh%2A> メソッドを呼び出します。 これにより、親の <xref:System.Windows.Forms.Control.Size%2A> のようなプロパティが変更された場合に、`IMarqueeWidget` オブジェクトが適切に再描画されます。

### <a name="to-handle-component-changes"></a>コンポーネントの変更を処理するには

1. **コードエディター**で `MarqueeControlRootDesigner` ソースファイルを開き、<xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> メソッドをオーバーライドします。 <xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> の基本実装を呼び出し、<xref:System.ComponentModel.Design.IComponentChangeService>に対してクエリを実行します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#580](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#580)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#580](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#580)]

2. <xref:System.ComponentModel.Design.IComponentChangeService.OnComponentChanged%2A> イベントハンドラーを実装します。 送信コンポーネントの型をテストし、それが `IMarqueeWidget`である場合は、その <xref:System.Windows.Forms.Control.Refresh%2A> メソッドを呼び出します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#560](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#560)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#560](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#560)]

## <a name="add-designer-verbs-to-your-custom-designer"></a>デザイナー動詞をカスタムデザイナーに追加する

デザイナー動詞は、イベント ハンドラーにリンクされたメニュー コマンドです。 デザイナー動詞は、デザイン時にコンポーネントのショートカットメニューに追加されます。 詳細については、<xref:System.ComponentModel.Design.DesignerVerb> を参照してください。

デザイナーに2つのデザイナー動詞を追加します。**テストを実行**し、**テストを停止**します。 これらの動詞を使用すると、デザイン時に `MarqueeControl` の実行時の動作を表示できます。 これらの動詞は `MarqueeControlRootDesigner`に追加されます。

**実行テスト**が呼び出されると、動詞イベントハンドラーが `MarqueeControl`で `StartMarquee` メソッドを呼び出します。 **停止テスト**が呼び出されると、動詞イベントハンドラーは `MarqueeControl`で `StopMarquee` メソッドを呼び出します。 `StartMarquee` メソッドと `StopMarquee` メソッドの実装では、`IMarqueeWidget`を実装する含まれるコントロールに対してこれらのメソッドを呼び出します。そのため、含まれているすべての `IMarqueeWidget` コントロールもテストに関与します。

### <a name="to-add-designer-verbs-to-your-custom-designers"></a>デザイナー動詞をカスタムデザイナーに追加するには

1. `MarqueeControlRootDesigner` クラスで、`OnVerbRunTest` と `OnVerbStopTest`という名前のイベントハンドラーを追加します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#570](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#570)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#570](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#570)]

2. これらのイベントハンドラーを、対応するデザイナー動詞に接続します。 `MarqueeControlRootDesigner` は、基本クラスから <xref:System.ComponentModel.Design.DesignerVerbCollection> を継承します。 2つの新しい <xref:System.ComponentModel.Design.DesignerVerb> オブジェクトを作成し、<xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> メソッドでこのコレクションに追加します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#590](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#590)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#590](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#590)]

## <a name="create-a-custom-uitypeeditor"></a>カスタム UITypeEditor を作成する

ユーザーに対してカスタムのデザイン時エクスペリエンスを作成する場合、多くの場合、プロパティウィンドウとのカスタム操作を作成することをお勧めします。 これを行うには、<xref:System.Drawing.Design.UITypeEditor>を作成します。

`MarqueeBorder` コントロールは、プロパティウィンドウ内のいくつかのプロパティを公開します。 これらのプロパティのうち、`MarqueeSpinDirection` と `MarqueeLightShape` の2つが列挙体によって表されます。 UI 型エディターの使用方法を示すために、`MarqueeLightShape` プロパティには関連付けられた <xref:System.Drawing.Design.UITypeEditor> クラスがあります。

### <a name="to-create-a-custom-ui-type-editor"></a>カスタム UI 型エディターを作成するには

1. **コードエディター**で `MarqueeBorder` ソースファイルを開きます。

2. `MarqueeBorder` クラスの定義で、<xref:System.Drawing.Design.UITypeEditor>から派生する `LightShapeEditor` というクラスを宣言します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#96](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#96)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#96](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#96)]

3. `editorService`と呼ばれる <xref:System.Windows.Forms.Design.IWindowsFormsEditorService> インスタンス変数を宣言します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#92](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#92)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#92](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#92)]

4. <xref:System.Drawing.Design.UITypeEditor.GetEditStyle%2A> メソッドをオーバーライドします。 この実装は <xref:System.Drawing.Design.UITypeEditorEditStyle.DropDown>を返します。これは、デザイン環境に `LightShapeEditor`を表示する方法を示します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#93](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#93)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#93](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#93)]

5. <xref:System.Drawing.Design.UITypeEditor.EditValue%2A> メソッドをオーバーライドします。 この実装は、<xref:System.Windows.Forms.Design.IWindowsFormsEditorService> オブジェクトのデザイン環境に対してクエリを行います。 成功すると、`LightShapeSelectionControl`が作成されます。 <xref:System.Windows.Forms.Design.IWindowsFormsEditorService.DropDownControl%2A> メソッドは、`LightShapeEditor`を開始するために呼び出されます。 この呼び出しからの戻り値がデザイン環境に返されます。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#94](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#94)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#94](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#94)]

## <a name="create-a-view-control-for-your-custom-uitypeeditor"></a>カスタム UITypeEditor のビューコントロールを作成する

`MarqueeLightShape` プロパティは、`Square` と `Circle`の2種類のライト形状をサポートしています。 プロパティウィンドウでこれらの値をグラフィカルに表示する目的でのみ使用されるカスタムコントロールを作成します。 このカスタムコントロールは、プロパティウィンドウと対話するために <xref:System.Drawing.Design.UITypeEditor> によって使用されます。

### <a name="to-create-a-view-control-for-your-custom-ui-type-editor"></a>カスタム UI 型エディターのビューコントロールを作成するには

1. 新しい <xref:System.Windows.Forms.UserControl> 項目を `MarqueeControlLibrary` プロジェクトに追加します。 新しいソースファイルに**LightShapeSelectionControl**のベース名を指定します。

2. **ツールボックス**から2つの <xref:System.Windows.Forms.Panel> コントロールを `LightShapeSelectionControl`にドラッグします。 名前を `squarePanel` し、`circlePanel`します。 それらを並べて配置します。 両方の <xref:System.Windows.Forms.Panel> コントロールの <xref:System.Windows.Forms.Control.Size%2A> プロパティを **(60, 60)** に設定します。 `squarePanel` コントロールの <xref:System.Windows.Forms.Control.Location%2A> プロパティを **(8, 10)** に設定します。 `circlePanel` コントロールの <xref:System.Windows.Forms.Control.Location%2A> プロパティを **(80, 10)** に設定します。 最後に、`LightShapeSelectionControl` の <xref:System.Windows.Forms.Control.Size%2A> プロパティを **(150, 80)** に設定します。

3. **コードエディター**で `LightShapeSelectionControl` ソースファイルを開きます。 ファイルの先頭に、<xref:System.Windows.Forms.Design?displayProperty=nameWithType> 名前空間をインポートします。

   ```vb
   Imports System.Windows.Forms.Design
   ```

   ```csharp
   using System.Windows.Forms.Design;
   ```

4. `squarePanel` コントロールと `circlePanel` コントロールに対して <xref:System.Windows.Forms.Control.Click> イベントハンドラーを実装します。 これらのメソッドは <xref:System.Windows.Forms.Design.IWindowsFormsEditorService.CloseDropDown%2A> を呼び出して、カスタム <xref:System.Drawing.Design.UITypeEditor> 編集セッションを終了します。

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#390](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#390)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#390](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#390)]

5. `editorService`と呼ばれる <xref:System.Windows.Forms.Design.IWindowsFormsEditorService> インスタンス変数を宣言します。

   ```vb
   Private editorService As IWindowsFormsEditorService
   ```

   ```csharp
   private IWindowsFormsEditorService editorService;
   ```

6. `lightShapeValue`と呼ばれる `MarqueeLightShape` インスタンス変数を宣言します。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#330](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#330)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#330](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#330)]

7. `LightShapeSelectionControl` コンストラクターで、<xref:System.Windows.Forms.Control.Click> イベントハンドラーを `squarePanel` および `circlePanel` コントロールの <xref:System.Windows.Forms.Control.Click> イベントにアタッチします。 また、デザイン環境から `lightShapeValue` フィールドに `MarqueeLightShape` 値を割り当てるコンストラクターのオーバーロードを定義します。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#340](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#340)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#340](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#340)]

8. <xref:System.ComponentModel.Component.Dispose%2A> メソッドで、<xref:System.Windows.Forms.Control.Click> イベントハンドラーをデタッチします。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#350](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#350)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#350](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#350)]

9. **ソリューション エクスプローラー**で、 **[すべてのファイルを表示]** ボタンをクリックします。 LightShapeSelectionControl.Designer.cs または LightShapeSelectionControl ファイルを開き、<xref:System.ComponentModel.Component.Dispose%2A> メソッドの既定の定義を削除します。

10. `LightShape` プロパティを実装します。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#360](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#360)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#360](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#360)]

11. <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドをオーバーライドします。 この実装では、塗りつぶされた正方形と円を描画します。 また、1つの図形の周りに境界線を描画して、選択された値を強調表示します。

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#380](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#380)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#380](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#380)]

## <a name="test-your-custom-control-in-the-designer"></a>デザイナーでカスタムコントロールをテストする

この時点で、`MarqueeControlLibrary` プロジェクトをビルドできます。 `MarqueeControl` クラスから継承し、フォームで使用するコントロールを作成して、実装をテストします。

### <a name="to-create-a-custom-marqueecontrol-implementation"></a>カスタムの MarqueeControl 実装を作成するには

1. Windows フォーム デザイナーで `DemoMarqueeControl` を開きます。 これにより `DemoMarqueeControl` 型のインスタンスが作成され、`MarqueeControlRootDesigner` 型のインスタンスに表示されます。

2. **ツールボックス**で、 **[MarqueeControlLibrary Components]** タブを開きます。選択できる `MarqueeBorder` および `MarqueeText` コントロールが表示されます。

3. `MarqueeBorder` コントロールのインスタンスを `DemoMarqueeControl` デザインサーフェイスにドラッグします。 この `MarqueeBorder` コントロールを親コントロールにドッキングします。

4. `MarqueeText` コントロールのインスタンスを `DemoMarqueeControl` デザインサーフェイスにドラッグします。

5. ソリューションをビルドします。

6. `DemoMarqueeControl` を右クリックし、ショートカットメニューの **[テストの実行]** オプションを選択して、アニメーションを開始します。 アニメーションを停止するには、 **[テストの停止]** をクリックします。

7. デザイン ビューで **[Form1]** を開きます。

8. フォームに2つの <xref:System.Windows.Forms.Button> コントロールを配置します。 名前を `startButton` して `stopButton`し、<xref:System.Windows.Forms.Control.Text%2A> プロパティの値をそれぞれ**Start**および**Stop**に変更します。

9. 両方の <xref:System.Windows.Forms.Button> コントロールに <xref:System.Windows.Forms.Control.Click> イベントハンドラーを実装します。

10. **ツールボックス**で、 **[MarqueeControlTest Components]** タブを開きます。選択できる `DemoMarqueeControl` が表示されます。

11. `DemoMarqueeControl` のインスタンスを**Form1**デザインサーフェイスにドラッグします。

12. <xref:System.Windows.Forms.Control.Click> イベントハンドラーで、`DemoMarqueeControl`の `Start` および `Stop` メソッドを呼び出します。

    ```vb
    Private Sub startButton_Click(sender As Object, e As System.EventArgs)
        Me.demoMarqueeControl1.Start()
    End Sub 'startButton_Click

    Private Sub stopButton_Click(sender As Object, e As System.EventArgs)
    Me.demoMarqueeControl1.Stop()
    End Sub 'stopButton_Click
    ```

    ```csharp
    private void startButton_Click(object sender, System.EventArgs e)
    {
        this.demoMarqueeControl1.Start();
    }

    private void stopButton_Click(object sender, System.EventArgs e)
    {
        this.demoMarqueeControl1.Stop();
    }
    ```

13. `MarqueeControlTest` プロジェクトをスタートアッププロジェクトとして設定し、実行します。 `DemoMarqueeControl`を表示するフォームが表示されます。 **[開始]** ボタンを選択して、アニメーションを開始します。 テキストが点滅し、ライトが境界内を移動していることを確認します。

## <a name="next-steps"></a>次のステップ

`MarqueeControlLibrary` は、カスタムコントロールおよび関連するデザイナーの単純な実装を示しています。 このサンプルは、いくつかの方法でより高度なものにすることができます。

- デザイナーで `DemoMarqueeControl` のプロパティ値を変更します。 `MarqueBorder` コントロールを追加し、親インスタンス内にドッキングして、入れ子になった効果を作成します。 `UpdatePeriod` とライト関連のプロパティに対して異なる設定を試してみてください。

- `IMarqueeWidget`の独自の実装を作成します。 たとえば、点滅する "ネオンサイン" や、複数のイメージを使用したアニメーション化された記号を作成することができます。

- デザイン時のエクスペリエンスをさらにカスタマイズします。 <xref:System.Windows.Forms.Control.Enabled%2A> と <xref:System.Windows.Forms.Control.Visible%2A>よりも多くのプロパティをシャドウすることができます。また、新しいプロパティを追加することもできます。 新しいデザイナー動詞を追加して、子コントロールのドッキングなどの一般的なタスクを簡略化します。

- `MarqueeControl`のライセンスを付与します。

- コントロールのシリアル化方法とコードの生成方法を制御します。 詳細については、「[動的なソースコードの生成とコンパイル](../../reflection-and-codedom/dynamic-source-code-generation-and-compilation.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.UserControl>
- <xref:System.Windows.Forms.Design.ParentControlDesigner>
- <xref:System.Windows.Forms.Design.DocumentDesigner>
- <xref:System.ComponentModel.Design.IRootDesigner>
- <xref:System.ComponentModel.Design.DesignerVerb>
- <xref:System.Drawing.Design.UITypeEditor>
- <xref:System.ComponentModel.BackgroundWorker>
