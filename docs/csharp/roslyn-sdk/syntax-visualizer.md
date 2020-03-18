---
title: Visual Studio で Roslyn Syntax Visualizer を使ってコードを調べる
description: Syntax Visualizer は、.NET Compiler Platform SDK がコード用に生成したモデルを調べるためのビジュアル ツールを提供します。
ms.date: 03/07/2018
ms.custom: mvc, vs-dotnet
ms.openlocfilehash: 27e5a1f0b31dd2af2ac779223538b03cdb4db0c5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156988"
---
# <a name="explore-code-with-the-roslyn-syntax-visualizer-in-visual-studio"></a>Visual Studio で Roslyn Syntax Visualizer を使ってコードを調べる

この記事では、.NET Compiler Platform ("Roslyn") SDK の一部として同梱されている Syntax Visualizer ツールの概要を説明します。 Syntax Visualizer は、構文ツリーを検査および調査するときに役立つツール ウィンドウです。 これは、分析するコードのモデルを理解するために欠かせないツールです。 .NET Compiler Platform (“Roslyn”) SDK を使用して、独自のアプリケーションを開発するときのデバッグにも役立ちます。 最初のアナライザーを作成するときに、このツールを開きます。 この Visualizer は、API で使用されるモデルを理解するのに役立ちます。 [SharpLab](https://sharplab.io) や [LINQPad](https://www.linqpad.net/) などのツールを使用しても、コードを調査し、構文ツリーを理解することができます。

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

「[.NET Compiler Platform SDK の概念とオブジェクト モデル](compiler-api-model.md)」の記事を読んで、.NET Compiler Platform SDK で使われている概念をよく理解してください。 この記事では、構文ツリー、ノード、トークン、およびトリビアについて説明されています。

## <a name="syntax-visualizer"></a>Syntax Visualizer

**Syntax Visualizer** を使用すると、Visual Studio IDE 内の現在アクティブなエディター ウィンドウで、C# または Visual Basic のコード ファイルの構文ツリーを検査できます。 Syntax Visualizer を起動するには、 **[ビュー]**  >  **[その他のウィンドウ]**  >  **[Syntax Visualizer]** の順にクリックします。  右上隅の **[サイド リンク バー]** ツールバーを使用することもできます。 「syntax」と開くコマンドを入力すると、**Syntax Visualizer** が表示されます。

このコマンドにより、Syntax Visualizer がフローティング ツール ウィンドウとして開きます。 開いているコード エディター ウィンドウがない場合は、次の図に示すように、表示は空白になります。

![Syntax Visualizer ツール ウィンドウ](media/syntax-visualizer/syntax-visualizer.png)

このツール ウィンドウを、Visual Studio 内の任意の場所 (左側など) にドッキングします。 Visualizer によって、現在のコード ファイルに関する情報が表示されます。

**[ファイル]**  >  **[新しいプロジェクト]** コマンドを使用して、新しいプロジェクトを作成します。 Visual Basic または C# のプロジェクトを作成することができます。 Visual Studio でこのプロジェクトのメイン コード ファイルが開かれるときに、Visualizer によってその構文ツリーが表示されます。 この Visual Studio インスタンスで既存の C#/Visual Basic ファイルを開き、Visualizer でそのファイルの構文ツリーを表示することができます。 Visual Studio 内で複数のコード ファイルを開いている場合、Visualizer は現在アクティブなコード ファイル (キーボードのフォーカスがあるコード ファイル) の構文ツリーを表示します。

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)
![C# 構文ツリーの視覚化](media/syntax-visualizer/visualize-csharp.png)

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)
![Visual Basic 構文ツリーの視覚化](media/syntax-visualizer/visualize-visual-basic.png)

---

前出の画像のように、Visualizer ツール ウィンドウでは、上部に構文ツリーが表示され、下部にプロパティ グリッドが表示されます。 プロパティ グリッドには、ツリーで現在選択されている項目のプロパティが表示されます。これには、項目の .NET *型*と*種類* (SyntaxKind) が含まれます。

構文ツリーは、*ノード*、*トークン*、および*トリビア*の 3 つの項目の型で構成されています。 これらの型については、[構文の使用](work-with-syntax.md) の記事で詳しく説明されています。 項目は型ごとに異なる色を使用して表されています。 使用されている色の概要については、[凡例] ボタンをクリックします。

ツリー内の各項目には、独自の**スパン**も表示されています。 **スパン**は、テキスト ファイル内のそのノードのインデックス (開始位置と終了位置) です。  前出の C# の例では、選択されている "UsingKeyword [0..5)" トークンには、5 文字の幅の**スパン** [0..5) があります。 "[..)" の表記は、開始インデックスはスパンの一部ですが、終了インデックスはそうではないことを意味します。

