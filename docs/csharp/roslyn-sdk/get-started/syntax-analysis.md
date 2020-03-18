---
title: 構文解析の概要 (Roslyn API)
description: 構文ツリーの走査、クエリおよびウォークに関する概要。
ms.date: 02/05/2018
ms.custom: mvc
ms.openlocfilehash: 22d1303c9daa2ae35cf130b0c857cd7a5efdbe76
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240520"
---
# <a name="get-started-with-syntax-analysis"></a>構文解析の概要

このチュートリアルでは、**Syntax API** について学習します。 Syntax API では、C# または Visual Basic プログラムを記述するデータ構造へのアクセスが提供されます。 これらのデータ構造には、あらゆるサイズのあらゆるプログラムを完全に表すことができる十分な情報があります。 これらの構造では、正しくコンパイルして実行される完全なプログラムを記述できます。 エディターでは、書き込み時に、不完全なプログラムを記述することもできます。

この優れた式を有効にする場合、Syntax API を構成する API とデータ構造が必然的に複雑になります。 まずは、一般的な "Hello World" プログラムのデータ構造がどのようになるかを見てみましょう。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

上のプログラムのテキストを見てください。 使い慣れた要素であることがわかります。 テキスト全体は単一のソース ファイル (**コンパイル ユニット**) を表しています。 そのソース ファイルの最初の 3 行は **using ディレクティブ**です。 残りのソースは**名前空間宣言**に含まれています。 名前空間宣言には子**クラス宣言**が含まれています。 クラス宣言には 1 つの**メソッド宣言**が含まれています。

Syntax API では、コンパイル ユニットを表すルートを含むツリー構造が作成されます。 ツリー内のノードは、using ディレクティブ、名前空間宣言およびプログラムの他のすべての要素を表しています。 ツリー構造は最下位レベルまで続きます。文字列 "Hello World!" は、**引数**の子孫である**文字列リテラル トークン**です。 Syntax API では、プログラムの構造体へのアクセスが提供されます。 特定のコード プラクティスに対してクエリを実行し、ツリー全体をウォークしてコードを理解し、既存のツリーを変更して新しいツリーを作成することができます。

その簡単な説明では、Syntax API を使用してアクセスできる情報の種類の概要を示します。 Syntax API は、C# の使い慣れたコード コンストラクトを記述する正式な API にすぎません。 完全な機能には、改行、空白、インデントを含め、コードの書式設定方法に関する情報が含まれます。 この情報を使用して、人間のプログラマまたはコンパイラによって書き込まれ、読み取られるコードを完全に表すことができます。 この構造を使用することで、深い意味のあるレベルのソース コードと対話することができます。 テキスト文字列はもう存在しませんが、C# プログラムの構造を表すデータはあります。

まず **.NET Compiler Platform SDK** をインストールする必要があります。

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="understanding-syntax-trees"></a>構文ツリーについて

C# コードの構造の分析には Syntax API を使用します。 **Syntax API** では、パーサー、構文ツリー、および構文ツリーを分析して構築するためのユーティリティを公開します。 これを使用して、特定の構文要素のコードの検索またはプログラムのコードの読み取りを行います。

構文ツリーは、C# および Visual Basic プログラムを理解するために C# および Visual Basic コンパイラで使用されるデータ構造です。 構文ツリーは、プロジェクトのビルド時、または開発者が F5 キーを押したときに実行されるのと同じパーサーによって生成されます。 構文ツリーは言語に対して完全に忠実であり、コード ファイル内のすべての情報はツリーで表されます。 構文ツリーをテキストに書き込むことで、解析された元の正確なテキストが再現されます。 構文ツリーは**不変**でもあります。構文ツリーを作成した後で変更することはできません。 ツリーのコンシューマーは、データが変更されないことを認識したうえで、ロックやその他のコンカレンシー手段を使用せずに、複数のスレッドでツリーを分析できます。 API を使用して、新しいツリーを作成することができます。その場合、既存のツリーを変更します。

構文ツリーの 4 つの基本的な構成要素は次のとおりです。

