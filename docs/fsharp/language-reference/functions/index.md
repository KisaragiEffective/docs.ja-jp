---
title: 関数
description: での関数とF# 、でF#共通の関数型プログラミングコンストラクトをサポートする方法について説明します。
ms.date: 05/16/2016
ms.openlocfilehash: c6b8307f51ffcdc77fe4352b2305fca1f247ccbb
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73423951"
---
# <a name="functions"></a>関数

関数は、あらゆるプログラミング言語においてプログラムの実行の基本となる単位です。 他の言語の場合と同様に、F# の関数にもそれぞれ名前と本体があり、パラメーターや引数を受け取ることができます。 F# ではさらに、関数型プログラミング構成要素もサポートしています。たとえば、関数を値として処理したり、名前のない関数を式で使用したりできます。また、関数の合成による新しい関数の作成、カリー化関数、関数の引数の部分適用による関数の暗黙の定義などがサポートされます。

関数を定義するには `let` キーワードを使用します。再帰関数の場合は、`let rec` というキーワードの組み合わせを使用します。

## <a name="syntax"></a>構文

```fsharp
// Non-recursive function definition.
let [inline] function-name parameter-list [ : return-type ] = function-body
// Recursive function definition.
let rec function-name parameter-list = recursive-function-body
```

## <a name="remarks"></a>Remarks

*function-name* は、関数を表す識別子です。 *parameter-list* は、スペースで区切られた一連のパラメーターで構成されます。 「パラメーター」で説明するように、各パラメーターの明示的な型を指定できます。 特定の引数型を指定しない場合は、コンパイラによって型が関数本体から推測されます。 *function-body* は式で構成されます。 関数本体を構成する式は、通常、複数の式から成る複合式で、その最後の式が戻り値になります。 *return-type* では、コロンの後に戻り値の型を指定します。これは省略可能です。 戻り値の型を明示的に指定しない場合は、コンパイラによって最後の式から特定されます。

単純な関数定義は、次のようになります。

```fsharp
let f x = x + 1
```

この例では、関数の名前は `f`、引数は `x`、引数の型は `int`、関数本体は `x + 1`、戻り値の型は `int` です。

関数は、`inline` としてマークできます。 `inline` の詳細については、「[Inline Functions](../functions/inline-functions.md)」(インライン関数) を参照してください。

## <a name="scope"></a>[スコープ]

モジュール スコープ以外のスコープの任意のレベルでは、値または関数の名前を再利用してもエラーになりません。 名前を再利用する場合、後から宣言した名前が前に宣言した名前をシャドウします。 ただし、モジュールの最上位のスコープでは名前が一意である必要があります。 たとえば次のコードは、モジュール スコープではエラーになりますが、関数内ではエラーになりません。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet101.fs)]

しかし、次のコードはどのスコープ レベルでも許容されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet102.fs)]

### <a name="parameters"></a>パラメーター

パラメーターの名前は、関数名の後に指定されます。 次の例のように、パラメーターの型を指定できます。

```fsharp
let f (x : int) = x + 1
```

型を指定する場合は、パラメーターの名前の後にコロンで区切って指定します。 パラメーターの型を省略した場合、パラメーターの型がコンパイラによって推論されます。 たとえば、次の関数定義で、1 の型が `int` であるため、引数 `x` の型は `int` と推論されます。

```fsharp
let f x = x + 1
```

ただし、コンパイラは、可能な限り、関数を汎用的にしようとします。 たとえば次のようなコードがあるとします。

```fsharp
let f x = (x, x)
```

関数は、任意の型の 1 つの引数からタプルを作成します。 型が指定されていないために、任意の引数の型と関数を使用できます。 詳細については、「[自動ジェネリック化](../generics/automatic-generalization.md)」を参照してください。

## <a name="function-bodies"></a>関数本体

関数本体には、ローカル変数と関数の定義を含めることができます。 それらの変数と関数のスコープは、現在の関数の本体内に限られます。 軽量構文オプションを有効にしている場合は、次の例に示すように、定義が関数本体に含まれていることをインデントによって示す必要があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet103.fs)]

詳細については、「[コードのフォーマットに関するガイドライン](../../style-guide/formatting.md)」および「[Verbose Syntax](../verbose-syntax.md)」 (冗語構文) を参照してください。

## <a name="return-values"></a>戻り値

コンパイラは、関数本体の最後の式を使用して戻り値とその型を特定します。 最後の式の型が前の式から推論される場合もあります。 前のセクションの関数 `cylinderVolume` では、`pi` の型は、リテラル `3.14159` の型から `float` と特定されます。 コンパイラは、`pi` の型を使用して、式 `h * pi * r * r` の型が `float` であると判断します。 このため、関数全体の戻り値の型が `float` になります。

戻り値を明示的に指定するには、次のようにコードを記述します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet105.fs)]

上記のコードが書き込まれると、コンパイラは関数全体に **float** を適用します。パラメーターの型にも適用する場合は、次のコードを使用します。

```fsharp
let cylinderVolume (radius : float) (length : float) : float
```

## <a name="calling-a-function"></a>関数の呼び出し

関数を呼び出すには、関数名の後にスペースを入れ、その後にスペースで区切った引数を指定します。 たとえば、関数 **cylinderVolume** を呼び出して結果を値 **vol** に割り当てるには、次のコードを記述します。

