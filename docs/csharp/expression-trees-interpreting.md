---
title: 式の解釈
description: 式ツリーの構造を調べるためのコードの記述方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: adf73dde-1e52-4df3-9929-2e0670e28e16
ms.openlocfilehash: 1283d7d957c72558652b96cb428efd0f071f0184
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146009"
---
# <a name="interpreting-expressions"></a>式の解釈

[前へ -- 式の実行](expression-trees-execution.md)

ここでは、*式ツリー*の構造を調べるコードを作成します。 式ツリー内のすべてのノードは、`Expression` から派生したクラスのオブジェクトになります。

そうした設計上、式ツリーのすべてのノードへのアクセスには、比較的単純な再帰的操作を使用します。 一般的な方法では、はじめにルート ノードの種類を確認します。

ノード型に子がある場合は、再帰的に子にアクセスします。 それぞれの子ノードで、ルート ノードで使用したプロセスを繰り返します。つまり、種類を確認し、型に子がある場合は、それぞれの子にアクセスします。

## <a name="examining-an-expression-with-no-children"></a>子を持たない式の確認
ここでは、非常に単純な式ツリーの各ノードにアクセスします。
次に示すのは、定数式を作成し、次にそのプロパティを調べるコードです。

```csharp
var constant = Expression.Constant(24, typeof(int));

Console.WriteLine($"This is a/an {constant.NodeType} expression type");
Console.WriteLine($"The type of the constant value is {constant.Type}");
Console.WriteLine($"The value of the constant value is {constant.Value}");
```

このコードの出力は、次のようになります。

```output
This is an Constant expression type
The type of the constant value is System.Int32
The value of the constant value is 24
```

では、この式を調べ、式に関する重要なプロパティを書き込むコードを作成します。 次に示すのがそのコードです。

## <a name="examining-a-simple-addition-expression"></a>単純な加算式の確認

このセクションの導入として、加算のサンプルから始めます。

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

> この式ツリーを宣言するために `var` を使用していません。それは代入の右側は暗黙的に型指定されているためです。 このことを詳しく理解するには、[ここ](implicitly-typed-lambda-expressions.md)をお読みください。

ルート ノードは `LambdaExpression` です。 `=>` 演算子の右側で関心があるコードを取得するため、`LambdaExpression` の子のいずれかを検索する必要があります。 このセクションのすべての式について、この操作を行います。 親ノードは `LambdaExpression` の戻り値の型を検索するのに役立ちます。

この式の各ノードを調べるには、いくつかのノードに再帰的にアクセスする必要があります。 次に示すのは、最初の単純な実装です。

```csharp
Expression<Func<int, int, int>> addition = (a, b) => a + b;

Console.WriteLine($"This expression is a {addition.NodeType} expression type");
Console.WriteLine($"The name of the lambda is {((addition.Name == null) ? "<null>" : addition.Name)}");
Console.WriteLine($"The return type is {addition.ReturnType.ToString()}");
Console.WriteLine($"The expression has {addition.Parameters.Count} arguments. They are:");
foreach(var argumentExpression in addition.Parameters)
{
    Console.WriteLine($"\tParameter Type: {argumentExpression.Type.ToString()}, Name: {argumentExpression.Name}");
}

var additionBody = (BinaryExpression)addition.Body;
Console.WriteLine($"The body is a {additionBody.NodeType} expression");
Console.WriteLine($"The left side is a {additionBody.Left.NodeType} expression");
var left = (ParameterExpression)additionBody.Left;
Console.WriteLine($"\tParameter Type: {left.Type.ToString()}, Name: {left.Name}");
Console.WriteLine($"The right side is a {additionBody.Right.NodeType} expression");
var right= (ParameterExpression)additionBody.Right;
Console.WriteLine($"\tParameter Type: {right.Type.ToString()}, Name: {right.Name}");
```

このサンプル コードから、次のような出力が得られます。

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 arguments. They are:
        Parameter Type: System.Int32, Name: a
        Parameter Type: System.Int32, Name: b
The body is a/an Add expression
The left side is a Parameter expression
        Parameter Type: System.Int32, Name: a
The right side is a Parameter expression
        Parameter Type: System.Int32, Name: b
```

上記のサンプル コードには、繰り返しが多数あることがわかります。
ここで、繰り返しを整理して、より汎用的な式ノード ビジターを作成します。 それには、再帰的なアルゴリズムを記述する必要があります。 どのノードにも子を持つ型である可能性があります。
子を持つすべてのノードで、その子にアクセスし、ノードの種類を確認することが必要です。 次に示す整理したバージョンでは、加算操作にアクセスするために再帰を使用しています。

```csharp
// Base Visitor class:
public abstract class Visitor
{
    private readonly Expression node;