* <xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> クラス。インスタンスで解析ツリー全体を表します。 <xref:Microsoft.CodeAnalysis.SyntaxTree> は、言語固有の派生物を持つ抽象クラスです。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxTree?displayProperty=nameWithType> (または <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicSyntaxTree?displayProperty=nameWithType>) クラスの解析メソッドを使用して、C# または Visual Basic のテキストを解析します。
* <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> クラス。インスタンスで、宣言、ステートメント、句、および式などの構文構造を表します。
* <xref:Microsoft.CodeAnalysis.SyntaxToken?displayProperty=nameWithType> 構造。個々のキーワード、ID、演算子、または句読点を表します。
* 最後は <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> 構造です。これは、トークン、プリプロセス ディレクティブ、およびコメントの間の空白など、重要でない情報を構文的に表します。

トリビア、トークン、およびノードは、Visual Basic または C# コードのフラグメント内のすべてを完全に表すツリーを形成するために階層的に構成されます。 この構造は、**Syntax Visualizer** ウィンドウを使用して確認することができます。 Visual Studio で、 **[ビュー]**  >  **[その他のウィンドウ]**  >  **[Syntax Visualizer]\(Syntax Visualizer\)** の順に選択します。 たとえば、**Syntax Visualizer** を使用して調べた上記の C# ソース ファイルは、次の図のようになります。

