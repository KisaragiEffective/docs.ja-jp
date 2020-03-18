---
title: Visual Studio for Mac を使用した完全な .NET Core ソリューションの構築
description: この記事では、再利用可能なライブラリと単体テストを含む .NET Core ソリューションの構築方法を示します。
ms.date: 12/19/2019
ms.openlocfilehash: 8c9fcca404a3875b6bb7f9cf20551a017ff553c5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78239964"
---
# <a name="build-a-complete-net-core-solution-on-macos-using-visual-studio-for-mac"></a>Visual Studio for Mac を使用した macOS での完全な .NET Core ソリューションの構築

Visual Studio for Mac では、.NET Core アプリケーション開発用の機能をすべて備えた統合開発環境 (IDE) が提供されます。 この記事では、再利用可能なライブラリと単体テストを含む .NET Core ソリューションの構築方法について説明します。

このチュートリアルでは、ユーザーが入力した検索語とテキスト文字列を受け入れ、クラス ライブラリでメソッドを使用した場合に文字列に検索語が表示される回数をカウントし、ユーザーに結果を返すアプリケーションの作成方法を示します。 ソリューションには、単体テストの概念を紹介するため、クラス ライブラリの単体テストも含まれています。 完全なサンプルでチュートリアルを続行する場合は、[サンプル ソリューション](https://github.com/dotnet/samples/blob/master/core/tutorials/using-on-mac-vs-full-solution/WordCounter)をダウンロードしてください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

> [!NOTE]
> お客様のフィードバックは非常に貴重です。 次の 2 つの方法で Visual Studio for Mac の開発チームにフィードバックを送信できます。
>
> - Visual Studio for Mac で、メニューから **[ヘルプ]**  >  **[問題の報告]** の順に選択するか、ようこそ画面から **[問題の報告]** を選択して、バグ報告を提出するためのウィンドウを開きます。 お客様のフィードバックは、[開発者コミュニティ](https://developercommunity.visualstudio.com/spaces/41/index.html) ポータルで追跡することができます。
> - 提案するには、メニューから **[ヘルプ]**  >  **[提案の送信]** の順に選択するか、ようこそ画面から **[提案の送信]** を選択し、[Visual Studio for Mac の開発者コミュニティの Web ページ](https://developercommunity.visualstudio.com/content/idea/post.html?space=41)に移動します。

## <a name="prerequisites"></a>必須コンポーネント

- [.NET Core SDK 3.1 以降](https://dotnet.microsoft.com/download)
- [Visual Studio 2019 for Mac](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)

前提条件の詳細については、[.NET Core の依存関係と要件](../install/dependencies.md?pivots=os-macos)に関する記事を参照してください。 Visual Studio 2019 for Mac の完全なシステム要件については、「[Visual Studio 2019 for Mac 製品ファミリのシステム要件](/visualstudio/productinfo/vs2019-system-requirements-mac)」をご覧ください。

## <a name="building-a-library"></a>ライブラリのビルド

1. スタート ウィンドウで、 **[新しいプロジェクト]** を選択します。 **[新しいプロジェクト]** ダイアログで、 **[.NET Core]** ノードの下にある **[.NET Standard ライブラリ]** テンプレートを選択します。 このコマンドより、.NET Core を対象とする .NET Standard ライブラリと、[.NET Standard](../../standard/net-standard.md) のバージョン 2.1 をサポートするその他の .NET 実装が作成されます。 複数のバージョンの .NET Core SDK がインストールされている場合は、お使いのライブラリに異なるバージョンの .NET Standard を選択できます。 [.NET Standard 2.1] を選択し、 **[次へ]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の [新しいプロジェクト] ダイアログ](./media/using-on-mac-vs-full-solution/visual-studio-mac-new-project.png)

1. プロジェクトには "TextUtils" ("Text Utilities" の短い名前)、ソリューションには "WordCounter" という名前を付けます。 **[ソリューション ディレクトリ内にプロジェクト ディレクトリを作成します]** チェック ボックスはそのままにしておきます。 **[作成]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の [新しいプロジェクト] ダイアログのオプション](./media/using-on-mac-vs-full-solution/visual-studio-mac-new-project-options.png)

1. **[ソリューション]** パッドで、`TextUtils` ノードを展開し、テンプレートで提供される *Class1.cs* というクラス ファイルを表示します。 Ctrl キーを押しながらそのファイルをクリックし、コンテキスト メニューから **[名前の変更]** を選択して、ファイル名を *WordCount.cs* に変更します。 ファイルを開き、内容を次のコードに置き換えます。

   [!code-csharp[Main](../../../samples/snippets/core/tutorials/using-on-mac-vs-full-solution/csharp/TextUtils/WordCount.cs)]

1. ファイルを保存します。その場合、3 つの異なる方法 (キーボード ショートカット <kbd>&#8984;</kbd>+<kbd>s</kbd> を使用する、メニューから **[ファイル]**  >  **[保存]** の順に選択する、Ctrl キーを押しながらファイルのタブをクリックしてコンテキスト メニューから **[保存]** を選択する) のいずれかを使用します。 次のイメージは IDE ウィンドウを示しています。

   > [!div class="mx-imgBorder"]
   > ![クラス ライブラリ ファイルとメソッドが示された Visual Studio for Mac IDE ウィンドウ](./media/using-on-mac-vs-full-solution/visual-studio-mac-editor.png)

1. IDE ウィンドウの下部の余白にある **[エラー]** を選択して、 **[エラー]** パネルを開きます。 **[ビルド出力]** ボタンを選択します。

   > [!div class="mx-imgBorder"]
   > ![[エラー] ボタンが示された Visual Studio Mac IDE の下部余白](./media/using-on-mac-vs-full-solution/visual-studio-mac-error-button.png)

1. メニューから **[ビルド]**  >  **[すべてビルド]** の順に選択します。

   ソリューションがビルドされます。 ビルド出力パネルに、ビルドが成功したことが示されます。

   > [!div class="mx-imgBorder"]
   > ![ビルドの成功メッセージが表示された、[エラー] パネルの Visual Studio Mac のビルド出力ウィンドウ](./media/using-on-mac-vs-full-solution/visual-studio-mac-build-panel.png)

## <a name="creating-a-test-project"></a>テスト プロジェクトの作成

単体テストでは、開発および公開時に自動化されたソフトウェア テストが提供されます。 このチュートリアルで使用するテスト フレームワークは [xUnit (バージョン 2.4.0 以降)](https://xunit.github.io/) です。これは、以下の手順で xUnit テスト プロジェクトをソリューションに追加したときに自動的にインストールされます。

1. **[ソリューション]** サイドバーで、Ctrl キーを押しながら `WordCounter` ソリューションをクリックし、 **[追加]**  >  **[新しいプロジェクトの追加]** の順に選択します。

1. **[新しいプロジェクト]** ダイアログで、 **[.NET Core]** ノードから **[テスト]** を選択します。 **[xUnit Test Project (xUnit テスト プロジェクト)]** を選択してから **[次へ]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![xUnit テスト プロジェクトを作成する Visual Studio Mac の [新しいプロジェクト] ダイアログ](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-project.png)

1. 複数のバージョンの .NET Core SDK がある場合は、このプロジェクトのバージョンを選択する必要があります。 **.NET Core 3.1** を選択します。 新しいプロジェクトに "TestLibrary" という名前を付けて、 **[作成]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![プロジェクト名を指定する Visual Studio Mac の [新しいプロジェクト] ダイアログ](./media/using-on-mac-vs-full-solution/visual-studio-mac-new-project-name.png)

1. テスト ライブラリを `WordCount` クラスで使用するには、`TextUtils` プロジェクトへの参照を追加します。 **[ソリューション]** サイドバーで、**TestLibrary** の下にある **[依存関係]** を Ctrl キーを押しながらクリックします。 コンテキスト メニューから **[参照の編集]** を選択します。

1. **[参照の編集]** ダイアログで、 **[プロジェクト]** タブの **[TextUtils]** プロジェクトを選択します。 **[OK]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio Mac の [参照の編集] ダイアログ](./media/using-on-mac-vs-full-solution/visual-studio-mac-edit-references.png)

1. **[TestLibrary]** プロジェクトで、*UnitTest1.cs* ファイルの名前を *TextUtilsTests.cs* に変更します。

1. そのファイルを開き、コードを次のコードに置き換えます。

   ```csharp
   using Xunit;
   using TextUtils;
   using System.Diagnostics;

   namespace TestLibrary
   {
       public class TextUtils_GetWordCountShould
       {
           [Fact]
           public void IgnoreCasing()
           {
               var wordCount = WordCount.GetWordCount("Jack", "Jack jack");

               Assert.NotEqual(2, wordCount);
           }
       }
   }
   ```

   次のイメージは、単体テスト コードが配置済みの IDE を示しています。 `Assert.NotEqual` ステートメントに注目してください。

   > [!div class="mx-imgBorder"]
   > ![IDE のメイン ウィンドウでの Visual Studio for Mac の最初の単体テスト](./media/using-on-mac-vs-full-solution/visual-studio-mac-assert-test.png)

   新しいテストを一度失敗させてテスト ロジックが正しいことを確認することが重要です。 メソッドは "Jack" (先頭が大文字) という名前と、"Jack" および "jack" (先頭が大文字のものと小文字のもの) を含む文字列を渡します。 `GetWordCount` メソッドが正しく機能している場合は、検索語の 2 つのインスタンスのカウントが返されます。 このテストを意図的に失敗させるには、まず、検索語 "Jack" の 2 つのインスタンスが `GetWordCount` メソッドで返されないことをアサートするテストを実装します。 次の手順に進んで意図的にテストを失敗させます。

1. 画面の右側の **[単体テスト]** パネルを開きます。 メニューから **[表示]**  >  **[テスト]** を選択します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の [単体テスト] パネル](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-panel.png)

1. **[ドッキング]** アイコンをクリックして、パネルを開いたままにします。 (次の画像で強調表示されています。)

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の [単体テスト] パネルの [ドッキング] アイコン](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-dock-icon.png)

1. **[すべて実行]** ボタンをクリックします。

   テストは失敗します。これが正しい結果です。 テスト メソッドは、`GetWordCount` メソッドに指定された文字列 "Jack jack" から `inputString` "Jack" の 2 つのインスタンスが返されないことをアサートします。 単語の大文字と小文字は `GetWordCount` メソッドでは考慮されないため、2 つのインスタンスが返されます。 2 *is not equal to* 2 というアサーションは失敗します。 これは正しい結果であり、テストのロジックは適切です。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac のテスト失敗の表示](./media/using-on-mac-vs-full-solution/visual-studio-for-mac-unit-test-failure.png)

