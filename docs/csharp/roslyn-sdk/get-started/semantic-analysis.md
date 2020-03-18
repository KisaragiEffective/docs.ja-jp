---
title: セマンティック解析の概要
description: このチュートリアルでは、.NET コンパイラ SDK を使用したセマンティック解析の概要を説明します。
ms.date: 02/06/2018
ms.custom: mvc
ms.openlocfilehash: a6dcaeeb86acb5c0e1602f01dc5952ffd9d5e3f5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240511"
---
# <a name="get-started-with-semantic-analysis"></a>セマンティック解析の概要

このチュートリアルでは、構文 API の知識を前提としています。 「[構文解析の概要](syntax-analysis.md)」という記事が入門編になっています。

このチュートリアルでは、**シンボル API** と**バインドの API** について学習します。 これらの API は、プログラムの_意味論的意味_に関する情報を提供します。 プログラムのシンボルが表す型について質問したり、回答したりできます。

**.NET Compiler Platform SDK** をインストールする必要があります。

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="understanding-compilations-and-symbols"></a>コンパイルとシンボルについて

.NET コンパイラ SDK での作業が増えると、構文 API とセマンティック API の違いに詳しくなります。 **構文 API** では、プログラムの_構造_を見ることができます。 ただし、多くの場合、プログラムの意味論または_意味_に関する豊富な情報が必要になります。 Visual Basic または C# の緩いコード ファイルまたはスニペットは分離して構文的に解析できますが、孤立状態では、"この変数の型は何ですか" のような質問を問うことに意味がありません。 型名の意味は、アセンブリ参照、名前空間インポート、その他のコード ファイルに依存することがあります。 このような問いには、**セマンティック API** で、具体的には <xref:Microsoft.CodeAnalysis.Compilation?displayProperty=nameWithType> クラスで答えられます。

<xref:Microsoft.CodeAnalysis.Compilation> のインスタンスはコンパイラで見られるように 1 つのプロジェクトに類似し、Visual Basic または C# のプログラムをコンパイルするために必要なすべてを表します。 **コンパイル**には、コンパイルするソース ファイルのセット、アセンブリ参照、コンパイラ オプションが含まれます。 この文脈のその他すべての情報を利用し、コードの意味を推論できます。 <xref:Microsoft.CodeAnalysis.Compilation> では、型、名前空間、メンバー、名前やその他の式が参照する変数などのエンティティである**シンボル**を見つけることができます。 名前や式を**シンボル**と関連付けるプロセスを**バインド**と呼んでいます。

<xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> と同様に、<xref:Microsoft.CodeAnalysis.Compilation> は言語固有の派生物を持つ抽象クラスです。 コンパイルのインスタンスを作成するとき、<xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation?displayProperty=nameWithType> (または <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicCompilation?displayProperty=nameWithType>) クラスでファクトリ メソッドを呼び出す必要があります。

## <a name="querying-symbols"></a>シンボルにクエリを実行する

このチュートリアルでは、"Hello World" プログラムをもう一度見てみます。 今回、プログラムの中のシンボルにクエリを実行し、そのシンボルが表す型を理解します。 名前空間の型について問い、型で利用できるメソッドを確認します。

このサンプルの完成したコードは、[GitHub のリポジトリ](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/SemanticQuickStart)で確認できます。

> [!NOTE]
> 構文ツリー型では継承を使用して、プログラムのさまざまな場所で有効なさまざまな構文要素を記述します。 これらの API を使用することは、多くの場合、特定の派生型にプロパティまたはコレクション メンバーをキャストすることを意味します。 次の例では、割り当てとキャストは別のステートメントであり、明示的に型指定された変数を使用します。 コードを読み取り、API の戻り値の型と返されるオブジェクトのランタイム型を確認することができます。 実際には、暗黙的に型指定された変数を使用して、API 名に依存して、調べられるオブジェクトの型を記述するのがより一般的です。

次のようにして、新しい C# の **Stand-Alone Code Analysis Tool** プロジェクトを作成します。