    protected Visitor(Expression node)
    {
        this.node = node;
    }

    public abstract void Visit(string prefix);

    public ExpressionType NodeType => this.node.NodeType;
    public static Visitor CreateFromExpression(Expression node)
    {
        switch(node.NodeType)
        {
            case ExpressionType.Constant:
                return new ConstantVisitor((ConstantExpression)node);
            case ExpressionType.Lambda:
                return new LambdaVisitor((LambdaExpression)node);
            case ExpressionType.Parameter:
                return new ParameterVisitor((ParameterExpression)node);
            case ExpressionType.Add:
                return new BinaryVisitor((BinaryExpression)node);
            default:
                Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
                return default(Visitor);
        }
    }
}

// Lambda Visitor
public class LambdaVisitor : Visitor
{
    private readonly LambdaExpression node;
    public LambdaVisitor(LambdaExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression type");
        Console.WriteLine($"{prefix}The name of the lambda is {((node.Name == null) ? "<null>" : node.Name)}");
        Console.WriteLine($"{prefix}The return type is {node.ReturnType.ToString()}");
        Console.WriteLine($"{prefix}The expression has {node.Parameters.Count} argument(s). They are:");
        // Visit each parameter:
        foreach (var argumentExpression in node.Parameters)
        {
            var argumentVisitor = Visitor.CreateFromExpression(argumentExpression);
            argumentVisitor.Visit(prefix + "\t");
        }
        Console.WriteLine($"{prefix}The expression body is:");
        // Visit the body:
        var bodyVisitor = Visitor.CreateFromExpression(node.Body);
        bodyVisitor.Visit(prefix + "\t");
    }
}

// Binary Expression Visitor:
public class BinaryVisitor : Visitor
{
    private readonly BinaryExpression node;
    public BinaryVisitor(BinaryExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This binary expression is a {NodeType} expression");
        var left = Visitor.CreateFromExpression(node.Left);
        Console.WriteLine($"{prefix}The Left argument is:");
        left.Visit(prefix + "\t");
        var right = Visitor.CreateFromExpression(node.Right);
        Console.WriteLine($"{prefix}The Right argument is:");
        right.Visit(prefix + "\t");
    }
}

// Parameter visitor:
public class ParameterVisitor : Visitor
{
    private readonly ParameterExpression node;
    public ParameterVisitor(ParameterExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}Type: {node.Type.ToString()}, Name: {node.Name}, ByRef: {node.IsByRef}");
    }
}

// Constant visitor:
public class ConstantVisitor : Visitor
{
    private readonly ConstantExpression node;
    public ConstantVisitor(ConstantExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}The type of the constant value is {node.Type}");
        Console.WriteLine($"{prefix}The value of the constant value is {node.Value}");
    }
}
```

このアルゴリズムは、任意の `LambdaExpression` にアクセスするアルゴリズムの基礎となります。 このコードには穴が多数あります。つまり、ここで作成したコードは、検出する可能性がある一連の式ツリーのノードのうち、ごく一部のサンプルしか探索しません。 それでも、このコードからかなりのことを学ぶことができます。 (`Visitor.CreateFromExpression` メソッドの default case では、新しいノード型が検出されると、エラー コンソールにメッセージが出力されます。 これによって、新しい式の型を追加する必要があることがわかります。)

上記の加算式でこのビジターを実行すると、次のような出力が得られます。

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: b, ByRef: False
```

こうして、より汎用的なビジターを実装すると、多種多様な式の型にアクセスして処理できるようになります。

## <a name="examining-an-addition-expression-with-many-levels"></a>さまざまなレベルの加算式の確認

次に、もっと複雑な例を取り上げます。ただし、ノード型は引き続き加算だけにとどめます。

```csharp
Expression<Func<int>> sum = () => 1 + 2 + 3 + 4;
```

これをビジター アルゴリズムで実行する前に、出力がどうなるか、思考訓練をしてください。 `+` 演算子は*二項演算子*です。つまり、左側オペランドと右側オペランドを表す 2 つの子を持っている必要があります。 正しいと考えられるツリーを構築する方法はいくつかあります。

