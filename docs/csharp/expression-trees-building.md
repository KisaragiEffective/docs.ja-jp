---
title: 式ツリーの構築
description: 式ツリーを構築するためのテクニックについて説明します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 542754a9-7f40-4293-b299-b9f80241902c
ms.openlocfilehash: c93eb16ebf2ff66dc0162afb6841f2cadfce174e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146048"
---
# <a name="building-expression-trees"></a>式ツリーの構築

[前回 -- 式の解釈](expression-trees-interpreting.md)

これまでに説明した式ツリーは、いずれも C# コンパイラで作成したものです。 `Expression<Func<T>>` や他の同様の型として型指定された変数に割り当てるラムダ式を作成するだけの手順でしたが、 式ツリーを作成する方法は他にもあります。 実行時にメモリ内で式を構築する必要があるというシナリオはよくあります。

式ツリーは不変なので、式ツリーの構築は複雑です。 不変とは、リーフからルートにいたるまでツリーを構築する必要があることを意味します。 式ツリーの構築に使用する API がこれを反映しています。ノードの構築に使用するメソッドは、そのすべての子を引数として取得します。 いくつかの例を挙げながら手法を説明します。

## <a name="creating-nodes"></a>ノードの作成

比較的単純な例から始めましょう。 これまでに使用してきた加算式を使用します。

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

式ツリーを構築するには、リーフ ノードを構築する必要があります。
リーフ ノードは定数なので、`Expression.Constant` メソッドを使用してノードを作成できます。

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
```

次に、加算式を構築します。

```csharp
var addition = Expression.Add(one, two);
```

加算式を構築したら、ラムダ式を作成できます。

```csharp
var lambda = Expression.Lambda(addition);
```

引数がないので、これはとても単純なラムダ式です。
このセクションの後半では、引数をパラメーターにマップし、より複雑な式を構築する方法について説明します。

この例のように単純な式の場合、すべての呼び出しを 1 つのステートメントにまとめることもできます。

```csharp
var lambda = Expression.Lambda(
    Expression.Add(
        Expression.Constant(1, typeof(int)),
        Expression.Constant(2, typeof(int))
    )
);
```

## <a name="building-a-tree"></a>ツリーの構築

メモリ内に式ツリーを構築する基本的な方法です。 一般的に、複雑なツリーの場合はノードの種類が多く、ツリー内のノード数も多くなります。 もう 1 つの例を使って、式ツリーを作成するときに一般的に構築する 2 つのノードについて説明します。引数ノードとメソッド呼び出しノードです。

式ツリーを構築して次の式を作成しましょう。

```csharp
Expression<Func<double, double, double>> distanceCalc =
    (x, y) => Math.Sqrt(x * x + y * y);
```

まず `x` と `y` のパラメーター式を作成します。

```csharp
var xParameter = Expression.Parameter(typeof(double), "x");
var yParameter = Expression.Parameter(typeof(double), "y");
```

乗算式と加算式の作成方法は、これまでに説明したパターンどおりです。

```csharp
var xSquared = Expression.Multiply(xParameter, xParameter);
var ySquared = Expression.Multiply(yParameter, yParameter);
var sum = Expression.Add(xSquared, ySquared);
```

次に、`Math.Sqrt` の呼び出しのために、メソッド呼び出し式を作成する必要があります。

```csharp
var sqrtMethod = typeof(Math).GetMethod("Sqrt", new[] { typeof(double) });
var distance = Expression.Call(sqrtMethod, sum);
```

最後に、メソッド呼び出しをラムダ式に組み込み、忘れずにラムダ式の引数を定義します。

```csharp
var distanceLambda = Expression.Lambda(
    distance,
    xParameter,
    yParameter);
```

このやや複雑な例では、式ツリーの作成に必要になることが多い手法がいくつか見られます。

まず、パラメーターまたはローカル変数を表すオブジェクトは、使用前に作成しておく必要があります。 オブジェクトの作成後は、式ツリーのどこでも必要な場所で使用できます。

次に、`MethodInfo` オブジェクトを作成するには、リフレクション API の一部を使用する必要があります。これは、そのメソッドにアクセスする式ツリーを作成できるようにするためです。 使用するリフレクション API は、.NET Core プラットフォームで使用できるものに限定する必要があります。 繰り返しになりますが、こうした手法は他の式ツリーにも応用されます。

## <a name="building-code-in-depth"></a>コードの構築の詳細

このような API を使用して構築できるものに制限はありませんが、 構築する式ツリーが複雑になるほど、コードの管理と読み取りが困難になります。

このコードと同じ式ツリーを構築してみましょう。

```csharp
Func<int, int> factorialFunc = (n) =>
{
    var res = 1;
    while (n > 1)
    {
        res = res * n;
        n--;
    }
    return res;
};
```

上の例では、式ツリーを構築せず、デリゲートのみを構築しました。 `Expression` クラスを使用して、ステートメント形式のラムダを構築することはできません。 同じ機能を構築するために必要なコード例を次に示します。 この例が複雑なのは、`while` ループを構築する API がなく、条件テストを含むループと、ループを抜けるためのラベル ターゲットを構築する必要があるためです。

```csharp
var nArgument = Expression.Parameter(typeof(int), "n");
var result = Expression.Variable(typeof(int), "result");

// Creating a label that represents the return value
LabelTarget label = Expression.Label(typeof(int));

var initializeResult = Expression.Assign(result, Expression.Constant(1));

// This is the inner block that performs the multiplication,
// and decrements the value of 'n'
var block = Expression.Block(
    Expression.Assign(result,
        Expression.Multiply(result, nArgument)),
    Expression.PostDecrementAssign(nArgument)
);

// Creating a method body.
BlockExpression body = Expression.Block(
    new[] { result },
    initializeResult,
    Expression.Loop(
        Expression.IfThenElse(
            Expression.GreaterThan(nArgument, Expression.Constant(1)),
            block,
            Expression.Break(label, result)
        ),
        label
    )
);
```

階乗関数の式ツリーを構築するコードは、かなり長く複雑になります。また、通常のコーディング作業では避けたいラベルや break ステートメントなどの要素が多くなってしまいます。

このセクションでは、この式ツリーのすべてのノードにアクセスするビジター コードも更新し、このサンプルで作成したノードに関する情報を書き出しました。 GitHub の dotnet/docs レポジトリで、[サンプル コードを表示またはダウンロード](https://github.com/dotnet/samples/tree/master/csharp/expression-trees)することができます。 自分でサンプルをビルドし、実行してみてください。 ダウンロード方法については、「[サンプルおよびチュートリアル](../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

## <a name="examining-the-apis"></a>API の確認

.NET Core では、式ツリーの API をたどることがやや難しくはありますが、問題ありません。 その目的は、実行時にコードを生成するコードを作成するという、かなり複雑な作業だからです。 C# 言語で使用できるすべてのコントロール構造体をサポートしながら、API のアクセス領域をできるだけ少なくするバランスを取るため、必然的に複雑な作業になります。 このバランスとは、多くのコントロール構造体は、C# コンストラクトではなく、上位のコンストラクトからコンパイラが生成する基のロジックを示すコンストラクトで表されることを意味します。

また、現時点では、`Expression` クラス メソッドを使用して直接構築できない C# 式があります。 一般的に、C# 5 と C# 6 で追加された最新の演算子と式です (たとえば、`async` は構築できず、新しい `?.` 演算子は直接作成できません)。

[次回 -- 式の変換](expression-trees-translating.md)