ツリーを移動するには、次の 2 つの方法があります。

* ツリー内の項目を展開またはクリックします。 Visualizer により、この項目のスパンに対応するテキストがコード エディターで自動的に選択されます。
* コード エディターで、テキストをクリックまたは選択します。 前出の Visual Basic の例では、コード エディターで "Module Module1" を含む行を選択すると、Visualizer によってツリー内の対応する ModuleStatement ノードに自動的に移動されます。

スパンがエディターで選択したテキストのスパンと最も一致する項目が、Visualizer により強調表示されます。

Visualizer は、アクティブなコード ファイル内の変更に合わせてツリーを更新します。 `Console.WriteLine()` への呼び出しを `Main()` 内に追加します。 入力すると、Visualizer によってツリーが更新されます。

`Console.` を入力したら、入力を一旦停止します。 ツリーには、ピンク色の項目がいくつかあります。 この時点では、型指定されたコードにはエラー ('診断' とも呼ばれる) があります。 これらのエラーは、構文ツリー内のノード、トークン、およびトリビアに添付されます。 エラーが添付された項目が、Visualizer によってピンク色の背景で強調表示されます。 ピンク色で表示されている任意の項目の上にポインターを置くと、エラーを調査することができます。 Visualizer では、構文エラー (型指定されたコードの構文に関連するエラー) のみが表示され、セマンティック エラーは表示されません。

## <a name="syntax-graphs"></a>構文グラフ

ツリー内の任意の項目を右クリックし、 **[View Directed Syntax Graph]\(有向構文グラフの表示\)** をクリックします。

# <a name="c"></a>[C#](#tab/csharp)

Visualizer により、選択した項目をルートとするサブツリーのグラフィカル表現が表示されます。 C# の例で **のメソッドに対応する**MethodDeclaration`Main()` ノードに、これらの手順を試してみます。 Visualizer により、次のような構文グラフが表示されます。

