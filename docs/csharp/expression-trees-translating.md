---
title: 式ツリーの変換
description: 式ツリーの各ノードにアクセスし、その式ツリーに変更を加えたコピーを構築する方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: b453c591-acc6-4e08-8175-97e5bc65958e
ms.openlocfilehash: f60c447d5c89aa83f85073e642e621608131ed8d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76115773"
---
# <a name="translate-expression-trees"></a>式ツリーを変換する

[前回 -- 式の構築](expression-trees-building.md)

この最終セクションでは、式ツリーの各ノードにアクセスし、その式ツリーに変更を加えたコピーを構築する方法について説明します。 これらの手法は、2 つの重要なシナリオで使用されます。 1 つ目は、別の環境に変換するために、式ツリーで表現されるアルゴリズムを理解する場合です。 2 つ目は、作成したアルゴリズムを変更する場合です。 この目的として、ログ記録の追加、メソッド呼び出しの取得と追跡などがあります。

## <a name="translating-is-visiting"></a>変換はアクセスすること

式ツリーを変換するために構築するコードは、既に説明した、ツリーのすべてのノードにアクセスする式です。 式ツリーを変換するときに、すべてのノードにアクセスします。また、アクセスしながら新しいツリーを構築します。 新しいツリーには、元のノードの参照や、ツリーに配置した新しいノードが含まれる場合があります。

それでは、実際に式ツリーにアクセスし、ノードの置き換えを含む新しいツリーを作成してみましょう。 この例では、定数を 10 倍大きい定数に置き換えます。
それ以外に式ツリーの変更はありません。 定数の値を読み取って新しい定数で置き換えるのではなく、定数ノードを、乗算を実行する新しいノードで置き換えます。

ここで定数ノードを見つけたら、子が元の定数である新しい乗算ノードと定数 `10` を作成します。

```csharp
private static Expression ReplaceNodes(Expression original)
{
    if (original.NodeType == ExpressionType.Constant)
    {
        return Expression.Multiply(original, Expression.Constant(10));
    }
    else if (original.NodeType == ExpressionType.Add)
    {
        var binaryExpression = (BinaryExpression)original;
        return Expression.Add(
            ReplaceNodes(binaryExpression.Left),
            ReplaceNodes(binaryExpression.Right));
    }
    return original;
}
```

元のノードを置き換えるには、変更を含む新しいツリーを構築します。 置き換えたツリーをコンパイルし、実行して確認することができます。

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var addition = Expression.Add(one, two);
var sum = ReplaceNodes(addition);
var executableFunc = Expression.Lambda(sum);

var func = (Func<int>)executableFunc.Compile();
var answer = func();
Console.WriteLine(answer);
```

新しいツリーの構築は、既存のツリー内のノードへのアクセスと、新しいノードの作成とツリーへの挿入を組み合わせた操作です。

この例は、式ツリーが不変である重要性を示しています。 上の作成した新しいツリーには、新しく作成したノード、既存のツリーのノードが混在しています。 既存のツリーのノードは変更できないので、安全です。 また、メモリ効率も大幅に向上します。
同じノードを 1 つのツリー全体、または複数の式ツリーで使用できます。 ノードは変更できないので、必要に応じていつでも同じノードを再利用できます。

## <a name="traversing-and-executing-an-addition"></a>加算の走査と実行

加算ノードのツリーをたどり、結果を計算する 2 つ目のビジターを構築して、このことを検証してみましょう。 そのために、これまでに見てきたビジターに何点か変更を加えます。 この新しいバージョンでは、この時点までの加算演算の部分的な合計が返されます。 定数式の場合、これは単に定数式の値です。 加算式の場合、ツリーの走査が完了すると、結果は左右のオペランドの合計になります。

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var three= Expression.Constant(3, typeof(int));
var four = Expression.Constant(4, typeof(int));
var addition = Expression.Add(one, two);
var add2 = Expression.Add(three, four);
var sum = Expression.Add(addition, add2);

// Declare the delegate, so we can call it
// from itself recursively:
Func<Expression, int> aggregate = null;
// Aggregate, return constants, or the sum of the left and right operand.
// Major simplification: Assume every binary expression is an addition.
aggregate = (exp) =>
    exp.NodeType == ExpressionType.Constant ?
    (int)((ConstantExpression)exp).Value :
    aggregate(((BinaryExpression)exp).Left) + aggregate(((BinaryExpression)exp).Right);

var theSum = aggregate(sum);
Console.WriteLine(theSum);
```