1. `Assert.NotEqual` から `Assert.Equal` に変更して `IgnoreCasing` テスト メソッドを変更します。 ファイルを保存します。その場合、キーボード ショートカット <kbd>Ctrl</kbd>+<kbd>s</kbd> を使用するか、メニューから **[ファイル]**  >  **[保存]** の順に選択するか、Ctrl キーを押しながらファイルのタブをクリックしてコンテキスト メニューから **[保存]** を選択します。

   `searchWord` "Jack" は `GetWordCount` に渡された `inputString` "Jack jack" を含む 2 つのインスタンスを返すことが予想されます。 **[単体テスト]** パネルの **[テストの実行]** ボタン、または画面下部の **[テスト結果]** パネルの **[テストの再実行]** ボタンをクリックして、テストを再実行します。 テストに合格します。 文字列 "Jack jack" (大文字と小文字の区別は無視) に "Jack" のインスタンスが 2 つあり、テスト アサーションは `true` です。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac のテスト成功の表示](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-pass.png)

1. `Fact` での個々の戻り値のテストは、単体テストで実行できることのほんの一部に過ぎません。 別の強力な手法では、`Theory` を使用して一度に複数の値をテストすることができます。 次のメソッドを `TextUtils_GetWordCountShould` クラスに追加します。 このメソッドを追加すると、クラスのメソッドは 2 つになります。

   ```csharp
   [Theory]
   [InlineData(0, "Ting", "Does not appear in the string.")]
   [InlineData(1, "Ting", "Ting appears once.")]
   [InlineData(2, "Ting", "Ting appears twice with Ting.")]
   public void CountInstancesCorrectly(int count,
                                       string searchWord,
                                       string inputString)
   {
       Assert.NotEqual(count, WordCount.GetWordCount(searchWord,
                                                  inputString));
   }
   ```

   `CountInstancesCorrectly` は、`GetWordCount` メソッドが正しくカウントすることを確認します。 `InlineData` は確認するカウント、検索語、および入力文字列を示します。 テスト メソッドはデータの行ごとに一度実行されます。 繰り返しますが、データのカウントが正しく、値が `GetWordCount` メソッドで返されるカウントと一致することがわかっている場合でも、まず、`Assert.NotEqual` を使用して失敗をアサートする必要があります。 意図的にテストを失敗させる手順を実行するのは、最初は時間の無駄と思えるかもしれませんが、まず、テストを失敗させてロジックを確認することはテストのロジックを確認するうえで重要なことです。 失敗させようとしたのに成功するテスト メソッドがある場合、そのテストのロジックにバグがあります。 テスト メソッドを作成するたびに、この手順を実行することをお勧めします。