```csharp
Expression<Func<int>> sum1 = () => 1 + (2 + (3 + 4));
Expression<Func<int>> sum2 = () => ((1 + 2) + 3) + 4;

Expression<Func<int>> sum3 = () => (1 + 2) + (3 + 4);
Expression<Func<int>> sum4 = () => 1 + ((2 + 3) + 4);
Expression<Func<int>> sum5 = () => (1 + (2 + 3)) + 4;
```

可能性のある答えを 2 つに分けて考えれば、最も見込みがある答えに着目できます。 1 つ目は、*結合規則が右から左*の式です。 2 つ目は、*結合規則が左から右*の式です。
これら 2 つの書式の利点は、任意の数の加算式に拡張できる点です。

この式をビジターで実行すると、表示された出力から、単純な加算式では*結合関係が左から右*に評価されることを検証できます。

このサンプルを実行して、完全な式ツリーを表示するために、ソースの式ツリーに 1 つ変更を加える必要がありました。 式ツリーに含まれるのがすべて定数である場合、結果のツリーに含まれるのは定数値の `10` だけです。 コンパイラはすべての加算を実行し、式を最も単純な形式に変換します。 元のツリーを確認するには、式に 1 つの変数を追加するだけで十分です。

```csharp
Expression<Func<int, int>> sum = (a) => 1 + a + 3 + 4;
```

これを合計するビジターを作成し、実行すれば、次の出力が得られます。

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This binary expression is a Add expression
                        The Left argument is:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                        The Right argument is:
                                This is an Parameter expression type
                                Type: System.Int32, Name: a, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
        The Right argument is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 4
```

このビジター コードを使用して、他のサンプルを実行すれば、そのコードが表わすツリーを確認できます。 次に示す例は、上記の `sum3` 式です (コンパイラが定数を計算するのを防ぐためにパラメーターを追加しています)。

```csharp
Expression<Func<int, int, int>> sum3 = (a, b) => (1 + a) + (3 + b);
```

ビジターからの出力は、次のようになります。

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 1
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: b, ByRef: False
```

かっこは出力の一部ではないことに注意してください。 入力式のかっこを表すノードは、式ツリーの中にありません。 式ツリーの構造体には、優先順位を伝達するために必要なすべての情報が含まれています。

## <a name="extending-from-this-sample"></a>このサンプルの拡張

このサンプルは、最も基本的な式ツリーのみを処理します。 このセクションに示されるコードでは、整数定数と二項演算子 `+` のみを扱います。 次に、最後のサンプルとして、より複雑な式を扱うビジターに更新します。 次の式に対応させることにします。

```csharp
Expression<Func<int, int>> factorial = (n) =>
    n == 0 ?
    1 :
    Enumerable.Range(1, n).Aggregate((product, factor) => product * factor);
```

このコードは、数学の*階乗*関数に対して考えられる実装の 1 つです。 このコードの記述方法によって、式にラムダ式を代入して式ツリーを構築する場合の 2 つの制限事項が強調されています。 第 1 に、ステートメント形式のラムダは使用できません。 つまり、C# で一般的なループ、ブロック、if / else ステートメント、その他の制御構造を使用できないということです。 式の使用だけに制限されます。 第 2 に、同じ式を再帰的に呼び出すことができません。
式が既にデリゲートになっている場合は呼び出せますが、式ツリー形式で呼び出すことはできません。 これらの制限を克服する手法については、「[式ツリーの構築](expression-trees-building.md)」セクションを参照してください。

この式では、次の型のノードすべてが使用されています。

1. 等号 (二項式)
2. 乗算 (二項式)
3. 条件付き (? : 式)
4. メソッド呼び出し式 (`Range()` と `Aggregate()` を呼び出す)

ビジター アルゴリズムを修正する方法の 1 つは、アルゴリズムの実行を繰り返し、`default` 句に行き着くたびにノード型を記述することです。 数回繰り返せば、検出の可能性がある各ノードを確認できます。 その段階で、必要なものがすべて揃います。 結果は次のようになります。

```csharp
public static Visitor CreateFromExpression(Expression node)
{
    switch(node.NodeType)
    {
        case ExpressionType.Constant:
            return new ConstantVisitor((ConstantExpression)node);
        case ExpressionType.Lambda:
            return new LambdaVisitor((LambdaExpression)node);
        case ExpressionType.Parameter:
            return new ParameterVisitor((ParameterExpression)node);
        case ExpressionType.Add:
        case ExpressionType.Equal:
        case ExpressionType.Multiply:
            return new BinaryVisitor((BinaryExpression)node);
        case ExpressionType.Conditional:
            return new ConditionalVisitor((ConditionalExpression)node);
        case ExpressionType.Call:
            return new MethodCallVisitor((MethodCallExpression)node);
        default:
            Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
            return default(Visitor);
    }
}
```