```fsharp
let vol = cylinderVolume 2.0 3.0
```

## <a name="partial-application-of-arguments"></a>引数の部分適用

指定されている数より少ない数の引数を渡すと、残りの引数を使用する新しい関数が作成されます。 これは、*カリー化*と呼ばれる引数の処理方法で、F# のような関数型プログラミング言語の特徴の 1 つです。 たとえば、半径がそれぞれ **2.0** と **3.0** の 2 つのパイプを使用しているとします。 次のようにして、パイプの体積を特定する関数を作成することができます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet106.fs)]

その後、必要に応じて、2 つのサイズのパイプのさまざまな長さを追加の引数として指定します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet107.fs)]

## <a name="recursive-functions"></a>再帰関数

*再帰関数*はそれらの関数自体を呼び出す関数です。 再帰関数を使用するには、**let** キーワードの後に **rec** キーワードを指定する必要があります。 関数の本体から再帰関数を呼び出す方法は、他の関数呼び出しの場合と変わりません。 次の再帰関数は、 *n*<sup>番目</sup>のフィボナッチ数列を計算します。 フィボナッチ数列は、古代から知られている数列で、数例の各数値が、前の 2 つの連続する数値の和になります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet108.fs)]

再帰関数は、特殊な手法 (アキュムレータや継続の使用など) を意識して慎重に使用しないと、プログラム スタックのオーバーフローを引き起こしたり、プログラムの実行効率が低下したりする可能性があります。

## <a name="function-values"></a>関数の値

F# では、すべての関数が値と見なされ、実際に、*関数値*と呼ばれています。 関数は値であるため、他の関数の引数として使用したり、値が使用されるその他のコンテキストで使用したりできます。 次に関数値を引数として受け取る関数の例を示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet109.fs)]

関数値の型を指定するには、`->` トークンを使用します。 このトークンの左側に引数の型を、右側に戻り値の型を指定します。 上の例の `apply1` は、関数 `transform` を引数として受け取る関数で、`transform` は、整数を受け取る、別の整数を返す関数です。 `apply1` の使用例を次のコードに示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet110.fs)]

前のコードを実行した後の `result` の値は 101 になります。

複数の引数を指定するには、次の例に示すように、連続する `->` トークンで区切ります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet111.fs)]

結果は 200 になります。

## <a name="lambda-expressions"></a>ラムダ式

*ラムダ式*とは、名前のない関数です。 前の例では、名前のある関数 **increment** と **mul** を定義しましたが、次のように、代わりにラムダ式を使用することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet112.fs)]

ラムダ式を定義するには、`fun` キーワードを使用します。 ラムダ式は関数定義に似ていますが、引数リストと関数本体の区切りに `=` トークンではなく `->` トークンを使用する点が異なります。 通常の関数定義と同様に、引数の型は、推論されるようにすることも、明示的に指定することもできます。ラムダ式の戻り値の型も、本体の最後の式の型から推論されます。 詳細については、「[ラムダ式: `fun` キーワード](../functions/lambda-expressions-the-fun-keyword.md)」を参照してください。

## <a name="function-composition-and-pipelining"></a>関数合成とパイプライン処理

F# の関数は、その他の関数から構成することができます。 **function1** と **function2** という 2 つの関数を合成すると、**function1** に続いて **function2** を適用する別の関数になります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet113.fs)]

結果は 202 になります。

パイプライン処理を使用すると、複数の関数呼び出しを一連の操作として連結することができます。 パイプラインは次のように処理されます。

```fsharp
let result = 100 |> function1 |> function2
```

結果は再び 202 になります。

合成演算子は 2 個の関数を受け取り、関数を返します。これに対し、パイプライン演算子は、関数と引数を受け取り、値を返します。 次のコード例は、関数のシグネチャと使用の違いを示すことによって、パイプラインと合成演算子の違いを示しています。

```fsharp
// Function composition and pipeline operators compared.

let addOne x = x + 1
let timesTwo x = 2 * x

// Composition operator
// ( >> ) : ('T1 -> 'T2) -> ('T2 -> 'T3) -> 'T1 -> 'T3
let Compose2 = addOne >> timesTwo

// Backward composition operator
// ( << ) : ('T2 -> 'T3) -> ('T1 -> 'T2) -> 'T1 -> 'T3
let Compose1 = addOne << timesTwo

// Result is 5
let result1 = Compose1 2

// Result is 6
let result2 = Compose2 2

// Pipelining
// Pipeline operator
// ( |> ) : 'T1 -> ('T1 -> 'U) -> 'U
let Pipeline2 x = addOne x |> timesTwo

// Backward pipeline operator
// ( <| ) : ('T -> 'U) -> 'T -> 'U
let Pipeline1 x = addOne <| timesTwo x

// Result is 5
let result3 = Pipeline1 2

// Result is 6
let result4 = Pipeline2 2
```

## <a name="overloading-functions"></a>オーバーロードの対象となる関数

関数ではなく型のメソッドをオーバー ロードすることができます。 詳細については、「[メソッド](../members/methods.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [値](../values/index.md)
- [F# 言語リファレンス](../index.md)
