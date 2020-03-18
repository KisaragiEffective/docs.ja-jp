---
title: 構文変換の概要 (Roslyn API)
description: 構文ツリーの走査、クエリおよびウォークに関する概要。
ms.date: 06/01/2018
ms.custom: mvc
ms.openlocfilehash: 5045dca839daba1070b34720e72cc9c4f7b94828
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240611"
---
# <a name="get-started-with-syntax-transformation"></a>構文変換の概要

このチュートリアルは、「[構文解析の概要](syntax-analysis.md)」および「[セマンティック解析の概要](semantic-analysis.md)」クイック スタートで説明した概念と手法に基づいて作成されています。 これらのクイック スタートをまだ完了していない場合は、このクイック スタートを始める前に完了する必要があります。

このクイック スタートでは、構文ツリーを作成および変換する手法を学習します。 前のクイック スタートで学習した手法と組み合わせて、初めてのコマンド ライン リファクタリングを作成します。

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="immutability-and-the-net-compiler-platform"></a>不変性と .NET コンパイラ プラットフォーム

**不変性**は、.NET コンパイラ プラットフォームの基本原則です。 不変データ構造は、作成後には変更できません。 不変データ構造は、複数のコンシューマーから安全かつ同時に共有、分析できます。 コンシューマーが、予期できない方法で別のコンシューマーに影響を及ぼす危険はありません。 アナライザーには、ロックやその他のコンカレンシー手段は不要です。 この規則は、構文ツリー、コンパイル、記号、セマンティック モデルなど、出現するすべてのデータ構造に当てはまります。 既存の構造体を変更するのではなく、API は古いオブジェクトに対して指定された相違点に基づいて、新しいオブジェクトを作成します。 この概念を構文ツリーに適用して、変換を使用して新しいツリーを作成します。

## <a name="create-and-transform-trees"></a>ツリーの作成と変換

構文の変換では、2 つの方法のうち 1 つを選択します。 **ファクトリ メソッド**は、置き換える特定のノードや、新しいコードを挿入する特定の場所を検索するときに最もよく使用されます。 **リライター**は、プロジェクト全体をスキャンして、置き換えるコード パターンを探す場合に最適です。

### <a name="create-nodes-with-factory-methods"></a>ファクトリ メソッドを使用してノードを作成する

最初の構文変換では、ファクトリ メソッドを使用します。 `using System.Collections;` ステートメントを `using System.Collections.Generic;` ステートメントで置き換えます。 この例は、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType> ファクトリ メソッドを使用して <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxNode?displayProperty=nameWithType> オブジェクトを作成する方法を示しています。 **ノード**、**トークン**、**トリビア**の各種類に対して、その種類のインスタンスを作成するファクトリ メソッドが用意されています。 ボトムアップ方式でノードを階層的に構成して、構文ツリーを作成します。 次に、既存のプログラムを変換して、既存のノードを作成した新しいツリーで置き換えます。

Visual Studio を起動し、新しい C# の **Stand-Alone Code Analysis Tool** プロジェクトを作成します。 Visual Studio で、 **[ファイル]**  >  **[新規]**  >  **[プロジェクト]** の順に選択して、[新しいプロジェクト] ダイアログを表示します。 **[Visual C#]**  >  **[機能拡張]** で、 **[Stand-Alone Code Analysis Tool]** を選択します。 このクイック スタートには 2 つのサンプル プロジェクトがあるため、ソリューションに「**SyntaxTransformationQuickStart**」、プロジェクトに「**ConstructionCS**」という名前を付けます。 **[OK]** をクリックします。

このプロジェクトでは、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType> クラスのメソッドを使用して、`System.Collections.Generic` 名前空間を表す <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType> を構築します。

`Program.cs` ファイルの先頭に次の using ディレクティブを追加して、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory> クラスのファクトリ メソッドと <xref:System.Console> のメソッドをインポートし、後で修飾せずに使用できるようにします。

