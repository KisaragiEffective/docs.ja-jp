---
title: Expression Trees
ms.date: 07/20/2015
ms.assetid: 8bbbb02d-7ffc-476b-8c25-118d82bf5d46
ms.openlocfilehash: 4ca3b56f48368e465560fc5edd60c0df8dd4e1c0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344701"
---
# <a name="expression-trees-visual-basic"></a>式ツリー (Visual Basic)
式ツリーでは、コードがツリー状のデータ構造で表示されます。各ノードは 1 つの式に対応しています。たとえば、メソッドの呼び出しや `x < y` のような二項演算などです。  
  
 式ツリーで表されるコードはコンパイルおよび実行できます。 これによって、実行可能なコードの動的な変更、さまざまなデータベースでの LINQ クエリの実行、および動的クエリの作成が可能になります。 LINQ の式ツリーの詳細については、「[How to: Use Expression Trees to Build Dynamic Queries (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-use-expression-trees-to-build-dynamic-queries.md)」 (方法: 式ツリーを使用して動的クエリをビルドする (Visual Basic)) を参照してください。  
  
 また、式ツリーを動的言語ランタイム (DLR) に使用することで、動的言語と .NET Framework 間の相互運用性が実現し、コンパイラ ライターで Microsoft Intermediate language (MSIL) ではなく式ツリーを出力できるようになります。 DLR の詳細については、「[動的言語ランタイムの概要](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)」を参照してください。  
  
 匿名のラムダ式に基づいて、C# と Visual Basic コンパイラで式ツリーを作成できます。または、<xref:System.Linq.Expressions> 名前空間を使用して手動で式ツリーを作成できます。  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>ラムダ式からの式ツリーの作成  
 ラムダ式が <xref:System.Linq.Expressions.Expression%601> 型の変数に割り当てられている場合、コンパイラはラムダ式を表す式ツリーを構築するコードを出力します。  
  
 Visual Basic コンパイラは、式形式のラムダ (つまり単一行のラムダ) からのみ式ツリーを生成できます。 ステートメント形式のラムダ (つまり複数行のラムダ) は解析できません。 Visual Basic のラムダ式の詳細については、「[ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」を参照してください。  
  
 次のコード例は、ラムダ式 `Function(num) num < 5` を表す式ツリーを Visual Basic コンパイラで作成する方法を示しています。  
  
```vb  
Dim lambda As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>API を使用した式ツリーの作成  
 API を使用して式ツリーを作成するには、<xref:System.Linq.Expressions.Expression> クラスを使用します。 このクラスには、特定の型を持つ式ツリー ノードを作成する静的ファクトリ メソッドが含まれます。たとえば、変数またはパラメーターを表す <xref:System.Linq.Expressions.ParameterExpression> や、メソッドの呼び出しを表す <xref:System.Linq.Expressions.MethodCallExpression> などです。 <xref:System.Linq.Expressions.ParameterExpression>、<xref:System.Linq.Expressions.MethodCallExpression> などの式固有の型も、<xref:System.Linq.Expressions> 名前空間で定義されます。 これらの型は、<xref:System.Linq.Expressions.Expression> 抽象型から派生したものです。  
  
 次のコード例は、API を使用して、ラムダ式 `Function(num) num < 5` を表す式ツリーを作成する方法を示しています。  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Manually build the expression tree for the lambda expression num => num < 5.  
Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer), "num")  
Dim five As ConstantExpression = Expression.Constant(5, GetType(Integer))  
Dim numLessThanFive As BinaryExpression = Expression.LessThan(numParam, five)  
Dim lambda1 As Expression(Of Func(Of Integer, Boolean)) =  
  Expression.Lambda(Of Func(Of Integer, Boolean))(  
        numLessThanFive,  
        New ParameterExpression() {numParam})  
```  
  
 .NET Framework 4 以降の式ツリー API は、割り当て式、制御フロー式 (ループ、条件ブロック)、および `try-catch` ブロックもサポートしています。 API を使用すると、Visual Basic コンパイラのラムダ式から作成できる式ツリーよりも複雑な式ツリーを作成できます。 次の例は、数値の階乗を計算する式ツリーを作成する方法を示しています。  
  
```vb  
' Creating a parameter expression.  
Dim value As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "value")  
  
' Creating an expression to hold a local variable.   
Dim result As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "result")  
  
' Creating a label to jump to from a loop.  
Dim label As LabelTarget = Expression.Label(GetType(Integer))  
  
' Creating a method body.  
Dim block As BlockExpression = Expression.Block(  
    New ParameterExpression() {result},  
    Expression.Assign(result, Expression.Constant(1)),  
    Expression.Loop(  
        Expression.IfThenElse(  
            Expression.GreaterThan(value, Expression.Constant(1)),  
            Expression.MultiplyAssign(result,  
                Expression.PostDecrementAssign(value)),  
            Expression.Break(label, result)  
        ),  
        label  
    )  
)  
  
' Compile an expression tree and return a delegate.  
Dim factorial As Integer =  
    Expression.Lambda(Of Func(Of Integer, Integer))(block, value).Compile()(5)  
  
Console.WriteLine(factorial)  
' Prints 120.  
```

詳細については、「[Generating Dynamic Methods with Expression Trees in Visual Studio 2010](https://devblogs.microsoft.com/csharpfaq/generating-dynamic-methods-with-expression-trees-in-visual-studio-2010/) (Visual Studio 2010 での式ツリーによる動的メソッドの生成)」を参照し、Visual Studio 2010 以降のバージョンについても同じ方法を適用してください。
  
## <a name="parsing-expression-trees"></a>式ツリーの解析  
 次のコード例は、ラムダ式 `Function(num) num < 5` を表す式ツリーを各部分に分解する方法を示しています。  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Create an expression tree.  
Dim exprTree As Expression(Of Func(Of Integer, Boolean)) = Function(num) num < 5  
  
' Decompose the expression tree.  
Dim param As ParameterExpression = exprTree.Parameters(0)  
Dim operation As BinaryExpression = exprTree.Body  
Dim left As ParameterExpression = operation.Left  
Dim right As ConstantExpression = operation.Right  
  
Console.WriteLine(String.Format("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value))  
  
' This code produces the following output:  
'  
' Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>式ツリーの不変性  
 式ツリーは変更できません。 つまり、式ツリーを変更するには、既存の式ツリーをコピーしてツリー内のノードを置き換えることで、新しい式ツリーを作成する必要があります。 式ツリー ビジターを使用して、既存の式ツリーを走査することができます。 詳細については、「[方法: 式ツリーを変更する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md)」を参照してください。  
  
## <a name="compiling-expression-trees"></a>式ツリーのコンパイル  
 <xref:System.Linq.Expressions.Expression%601> 型に含まれる <xref:System.Linq.Expressions.Expression%601.Compile%2A> メソッドにより、式ツリーが表すコードを実行可能なデリゲートにコンパイルします。  
  
 次のコード例は、式ツリーをコンパイルして結果のコードを実行する方法を示しています。  
  
```vb  
' Creating an expression tree.  
Dim expr As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
  
' Compiling the expression tree into a delegate.  
Dim result As Func(Of Integer, Boolean) = expr.Compile()  
  
' Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4))  
  
' Prints True.  
  
' You can also use simplified syntax  
' to compile and run an expression tree.  
' The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4))  
  
' Also prints True.  
```  
  
 詳細については、「[方法: 式ツリーを実行する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-execute-expression-trees.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Linq.Expressions>
- [方法: 式ツリーを実行する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-execute-expression-trees.md)
- [方法: 式ツリーを変更する (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md)
- [ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [動的言語ランタイムの概要](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)
- [プログラミングの概念 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/index.md)