1. ファイルを保存し、もう一度テストを実行します。 大文字と小文字のテストは成功しますが、3 つのカウント テストは失敗します。 これは予想したとおりの結果です。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の予測されるテスト失敗](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-failure.png)

1. `Assert.NotEqual` から `Assert.Equal` に変更して `CountInstancesCorrectly` テスト メソッドを変更します。 ファイルを保存します。 テストをもう一度実行します。 すべてのテストに成功します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の予測されるテスト成功](./media/using-on-mac-vs-full-solution/visual-studio-mac-unit-test-pass.png)

## <a name="adding-a-console-app"></a>コンソール アプリの追加

1. **[ソリューション]** サイドバーで、Ctrl キーを押しながら `WordCounter` ソリューションをクリックします。 **[.NET Core]**  >  **[アプリ]** テンプレートの順に移動してテンプレートを選択し、新しい**コンソール アプリケーション**プロジェクトを追加します。 **[次へ]** を選択します。 プロジェクトに **WordCounterApp** という名前を付けます。 **[作成]** を選択し、ソリューションでプロジェクトを作成します。

1. **[ソリューション]** サイドバーで、新しい **WordCounterApp** プロジェクトの **[依存関係]** ノードを Ctrl キーを押しながらクリックします。 **[参照の編集]** ダイアログで、**TextUtils** にチェック マークを付けて **[OK]** を選択します。

