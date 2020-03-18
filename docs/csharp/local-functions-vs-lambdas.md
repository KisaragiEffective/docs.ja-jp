---
title: ローカル関数とラムダ式の比較
description: ローカル関数がラムダ式よりも適した選択肢となり得る理由について。
ms.date: 06/27/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 368d1752-3659-489a-97b4-f15d87e49ae3
ms.openlocfilehash: 13cc3fe47bbcd6a465347a6c991b2006586c78fa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173342"
---
# <a name="local-functions-compared-to-lambda-expressions"></a>ローカル関数とラムダ式の比較

一見したところ、[ローカル関数](programming-guide/classes-and-structs/local-functions.md)と[ラムダ式](./programming-guide/statements-expressions-operators/lambda-expressions.md)は、非常に似ています。 多くの場合、ラムダ式とローカル関数の使用のどちらを選択するかは、スタイルと個人的な好みの問題です。 ただし、どちらか一方を使用できる場合、認識しておくべき実質的な違いがあります。

階乗アルゴリズムのローカル関数とラムダ式の実装の違いについて見てみましょう。 まずは、ローカル関数を使用するバージョンです。

[!code-csharp[LocalFunctionFactorial](../../samples/snippets/csharp/new-in-7/MathUtilities.cs#37_LocalFunctionFactorial "Recursive factorial using local function")]

ラムダ式を使用するバージョンの実装と比較します。

[!code-csharp[26_LambdaFactorial](../../samples/snippets/csharp/new-in-7/MathUtilities.cs#38_LambdaFactorial "Recursive factorial using lambda expressions")]

ローカル関数には名前があります。 ラムダ式は匿名メソッドであり、`Func` または `Action` 型である変数に割り当てられます。 ローカル関数を宣言する場合、引数の型と戻り値の型は関数宣言の一部となります。 引数の型と戻り値の型は、ラムダ式の本体の一部ではなく、ラムダ式の変数型宣言の一部となります。 これら 2 つの違いにより、コードがわかりやすくなる場合があります。

ローカル関数には、ラムダ式とは異なる確実な代入のルールがあります。 ローカル関数宣言は、スコープ内にある任意のコードの場所から参照できます。 ラムダ式はデリゲート変数に割り当てないと、アクセスできません (ラムダ式を参照するデリゲートを通じて呼び出すこともできません)ラムダ式を使用したバージョンでは、ラムダ式 `nthFactorial` を定義する前に、宣言と初期化を行う必要があります。 その手順を踏まないと、`nthFactorial` の割り当て前に参照することによるコンパイル時エラーが発生します。
これらの違いは、再帰的なアルゴリズムの作成はローカル関数を使用する方が簡単であることを意味します。 自身を呼び出すローカル関数を宣言して定義することができます。 ラムダ式は宣言して、既定値を割り当てないと、同じラムダ式を参照する本体に再割り当てできません。

確実な代入ルールは、ローカル関数またはラムダ式でキャプチャされる変数にも影響します。 ローカル関数とラムダ式の両方のルールでは、ローカル関数またはラムダ式がデリゲートに変換された時点で、キャプチャされた変数が確実に代入されることが要求されます。 違いは、ラムダ式が宣言時にデリゲートに変換されることです。 ローカル関数は、デリゲートとして使用される場合にのみ、デリゲートに変換されます。 ローカル関数を宣言し、メソッドのように呼び出して参照のみを行う場合は、デリゲートに変換されません。 このルールでは、外側のスコープ内の便利な場所でローカル関数を宣言できます。 親メソッドの末尾 (すべての return ステートメントの後) にローカル関数を宣言するのが一般的です。

3 つ目は、コンパイラは静的分析を実行できることです。これにより、ローカル関数で外側のスコープ内のキャプチャされた変数を確実に割り当てることができます。 次の例について考えます。

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

コンパイラは、呼び出し時に `LocalFunction` が `y` を確実に割り当てるかどうかを確認できます。 `return` ステートメントの前に `LocalFunction` が呼び出されるため、`y` は `return` ステートメントで確実に割り当てられます。

分析例を使用する分析では、4 つ目の違いを確認できます。
ローカル関数では、その使用に応じて、ラムダ式では常に必要なヒープの割り当てを回避できます。 ローカル関数がデリゲートに変換されておらず、ローカル関数でキャプチャされたいずれの変数も、デリゲートに変換された他のラムダやローカル関数でキャプチャされていない場合、コンパイラはヒープの割り当てを回避できます。

次の非同期の例について考えます。

[!code-csharp[TaskLambdaExample](../../samples/snippets/csharp/new-in-7/AsyncWork.cs#36_TaskLambdaExample "Task returning method with lambda expression")]

このラムダ式のクロージャに含まれるのは、`address`、`index`、および `name` 変数です。 ローカル関数の場合、クロージャを実装するオブジェクトが `struct` になる場合があります。 その構造体型はローカル関数に参照によって渡されます。 この実装の違いにより、割り当てが少なくなります。

ラムダ式に必要なインスタンス化では、余分なメモリの割り当てが必要となり、タイム クリティカルなコード パスに影響を与えるパフォーマンス因子となる可能性があります。
ローカル関数では、このオーバーヘッドは発生しません。 上記の例では、ローカル関数のバージョンは、ラムダ式のバージョンよりも割り当てが 2 つ少なくなっています。

> [!NOTE]
> このメソッドのローカル関数と同等のものも、同じクロージャのクラスを使用します。 ローカル関数のクロージャが `class` として実装される場合でも、実装の詳細が `struct` である場合でも同様です。 ローカル関数は `struct` を使用する場合がありますが、ラムダは常に `class` を使用します。

[!code-csharp[TaskLocalFunctionExample](../../samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

この例では説明しませんが、最後の 1 つの利点は値のシーケンスを生成するために `yield return` 構文を使用して、ローカル関数を反復子として実装できることです。 ラムダ式では `yield return` ステートメントは許可されません。

ローカル関数はラムダ式より冗長に思えるかもしれませんが、実際にはさまざまな目的に役立ち、用途もさまざまです。
ローカル関数は、別のメソッドのコンテキストからのみ呼び出される関数を記述する場合に、より効率が高くなります。