長いコードですが、概念はとてもわかりやすいものです。
このコードでは、深さ優先検索で子にアクセスします。 定数ノードが検出されると、ビジターから定数値が返されます。 ビジターが両方の子にアクセスすると、それらの子のサブツリーの合計が計算されます。 加算ノードで、合計を計算できるようになります。
式ツリー内のすべてのノードがアクセスされると、合計が計算されます。 実行をトレースするには、デバッガーでサンプルを実行して実行をトレースします。

ツリーを走査して、ノードを分析し、合計を計算する方法を簡単にしてみましょう。 大量のトレース情報を含む更新版の Aggregate メソッドを次に示します。

```csharp
private static int Aggregate(Expression exp)
{
    if (exp.NodeType == ExpressionType.Constant)
    {
        var constantExp = (ConstantExpression)exp;
        Console.Error.WriteLine($"Found Constant: {constantExp.Value}");
        return (int)constantExp.Value;
    }
    else if (exp.NodeType == ExpressionType.Add)
    {
        var addExp = (BinaryExpression)exp;
        Console.Error.WriteLine("Found Addition Expression");
        Console.Error.WriteLine("Computing Left node");
        var leftOperand = Aggregate(addExp.Left);
        Console.Error.WriteLine($"Left is: {leftOperand}");
        Console.Error.WriteLine("Computing Right node");
        var rightOperand = Aggregate(addExp.Right);
        Console.Error.WriteLine($"Right is: {rightOperand}");
        var sum = leftOperand + rightOperand;
        Console.Error.WriteLine($"Computed sum: {sum}");
        return sum;
    }
    else throw new NotSupportedException("Haven't written this yet");
}
```

同じ式に対して実行すると、次の出力が生成されます。

```output
10
Found Addition Expression
Computing Left node
Found Addition Expression
Computing Left node
Found Constant: 1
Left is: 1
Computing Right node
Found Constant: 2
Right is: 2
Computed sum: 3
Left is: 3
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 3
Left is: 3
Computing Right node
Found Constant: 4
Right is: 4
Computed sum: 7
Right is: 7
Computed sum: 10
10
```

出力をトレースし、上のコードの手順を確認します。 コードがツリーをたどって合計を見つけるときに、各ノードにアクセスし、合計を計算する方法を理解できます。

次に、`sum1` が指定された式を使用して、別の実行処理を見てみましょう。

```csharp
Expression<Func<int> sum1 = () => 1 + (2 + (3 + 4));
```

この式の実行の出力を次に示します。

```output
Found Addition Expression
Computing Left node
Found Constant: 1
Left is: 1
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 2
Left is: 2
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 3
Left is: 3
Computing Right node
Found Constant: 4
Right is: 4
Computed sum: 7
Right is: 7
Computed sum: 9
Right is: 9
Computed sum: 10
10
```

最終的な答えは同じですが、ツリーの走査はまったく異なります。 ノードは別の順序で走査されます。これは、優先する演算が異なるツリーが構成されていたためです。

## <a name="learning-more"></a>詳細情報

このサンプルは、式ツリーで表されるアルゴリズムを走査し、解釈するために構築するコードのごく一部です。 式ツリーを別の言語に変換する汎用的なライブラリを構築するために必要なすべての作業の説明については、Matt Warren の[このシリーズ](https://docs.microsoft.com/archive/blogs/mattwar/linq-building-an-iqueryable-provider-series)を参照してください。 式ツリーに含まれる任意のコードを変換する方法について、詳しく説明されています。

式ツリーの真の力がおわかりいただけたでしょうか。
コードのセットを確認し、そのコードに必要な変更を加え、変更されたバージョンを実行することができます。 式ツリーは不変なので、既存のツリーのコンポーネントを使用して新しいツリーを作成できます。 その結果、変更した式ツリーの作成に必要なメモリ量が最小限に抑えられます。

[次回 -- まとめ](expression-trees-summary.md)