![C# の構文グラフの表示](media/syntax-visualizer/csharp-syntax-graph.png)

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

前出の Visual Basic の例の **メソッドに対応する**SubBlock`Main()` ノードにも同じことを試してみます。 Visualizer により、次のような構文グラフが表示されます。

![Visual Basic 構文グラフの表示](media/syntax-visualizer/visual-basic-syntax-graph.png)

---

構文グラフ ビューアーには、その色分けスキームの凡例を表示するオプションがあります。 マウスを使って、構文グラフ内の個々の項目にポインターを合わせても、その項目に対応するプロパティを表示することができます。

ツリー内の異なる項目の構文グラフを繰り返し表示することができます。グラフは常に Visual Studio 内の同じウィンドウに表示されます。 このウィンドウを Visual Studio 内の任意の場所にドッキングすることで、新しい構文グラフを表示するためにタブを切り替える必要がなくなります。 多くの場合、下部 (コード エディター ウィンドウの下) にドッキングすると便利です。

Visualizer ツール ウィンドウと構文グラフ ウィンドウを使用するためのドッキング レイアウトを次に示します。

![Visualizer と構文グラフ ウィンドウのためのドッキング レイアウト](media/syntax-visualizer/docking-layout.png)

デュアル モニターの設定では、2 つ目のモニターに構文グラフ ウィンドウを配置することもできます。

## <a name="inspecting-semantics"></a>セマンティクスの検査

Syntax Visualizer を使用すると、シンボルとセマンティクス情報についての基本的な検査ができます。 C# の例で Main() 内に `double x = 1 + 1;` を入力します。 次に、コード エディター ウィンドウで式 `1 + 1` を選択します。 Visualizer で **AddExpression** ノードが強調表示されます。 この **[AddExpression]** をクリックし、 **[View Symbol (if any)]\(シンボルの表示 (ある場合)\)** をクリックします。 メニュー項目のほとんどに "if any" 修飾子があることに注目してください。 Syntax Visualizer は、ノードのプロパティ (すべてのノードに提示されない場合があるプロパティを含む) を検査します。

Visualizer 内のプロパティ グリッドが、次の図のように更新されます。式のシンボルは、**SynthesizedIntrinsicOperatorSymbol** と **Kind = Method** です。

![シンボル プロパティ](media/syntax-visualizer/symbol-properties.png)

同じ **AddExpression** ノードに対し、 **[View TypeSymbol (if any)]\(TypeSymbol の表示 (ある場合)\)** を試してみます。 Visualizer のプロパティ グリッドが、次の図に示すように更新され、選択された式の型が `Int32` であることを示します。

![TypeSymbol プロパティ](media/syntax-visualizer/type-symbol-properties.png)

同じ **AddExpression** ノードに対し、 **[View Converted TypeSymbol (if any)]\(変換された TypeSymbol の表示 (ある場合)\)** を試してみます。 プロパティ グリッドが更新され、次の図に示すように、式の型は `Int32` で、式の変換後の型は `Double` であることを示します。 `Int32` に変換する必要があるコンテキストで `Double` 式が発生するため、このノードには変換後の型のシンボル情報が含まれます。 この変換により、代入演算子の左側にある変数 `Double` に指定された `x` 型を満たします。

![変換された TypeSymbol プロパティ](media/syntax-visualizer/converted-type-symbol-properties.png)

最後に、同じ **AddExpression** ノードに対し、 **[View Constant Value (if any)]\(定数値の表示 (ある場合)\)** を試してみます。 プロパティ グリッドには、式の値が、値 `2` を持つコンパイル時の定数であることが示されます。

![定数値](media/syntax-visualizer/constant-value.png)

前の例は Visual Basic でもレプリケートできます。 Visual Basic ファイルで `Dim x As Double = 1 + 1` を入力します。 コード エディター ウィンドウで式 `1 + 1` を選択します。 Visualizer で対応する **AddExpression** ノードが強調表示されます。 この **AddExpression** に対して上記の手順を繰り返すと、同一の結果になるはずです。

Visual Basic でさらに多くのコードを調べます。 メインの Visual Basic ファイルを次のコードで更新します。

```vb
Imports C = System.Console

Module Program
    Sub Main(args As String())
        C.WriteLine()
    End Sub
End Module
```

このコードは、ファイルの上部にある型 `C` にマップする `System.Console` という名前の別名を導入し、この別名を `Main()` の内部で使用します。 `C` メソッド内部で、この別名の使用を選択します (`C.WriteLine()` の `Main()`)。 Visualizer で、対応する **IdentifierName** ノードが選択されます。 このノードを右クリックし、 **[View Symbol (if any)]\(シンボルの表示 (ある場合)\)** をクリックします。 プロパティ グリッドには、次の図に示すように、この識別子が型 `System.Console` にバインドされていることが示されます。

![シンボル プロパティ](media/syntax-visualizer/symbol-visual-basic.png)

同じ **IdentifierName** ノードに対して、 **[View AliasSymbol (if any)]\(AliasSymbol の表示 (ある場合)\)** を試してみます。 プロパティ グリッドには、識別子が `C` ターゲットにバインドされている `System.Console` という名前の別名であることが示されます。 つまり、プロパティ グリッドでは、識別子 **に対応する**AliasSymbol`C` に関する情報が提供されます。

![AliasSymbol プロパティ](media/syntax-visualizer/alias-symbol.png)

任意の宣言された型、メソッド、プロパティに対応するシンボルを検査します。 Visualizer で対応するノードを選択し、 **[View Symbol (if any)]\(シンボルの表示 (ある場合)\)** をクリックします。 `Sub Main()` メソッドを選択します。メソッドの本文も含めます。 Visualizer で対応する **SubBlock** ノードに対し、 **[View Symbol (if any)]\(シンボルの表示 (ある場合)\)** をクリックします。 プロパティ グリッドには、この **SubBlock** の **MethodSymbol** の名前が `Main` で、戻り値の型が `Void` であることが示されます。

![メソッドの宣言のシンボルを表示する](media/syntax-visualizer/method-symbol.png)

上記の Visual Basic の例は、C# で簡単にレプリケートできます。 別名の `using C = System.Console;` の代わりに `Imports C = System.Console` を入力します。 C# で上記の手順を行うと、Visualizer ウィンドウの結果はまったく同じになります。

セマンティックの検査操作は、ノードでのみ使用できます。 トークンまたはトリビアでは使用できません。 すべてのノードに検査する興味深いセマンティック情報があるわけではありません。 ノードに興味深いセマンティック情報がない場合は、 **[View \* Symbol (if any)]\(シンボルの表示 (ある場合)\)** をクリックすると、空白のプロパティ グリッドが表示されます。

セマンティック分析を実行するための API の詳細については、「[セマンティクスの使用](work-with-semantics.md)」概要ドキュメントを参照してください。

## <a name="closing-the-syntax-visualizer"></a>Syntax Visualizer を閉じる

Syntax Visualizer ウィンドウは、ソース コードを調べるのに使用しない場合には閉じることができます。 Syntax Visualizer では、コード内を移動してソースを編集および変更するたびに表示が更新されます。 使用しない場合は、邪魔に感じる場合があります。