[!code-csharp[import the SyntaxFactory class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#StaticUsings "import the Syntax Factory class and the System.Console class")]

`using System.Collections.Generic;` ステートメントを表すツリーをビルドするための**名前構文ノード**を作成します。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> は、C# に現れる 4 つの型の基底クラスです。 これらの 4 つの型の名前を組み合わせて、C# 言語中に出現するすべての名前を作成できます。

* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>: `System` や `Microsoft` のような、単一のシンプルな名前を表します。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.GenericNameSyntax?displayProperty=nameWithType>: `List<int>` のような、ジェネリック型またはジェネリック メソッドの名前を表します。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax?displayProperty=nameWithType>: `System.IO` のような、`<left-name>.<right-identifier-or-generic-name>` 形式の修飾名を表します。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.AliasQualifiedNameSyntax?displayProperty=nameWithType>: `LibraryV2::Foo` のような、アセンブリ外部エイリアスを使用した名前を表します。

<xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory.IdentifierName(System.String)> メソッドを使用して、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> ノードを作成します。 `Program.cs` で、`Main` メソッドに次のコードを追加します。

[!code-csharp[create the system identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateIdentifierName "Create and display the system name identifier")]

上のコードでは、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> オブジェクトを作成して変数 `name` に割り当てます。 Roslyn API の多くは、関連の型を使用しやすくするために基底クラスを返します。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> である変数 `name` は、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> をビルドするときに再利用できます。 サンプルをビルドするときに、型の推定を使用しないでください。 このプロジェクトではそのステップを自動化します。

これで名前が作成されました。 次に、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> をビルドして、ツリー内にさらに多くのノードをビルドします。 新しいツリーでは、`name` を名前の左側として使用し、`Collections` 名前空間の新しい <xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> を <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> の右側として使用します。 `program.cs` に次のコードを追加します。

[!code-csharp[create the collections identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateQualifiedIdentifierName "Build the System.Collections identifier")]

コードを再度実行し、結果を確認します。 コードを表すノードのツリーをビルドします。 このパターンを繰り返して、名前空間 `System.Collections.Generic` の <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> をビルドします。 `Program.cs` に次のコードを追加します。

[!code-csharp[create the full identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateFullNamespace "Build the System.Collections.Generic identifier")]

プログラムを再度実行して、追加するコード用のツリーがビルドされたことを確認します。

### <a name="create-a-modified-tree"></a>変更されたツリーを作成する

これで、1 つのステートメントを含む小さな構文ツリーがビルドされました。 新しいノードを作成するための API は、単一のステートメントや他の小規模なコード ブロックを作成するときに適した選択肢です。 しかし、より大規模なコード ブロックをビルドするときは、ノードを置き換えたり既存のツリーにノードを挿入するためのメソッドを使用する必要があります。 すでに説明したように、構文ツリーは不変です。 **構文 API** には、作成後に既存の構文ツリーを変更するメカニズムはありません。 その代わり、既存のツリーへの変更に基づいて新しいツリーを生成するメソッドが用意されています。 `With*` メソッドは、<xref:Microsoft.CodeAnalysis.SyntaxNode> から派生した具象クラス内、または <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions> クラスで宣言された拡張メソッド内で定義されます。 これらのメソッドは、既存のノードの子プロパティに変更を適用することで新しいノードを作成します。 さらに、<xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> 拡張メソッドを使用すると、サブツリー内の子孫ノードを置き換えることができます。 このメソッドも、新しく作成された子ノードをポイントするように親を更新し、このプロセスをツリー全体で上位に向かって繰り返します (このプロセスを、ツリーの "_再スピン_" と呼びます)。

次のステップでは、(小規模な) プログラム全体を表すツリーを作成した後で、それを変更します。 `Program` クラスの先頭に次のコードを追加します。

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#DeclareSampleCode "Create a tree that represents a small program")]

> [!NOTE]
> このコード例では、`System.Collections.Generic` 名前空間ではなく `System.Collections` 名前空間を使用します。

次に、`Main` メソッドの末尾に、テキストを解析してツリーを作成する次のコードを追加します。

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateParseTree "Create a tree that represents a small program")]

この例では、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)?displayProperty=NameWithType> メソッドを使用して、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> ノード内の名前を先ほどのコードで作成した名前に置き換えます。

名前 `System.Collections` を先ほどのコードで作成した名前に置き換えるには、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)> メソッドを使用して新しい <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> ノードを作成します。 `Main` メソッドの末尾に次のコードを追加します。

