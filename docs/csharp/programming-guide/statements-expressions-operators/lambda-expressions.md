---
title: ラムダ式 - C# プログラミング ガイド
ms.date: 07/29/2019
helpviewer_keywords:
- lambda expressions [C#]
- outer variables [C#]
- statement lambda [C#]
- expression lambda [C#]
- expressions [C#], lambda
ms.assetid: 57e3ba27-9a82-4067-aca7-5ca446b7bf93
ms.openlocfilehash: c549b9fcc91401aed846afd39e656b60e16afb74
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937600"
---
# <a name="lambda-expressions-c-programming-guide"></a>ラムダ式 (C# プログラミング ガイド)

"*ラムダ式*" は、次の 2 つのいずれかの形式を持つ式です。

- [式形式のラムダ](#expression-lambdas)は、式本体に式が含まれます。

  ```csharp
  (input-parameters) => expression
  ```

- [ステートメント形式のラムダ](#statement-lambdas)は、式本体にステートメント ブロックが含まれます。

  ```csharp  
  (input-parameters) => { <sequence-of-statements> }
  ```

[ラムダ宣言演算子`=>`](../../language-reference/operators/lambda-operator.md)を使用して、ラムダのパラメーター リストを式本体から分離します。 ラムダ式を作成するには、ラムダ演算子の左辺に入力パラメーターを指定し (ある場合)、右辺に式またはステートメント ブロックを指定します。

ラムダ式は、[デリゲート](../../language-reference/builtin-types/reference-types.md#the-delegate-type)型に変換できます。 ラムダ式を変換できるデリゲート型は、パラメータと戻り値の型で定義されます。 ラムダ式が値を返さない場合は `Action` デリゲート型のいずれかに変換でき、値を返す場合は`Func` デリゲート型のいずれかに変換できます。 たとえば、2 つのパラメーターがあり、値を返さないラムダ式は、<xref:System.Action%602> デリゲートに変換できます。 1 つのパラメーターがあり、値を返すラムダ式は、<xref:System.Func%602> デリゲートに変換できます。 次の例では、`x` という名前のパラメーターを指定し、`x` の二乗値を返すラムダ式 `x => x * x` をデリゲート型の変数に割り当てます。

[!code-csharp-interactive[lambda is delegate](~/samples/snippets/csharp/programming-guide/lambda-expressions/Introduction.cs#Delegate)]

式形式のラムダは、次の例に示すように、[式ツリー](../concepts/expression-trees/index.md)型にも変換できます。

[!code-csharp-interactive[lambda is expression tree](~/samples/snippets/csharp/programming-guide/lambda-expressions/Introduction.cs#ExpressionTree)]

ラムダ式は、デリゲート型または式ツリーのインスタンスを必要とするすべてのコードで使用できます。たとえば、<xref:System.Threading.Tasks.Task.Run(System.Action)?displayProperty=nameWithType> メソッドの引数として使用すると、バックグラウンドで実行する必要があるコードを渡すことができます。 また、次の例に示すように、[C# で LINQ](../../linq/index.md) を作成する場合にもラムダ式を使用できます。

[!code-csharp-interactive[lambda is argument in LINQ](~/samples/snippets/csharp/programming-guide/lambda-expressions/Introduction.cs#Argument)]

メソッド ベースの構文を使用して <xref:System.Linq.Enumerable?displayProperty=nameWithType> クラス (たとえば、LINQ to Objects、LINQ to XML など) の <xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType> メソッドを呼び出すと、パラメーターはデリゲート型 <xref:System.Func%602?displayProperty=nameWithType> になります。 <xref:System.Linq.Queryable?displayProperty=nameWithType> クラス (たとえば、LINQ to SQL など) の <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType> メソッドを呼び出すと、パラメーター型は式ツリー型 [`Expression<Func<TSource,TResult>>`](<xref:System.Linq.Expressions.Expression%601>) になります。 どちらの場合も、同じラムダ式を使用してパラメーター値を指定できます。 これにより、2 つの `Select` 呼び出しは外観が似ていますが、ラムダから実際に作成されるオブジェクトの型は異なります。
  
## <a name="expression-lambdas"></a>式形式のラムダ

`=>` 演算子の右辺に式があるラムダ式を "*式形式のラムダ*" と呼びます。 式形式のラムダは、[式ツリー](../concepts/expression-trees/index.md)の構築に幅広く使用されます。 式形式のラムダは式の結果を返します。基本的な形式は次のとおりです。

```csharp
(input-parameters) => expression
```

かっこはラムダの入力パラメーターが 1 つの場合のみ省略可能で、それ以外の場合は必須です。

入力パラメーターがないことを指定するには、次のように空のかっこを使用します。  

[!code-csharp[zero parameters](~/samples/snippets/csharp/programming-guide/lambda-expressions/ExpressionAndStatementLambdas.cs#ZeroParameters)]

入力パラメーターが 2 つ以上ある場合は、かっこで囲んで各パラメーターをコンマで区切ります。

[!code-csharp[two parameters](~/samples/snippets/csharp/programming-guide/lambda-expressions/ExpressionAndStatementLambdas.cs#TwoParameters)]

コンパイラによる入力の型の推論が不可能な場合があります。 次の例のように、型を明示的に指定できます。

[!code-csharp[explicitly typed parameters](~/samples/snippets/csharp/programming-guide/lambda-expressions/ExpressionAndStatementLambdas.cs#ExplicitlyTypedParameters)]

入力パラメーターの型は、すべて明示的またはすべて暗黙的である必要があります。それ以外の場合は、[CS0748](../../misc/cs0748.md) コンパイラ エラーが発生します。

式形式のラムダの本体を、メソッド呼び出しで構成できます。 ただし、SQL Server などの .NET 共通言語ランタイムの外部で評価される式ツリーを作成する場合は、ラムダ式内でメソッド呼び出しを使用することはできません。 .NET 共通言語ランタイムのコンテキストの外部では、これらのメソッドは通用しません。

## <a name="statement-lambdas"></a>ステートメント形式のラムダ

ステートメント形式のラムダは式形式のラムダに似ていますが、ステートメントが中かっこで囲まれる点が異なります。

```csharp  
(input-parameters) => { <sequence-of-statements> }
```

ステートメント形式のラムダの本体は任意の数のステートメントで構成できますが、実際面では通常、2、3 個以下にします。

[!code-csharp-interactive[statement lambda](~/samples/snippets/csharp/programming-guide/lambda-expressions/ExpressionAndStatementLambdas.cs#StatementLambda)]

ステートメント形式のラムダを使用して式ツリーを作成することはできません。
  
## <a name="async-lambdas"></a>非同期ラムダ

[async](../../language-reference/keywords/async.md) キーワードと [await](../../language-reference/operators/await.md) キーワードを使用すると、非同期処理を組み込んだラムダ式およびステートメントを簡単に作成できます。 たとえば、次に示す Windows フォーム例には、非同期メソッド `ExampleMethodAsync`を呼び出して待機するイベント ハンドラーが含まれています。

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        button1.Click += button1_Click;
    }

    private async void button1_Click(object sender, EventArgs e)
    {
        await ExampleMethodAsync();
        textBox1.Text += "\r\nControl returned to Click event handler.\n";
    }

    private async Task ExampleMethodAsync()
    {
        // The following line simulates a task-returning asynchronous process.
        await Task.Delay(1000);
    }
}
```

非同期ラムダを使用して、同じイベント ハンドラーを追加できます。 このハンドラーを追加するには、次の例に示すように、ラムダ パラメーター リストの前に `async` 修飾子を追加します。

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        button1.Click += async (sender, e) =>
        {
            await ExampleMethodAsync();
            textBox1.Text += "\r\nControl returned to Click event handler.\n";
        };
    }

    private async Task ExampleMethodAsync()
    {
        // The following line simulates a task-returning asynchronous process.
        await Task.Delay(1000);
    }
}
```

非同期メソッドの作成および使用方法の詳細については、「[Async および Await を使用した非同期プログラミング](../concepts/async/index.md)」を参照してください。

## <a name="lambda-expressions-and-tuples"></a>ラムダ式とタプル

C# 7.0 以降、C# 言語には、[タプル](../../tuples.md)のサポートが組み込まれています。 タプルは、ラムダ式への引数として指定できるほか、ラムダ式で返すこともできます。 場合によっては、C# コンパイラは、型の推定を使用して、タプル コンポーネントの型を判定することもあります。

タプルを定義するには、そのコンポーネントのコンマ区切りリストをかっこで囲みます。 次の例では、3 つのコンポーネントを持つタプルを使用して、ラムダ式に連続した数値を渡します。このラムダ式により、各値が 2 倍になり、乗算の結果を格納する 3 つのコンポーネントを持つタプルが返されます。

[!code-csharp-interactive[lambda and tuples](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasAndTuples.cs#WithoutComponentName)]

通常、タプルのフィールド名は `Item1`、`Item2` のようになります。ただし、次の例のとおり、名前付きのコンポーネントを持つタプルを定義することができます。

[!code-csharp-interactive[lambda and named tuples](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasAndTuples.cs#WithComponentName)]

C# のタプルの詳細については、「[C# のタプル型](../../tuples.md)」を参照してください。

## <a name="lambdas-with-the-standard-query-operators"></a>標準クエリ演算子を使用したラムダ

いくつかある実装の中で特に、LINQ to Objects は、汎用デリゲートの <xref:System.Func%601> ファミリに属する型の入力パラメーターを持ちます。 これらのデリゲートは、型パラメーターを使用して、入力パラメーターの数と型に加え、デリゲートの戻り値の型を定義します。 `Func` デリゲートは、ソース データのセット内の各要素に適用されるユーザー定義の式をカプセル化する場合に非常に便利です。 たとえば、<xref:System.Func%602>デリゲート型を考えてみましょう。  

```csharp
public delegate TResult Func<in T, out TResult>(T arg)
```

このデリゲートを `Func<int, bool>` としてインスタンス化できます。 `int` は入力パラメーター、`bool` は戻り値です。 戻り値は必ず最後の型パラメーターで指定されます。 たとえば、`Func<int, string, bool>` は 2 つの入力パラメーター ( `int` と `string`) と戻り値の型 `bool` を持つデリゲートを定義しています。 次の `Func` デリゲートを呼び出すと、入力パラメーターが 5 に等しいかどうかを示す true または false が返されます。

[!code-csharp-interactive[Func example](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasWithQueryMethods.cs#Func)]

たとえば、<xref:System.Linq.Queryable> 型で定義された標準クエリ演算子において、引数型が <xref:System.Linq.Expressions.Expression%601> の場合もラムダ式を使用できます。 <xref:System.Linq.Expressions.Expression%601> 引数を指定すると、ラムダは式ツリーにコンパイルされます。
  
次の例では、<xref:System.Linq.Enumerable.Count%2A> 標準クエリ演算子を使用します。

[!code-csharp-interactive[Count example](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasWithQueryMethods.cs#Count)]

入力パラメーターの型はコンパイラが推論できますが、明示的に指定することもできます。 この特定のラムダ式は、2 で除算したときに剰余が 1 になる整数 (`n`) をカウントします。

次の例では、`numbers` 配列内で 9 より前にある要素をすべて含むシーケンスを作成します。これは、9 がシーケンス内で条件を満たさない最初の数値であるためです。

[!code-csharp-interactive[TakeWhile example](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasWithQueryMethods.cs#TakeWhile)]

次の例では、複数の入力パラメーターをかっこで囲んで指定します。 このメソッドは、値が `numbers` 配列内のその位置を表す序数よりも小さい数値が出現するまで、その配列に含まれるすべての要素を返します。

[!code-csharp-interactive[TakeWhile example 2](~/samples/snippets/csharp/programming-guide/lambda-expressions/LambdasWithQueryMethods.cs#TakeWhileWithIndex)]

## <a name="type-inference-in-lambda-expressions"></a>ラムダ式の型の推定

ラムダを記述する際、多くの場合は入力パラメーターの型を指定する必要はありません。これは、ラムダ本体やパラメーターの型など C# 言語仕様に記述されている要素に基づいて、コンパイラが型を推定できるためです。 ほとんどの標準クエリ演算子では、最初の入力がソース シーケンス内の要素の型です。 `IEnumerable<Customer>` を問い合わせると、入力変数は `Customer` オブジェクトであると推論されます。これは、そのメソッドとプロパティにアクセスできることを意味します。  

```csharp
customers.Where(c => c.City == "London");
```

ラムダの型の推定の一般規則は、次のとおりです。

- ラムダにはデリゲート型と同じ数のパラメーターが含まれていなければなりません。

- ラムダに含まれる各入力パラメーターは、対応するデリゲート パラメーターに暗黙的に変換できなければなりません。

- ラムダの戻り値 (ある場合) は、デリゲートの戻り値の型に暗黙的に変換できなければなりません。
  
共通型システムには "ラムダ式" の概念が組み込まれていないため、ラムダ式自体は型を持ちません。 しかし、変則的ではあってもラムダ式の "型" を表現できると都合が良い場合もあります。 このような場合の型は、ラムダ式の変換後のデリゲート型または <xref:System.Linq.Expressions.Expression> 型を指します。

## <a name="capture-of-outer-variables-and-variable-scope-in-lambda-expressions"></a>ラムダ式における外部変数のキャプチャと変数のスコープ

ラムダでは "*外部変数*" を参照できます。 それは、そのラムダ式を定義しているメソッドのスコープ内にある変数、またはそのラムダ式を含む型のスコープ内にある変数です。 こうして取り込まれた変数は、ラムダ式で使用するために格納されます。これは、変数がスコープ外に出てガベージ コレクトされる場合でも変わりません。 外部変数は、ラムダ式で使用される前に明示的に代入する必要があります。 次の例は、こうした規則を示しています。

[!code-csharp[variable scope](~/samples/snippets/csharp/programming-guide/lambda-expressions/VariableScopeWithLambdas.cs#VariableScope)]

ラムダ式における変数のスコープには、次の規則が適用されます。  

- 取り込まれた変数は、その変数を参照するデリゲートがガベージ コレクションの対象になるまでガベージ コレクトされません。

- ラムダ式内に導入された変数は、外側のメソッドでは参照できません。

- ラムダ式は、外側のメソッドから [in](../../language-reference/keywords/in-parameter-modifier.md)、[ref](../../language-reference/keywords/ref.md)、または [out](../../language-reference/keywords/out-parameter-modifier.md) パラメーターを直接取り込むことはできません。

- ラムダ式に含まれる [return](../../language-reference/keywords/return.md) ステートメントで外側のメソッドが戻ることはありません。

- ジャンプ先がラムダ式ブロックの外側にある場合は、ラムダ式に [goto](../../language-reference/keywords/goto.md)、[break](../../language-reference/keywords/break.md)、または [continue](../../language-reference/keywords/continue.md) ステートメントを含めることはできません。 また、ジャンプ先がブロックの内部にある場合に、ラムダ式ブロックの外部でジャンプ ステートメントを使用するとエラーになります。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の[無名関数の式](~/_csharplang/spec/expressions.md#anonymous-function-expressions)に関するセクションを参照してください。

## <a name="featured-book-chapter"></a>参考書籍の該当する章

「[Delegates, Events, and Lambda Expressions (デリゲート、イベント、およびラムダ式)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518994%28v=orm.10%29)」(『[C# 3.0 Cookbook, Third Edition: More than 250 solutions for C# 3.0 programmers (C# 3.0 クックブック (第 3 版): C# 3.0 プログラマ向けの 250 以上のソリューション)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518995%28v=orm.10%29)』)  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [統合言語クエリ (LINQ)](../concepts/linq/index.md)
- [式ツリー](../concepts/expression-trees/index.md)
- [ローカル関数とラムダ式の比較](../../local-functions-vs-lambdas.md)
- [暗黙的に型指定されるラムダ式](../../implicitly-typed-lambda-expressions.md)
- [Visual Studio 2008 C# Samples (Visual Studio 2008 の C# サンプル) (LINQ サンプル クエリ ファイルと XQuery プログラムを参照してください)](https://code.msdn.microsoft.com/Visual-Studio-2008-C-d295cdba)
- [Recursive lambda expressions (再帰的なラムダ式)](https://docs.microsoft.com/archive/blogs/madst/recursive-lambda-expressions)
