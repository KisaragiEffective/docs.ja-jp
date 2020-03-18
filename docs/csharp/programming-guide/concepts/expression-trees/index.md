---
title: 式ツリー (C#)
ms.date: 07/20/2015
ms.assetid: 7d0ac21a-6d90-4e2e-8903-528cb78615b7
ms.openlocfilehash: f425ab38bf7bb54814fe777b7cb02180d022a8af
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79169637"
---
# <a name="expression-trees-c"></a>式ツリー (C#)
式ツリーでは、コードがツリー状のデータ構造で表示されます。各ノードは 1 つの式に対応しています。たとえば、メソッドの呼び出しや `x < y` のような二項演算などです。  
  
 式ツリーで表されるコードはコンパイルおよび実行できます。 これによって、実行可能なコードの動的な変更、さまざまなデータベースでの LINQ クエリの実行、および動的クエリの作成が可能になります。 LINQ の式ツリーの詳細については、「[式ツリーを使用して動的クエリを作成する方法 (C#)](./how-to-use-expression-trees-to-build-dynamic-queries.md)」をご覧ください。
  
 また、式ツリーを動的言語ランタイム (DLR) に使用することで、動的言語と .NET Framework 間の相互運用性が実現し、コンパイラ ライターで Microsoft Intermediate language (MSIL) ではなく式ツリーを出力できるようになります。 DLR の詳細については、「[動的言語ランタイムの概要](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)」を参照してください。  
  
 匿名のラムダ式に基づいて、C# と Visual Basic コンパイラで式ツリーを作成できます。または、<xref:System.Linq.Expressions> 名前空間を使用して手動で式ツリーを作成できます。  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>ラムダ式からの式ツリーの作成  
 ラムダ式が <xref:System.Linq.Expressions.Expression%601> 型の変数に割り当てられている場合、コンパイラはラムダ式を表す式ツリーを構築するコードを出力します。  
  
 C# コンパイラは、式形式のラムダ (つまり単一行のラムダ) からのみ式ツリーを生成できます。 ステートメント形式のラムダ (つまり複数行のラムダ) は解析できません。 C# のラムダ式の詳細については、「[ラムダ式](../../statements-expressions-operators/lambda-expressions.md)」を参照してください。  
  
 次のコード例は、ラムダ式 `num => num < 5` を表す式ツリーを C# コンパイラで作成する方法を示しています。  
  
```csharp  
Expression<Func<int, bool>> lambda = num => num < 5;  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>API を使用した式ツリーの作成  
 API を使用して式ツリーを作成するには、<xref:System.Linq.Expressions.Expression> クラスを使用します。 このクラスには、特定の型を持つ式ツリー ノードを作成する静的ファクトリ メソッドが含まれます。たとえば、変数またはパラメーターを表す <xref:System.Linq.Expressions.ParameterExpression> や、メソッドの呼び出しを表す <xref:System.Linq.Expressions.MethodCallExpression> などです。 <xref:System.Linq.Expressions.ParameterExpression>、<xref:System.Linq.Expressions.MethodCallExpression> などの式固有の型も、<xref:System.Linq.Expressions> 名前空間で定義されます。 これらの型は、<xref:System.Linq.Expressions.Expression> 抽象型から派生したものです。  
  
 次のコード例は、API を使用して、ラムダ式 `num => num < 5` を表す式ツリーを作成する方法を示しています。  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Manually build the expression tree for
// the lambda expression num => num < 5.  
ParameterExpression numParam = Expression.Parameter(typeof(int), "num");  
ConstantExpression five = Expression.Constant(5, typeof(int));  
BinaryExpression numLessThanFive = Expression.LessThan(numParam, five);  
Expression<Func<int, bool>> lambda1 =  
    Expression.Lambda<Func<int, bool>>(  
        numLessThanFive,  
        new ParameterExpression[] { numParam });  
```  
  
 .NET Framework 4 以降の式ツリー API は、割り当て式、制御フロー式 (ループ、条件ブロック)、および `try-catch` ブロックもサポートしています。 API を使用すると、C# コンパイラのラムダ式から作成できる式ツリーよりも複雑な式ツリーを作成できます。 次の例は、数値の階乗を計算する式ツリーを作成する方法を示しています。  
  
```csharp  
// Creating a parameter expression.  
ParameterExpression value = Expression.Parameter(typeof(int), "value");  
  
// Creating an expression to hold a local variable.
ParameterExpression result = Expression.Parameter(typeof(int), "result");  
  
// Creating a label to jump to from a loop.  
LabelTarget label = Expression.Label(typeof(int));  
  
// Creating a method body.  
BlockExpression block = Expression.Block(  
    // Adding a local variable.  
    new[] { result },  
    // Assigning a constant to a local variable: result = 1  
    Expression.Assign(result, Expression.Constant(1)),  
    // Adding a loop.  
        Expression.Loop(  
    // Adding a conditional block into the loop.  
           Expression.IfThenElse(  
    // Condition: value > 1  
               Expression.GreaterThan(value, Expression.Constant(1)),  
    // If true: result *= value --  
               Expression.MultiplyAssign(result,  
                   Expression.PostDecrementAssign(value)),  
    // If false, exit the loop and go to the label.  
               Expression.Break(label, result)  
           ),  
    // Label to jump to.  
       label  
    )  
);  
  
// Compile and execute an expression tree.  
int factorial = Expression.Lambda<Func<int, int>>(block, value).Compile()(5);  
  
Console.WriteLine(factorial);  
// Prints 120.  
```

詳細については、「[Generating Dynamic Methods with Expression Trees in Visual Studio 2010](https://devblogs.microsoft.com/csharpfaq/generating-dynamic-methods-with-expression-trees-in-visual-studio-2010/) (Visual Studio 2010 での式ツリーによる動的メソッドの生成)」を参照し、Visual Studio 2010 以降のバージョンについても同じ方法を適用してください。
  
## <a name="parsing-expression-trees"></a>式ツリーの解析  
 次のコード例は、ラムダ式 `num => num < 5` を表す式ツリーを各部分に分解する方法を示しています。  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Create an expression tree.  
Expression<Func<int, bool>> exprTree = num => num < 5;  
  
// Decompose the expression tree.  
ParameterExpression param = (ParameterExpression)exprTree.Parameters[0];  
BinaryExpression operation = (BinaryExpression)exprTree.Body;  
ParameterExpression left = (ParameterExpression)operation.Left;  
ConstantExpression right = (ConstantExpression)operation.Right;  
  
Console.WriteLine("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value);  
  
// This code produces the following output:  
  
// Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>式ツリーの不変性  
 式ツリーは変更できません。 つまり、式ツリーを変更するには、既存の式ツリーをコピーしてツリー内のノードを置き換えることで、新しい式ツリーを作成する必要があります。 式ツリー ビジターを使用して、既存の式ツリーを走査することができます。 詳細については、「[式ツリーを変更する方法 (C#)](./how-to-modify-expression-trees.md)」を参照してください。
  
## <a name="compiling-expression-trees"></a>式ツリーのコンパイル  
 <xref:System.Linq.Expressions.Expression%601> 型に含まれる <xref:System.Linq.Expressions.Expression%601.Compile%2A> メソッドにより、式ツリーが表すコードを実行可能なデリゲートにコンパイルします。  
  
 次のコード例は、式ツリーをコンパイルして結果のコードを実行する方法を示しています。  
  
```csharp  
// Creating an expression tree.  
Expression<Func<int, bool>> expr = num => num < 5;  
  
// Compiling the expression tree into a delegate.  
Func<int, bool> result = expr.Compile();  
  
// Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4));  
  
// Prints True.  
  
// You can also use simplified syntax  
// to compile and run an expression tree.  
// The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4));  
  
// Also prints True.  
```  
  
 詳細については、「[式ツリーを実行する方法 (C#)](./how-to-execute-expression-trees.md)」を参照してください。
  
## <a name="see-also"></a>参照

- <xref:System.Linq.Expressions>
- [式ツリーを実行する方法 (C#)](./how-to-execute-expression-trees.md)
- [式ツリーを変更する方法 (C#)](./how-to-modify-expression-trees.md)
- [ラムダ式](../../statements-expressions-operators/lambda-expressions.md)
- [動的言語ランタイムの概要](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)
- [プログラミングの概念 (C#)](../index.md)