[!code-csharp[create a new subtree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#BuildNewUsing "Create the subtree with the replaced namespace")]

プログラムを実行し、出力を注意深く見てください。 ルート ツリーに `newusing` が配置されていません。 元のツリーは変更されていません。

新しいツリーを作成するために、<xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> 拡張メソッドを使用した次のコードを追加します。 新しいツリーが、既存のインポートを更新された `newUsing` ノードで置き換えた結果として作成されます。 この新しいツリーを既存の `root` に割り当てます。

[!code-csharp[create a new root tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#TransformTree "Create the transformed root tree with the replaced namespace")]

再びプログラムを実行します。 今度は、ツリーに `System.Collections.Generic` 名前空間が正しくインポートされます。

### <a name="transform-trees-using-syntaxrewriters"></a>`SyntaxRewriters` を使用してツリーを変換する

`With*` メソッドと <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> メソッドは、構文ツリーの個々のブランチを変換するのに便利な手段です。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> クラスは、構文ツリー上で複数の変換を実行します。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> クラスは <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor%601?displayProperty=nameWithType> のサブクラスです。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> は、特定の型の <xref:Microsoft.CodeAnalysis.SyntaxNode> に変換を適用します。 構文ツリー内にその型が存在すれば、複数の型の <xref:Microsoft.CodeAnalysis.SyntaxNode> オブジェクトに変換を適用できます。 このクイック スタートの 2 番目のプロジェクトでは、型の推定が使用される可能性があるすべての場所のローカル変数宣言に含まれる明示的な型を削除する、コマンド ライン リファクタリングを作成します。

新しい C# の **Stand-Alone Code Analysis Tool** プロジェクトを作成します。 Visual Studio で、`SyntaxTransformationQuickStart` ソリューション ノードを右クリックします。 **[追加]**  >  **[新しいプロジェクト]** を選択して、 **[新しいプロジェクト] ダイアログ**を表示します。 **[Visual C#]**  >  **[機能拡張]** で、 **[Stand-Alone Code Analysis Tool]** を選択します。 プロジェクトに「`TransformationCS`」という名前を付けて、[OK] をクリックします。

最初のステップは、変換を実行するための <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> から派生したクラスを作成することです。 新しいクラスのファイルをプロジェクトに追加します。 Visual Studio で、 **[プロジェクト]**  >  **[クラスの追加...]** を選択します。 **[新しい項目の追加]** ダイアログで、ファイル名として「`TypeInferenceRewriter.cs`」を入力します。

`TypeInferenceRewriter.cs` ファイルに次の using ディレクティブを追加します。

[!code-csharp[add necessary usings](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#AddUsings "Add required usings")]

次に、`TypeInferenceRewriter` クラスで <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> クラスを拡張します。

[!code-csharp[add base class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BaseClass "Add base class")]

次のコードを追加して、<xref:Microsoft.CodeAnalysis.SemanticModel> を保持する読み取り専用の private フィールドを宣言し、コンストラクター内で初期化します。 このフィールドは、後で型の推定を使用できる場所を特定するために必要になります。

[!code-csharp[initialize members](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Construction "Declare and initialize member variables")]

<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)> メソッドをオーバーライドします。

```csharp
public override SyntaxNode VisitLocalDeclarationStatement(LocalDeclarationStatementSyntax node)
{

}
```

> [!NOTE]
> Roslyn API の多くは、返される実際の ランタイム型の基底クラスである戻り値の型を宣言します。 多くのシナリオでは、ノードが別の種類のノードに完全に置き換えられることがあり、削除される場合もあります。 この例では、<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)> メソッドは <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> の派生型ではなく <xref:Microsoft.CodeAnalysis.SyntaxNode> を返します。 このリライターは、既存のノードに基づいて新しい <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> ノードを返します。

このクイック スタートでは、ローカル変数宣言を処理します。 これは `foreach` ループ、`for` ループ、LINQ 式、ラムダ式などの他の宣言にも拡張できます。 さらに、このリライターは、次のような最も単純な形式の宣言しか変換しません。

```csharp
Type variable = expression;
```

自分で探究したい場合は、次のような変数宣言の型の完成したサンプルを拡張することを検討してください。

```csharp
// Multiple variables in a single declaration.
Type variable1 = expression1,
     variable2 = expression2;
// No initializer.
Type variable;
```

次のコードを `VisitLocalDeclarationStatement` メソッドの本体に追加して、これらの形式の宣言の書き換えをスキップするようにします。

[!code-csharp[exclude other declarations](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Exclusions "Exclude variables declarations not processed by this sample")]

このメソッドは、`node` パラメーターを変更せずに返すことにより、書き換えが行われないことを示します。 いずれの `if` 式も true でない場合、ノードは初期化で可能な宣言を表します。 以下のステートメントを追加して、宣言で指定されている型の名前を抽出し、それを <xref:Microsoft.CodeAnalysis.SemanticModel> フィールドを使用してバインドして、型のシンボルを取得できるようにします。

[!code-csharp[extract type name](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ExtractTypeSymbol "Extract the type name specified by the declaration")]

さらに、次のステートメントを追加して、初期化子式をバインドします。

[!code-csharp[bind initializer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BindInitializer "Bind the initializer expressions")]

最後に、次の `if` ステートメントを追加して、初期化子式の型が指定の型と一致した場合に既存の型名を `var` キーワードで置き換えるようにします。

[!code-csharp[ReplaceNode](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ReplaceNode "Replace the initializer node")]

宣言では初期化子式が基底クラスまたはインターフェイスにキャストされる場合があるので、この条件が必要です。 必要な場合は、割り当ての左側の型と右側の型が一致しません。 このようなケースで明示的な型を削除すると、プログラムのセマンティクスが変わってしまいます。 `var` はコンテキスト キーワードであるため、`var` はキーワードではなく識別子として指定されます。 垂直方向の空白とインデントを維持するために、先頭および末尾のトリビア (空白) が古い型名から `var` キーワードへと転送されています。 型名は実際には宣言ステートメントの孫であるため、`With*` よりも `ReplaceNode` を使用して <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> を変換するほうが簡単です。

これで `TypeInferenceRewriter` が完成しました。 `Program.cs` ファイルに戻って例を完成させましょう。 テスト用の <xref:Microsoft.CodeAnalysis.Compilation> を作成し、そこから <xref:Microsoft.CodeAnalysis.SemanticModel> を取得します。 その <xref:Microsoft.CodeAnalysis.SemanticModel> を使用して、`TypeInferenceRewriter` を試します。 このステップは最後に実行します。 それまでの間、テスト用のコンパイルを表すプレースホルダー変数を宣言しておきます。

[!code-csharp[DeclareCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#DeclareTestCompilation "Declare the test compilation")]

しばらくすると、`CreateTestCompilation` メソッドが存在しないことを通知するエラーの波線が表示されます。 **Ctrl + .** キーを押して電球を開き、Enter キーを押して **[メソッド スタブの生成]** コマンドを呼び出します。 このコマンドにより、`Program` クラス内に `CreateTestCompilation` メソッドのメソッド スタブが生成されます。 このメソッドについては後で説明します。

![C# 使用例からのメソッドの生成](./media/syntax-transformation/generate-from-usage.png)

テスト用の <xref:Microsoft.CodeAnalysis.Compilation> 内の各 <xref:Microsoft.CodeAnalysis.SyntaxTree> を反復処理する次のコードを記述します。 処理ごとに、そのツリーの <xref:Microsoft.CodeAnalysis.SemanticModel> を持った 新しい `TypeInferenceRewriter` が初期化されます。

[!code-csharp[IterateTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#IterateTrees "Iterate all the source trees in the test compilation")]

作成した `foreach` ステートメント内に次のコードを追加して、各ソース ツリーで変換が実行されるようにします。 このコードは、何らかの編集が行われた場合に、変換された新しいツリーを条件付きで書き出します。 リライターは、型の推定を使用して単純化される可能性があるローカル変数宣言が 1 つ以上見つかった場合にのみ、ツリーを変更します。

[!code-csharp[TransformTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#TransformTrees "Transform and save any trees that are modified by the rewriter")]

`File.WriteAllText` コードの下に波線が表示されるはずです。 電球を選択し、必要な `using System.IO;` ステートメントを追加します。

完了までもう少しです。 最後の 1 ステップは、<xref:Microsoft.CodeAnalysis.Compilation> の作成です。 このクイック スタートでは型の推定を一度も使用していないので、テスト ケースとするには完璧です。 残念ながら、C# プロジェクト ファイルからコンパイルを作成する方法については、このチュートリアルの対象外です。 しかし幸いなことに、これまでの手順に慎重に従ってきたならば希望が持てます。 `CreateTestCompilation` メソッドの内容を次のコードに置き換えます。 このクイック スタートで説明したプロジェクトに偶然にも一致するテスト用コンパイルが作成されます。

[!code-csharp[CreateTestCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#CreateTestCompilation "Create a test compilation using the code written for this quickstart.")]

幸運を祈ってプロジェクトを実行しましょう。 Visual Studio で、 **[デバッグ]**  >  **[デバッグの開始]** を選択します。 Visual Studio で、プロジェクト内のファイルが変更されたという通知が表示されるはずです。 **[すべてに適用]** をクリックして、変更されたファイルをリロードします。 それらを調べて成果を確認しましょう。 明示的で冗長な型指定子がすべてなくなるとどれほどコードがすっきり見えるかに注目してください。

おめでとうございます! **コンパイラ API** を使用して、C# プロジェクト内のすべてのファイルで特定の構文パターンを検索し、それらのパターンに一致するソース コードのセマンティクスを分析して変換する独自のリファクタリングを作成できました。 これであなたも正式なリファクタリングの作成者です!
