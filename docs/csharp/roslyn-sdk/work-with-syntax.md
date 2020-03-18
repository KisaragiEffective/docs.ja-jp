---
title: .NET Compiler Platform SDK 構文モデルを使用する
description: この概要は、構文ノードを理解して操作するために使用する型を理解するためのものです。
ms.date: 10/15/2017
ms.custom: mvc
ms.openlocfilehash: fc1b1f5ae5ec985425c8d6aec49ef7f830ea9162
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740474"
---
# <a name="work-with-syntax"></a>構文の使用

**構文ツリー**は、コンパイラ API によって公開される基本的なデータ構造です。 これらのツリーは、ソース コードの字句および構文構造を表します。 これらは、次の 2 つの重要な目的を果たします。

1. IDE、アドイン、コード分析ツール、リファクタリングなどのツールで、ユーザーのプロジェクトのソース コードの構文構造を表示および処理できるようにします。
2. リファクタリングや IDE などのツールで、テキストを直接編集せず、自然な方法でソース コードを作成、変更、再配置できるようにします。 ツリーを作成して操作することで、ツールでソース コードを簡単に作成して再配置することができます。

## <a name="syntax-trees"></a>構文ツリー

構文ツリーは、コンパイル、コード分析、バインディング、リファクタリング、IDE 機能、コード生成に使用されるプライマリ構造です。 ソース コードは、最初に識別され、多くのよく知られている構造的な言語要素の 1 つに分類されないと、どの部分も認識されません。

構文ツリーには、3 つのキー属性があります。 1 番目の属性は、構文ツリーがすべてのソース情報を正確に保持するためのものです。 これは、構文ツリーには、ソース テキスト内で見つかったすべての情報、すべての文法的なコンストラクト、すべての構文トークン、およびその間にある他のすべてのもの (空白文字、コメント、プリプロセッサ ディレクティブを含む) が含まれることを意味します。 たとえば、ソースに記載されている各リテラルは、入力されているとおりに表されます。 構文ツリーは、プログラムが不完全または形式が正しくない場合にも、構文ツリー内のスキップされたトークンまたは見つからないトークンを表すことで、ソース コードのエラーを表します。

これにより、構文ツリーの 2 番目の属性が有効になります。 パーサーから取得された構文ツリーは、解析元の正確なテキストを生成できます。 任意の構文ノードから、そのノードで root 化されたサブツリーのテキスト表現を取得することができます。 これは、ソース テキストを構築および編集する方法として構文ツリーが使用できることを意味します。 ツリーを作成することによって、暗黙的に同じテキストを作成しており、構文ツリーを編集することで、既存のツリーへの変更から新しいツリーを作成し、効果的にテキストを編集しています。

構文ツリーの 3 番目の属性は、変更不可でスレッド セーフです。  つまり、取得後のツリーは、コードの現在の状態のスナップショットで、変化しないことを意味します。 これにより、ロックや重複を発生させずに、複数のユーザーが異なるスレッドで同時に同じ構文ツリーと対話できます。 ツリーは変更不可でツリーを直接変更することはできないため、ツリーの追加のスナップショットを作成することで構文ツリーを作成および変更するファクトリ メソッドが役立ちます。 基になるノードを再利用するという点でツリーは効果的であるため、追加のメモリをほとんど使用せずに、新しいバージョンをすばやく再構築することができます。

構文ツリーは、文字どおりのツリー データ構造で、非終端構造の要素が他の要素の親となります。 各構文ツリーは、ノード、トークン、およびトリビアで構成されます。

## <a name="syntax-nodes"></a>構文ノード

構文ノードは、構文ツリーのプライマリ要素の 1 つです。 これらのノードは、宣言、ステートメント、句、および式などの構文構造を表します。 構文ノードの各カテゴリは、<xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> から派生した別のクラスによって表されます。 一連のノード クラスは拡張できません。

すべての構文ノードは、構文ツリー内の非終端ノードで、これは他のノードとトークンを常に子として持つことを意味します。 別のノードの子として、各ノードは <xref:Microsoft.CodeAnalysis.SyntaxNode.Parent?displayProperty=nameWithType> プロパティからアクセスできる親ノードを持ちます。 ノードとツリーは変更できないため、ノードの親が変わることはありません。 ツリーのルートには Null 親があります。

