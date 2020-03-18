---
title: Visual Studio 用開発者コマンド プロンプト
ms.date: 01/05/2020
helpviewer_keywords:
- command prompt, Windows SDK
- Visual Studio command prompt
- command prompt, Visual Studio
- SDK command prompt
- tools [.NET Framework], setting environment variables
- environment variables, setting for tools
- developer command prompt
ms.assetid: 94fcf524-9045-4993-bfb2-e2d8bad44219
ms.openlocfilehash: f028281d477284acf3ac4dac63f5ddbbd79f5259
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75715843"
---
# <a name="developer-command-prompt-for-visual-studio"></a>Visual Studio 用開発者コマンド プロンプト

Visual Studio 用開発者コマンド プロンプトでは、.NET Framework ツールをもっと簡単に使用できます。 それは、特定の環境変数を自動的に設定するコマンド プロンプトです。 開発者コマンド プロンプトを開いた後、`ildasm` や `clrver` などの [.NET Framework ツール](index.md)のコマンドを入力できます。

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="search-for-the-command-prompt-on-your-machine"></a>コンピューター上でのコマンド プロンプトの検索

Visual Studio のバージョンと、インストールした追加の SDK およびワークロードに応じて、複数のコマンド プロンプトがある場合があります。 次の手順でうまくいかない場合は、[マシン上のファイルを手動で探す](#manually-locate-the-files-on-your-machine)か、[Visual Studio 内からコマンド プロンプトを開始](#start-the-command-prompt-from-inside-visual-studio)してみることができます。

### <a name="windows-10"></a>Windows 10

1. **スタート** ![キーボードの Windows ロゴ キー](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png)を選択し、 文字 **V** までスクロールします。

1. **[Visual Studio 2019]** フォルダーを展開します。

1. **VS 2019 用開発者コマンド プロンプト** (または、使用するコマンド プロンプト) を選択します。

   また、最初にタスク バーの [検索] ボックスにコマンド プロンプトの名前を入力することもできます。その名前と一致するコマンド プロンプトが結果一覧に表示されたら、その中から必要なものを選択します。

   ![Windows 10 での検索動作を示すアニメーション GIF](./media/developer-command-prompt-for-vs/windows10-search.gif)

### <a name="windows-81"></a>Windows 8.1

1. キーボードの Windows ロゴ キー ![キーボードの Windows ロゴ キー](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png) を押すなどして、 **[スタート]** 画面に 移動します。

1. **[スタート]** 画面で、**Ctrl**+**Tab** キーを押して **[アプリ]** の一覧を開き、**V** を押します。インストールされているすべての Visual Studio コマンド プロンプトが含まれた一覧が表示されます。

1. **VS 2019 用開発者コマンド プロンプト** (または、使用するコマンド プロンプト) を選択します。

### <a name="windows-7"></a>Windows 7

1. **[スタート]** を選択し、 **[すべてのプログラム]** を展開します。

1. **[Visual Studio 2019]**  >  **[Visual Studio Tools]**  > **VS 2019 用開発者コマンド プロンプト**、または使用するコマンド プロンプトを選択します。

   ![コマンド プロンプトが強調表示されている Windows 7 の [スタート] メニュー](./media/developer-command-prompt-for-vs/windows7-menu.png)

[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) や[それ以前のバージョン](https://developer.microsoft.com/windows/downloads/sdk-archive)など、他の SDK がインストールされている場合は、コマンド プロンプトがさらに表示されることがあります。 各ツールのドキュメントを参照して、どのバージョンのコマンド プロンプトを使用する必要があるかを確認してください。

## <a name="manually-locate-the-files-on-your-machine"></a>コンピューター上のファイルを手動で探す

インストール済みのコマンド プロンプトのショートカットは、通常、Visual Studio 用の **スタート メニュー** フォルダー ( *%ProgramData%\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools* 内など) にあります。 ただし、コマンド プロンプトを探しても、何らかの理由によって期待した結果を生成されない場合は、コンピューター上でそのショートカットを手動で探すことができます。 *VsDevCmd.bat* などのコマンド プロンプトのファイル名を検索するか、または Tools フォルダー ( *%ProgramFiles(x86)%\Microsoft Visual Studio\2019\Community\Common7\Tools* など) に移動します (パスは、Visual Studio のバージョン、エディション、およびインストール先に応じて変わります)。

## <a name="start-the-command-prompt-from-inside-visual-studio"></a>Visual Studio 内からコマンド プロンプトを開始する

簡単にアクセスできるように、開発者コマンド プロンプトまたは他のコマンド プロンプトを Visual Studio の [ツール] メニューに追加できます。 ツールを使用できるようにするには、外部ツール一覧にそれを追加します。 次に手順を示します。

1. Visual Studio を開きます。

1. スタート ウィンドウで、 **[コードなしで続行]** を選択します。

1. メニュー バーで **[ツール]**  >  **[外部ツール]** の順に選択します。

1. **[外部ツール]** ダイアログ ボックスで、 **[追加]** ボタンをクリックします。 新しいエントリが表示されます。

1. 新しいメニュー項目の **[タイトル]** を入力します (「`Command Prompt`」など)。

1. **[コマンド]** フィールドに、起動するファイル (`%comspec%` や `C:\Windows\System32\cmd.exe` など) を指定します。

1. **[引数]** フィールドに、使用する特定のコマンド プロンプトのある場所を指定します (`/k "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\VsDevCmd.bat"` など)。 このコマンドによって、Visual Studio 2019 Community でインストールされた開発者コマンド プロンプトが起動します。 この値は、Visual Studio のバージョン、エディション、インストール先に応じて変更します。

1. **[初期ディレクトリ]** フィールドで、コマンド プロンプトが開始するディレクトリを指定します。 フィールドの横にある矢印を選択して、 **[プロジェクト ディレクトリ]** などの値を選択します。

1. **[OK]** を選択します。

   ![フィールドに値が入力された [外部ツール] ダイアログ。](./media/developer-command-prompt-for-vs/add-external-tool.png)

   新しいメニュー項目が追加され、このコマンド プロンプトに [ツール] メニューからアクセスできるようになります。

   ![Visual Studio でのコマンド プロンプト メニュー項目](./media/developer-command-prompt-for-vs/command-prompt-vs-menu.png)

## <a name="see-also"></a>関連項目

- [.NET Framework ツール](index.md)
- [Visual Studio の外部ツール](/visualstudio/ide/managing-external-tools)
- [コマンド ラインから Microsoft C++ ツールセットを使用する](/cpp/build/building-on-the-command-line)