ConditionalVisitor と MethodCallVisitor がこれら 2 つのノードを処理します。

```csharp
public class ConditionalVisitor : Visitor
{
    private readonly ConditionalExpression node;
    public ConditionalVisitor(ConditionalExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        var testVisitor = Visitor.CreateFromExpression(node.Test);
        Console.WriteLine($"{prefix}The Test for this expression is:");
        testVisitor.Visit(prefix + "\t");
        var trueVisitor = Visitor.CreateFromExpression(node.IfTrue);
        Console.WriteLine($"{prefix}The True clause for this expression is:");
        trueVisitor.Visit(prefix + "\t");
        var falseVisitor = Visitor.CreateFromExpression(node.IfFalse);
        Console.WriteLine($"{prefix}The False clause for this expression is:");
        falseVisitor.Visit(prefix + "\t");
    }
}

public class MethodCallVisitor : Visitor
{
    private readonly MethodCallExpression node;
    public MethodCallVisitor(MethodCallExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        if (node.Object == null)
            Console.WriteLine($"{prefix}This is a static method call");
        else
        {
            Console.WriteLine($"{prefix}The receiver (this) is:");
            var receiverVisitor = Visitor.CreateFromExpression(node.Object);
            receiverVisitor.Visit(prefix + "\t");
        }

        var methodInfo = node.Method;
        Console.WriteLine($"{prefix}The method name is {methodInfo.DeclaringType}.{methodInfo.Name}");
        // There is more here, like generic arguments, and so on.
        Console.WriteLine($"{prefix}The Arguments are:");
        foreach(var arg in node.Arguments)
        {
            var argVisitor = Visitor.CreateFromExpression(arg);
            argVisitor.Visit(prefix + "\t");
        }
    }
}
```

式ツリーの出力は、次のようになります。

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: n, ByRef: False
The expression body is:
        This expression is a Conditional expression
        The Test for this expression is:
                This binary expression is a Equal expression
                The Left argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: n, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 0
        The True clause for this expression is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 1
        The False clause for this expression is:
                This expression is a Call expression
                This is a static method call
                The method name is System.Linq.Enumerable.Aggregate
                The Arguments are:
                        This expression is a Call expression
                        This is a static method call
                        The method name is System.Linq.Enumerable.Range
                        The Arguments are:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                                This is an Parameter expression type
                                Type: System.Int32, Name: n, ByRef: False
                        This expression is a Lambda expression type
                        The name of the lambda is <null>
                        The return type is System.Int32
                        The expression has 2 arguments. They are:
                                This is an Parameter expression type
                                Type: System.Int32, Name: product, ByRef: False
                                This is an Parameter expression type
                                Type: System.Int32, Name: factor, ByRef: False
                        The expression body is:
                                This binary expression is a Multiply expression
                                The Left argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: product, ByRef: False
                                The Right argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: factor, ByRef: False
```

## <a name="extending-the-sample-library"></a>サンプル ライブラリの拡張

このセクションのサンプルでは、式ツリーのノードにアクセスして調べるための中核的な手法を示します。 ここでは、式ツリーのノードにアクセスする主要なタスクに重点的に取り組むために必要な多くのアクションについて説明しました。

第 1 に、ビジターは整数の定数だけを処理します。 定数値は他の数値型になることができ、C# 言語ではそれらの型同士の変換や昇格がサポートされています。 このコードのより堅牢なバージョンには、それらすべての機能が反映されています。

最後のサンプルでも、想定されるノード型のサブセットを認識できます。
コードの処理が失敗する式をさらに読み込ませることもできます。
完全な実装は、<xref:System.Linq.Expressions.ExpressionVisitor> という名前で .NET Standard に含まれていて、想定されるすべてのノード型を処理できます。

最後に、この記事で使用したライブラリはデモ用および学習用として構築しました。 ライブラリの最適化は行っていません。 ライブラリを記述したのは、使用した構造体を明確にし、ノードのアクセスに使用した手法を浮き彫りにして、その内容を分析するためです。 運用環境への実装では、今回試みたよりもさらにパフォーマンスに注意を払うことになります。

こうした制限があるとはいえ、式ツリーを読み、理解するアルゴリズムの作成を問題なく進めていけるはずです。

[次へ -- 式の構築](expression-trees-building.md)
