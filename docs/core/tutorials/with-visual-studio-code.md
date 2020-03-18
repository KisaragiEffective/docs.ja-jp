---
title: C# および Visual Studio Code の使用を開始する
description: Visual Studio Code を使用した、C# で初めての .NET Core アプリケーションを作成してデバッグする方法について説明します。
author: kendrahavens
ms.date: 12/05/2018
ms.openlocfilehash: 8eaf1ba2314dcab96db615a8691afed82c5011a7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79397885"
---
# <a name="get-started-with-c-and-visual-studio-code"></a>C# および Visual Studio Code の使用を開始する

.NET Core は、Windows、Linux および macOS で実行されるアプリケーションを作成するための、高速でモジュール型のプラットフォームを提供します。 Visual Studio Code を C# 拡張機能とともに使用して、C# IntelliSense の完全サポート (スマート コード補完) とデバッグによる強力な編集機能をご活用ください。

## <a name="prerequisites"></a>必須コンポーネント

1. [Visual Studio Code](https://code.visualstudio.com/) のインストール。
2. [.NET Core SDK](https://dotnet.microsoft.com/download) のインストール。
3. Visual Studio Code の [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)のインストール。 Visual Studio Code に拡張機能をインストールする方法については、[VS Code Extension Marketplace](https://code.visualstudio.com/docs/editor/extension-gallery) を参照してください。

## <a name="hello-world"></a>Hello World

.NET Core でシンプルな "Hello World" プログラムを作成してみましょう。

1. プロジェクトを開く

    - Visual Studio Code を開きます。
    - 左側のメニューで [エクスプローラー] アイコンをクリックし、 **[フォルダーを開く]** をクリックします。
    - メイン メニューから **[ファイル]**  >  **[フォルダーを開く]** の順に選択し、C# プロジェクトを保存するフォルダーを開き、 **[フォルダーの選択]** をクリックします。 ここで、*Hello World* という名前のプロジェクトのフォルダーを作成します。

      ![Visual Studio Code の [フォルダーを開く]](media/with-visual-studio-code/vs-code-open-folder.png)

2. C# プロジェクトを初期化する

    - Visual Studio Code から統合ターミナルを開きます。メイン メニューで **[表示]**  >  **[統合端末]** の順に選択してください。
    - ターミナル ウィンドウで、`dotnet new console` と入力します。
    - このコマンドは、*HelloWorld.csproj* という名前の C# プロジェクト ファイルと共に、単純な "Hello World" プログラムが既に書き込まれた *Program.cs* ファイルをフォルダーに作成します。

      ![dotnet new コマンド](media/with-visual-studio-code/dotnet-new-command.png)

3. ビルド資産を解決する

    - **.NET Core 1.x** の場合、「`dotnet restore`」と入力します。 `dotnet restore` を実行すると、プロジェクトのビルドに必要な .NET Core パッケージにアクセスします。

      ![dotnet restore コマンド](media/with-visual-studio-code/dotnet-restore-command.png)

      [!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

4. "Hello World" プログラムを実行する

    - 「`dotnet run`」と入力します。

      ![dotnet run コマンド](media/with-visual-studio-code/dotnet-run-command.png)

[Windows](https://channel9.msdn.com/Blogs/dotnet/Get-started-with-VS-Code-using-CSharp-and-NET-Core)、[macOS](https://channel9.msdn.com/Blogs/dotnet/Get-started-with-VS-Code-using-CSharp-and-NET-Core-on-MacOS)、または [Linux](https://channel9.msdn.com/Blogs/dotnet/Get-started-with-VS-Code-Csharp-dotnet-Core-Ubuntu) での詳細設定については、簡単なビデオ チュートリアルを見ることができます。

## <a name="debug"></a>デバッグ

1. *Program.cs* をクリックして開きます。 Visual Studio Code で初めて C# ファイルを開くと、[OmniSharp](https://www.omnisharp.net/) がエディターに読み込まれます。

    ![Program.cs ファイルを開く](media/with-visual-studio-code/open-program-cs.png)

2. Visual Studio Code で、アプリのビルドとデバッグに必要なアセットの追加を求められます。 **[はい]** を選択します。

    ![足りない資産の入力を求める](media/with-visual-studio-code/missing-assets.png)

3. デバッグ ビューを開くには、左側のメニューにある [デバッグ] アイコンをクリックします。

    ![Visual Studio Code で [デバッグ] タブを開く](media/with-visual-studio-code/open-debug-tab.png)

4. ウィンドウの上部で緑色の矢印を探します。 その横にあるドロップダウン リストで **[.NET Core Launch (console)]** \(.NET Core の起動 (コンソール)\) が選択されていることを確認します。

    ![Visual Studio Code で .NET Core を選択する](media/with-visual-studio-code/select-net-core.png)

5. 9 行目の横にある**エディター余白** (エディター内の行番号の左側の領域) をクリックして、プロジェクトにブレークポイントを追加するか、またはエディター内でテキスト カーソルを 9 行目に移動して <kbd>F9</kbd> キーを押します。

    ![ブレークポイントの設定](media/with-visual-studio-code/set-breakpoint-vs-code.png)

6. デバッグを開始するには、<kbd>F5</kbd> キーを押すか、緑色の矢印を選択します。 デバッガーは、前述の手順で設定したブレークポイントに達すると、プログラムの実行を停止します。
    - デバッグ中は左上のペインにローカル変数が表示され、デバッグ コンソールを使用できます。

7. 上部にある青色の矢印を選択してデバッグを継続するか、上部にある赤色の四角形を選択して停止します。

    ![Visual Studio Code の実行とデバッグ](media/with-visual-studio-code/run-debug-vs-code.png)

> [!TIP]
> Visual Studio Code で OmniSharp を使用した .NET Core のデバッグの詳細とトラブルシューティングのヒントについては、「[Instructions for setting up the .NET Core debugger](https://github.com/OmniSharp/omnisharp-vscode/blob/master/debugger.md)」 (.NET Core デバッガーの設定に関する指示) を参照してください。

## <a name="add-a-class"></a>クラスを追加する

1. 新しいクラスを追加するには、VSCode エクスプローラーを右クリックし、 **[新しいファイル]** を選択します。 これで、新しいファイルが VSCode で開いたフォルダーに追加されます。
2. ファイルに *MyClass.cs* という名前を指定します。 csharp ファイルとして認識されるには、最後に `.cs` 拡張子を付けて保存する必要があります。
3. 次のコードを追加して、1 つ目のクラスを作成します。 *Program.cs* ファイルから参照できるように、正しい名前空間を含めるようにします。

    ``` csharp
    using System;

    namespace HelloWorld
    {
        public class MyClass
        {
            public string ReturnMessage()
            {
                return "Happy coding!";
            }
        }
    }
    ```

4. 次のコードを追加して、*Program.cs* のメイン メソッドから新しいクラスを呼び出します。

    ```csharp
    using System;

    namespace HelloWorld
    {
        class Program
        {
            static void Main(string[] args)
            {
                var c1 = new MyClass();
                Console.WriteLine($"Hello World! {c1.ReturnMessage()}");
            }
        }
    }
    ```

5. 変更を保存し、プログラムを再度実行します。 新しいメッセージと共に追加した文字列が表示されます。

    ```dotnetcli
    dotnet run
    ```

    次の出力が得られます。

    ```console
    Hello World! Happy coding!
    ```

## <a name="faq"></a>よくあるご質問

### <a name="im-missing-required-assets-to-build-and-debug-c-in-visual-studio-code-my-debugger-says-no-configuration"></a>Visual Studio Code 内で C# をビルドおよびデバッグするのに必要な資産が欠落しています。 デバッガーには、"構成がありません" と表示されます。

Visual Studio Code C# の拡張機能では、ビルドおよびデバッグする資産を自動的に作成することができます。 C# プロジェクトを初めて開くと、これらの資産を作成するように Visual Studio Code から求められます。 資産を作成しなかった場合でも、このコマンドを実行する方法はあります。コマンド パレットを開き ( **[表示] > [コマンド パレット]** )、「>.NET:Generate Assets for Build and Debug」 と入力します。 これを選択すると、必要としている *.vscode*、*launch.json* および *tasks.json* の各構成ファイルが作成されます。

## <a name="see-also"></a>関連項目

- [Visual Studio Code の設定](https://code.visualstudio.com/docs/setup/setup-overview)
- [Visual Studio Code でのデバッグ](https://code.visualstudio.com/Docs/editor/debugging)