* Visual Studio で、 **[ファイル]**  >  **[新規]**  >  **[プロジェクト]** の順に選択して、[新しいプロジェクト] ダイアログを表示します。
* **[Visual C#]**  >  **[機能拡張]** で、 **[Stand-Alone Code Analysis Tool]** を選択します。
* プロジェクトに "**SemanticQuickStart**" という名前を付け、[OK] をクリックします。

前述の基本的な "Hello World!" プログラムを 分析します。
Hello World プログラムのテキストを `Program` クラスの定数として追加します。

[!code-csharp[Declare the program test](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#1 "Declare a constant string for the program text to analyze")]

次に、以下のコードを追加して、`programText` 定数のコード テキストの構文ツリーをビルドします。  次の行を `Main` メソッドに追加します。

[!code-csharp[Create the tree](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#2 "Create the syntax tree")]

次に、作成済みのツリーから <xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation> をビルドします。 "Hello World" サンプルは、<xref:System.String> 型と <xref:System.Console> 型に基づきます。 コンパイルでこの 2 つの型を宣言するアセンブリを参照する必要があります。 `Main` メソッドに次の行を追加し、適切なアセンブリの参照を含む、構文ツリーのコンパイルを作成します。

[!code-csharp[Create the compilation](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#3 "Create the compilation for the semantic model")]

<xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation.AddReferences%2A?displayProperty=nameWithType> メソッドはコンパイルに参照を追加します。 <xref:Microsoft.CodeAnalysis.MetadataReference.CreateFromFile%2A?displayProperty=nameWithType> メソッドは参照としてアセンブリを読み込みます。

## <a name="querying-the-semantic-model"></a>セマンティック モデルにクエリを実行する

<xref:Microsoft.CodeAnalysis.Compilation> が与えられたら、その <xref:Microsoft.CodeAnalysis.Compilation> に含まれている <xref:Microsoft.CodeAnalysis.SyntaxTree> について <xref:Microsoft.CodeAnalysis.SemanticModel> を求めることができます。 セマンティック モデルは、通常は IntelliSense から得られるすべての情報源としてとらえることができます。 <xref:Microsoft.CodeAnalysis.SemanticModel> は "この場所のスコープ内にはどのような名前があるか"、"このメソッドからアクセスできるメンバーはどれか"、"このテキストのブロックではどのような変数が使用されているか"、"この名前/式は何を参照しているか" のような質問に答えることができます。 このステートメントを追加し、セマンティック モデルを作成します。

[!code-csharp[Create the semantic model](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#4 "Create the semantic model")]

## <a name="binding-a-name"></a>名前をバインドする

<xref:Microsoft.CodeAnalysis.Compilation> は <xref:Microsoft.CodeAnalysis.SyntaxTree> から <xref:Microsoft.CodeAnalysis.SemanticModel> を作成します。 モデルを作成したら、それにクエリを実行し、最初の `using` ディレクティブを見つけ、`System` 名前空間のシンボル情報を取得できます。 この 2 つの行を `Main` メソッドに追加し、セマンティック モデルを作成し、最初の using ステートメントのシンボルを取得します。

[!code-csharp[Find the namespace symbol for the first using](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#5 "Find the namespace symbol for the first using")]

先のコードは、最初の `using` ディレクティブの名前をバインドして、`System` 名前空間の <xref:Microsoft.CodeAnalysis.SymbolInfo?displayProperty=nameWithType> を取得する方法を示しています。 先のコードでは、**構文モデル**を利用してコードの構造を見つけることも確認できます。**セマンティック モデル**を使用し、その意味を理解します。 **構文モデル**は、using ステートメントの文字列 `System` を見つけます。 **セマンティック モデル**には、`System` 名前空間に定義されている型に関するすべての情報があります。

<xref:Microsoft.CodeAnalysis.SymbolInfo> オブジェクトから、<xref:Microsoft.CodeAnalysis.SymbolInfo.Symbol?displayProperty=nameWithType> プロパティを利用して <xref:Microsoft.CodeAnalysis.ISymbol?displayProperty=nameWithType> を取得できます。 このプロパティは、この式が参照するシンボルを返します。 何も参照しない式の場合 (数値リテラルなど)、このプロパティは `null` です。 <xref:Microsoft.CodeAnalysis.SymbolInfo.Symbol?displayProperty=nameWithType> が null ではないとき、<xref:Microsoft.CodeAnalysis.ISymbol.Kind?displayProperty=nameWithType> はシンボルの型を示します。 この例では、<xref:Microsoft.CodeAnalysis.ISymbol.Kind?displayProperty=nameWithType> プロパティは <xref:Microsoft.CodeAnalysis.SymbolKind.Namespace?displayProperty=nameWithType> です。 次のコードを `Main` メソッドに追加します。 `System` 名前空間のシンボルを取得し、`System` 名前空間で宣言されているすべての子名前空間を表示します。

[!code-csharp[Display all the child namespaces](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#6 "Display all the child namespaces from this compilation")]

プログラムを実行します。次の出力が表示されるはずです。

```output
System.Collections
System.Configuration
System.Deployment
System.Diagnostics
System.Globalization
System.IO
System.Numerics
System.Reflection
System.Resources
System.Runtime
System.Security
System.StubHelpers
System.Text
System.Threading
Press any key to continue . . .
```

> [!NOTE]
> この出力には、`System` 名前空間の子名前空間の一部が含まれていません。 このコンパイルに存在し、`System.String` が宣言されているアセンブリのみを参照するすべての名前空間が表示されます。 他のアセンブリで宣言されている名前空間は、このコンパイルでは認識されません。

### <a name="binding-an-expression"></a>式をバインドする

先のコードでは、名前にバインドしてシンボルを見つける方法を確認できます。 C# プログラムには、バウンドできて名前ではない式が他にあります。 この機能を見るために、単純な文字列リテラルのバインドにアクセスしましょう。

"Hello World" プログラムに <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LiteralExpressionSyntax?displayProperty=nameWithType> が含まれ、"Hello, World!" 文字列がコンソールに表示されます。

プログラムの中で 1 つの文字列リテラルを見つけることで、 "Hello, World!" 文字列が見つかります。 構文ノードが見つかったら、セマンティック モデルからそのノードの型情報を取得します。 次のコードを `Main` メソッドに追加します。

[!code-csharp[Find the namespace symbol for the only using](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#7 "Find the namespace symbol for the only using")]

<xref:Microsoft.CodeAnalysis.TypeInfo?displayProperty=nameWithType> 構造体には <xref:Microsoft.CodeAnalysis.TypeInfo.Type?displayProperty=nameWithType> プロパティが含まれます。このプロパティによって、リテラルの型に関するセマンティック情報にアクセスできます。 この例では、`string` 型です。 このプロパティをローカル変数に割り当てる宣言を追加します。

[!code-csharp[Find the semantic information about the string type](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#8 "Use the string literal to access the semantic information in the string type.")]

このチュートリアルの終わりとして、`string` を返す `string` 型で宣言されているすべてのパブリック メソッドのシーケンスを作成する LINQ クエリをビルドしましょう。 このクエリは複雑になります。そのため、1 行ずつビルドしてから 1 つのクエリとして再構築します。 このクエリのソースは、`string` 型で宣言されているすべてのメンバーのシーケンスです。

[!code-csharp[Access the sequence of members on the string type](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#9 "Access the sequence of members on the string type.")]

そのソース シーケンスには、プロパティやフィールドなど、すべてのメンバーが含まれています。そのため、<xref:System.Collections.Immutable.ImmutableArray%601.OfType%2A?displayProperty=nameWithType> メソッドでフィルター処理し、<xref:Microsoft.CodeAnalysis.IMethodSymbol?displayProperty=nameWithType> オブジェクトである要素を見つけます。

[!code-csharp[Filter the sequence to only methods](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#10 "Find the subset of the collection that is the methods.")]

次に、パブリック メソッドのみを返す別のフィルターを追加し、`string` を返します。

[!code-csharp[Filter on return type and accessibility](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#11 "Find only the public methods that return a string.")]

名前プロパティのみを選択します。オーバーロードを削除し、別個の名前のみを選択します。

[!code-csharp[find the distinct names.](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#12 "Use the string literal to access the semantic information in the string type.")]

LINQ クエリ構文で完全クエリをビルドし、コンソールにすべてのメソッド名を表示することもできます。

[!code-csharp[build and display the results of this query.](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#13 "Build and display the results of the query.")]

プログラムをビルドして実行します。 次の出力が表示されます。

```output
Join
Substring
Trim
TrimStart
TrimEnd
Normalize
PadLeft
PadRight
ToLower
ToLowerInvariant
ToUpper
ToUpperInvariant
ToString
Insert
Replace
Remove
Format
Copy
Concat
Intern
IsInterned
Press any key to continue . . .
```

セマンティック API を使用し、このプログラムの含まれるシンボルに関する情報を見つけ、表示しました。