1. *Program.cs* ファイルを開きます。 このコードを次のコードに置き換えます。

   [!code-csharp[Main](../../../samples/snippets/core/tutorials/using-on-mac-vs-full-solution/csharp/WordCounterApp/Program.cs)]

1. Ctrl キーを押しながら `WordCounterApp` プロジェクトをクリックし、コンテキスト メニューから **[プロジェクトの実行]** を選択します。 アプリを実行する際に、コンソール ウィンドウで入力を求めるメッセージが表示されたら、検索語と入力文字列の値を入力します。 アプリには、文字列での検索語の表示回数が示されます。

   > [!div class="mx-imgBorder"]
   > ![アプリが実行中であることを示す Visual Studio for Mac のコンソール ウィンドウ](./media/using-on-mac-vs-full-solution/visual-studio-mac-console-window.png)

1. 確認する最後の機能は、Visual Studio for Mac でのデバッグです。 `Console.WriteLine` ステートメントにブレークポイントを設定します。23 行目の左余白を選択すると、コード行の横に赤い丸が表示されます。 コード行の任意の場所を選択し、メニューから **[実行]**  >  **[ブレークポイントの設定/解除]** の順に選択することもできます。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac のブレークポイントの設定](./media/using-on-mac-vs-full-solution/visual-studio-mac-breakpoint.png)

1. Ctrl キーを押しながら `WordCounterApp` プロジェクトをクリックします。 コンテキスト メニューから **[プロジェクトのデバッグを開始]** を選択します。 アプリが実行されたら、検索語 "cat" と検索文字列 "The dog chased the cat, but the cat escaped" を入力します。 `Console.WriteLine` ステートメントに達した時点でプログラムの実行が停止します。その後、ステートメントが実行されます。 **[ローカル]** タブで、`searchWord`、`inputString`、`wordCount`、および `pluralChar` の各値を確認できます。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の停止されたデバッガー プログラムの実行](./media/using-on-mac-vs-full-solution/visual-studio-mac-debugger.png)

1. **[イミディエイト]** ウィンドウで、「wordCount = 999;」と入力してから Enter キーを押します。 このコマンドで、`wordCount` 変数に 999 という意味のない値が割り当てられます。これは、デバッグ中に変数値の置き換えが可能であることを示します。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac の [イミディエイト] ウィンドウで値を変更する](./media/using-on-mac-vs-full-solution/visual-studio-mac-immediate-window.png)

1. ツールバーで、*続行*矢印をクリックします。 コンソール ウィンドウの出力を確認します。 アプリのデバッグ時に設定した 999 という不適切な値が報告されます。

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac ツールバーの続行ボタン](./media/using-on-mac-vs-full-solution/visual-studio-mac-toolbar.png)

   > [!div class="mx-imgBorder"]
   > ![Visual Studio for Mac のコンソール ウィンドウの出力](./media/using-on-mac-vs-full-solution/visual-studio-mac-output.png)

同じプロセスを使用して、単体テスト プロジェクトを使用してコードをデバッグできます。 WordCount アプリ プロジェクトを開始する代わりに、Ctrl キーを押しながら **[テスト ライブラリ]** プロジェクトをクリックし、コンテキスト メニューから **[プロジェクトのデバッグを開始]** を選択します。 デバッガーがアタッチされた状態で Visual Studio for Mac でテスト プロジェクトが開始されます。 実行は、テスト プロジェクトに追加したブレークポイント、または基になるライブラリ コードで停止します。

## <a name="see-also"></a>関連項目

- [Visual Studio 2019 for Mac リリース ノート](/visualstudio/releasenotes/vs2019-mac-relnotes)