各ノードには <xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes?displayProperty=nameWithType> メソッドがあり、子ノードのリストをソース テキスト内の位置に基づいて順番に返します。 このリストにはトークンは含まれていません。 各ノードには、<xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantTokens%2A>、または <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantTrivia%2A> などの子孫を調べるメソッドもあります。これらは、そのノードをルートとするサブツリー内に存在するすべてのノード、トークン、またはトリビアのリストを表しています。

さらに、構文ノードの各サブクラスは、厳密に型指定されたプロパティを通じて、まったく同じ子を公開します。 たとえば、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax> ノード クラスには二項演算子に固有の 3 つのプロパティ、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left>、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken>、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right> があります。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left> と <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right> の型は <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ExpressionSyntax> で、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken> の型は <xref:Microsoft.CodeAnalysis.SyntaxToken> です。

一部の構文ノードには、省略可能な子があります。 たとえば、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.IfStatementSyntax> には省略可能な <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ElseClauseSyntax> があります。 子が存在しない場合、プロパティは null を返します。

## <a name="syntax-tokens"></a>構文トークン

構文トークンとは、コードの最小の構文フラグメントを表す、言語文法の終端です。 これらが他のノードやトークンの親になることはありません。 構文トークンは、キーワード、識別子、リテラル、および句読点で構成されます。

効率を高めるため、<xref:Microsoft.CodeAnalysis.SyntaxToken> 型は CLR 値型になっています。 そのため、構文ノードとは異なり、すべての種類のトークンに対して 1 つの構造しかなく、プロパティを組み合わせることで、表現されているトークンの種類に応じて意味を持たせます。

たとえば、整数リテラル トークンは、数値を表します。 リテラル トークンには、トークンの範囲となる未加工のソース テキストに加え、正確にデコードされた整数値を示す <xref:Microsoft.CodeAnalysis.SyntaxToken.Value> プロパティがあります。 このプロパティは、多くのプリミティブ型の 1 つである場合があるため、<xref:System.Object> として型指定されます。

<xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> プロパティは <xref:Microsoft.CodeAnalysis.SyntaxToken.Value> プロパティと同じ情報を提供しますが、このプロパティは常に <xref:System.String> として型指定されます。 C# ソース テキスト内の識別子には、Unicode のエスケープ文字を含めることができますが、エスケープ シーケンスの構文自体は、識別子名の一部と見なされません。 そのため、トークンの範囲である未加工のテキストにはエスケープ シーケンスが含まれますが、<xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> プロパティには含まれません。 代わりに、エスケープで識別される Unicode 文字が含まれます。 たとえば、ソース テキストに `\u03C0` として記述された識別子が含まれている場合、このトークンの <xref:Microsoft.CodeAnalysis.SyntaxToken.ValueText> プロパティは `π` を返します。

## <a name="syntax-trivia"></a>構文トリビア

構文トリビアは、空白、コメント、およびプリプロセッサ ディレクティブなど、コードを通常に理解するためにはさほど重要ではないソース テキストの部分を表します。 構文トークンと同じく、トリビアも値型です。 トリビアのすべての種類を記述するために 1 つの <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> 型が使用されます。

トリビアは通常の言語構文の一部ではなく、任意の 2 つのトークンの間の任意の場所に表示できるため、トリビアはノードの子として構文ツリーには含まれません。 しかしトリビアは、リファクタリングなどの機能を実装するときや、ソース テキストへの忠実性を維持するには重要なため、構文ツリーの一部として存在します。

トークンの <xref:Microsoft.CodeAnalysis.SyntaxToken.LeadingTrivia?displayProperty=nameWithType> または <xref:Microsoft.CodeAnalysis.SyntaxToken.TrailingTrivia?displayProperty=nameWithType> コレクションを調べることで、トリビアにアクセスすることができます。 ソース テキストが解析される時に、トリビアのシーケンスがトークンに関連付けられます。 一般に、その後トークンは任意のトリビアを次のトークンまで同じ行で所有します。 その行以降のすべてのトリビアは、後続のトークンに関連付けられます。 ソース ファイル内の最初のトークンがすべての初期トリビアを取得し、ファイル内のトリビアの最後のシーケンスが EOF トークンに付加されます。それ以外の場合は幅が 0 になります。

構文ノードやトークンとは異なり、構文トリビアには親がありません。 しかし、構文トリビアはツリーの一部で、それぞれが 1 つのトークンに関連付けられているため、<xref:Microsoft.CodeAnalysis.SyntaxTrivia.Token?displayProperty=nameWithType> プロパティを使用して、トリビアが関連付けられているトークンにアクセスすることができます。

## <a name="spans"></a>範囲

