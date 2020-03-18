---
title: Visual Studioで F# を使い始める
description: Visual Studio でをF#使用する方法について説明します。
ms.date: 12/20/2019
ms.openlocfilehash: 9bf47fb08ea3df0aa0d5064043d94f030d45d568
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337341"
---
# <a name="get-started-with-f-in-visual-studio"></a>Visual Studioで F# を使い始める

F#また、visual F# Studio 統合開発環境 (IDE: integrated development environment) では、ビジュアルツールがサポートされています。

まず、 [Visual Studio がサポートと共F#にインストールされ](install-fsharp.md#install-f-with-visual-studio)ていることを確認します。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

Visual Studio の最も基本的なプロジェクトの1つは、コンソールアプリです。 作成手順は次のとおりです。

1. Visual Studio 2019 を開きます。

2. スタート ウィンドウで、 **[新しいプロジェクトの作成]** を選択します。

3. **[新しいプロジェクトの作成]** ページで、 **F#** [言語] ボックスの一覧からを選択します。

4. **[コンソールアプリ (.Net Core)]** テンプレートを選択し、 **[次へ]** をクリックします。

5. **[新しいプロジェクトの構成]** ページで、 **[プロジェクト名]** ボックスに名前を入力します。 次に、 **[作成]** を選択します。

   新しいF#プロジェクトが作成されます。 これは、[ソリューションエクスプローラー] ウィンドウで確認できます。

## <a name="write-the-code"></a>コードを記述する

まず、コードを記述してみましょう。 `Program.fs` ファイルが開いていることを確認し、その内容を次のものに置き換えます。

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

前のコードサンプルでは、`x` という名前の入力を受け取り、それをそれ自体に乗算する `square` という名前の関数を定義しています。 でF#は[型の推定](../language-reference/type-inference.md)が使用されるため、`x` の型を指定する必要はありません。 コンパイラF#は、乗算が有効な型を認識し、`square` の呼び出し方法に基づいて `x` に型を割り当てます。 マウスポインターを`square`に合わせると、次のように表示されます。

```fsharp
val square: x:int -> int
```

これは、関数の型シグネチャと呼ばれるものです。 これは、次のように読み取ることができます。 "Square は、x という整数を受け取り、整数を生成する関数です。" コンパイラは現時点で `int` 型を `square` しました。これは、*すべて*の型ではなく、型の閉じられたセットで乗算がジェネリックではないためです。 `float`F#などの別の入力型を使用して `square` を呼び出すと、コンパイラは型シグネチャを調整します。

`EntryPoint` 属性で修飾された別の関数 `main`が定義されています。 この属性は、 F#プログラムの実行を開始する必要があることをコンパイラに指示します。 他の [C スタイルのプログラミング言語](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)と同じ規則に従い、コマンド ライン引数をこの関数に渡し、整数値 (通常は `0`) を返す事ができます。

これは、エントリポイント関数 `main`で、`12`の引数を使用して `square` 関数を呼び出します。 次F#に、コンパイラは、`int -> int` する `square` の型 (つまり、`int` を受け取り、`int`を生成する関数) を割り当てます。 `printfn` の呼び出しは、書式指定文字列を使用して結果 (と改行) を出力する書式設定された印刷関数です。 書式指定文字列は、C スタイルのプログラミング言語と同様に、渡される引数 (この場合は `12` と `(square 12)`) に対応するパラメーター (`%d`) を持っています。

## <a name="run-the-code"></a>コードの実行

コードを実行し、F5 キーを押して結果を確認することができます **。+** **F5**キーを押します。 または、トップレベルのメニューバーから [デバッグ > 開始] を選択**して** **デバッグなしで開始**することもできます。 これにより、デバッグなしでプログラムが実行されます。

次の出力は、Visual Studio が開いたコンソールウィンドウに出力されます。

```console
12 squared is: 144!
```

おめでとうございます! Visual Studio で最初F#のプロジェクトを作成し、値をF#計算して出力する関数を記述し、プロジェクトを実行して結果を確認しました。

## <a name="next-steps"></a>次のステップ:

まだインストールしていない場合は、F# 言語のコア機能の一部をカバーする[F# のツアー](../tour.md) をご覧ください。 ここでは、Visual Studio にコピーしてF#実行できる、のいくつかの機能と十分なコードサンプルの概要について説明します。

> [!div class="nextstepaction"]
> [F# のツアー](../tour.md)

## <a name="see-also"></a>関連項目

- [F#言語リファレンス](../language-reference/index.md)
- [型推論](../language-reference/type-inference.md)
- [シンボルと演算子のリファレンス](../language-reference/symbol-and-operator-reference/index.md)
