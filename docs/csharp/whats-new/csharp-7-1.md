---
title: C# 7.1 の新機能
description: C# 7.1 の新機能の概要。
ms.date: 04/09/2019
ms.openlocfilehash: 5d2d6f51b6422f5b4db5c6bd275b5ffce1f695f8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398353"
---
# <a name="whats-new-in-c-71"></a>C# 7.1 の新機能

C# 7.1 は C# 言語の最初のポイント リリースです。 この言語のリリース サイクルが増えたことになります。 新機能は間もなく使用できます。理論的には、各々の新機能が用意できたときに使用できます。 C# 7.1 では、特定のバージョンの言語に合わせるようにコンパイラを構成する機能が追加されます。 ツールをアップグレードする決定と言語バージョンをアップグレードする決定を分けることができます。

C# 7.1 では、[言語バージョン選択](../language-reference/configure-language-version.md)構成要素、3 つの新しい言語機能、新しいコンパイラ動作が追加されます。

このリリースの新しい言語機能は次のとおりです。

- [`async` `Main`メソッド](#async-main)
  - アプリケーションのエントリ ポイントに `async` 修飾子を設定できます。
- [`default` リテラル式](#default-literal-expressions)
  - ターゲットの種類を推論できるとき、既定の値式で既定のリテラル式を使用できます。
- [推論されたタプル要素の名前](#inferred-tuple-element-names)
  - タプル要素の名前は、多くの場合、タプル初期化から推論できます。
- [ジェネリック型パラメーターのパターン マッチ](#pattern-matching-on-generic-type-parameters)
  - 型がジェネリック型パラメーターである変数にパターン マッチ式を使用できます。

最後に、コンパイラには、`-refout`参照アセンブリ生成`-refonly`を制御する 2 つのオプション、[ と ](#reference-assembly-generation) があります。

ポイント リリースで最新の機能を使用するには、[コンパイラ言語バージョンを構成](../language-reference/configure-language-version.md)し、バージョンを選択する必要があります。

この記事の残りでは、各機能の概要について説明します。 機能ごとに、その背後にある論拠のほか、 構文についても説明します。 `dotnet try` グローバル ツールを使って、これらの機能をご自身の環境で調べることができます。

1. [dotnet try](https://github.com/dotnet/try/blob/master/README.md#setup) グローバル ツールをインストールします。
1. [dotnet/try-samples](https://github.com/dotnet/try-samples) リポジトリを複製します。
1. 現在のディレクトリを、*try-samples* リポジトリの *csharp7* サブディレクトリに設定します。
1. `dotnet try` を実行します。

## <a name="async-main"></a>async main

*async main* メソッドにより、`await` メソッドで `Main` を使用できます。
以前は次のように記述する必要がありました。

```csharp
static int Main()
{
    return DoAsyncWork().GetAwaiter().GetResult();
}
```

それが次のように記述できるようになりました。

```csharp
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```

プログラムによって終了コードが返されない場合、`Main` を返す <xref:System.Threading.Tasks.Task> メソッドを宣言できます。

```csharp
static async Task Main()
{
    await SomeAsyncMethod();
}
```

プログラミング ガイドの [async main](../programming-guide/main-and-command-args/index.md) の記事に詳細があります。

## <a name="default-literal-expressions"></a>既定のリテラル式

既定のリテラル式は既定の値式の拡張版です。
これらの式によって変数が初期化され、既定値になります。 以前は次のように記述していました。

```csharp
Func<string, bool> whereClause = default(Func<string, bool>);
```

それが今では、初期化の右項で種類を省略できるようになりました。

```csharp
Func<string, bool> whereClause = default;
```

詳しくは、「[default 演算子](../language-reference/operators/default.md#default-literal)」記事の「[default リテラル](../language-reference/operators/default.md)」セクションをご覧ください。

## <a name="inferred-tuple-element-names"></a>推論されたタプル要素の名前

この機能は、C# 7.0 で導入されたタプル機能の小規模拡張です。 タプルを何度初期化しても、代入の右項に使用される変数は、タプル要素に使用するものと同じ名前になります。

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count: count, label: label);
```

タプル要素の名前は、C# 7.1 でタプルの初期化に使用される変数から推測できます。

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

この機能の詳細については、[タプル](../tuples.md)に関する記事を参照してください。

## <a name="pattern-matching-on-generic-type-parameters"></a>ジェネリック型パラメーターのパターン マッチ

C#7.1 以降では、`is` 型パターンと `switch` 型パターンのパターン式は、ジェネリック型パラメーターの型を持つことができます。 これは `struct` 型か `class` 型のいずれかである可能性がある型を確認し、ボックス化を回避するときに最も役立ちます。

## <a name="reference-assembly-generation"></a>参照アセンブリ生成

"*参照専用アセンブリ*" を生成する新しい 2 つのコンパイラ オプション [-refout](../language-reference/compiler-options/refout-compiler-option.md) と [-refonly](../language-reference/compiler-options/refonly-compiler-option.md) があります。
リンク先の記事には、オプションと参照アセンブリに関する詳細があります。