**SyntaxNode**: 青 | **SyntaxToken**: 緑 | **SyntaxTrivia**: 赤 ![C# コード ファイル](media/walkthrough-csharp-syntax-figure1.png)

このツリー構造を移動することで、ステートメント、式、トークン、またはコード ファイルのわずかな空白を見つけることができます。

Syntax API を使用してコード ファイルで何でも見つけることはできますが、ほとんどのシナリオでは、コードの小さなスニペットの確認や、特定のステートメントまたはフラグメントの検索が必要です。 以下の 2 つの例では、コードの構造の参照、または単一ステートメントの検索を行う場合の一般的な使用方法を示します。

## <a name="traversing-trees"></a>ツリーの走査

2 つの方法で構文ツリー内のノードを調べることができます。 ツリーを走査して、各ノードを調べることができます。あるいは、特定の要素やノードに対してクエリを実行することができます。

### <a name="manual-traversal"></a>手動による走査

このサンプルの完成したコードは、[GitHub のリポジトリ](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/SyntaxQuickStart)で確認できます。

> [!NOTE]
> 構文ツリー型では継承を使用して、プログラムのさまざまな場所で有効なさまざまな構文要素を記述します。 これらの API を使用することは、多くの場合、特定の派生型にプロパティまたはコレクション メンバーをキャストすることを意味します。 次の例では、割り当てとキャストは別のステートメントであり、明示的に型指定された変数を使用します。 コードを読み取り、API の戻り値の型と返されるオブジェクトのランタイム型を確認することができます。 実際には、暗黙的に型指定された変数を使用して、API 名に依存して、調べられるオブジェクトの型を記述するのがより一般的です。

次のようにして、新しい C# の **Stand-Alone Code Analysis Tool** プロジェクトを作成します。

* Visual Studio で、 **[ファイル]**  >  **[新規]**  >  **[プロジェクト]** の順に選択して、[新しいプロジェクト] ダイアログを表示します。
* **[Visual C#]**  >  **[機能拡張]** で、 **[Stand-Alone Code Analysis Tool]** を選択します。
* プロジェクトに "**SyntaxTreeManualTraversal**" という名前を付けて、[OK] をクリックします。

前述の基本的な "Hello World!" プログラムを 分析します。
Hello World プログラムのテキストを `Program` クラスの定数として追加します。

[!code-csharp[Declare the program text](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#1 "Declare a constant string for the program text to analyze")]

次に、以下のコードを追加して、`programText` 定数のコード テキストの**構文ツリー**をビルドします。  次の行を `Main` メソッドに追加します。

[!code-csharp[Create the tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#2 "Create the syntax tree")]

これら 2 行でツリーを作成し、そのツリーのルート ノードを取得します。 これでツリー内のノードを調べることができます。 以下の行を `Main` メソッドに追加して、ツリー内のルート ノードのプロパティをいくつか表示します。

[!code-csharp[Examine the root node](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#3 "Examine the root node")]

アプリケーションを実行して、このツリー内のルート ノードについて、コードで検出された内容を確認します。

通常、コードについて学習する場合、ツリーを走査します。 この例では、使い慣れたコードを分析して、API を調べます。 次のコードを追加して、`root` ノードの最初のメンバーを調べます。

[!code-csharp[Find the first member](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#4 "Find the first member")]

そのメンバーは <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NamespaceDeclarationSyntax?displayProperty=nameWithType> です。 `namespace HelloWorld` 宣言のスコープ内ですべてを表します。 次のコードを追加して、`HelloWorld` 名前空間内で宣言されているノードを調べます。

[!code-csharp[Find the class declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#5 "Find the class declaration")]

プログラムを実行して、学習した内容を確認します。

宣言が <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ClassDeclarationSyntax?displayProperty=nameWithType> であることがわかったので、その型の新しい変数を宣言して、クラス宣言を調べます。 このクラスには 1 つのメンバー (`Main` メソッド) のみが含まれます。 次のコードを追加して `Main` メソッドを見つけ、それを <xref:Microsoft.CodeAnalysis.CSharp.Syntax.MethodDeclarationSyntax?displayProperty=nameWithType> にキャストします。

[!code-csharp[Find the main declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#6 "Find the main declaration")]

メソッド宣言ノードには、メソッドに関するすべての構文情報が含まれています。 次は、`Main` メソッドの戻り値の型、引数の数と型、およびメソッドの本文を表示します。 次のコードを追加します。

[!code-csharp[Examine the syntax of the main method](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#7 "Display information about the main method")]

プログラムを実行して、このプログラムについて検出したすべての情報を表示します。

```text
The tree is a CompilationUnit node.
The tree has 1 elements in it.
The tree has 4 using statements. They are:
        System
        System.Collections
        System.Linq
        System.Text
The first member is a NamespaceDeclaration.
There are 1 members declared in this namespace.
The first member is a ClassDeclaration.
There are 1 members declared in the Program class.
The first member is a MethodDeclaration.
The return type of the Main method is void.
The method has 1 parameters.
The type of the args parameter is string[].
The body text of the Main method follows:
        {
            Console.WriteLine("Hello, World!");
        }
```

### <a name="query-methods"></a>クエリ メソッド

ツリーの走査に加え、<xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> で定義されているクエリ メソッドを使用して、構文ツリーを調べることもできます。 XPath を使い慣れていれば、これらのメソッドをすぐに使いこなすことができます。 LINQ でこれらのメソッドを使用することで、ツリー内の内容をすばやく検索できます。 <xref:Microsoft.CodeAnalysis.SyntaxNode> には、<xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.AncestorsAndSelf%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes%2A> などのクエリ メソッドがあります。

これらのクエリ メソッドを使用すれば、ツリーを移動せずに、`Main` メソッドに対する引数を検索することができます。 次のコードを `Main` メソッドの下部に追加します。

[!code-csharp[Query the tree for the arguments to Main](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#8 "Query the tree for the arguments to Main")]

最初のステートメントでは LINQ 式と <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A> メソッドを使用して、前の例と同じパラメーターを検索します。

プログラムを実行すると、ツリーを手動で移動する場合と同じパラメーターが LINQ 式で検出されたことがわかります。

このサンプルでは `WriteLine` ステートメントを使用して、走査された構文ツリーに関する情報を表示します。 デバッガーで完成したプログラムを実行して、さらに理解を深めることもできます。 hello world プログラム用に作成された構文ツリーの一部であるメソッドとプロパティをさらに調べることができます。

## <a name="syntax-walkers"></a>構文ウォーカー

多くの場合、構文ツリーで特定の型のノード (ファイル内のすべてのプロパティ宣言など) をすべて検索する必要があります。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker?displayProperty=nameWithType> クラスを拡張し、<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitPropertyDeclaration(Microsoft.CodeAnalysis.CSharp.Syntax.PropertyDeclarationSyntax)> メソッドをオーバーライドして、構造を事前に認識せずに、構文ツリーのすべてのプロパティ宣言を処理します。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> は、ノードとその各子に再帰的にアクセスする特定の種類の <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor> です。

この例では、構文ツリーを調べる <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> を実装します。 `System` 名前空間をインポートしていない `using` ディレクティブを収集します。

新しい C# の**Stand-Alone Code Analysis Tool** プロジェクトを作成し、"**SyntaxWalker**" という名前を付けます。

このサンプルの完成したコードは、[GitHub のリポジトリ](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/SyntaxQuickStart)で確認できます。 GitHub のサンプルには、このチュートリアルで説明されている両方のプロジェクトが含まれています。

上記のサンプルと同様に、分析しようとしているプログラムのテキストを保持する文字列定数を定義できます。

[!code-csharp[Define the code text to analyzer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#1 "Define the program text to analyze")]

このソース テキストには、4 つの異なる場所 (ファイル レベル、最上位の名前空間、2 つの入れ子になった名前空間) に分散されている `using` ディレクティブが含まれます。 この例では、コードに対してクエリを実行する <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> クラスを使用する主なシナリオに焦点を当てます。 using 宣言を検索するためにルート構文ツリー内のすべてのノードにアクセスするのは面倒です。 代わりに、派生クラスを作成し、ツリー内の現在のノードが using ディレクティブである場合にのみ、呼び出されるメソッドをオーバーライドします。 ビジターは他のノード型に対して何も行いません。 この単一のメソッドで各 `using` ステートメントを調べ、`System` 名前空間にはない名前空間のコレクションをビルドします。 すべての `using` ステートメント (`using` ステートメントのみ) を調べる <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> をビルドします。

これでプログラム テキストを定義したので、`SyntaxTree` を作成して、そのツリーのルートを取得する必要があります。

[!code-csharp[Create the Syntax tree and access the root](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#2 "Create the Syntax tree and access the root node.")]

次は、新しいクラスを作成します。 Visual Studio で、 **[プロジェクト]**  >  **[新しい項目の追加]** の順に選択します。 **[新しい項目の追加]** ダイアログで、ファイル名として「*UsingCollector.cs*」と入力します。

`UsingCollector` クラスで `using` ビジター機能を実装します。 まず、<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> から `UsingCollector` クラスを派生させます。

[!code-csharp[Declare the base class for the using collector](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#3 "Declare the base class for the UsingCollector")]

収集する名前空間ノードを保持する記憶域が必要です。  `UsingCollector` クラスでパブリックの読み取り専用プロパティを宣言します。その場合、以下の変数を使用して、検索する <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> ノードを格納します。

[!code-csharp[Declare storage for the using syntax nodes](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#4 "Declare storage for the using syntax nodes")]

基本クラスの <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> では、構文ツリー内の各ノードにアクセスするロジックを実装します。 派生クラスは、対象となる特定のノードに対して呼び出されたメソッドをオーバーライドします。 この例では、`using` ディレクティブが対象となります。 つまり、<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> メソッドをオーバーライドする必要があります。 このメソッドへの 1 つの引数は <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> オブジェクトです。 これがビジターを使用する最も重要は利点です。特定のノード型に既にキャストされている引数を使用して、オーバーライドされたメソッドを呼び出します。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> クラスには、インポートされる名前空間の名前を格納する <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.Name> プロパティがあります。 それは <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType> です。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> オーバーライドで次のコードを追加します。

[!code-csharp[Examine using nodes for the System namespace](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#5 "Examine all using nodes for the System namespace.")]

前の例と同様に、このメソッドを理解するのに役立つさまざまな `WriteLine` ステートメントを追加しました。 呼びされるタイミングと、毎回渡される引数を確認できます。

最後に、2 行のコードを追加して `UsingCollector` を作成し、ルート ノードにアクセスするようにして、`using` ステートメントをすべて収集する必要があります。 次に、`foreach` ループを追加して、コレクターが検出した `using` ステートメントをすべて表示します。

[!code-csharp[Create the UsingCollector and visit the root node.](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#6 "Create the UsingCollector and visit the root node.")]

プログラムをコンパイルして実行します。 次の出力が表示されます。

```console
        VisitUsingDirective called with System.
        VisitUsingDirective called with System.Collections.Generic.
        VisitUsingDirective called with System.Linq.
        VisitUsingDirective called with System.Text.
        VisitUsingDirective called with Microsoft.CodeAnalysis.
                Success. Adding Microsoft.CodeAnalysis.
        VisitUsingDirective called with Microsoft.CodeAnalysis.CSharp.
                Success. Adding Microsoft.CodeAnalysis.CSharp.
        VisitUsingDirective called with Microsoft.
                Success. Adding Microsoft.
        VisitUsingDirective called with System.ComponentModel.
        VisitUsingDirective called with Microsoft.Win32.
                Success. Adding Microsoft.Win32.
        VisitUsingDirective called with System.Runtime.InteropServices.
        VisitUsingDirective called with System.CodeDom.
        VisitUsingDirective called with Microsoft.CSharp.
                Success. Adding Microsoft.CSharp.
Microsoft.CodeAnalysis
Microsoft.CodeAnalysis.CSharp
Microsoft
Microsoft.Win32
Microsoft.CSharp
Press any key to continue . . .
```

おめでとうございます! **Syntax API** を使用して、C# ソース コードで特定の種類の C# ステートメントと宣言を検索しました。