ノード、トークン、またはトリビアはそれぞれ、ソース テキスト内の各自の位置と構成文字数を把握しています。 テキストの位置は、0 から始まる `char` インデックスの 32 ビット整数値として表されます。 <xref:Microsoft.CodeAnalysis.Text.TextSpan> オブジェクトは開始位置と文字数で、どちらも整数として表されます。 <xref:Microsoft.CodeAnalysis.Text.TextSpan> の長さが 0 の場合、2 つの文字の間の場所を参照します。

各ノードには、<xref:Microsoft.CodeAnalysis.Text.TextSpan>、<xref:Microsoft.CodeAnalysis.SyntaxNode.Span%2A> という 2 つの <xref:Microsoft.CodeAnalysis.SyntaxNode.FullSpan%2A> プロパティが含まれます。

<xref:Microsoft.CodeAnalysis.SyntaxNode.Span%2A> プロパティは、ノードのサブツリー内の最初のトークンの先頭から最後のトークンの末尾までのテキスト範囲です。 この範囲には、先頭または末尾のトリビアはいずれも含まれません。

<xref:Microsoft.CodeAnalysis.SyntaxNode.FullSpan%2A> プロパティは、ノードの通常の範囲に加え、任意の先頭または末尾のトリビアの範囲を含むテキスト範囲です。

次に例を示します。

``` csharp
      if (x > 3)
      {
||        // this is bad
          |throw new Exception("Not right.");|  // better exception?||
      }
```

ブロック内のステートメント ノードには、1 つの縦棒 (|) によって示される範囲があります。 これには文字 `throw new Exception("Not right.");` が含まれています。 完全な範囲は、二重の縦棒 (||) によって示されます。 これには、範囲と同じ文字と先頭および末尾のトリビアに関連付けられている文字が含まれます。

## <a name="kinds"></a>種類

ノード、トークン、またはトリビアにはそれぞれ、表される正確な構文要素を識別する <xref:Microsoft.CodeAnalysis.SyntaxNode.RawKind?displayProperty=nameWithType> 型の <xref:System.Int32?displayProperty=nameWithType> プロパティがあります。 この値は、言語固有の列挙型にキャストすることができます。 C# または Visual Basic の各言語には、文法で可能なすべてのノード、トークン、およびトリビア要素を一覧表示する、1 つの `SyntaxKind` 列挙型 (それぞれ <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind?displayProperty=nameWithType> と <xref:Microsoft.CodeAnalysis.VisualBasic.SyntaxKind?displayProperty=nameWithType>) があります。 この変換は、<xref:Microsoft.CodeAnalysis.CSharp.CSharpExtensions.Kind%2A?displayProperty=nameWithType> または <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicExtensions.Kind%2A?displayProperty=nameWithType> の拡張メソッドにアクセスすることで自動的に行われます。

<xref:Microsoft.CodeAnalysis.SyntaxToken.RawKind> プロパティは、同じノード クラスを共有する構文ノード型の簡単なあいまいさ排除を可能にします。 トークンとトリビアでは、このプロパティは要素の型を区別するための唯一の方法です。

たとえば、1 つの <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax> クラスに、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Left>、<xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.OperatorToken>、および <xref:Microsoft.CodeAnalysis.CSharp.Syntax.BinaryExpressionSyntax.Right> が子としてあるとします。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpExtensions.Kind%2A> プロパティは、構文ノードの種類が <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.AddExpression>、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.SubtractExpression>、または <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.MultiplyExpression> であるかどうかを識別します。

## <a name="errors"></a>エラー

ソース テキストに構文エラーが含まれている場合でも、ソースへのラウンドトリップが可能な完全な構文ツリーが公開されます。 パーサーは、定義されている言語の構文に準拠しないコードを検出すると、次の 2 つのいずれかの手法を使って構文ツリーを作成します。

1 つ目は、パーサーが特定の種類のトークンを想定していたがそれが見つからない場合、その見つからないトークンを構文ツリーの想定されていた場所に挿入します。 見つからないトークンは、想定されていた実際のトークンを表しますが、範囲は空で、その <xref:Microsoft.CodeAnalysis.SyntaxNode.IsMissing?displayProperty=nameWithType> プロパティは `true` を返します。

2 つ目は、パーサーは解析を続行できるトークンが見つかるまでトークンをスキップします。 この場合、スキップされたトークンは、<xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.SkippedTokensTrivia> の種類を持つトリビア ノードとしてアタッチされます。
