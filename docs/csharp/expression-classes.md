---
title: 式ツリーをサポートするフレームワークの型
description: 式ツリーをサポートするフレームワークの型、式ツリーの作成、式ツリー API の操作テクニックについて説明します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: e9c85021-0d36-48af-91b7-aaaa66f22654
ms.openlocfilehash: 8483c46dde3ea97138e55ab84a5924a3d2578730
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146087"
---
# <a name="framework-types-supporting-expression-trees"></a>式ツリーをサポートするフレームワークの型

[前へ -- 式ツリーの説明](expression-trees-explained.md)

式ツリーを使用する .NET Core framework には、多くのクラスがあります。
クラスの全リストは <xref:System.Linq.Expressions> で確認できます。
ここでは、リストのすべてを説明するのではなく、フレームワークのクラスがどう設計されているかを把握します。

言語設計の観点から言えば、式は、評価して値を返すコードの本体です。 式はごく単純な場合があります。定数式 `1` は定数値 1 を返します。 式が複雑になる場合もあります。式 `(-B + Math.Sqrt(B*B - 4 * A * C)) / (2 * A)` は二次方程式の 1 つの解を返します (式に解がある場合)。  

## <a name="it-all-starts-with-systemlinqexpression"></a>すべては System.Linq.Expression から始まる

式ツリーの使用が複雑になる理由の 1 つは、プログラムの多くの場所でさまざまな種類の式が有効になることです。 代入式を考えてみます。 代入式の右側には、定数値、変数、メソッドの呼び出し式、その他を含めることができます。 言語に柔軟性があるため、式ツリーをたどっていくと、ツリーのノードのあらゆる場所でさまざまな式の型が使用されていることに気づくはずです。 そのため、基本の式の型を使用して作業できるのであれば、それが最も簡単な操作方法になります。 しかし、場合によっては、それ以上の知識が必要です。
このために、基本の式クラスに `NodeType` プロパティが含まれています。
このプロパティは、あり得る式の型の列挙型である `ExpressionType` を返します。
ノードの型がわかれば、その型にキャストし、式ノードの型を識別する特定のアクションを実行できます。 特定の型のノードを検索し、その種類の式が持つ特定のプロパティを使用できます。

たとえば、次のコードでは変数アクセス式の変数名を出力します。 ここでは、ノードの型を確認し、次に変数アクセス式にキャストして、特定の式の型のプロパティを確認するという手順で進めています。

```csharp
Expression<Func<int, int>> addFive = (num) => num + 5;

if (addFive.NodeType == ExpressionType.Lambda)
{
    var lambdaExp = (LambdaExpression)addFive;

    var parameter = lambdaExp.Parameters.First();

    Console.WriteLine(parameter.Name);
    Console.WriteLine(parameter.Type);
}
```

## <a name="creating-expression-trees"></a>式ツリーの作成

`System.Linq.Expression` クラスには、式を作成する静的メソッドも数多く含まれています。 これらのメソッドは、その子に指定された引数を使用して式ノードを作成します。 このようにしてリーフ ノードから式を作成します。 たとえば、次のコードは Add 式を作成します。

```csharp
// Addition is an add expression for "1 + 2"
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var addition = Expression.Add(one, two);
```

この簡単な例からわかるように、さまざまな型が式ツリーの作成と操作に関わっています。 C# 言語が備える豊富な語彙が提供する機能を発揮するには、こうした複雑さが必要になります。

## <a name="navigating-the-apis"></a>API の操作
ほぼすべての C# 言語の構文要素に対してマップされる式ノード型がそれぞれ存在します。 各型には、その型の言語要素に特有のメソッドがあります。 一度に覚えておくべきことがたくさんあります。 ここではすべてを記憶しようとするのではなく、式ツリーを操作するときにふだん使うテクニックを紹介します。

1. `ExpressionType` 列挙型のメンバーを調べて、検証するノードを特定します。 式ツリーをたどって理解するときに、この方法が実に役立ちます。
2. `Expression` クラスの静的メンバーを調べて、式を作成します。 これらのメソッドでは、その子ノードから任意の式の型を作成できます。
3. `ExpressionVisitor` クラスを調べて、変更された式ツリーを作成します。

これら 3 つの部分をそれぞれ調べれば、さらに多くのことがわかります。 これら 3 つのステップのいずれかを実行すれば、必要なことが必ず見つかります。

 [次へ -- 式ツリーの実行](expression-trees-execution.md)
