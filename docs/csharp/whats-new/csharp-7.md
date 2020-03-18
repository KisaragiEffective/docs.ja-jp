---
title: C# 7.0 の新機能 - C# ガイド
description: C# 言語のバージョン 7.0 での新機能の概要を説明します。
ms.date: 02/20/2019
ms.assetid: fd41596d-d0c2-4816-b94d-c4d00a5d0243
ms.openlocfilehash: a6ac5c00ceb2ce8e5e56e2a86a8cde937d5108e2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398341"
---
# <a name="whats-new-in-c-70"></a>C# 7.0 の新機能

C# 7.0 では、C# 言語に多くの新機能が追加されます。

- [`out` 変数](#out-variables)
  - `out` の値は、それが使用されるメソッドの引数としてインラインで宣言できます。
- [タプル](#tuples)
  - 複数のパブリック フィールドを含む、軽量で名前のない型を作成できます。 コンパイラおよび IDE ツールでは、このような型のセマンティクスが認識されます。
- [破棄](#discards)
  - 破棄は、割り当てられた値を考慮しない場合に割り当てで使用された、一時的な書き込み専用の値です。 タプルおよびユーザー定義の型を分解する場合や、メソッドを `out` パラメーターを使用して呼び出す場合に特に便利です。
- [パターン一致](#pattern-matching)
  - これらの型のメンバーの任意の型と値に基づいて、分岐ロジックを作成できます。
- [`ref` ローカル変数と戻り値](#ref-locals-and-returns)
  - メソッドのローカル変数と戻り値は、他のストレージへの参照になります。
- [ローカル関数](#local-functions)
  - 関数を他の関数の中に入れ子にして、関数のスコープと可視性を制限することができます。
- [式形式のメンバーの追加](#more-expression-bodied-members)
  - 式を使用して作成できるメンバーが増加しました。
- [`throw` 式](#throw-expressions)
  - `throw` がステートメントだったためにこれまで許可されなかったコード コンストラクトで例外をスローできるようになりました。
- [一般化された async の戻り値の型](#generalized-async-return-types)
  - `async` 修飾子を使って宣言したメソッドは、`Task` と `Task<T>` に加えて他の型を返すことができます。
- [数値リテラルの構文の改善](#numeric-literal-syntax-improvements)
  - 新しいトークンにより、数値定数の読みやすさが向上します。

この記事の残りでは、各機能の概要について説明します。 機能ごとに、その背後にある論拠のほか、 構文についても説明します。 `dotnet try` グローバル ツールを使って、これらの機能をご自身の環境で調べることができます。

1. [dotnet try](https://github.com/dotnet/try/blob/master/README.md#setup) グローバル ツールをインストールします。
1. [dotnet/try-samples](https://github.com/dotnet/try-samples) リポジトリを複製します。
1. 現在のディレクトリを、*try-samples* リポジトリの *csharp7* サブディレクトリに設定します。
1. `dotnet try` を実行します。

## <a name="out-variables"></a>`out` 変数

`out` パラメーターをサポートする既存の構文は、このバージョンで改良されました。 現在は、別の宣言ステートメントを記述するのではなく、メソッド呼び出しの引数リストで `out` 変数を宣言できるようになりました。

[!code-csharp[OutVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVariableDeclarations "Out variable declarations")]

上記のように、わかりやすくするために `out` 変数の型を指定することができます。 ただし、この言語では、次のように暗黙的に型指定されたローカル変数を使用できます。

[!code-csharp[OutVarVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVarVariableDeclarations "Implicitly typed Out variable")]

- コードが読みやすくなる。
  - out 変数は、使用する場所で宣言します。その場所より上にある別の行で宣言しません。
- 初期値を割り当てる必要がない。
  - `out` 変数は、メソッド呼び出し内の使用場所で宣言することにより、割り当てる前に誤って使用することがなくなります。

## <a name="tuples"></a>タプル

C# には、設計の意図を説明するために使用される、クラスと構造体の豊富な構文が用意されています。 ところが、場合によっては、その豊富な構文を使用するために、余分な作業が必要になることがあります。この場合、メリットはごくわずかです。 複数のデータ要素を含む単純な構造を必要とするメソッドを記述することはよくあります。 このようなシナリオをサポートするために、C# には "*タプル*" が追加されました。 タプルとは、データ メンバーを表す複数のフィールドを含む軽量なデータ構造です。
フィールドは検証されず、独自のメソッドを定義することはできません。

> [!NOTE]
> タプルは C# 7.0 より前で使用できましたが、効率的でなく、言語サポートがありませんでした。
> これは、タプル要素が `Item1` や `Item2` などとしてのみ参照できることを意味しました。 C# 7.0 では、タプルの言語サポートが導入されたことで、新しい、より効率的なタプル型を使用するフィールドのセマンティック名が有効になります。

各メンバーに値を割り当て、任意でタプルの各メンバーにセマンティック名を付けることでタプルを作成できます。

[!code-csharp[NamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#NamedTuple "Named tuple")]

`namedLetters` タプルには、`Alpha` と `Beta` と呼ばれるフィールドが含まれています。 これらの名前は、コンパイル時にのみ存在し、実行時にリフレクションを使用してタプルを検査するときなどには保持されません。

タプルの割り当てでは、代入の右辺でフィールドの名前を指定することもできます。

[!code-csharp[ImplicitNamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#ImplicitNamedTuple "Implicitly named tuple")]

状況によっては、メソッドから返されたタプルのメンバーをばらすことが必要になる場合もあります。  そのためには、タプル内のそれぞれの値に対して別個の変数を宣言します。 このばらす行為は、タプルの*分解*と呼ばれます。

[!code-csharp[CallingWithDeconstructor](~/samples/snippets/csharp/new-in-7/program.cs#CallingWithDeconstructor "Deconstructing a tuple")]

.NET でも任意の型に同様の分解を指定することができます。 クラスのメンバーとして `Deconstruct` メソッドを記述します。 その `Deconstruct` メソッドは、抽出する各プロパティ用に一連の `out` 引数を提供します。 次の `Point` クラスを考えてみましょう。このクラスは、`X` 座標と `Y` 座標を抽出するデコンストラクター メソッドを指定しています。

[!code-csharp[PointWithDeconstruction](~/samples/snippets/csharp/new-in-7/point.cs#PointWithDeconstruction "Point with deconstruction method")]

`Point` をタプルに割り当てて、個々のフィールドを抽出できます。

[!code-csharp[DeconstructPoint](~/samples/snippets/csharp/new-in-7/program.cs#DeconstructPoint "Deconstruct a point")]

タプルの詳細については、[タプルに関する記事](../tuples.md)を参照してください。

## <a name="discards"></a>破棄

`out` パラメーターを使用してタプルを分解したりメソッドを呼び出したりする場合に、使用する予定がなく、考慮にも入れない値の変数の定義を強制されることが多くあります。 C# ではこのシナリオを処理するために*破棄*のサポートを追加しています。 破棄は名前が `_` (アンダースコア (_) 文字) の書き込み専用の変数で、破棄するすべての値をこの 1 つの変数に割り当てることができます。 破棄は未割り当ての変数に似ています。代入ステートメントとは異なり、破棄はコードで使用できません。

次のシナリオでは破棄はサポートされません。

- タプルまたはユーザー定義の型を分解する場合。
- [out](../language-reference/keywords/out-parameter-modifier.md) パラメーターを使用してメソッドを呼び出す場合。
- [is](../language-reference/keywords/is.md) および [switch](../language-reference/keywords/switch.md) ステートメントによるパターン マッチング操作。
- 割り当ての値を破棄として明示的に識別する必要がある場合の、スタンドアロン識別子。

次の例では、ある都市の 2 つの異なる年度のデータを含む 6 つのタプルを戻す `QueryCityDataForYears` メソッドを定義しています。 この例のメソッド呼び出しでは、メソッドによって戻された 2 つの人口の値のみが考慮されているため、タプルの残りの値はタプルの分解時に破棄として扱われます。

[!code-csharp[Tuple-discard](~/samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

詳細については、[破棄](../discards.md)に関するページを参照してください。

## <a name="pattern-matching"></a>パターン マッチング

"*パターン マッチング*" は、オブジェクトの型以外のプロパティでメソッドのディスパッチを実装できるようにする機能です。 オブジェクトの型に基づいたメソッドのディスパッチに慣れている方も多いはずです。 オブジェクト指向プログラミングでは、仮想メソッドとオーバーライド メソッドには、オブジェクトの型に基づくメソッドのディスパッチを実装するための言語構文が用意されています。 基底クラスと派生クラスは異なる実装を提供します。
パターン マッチング式により、この概念が拡張されたため、継承階層を介して関連していない型やデータ要素に同様のディスパッチ パターンを簡単に実装できるようになります。

パターン マッチングでは、`is` 式と `switch` 式がサポートされています。 どちらの式でも、オブジェクトとそのプロパティを検査して、そのオブジェクトが必要なパターンを満たしているかどうかを判定できます。 パターンに追加の規則を指定するには、`when` キーワードを使用します。

`is` パターン式を使用すると、使い慣れた [`is` 演算子](../language-reference/keywords/is.md#pattern-matching-with-is)を拡張し、その型を超えてオブジェクトを照会したり、1 つの命令で結果を割り当てたりできます。 次のコードでは、変数が `int` であるかどうかが確認されます。int の場合、現在の合計に追加されます。

```csharp
if (input is int count)
    sum += count;
```

先の小さな例では、`is` 式の拡張が示されています。 値の型や参照型に対してテストしたり、正しい型の新しい変数に成功した結果を割り当てたりできます。

switch 一致式には、既に C# 言語に含まれている `switch` ステートメントに基づいた、使い慣れた構文があります。 更新された switch 式には新しいコンストラクトがいくつか含まれます。

- `switch` 式を制御する型は、整数型、`Enum` 型、`string`、あるいはそれらの型のいずれかに対応する null 許容型に制限されなくなります。 任意の型を使用できます。
- 各 `switch` ラベルで `case` 式の型をテストできます。 `is` 式と同様に、その型に新しい変数を割り当てることができます。
- `when` 句を追加し、その変数で条件をさらにテストできます。
- `case` ラベルの順序が重要になります。 一致する最初の分岐が実行されます。他の分岐はスキップされます。

次のコードでこれらの新しい機能を確認できます。

```csharp
public static int SumPositiveNumbers(IEnumerable<object> sequence)
{
    int sum = 0;
    foreach (var i in sequence)
    {
        switch (i)
        {
            case 0:
                break;
            case IEnumerable<int> childSequence:
            {
                foreach(var item in childSequence)
                    sum += (item > 0) ? item : 0;
                break;
            }
            case int n when n > 0:
                sum += n;
                break;
            case null:
                throw new NullReferenceException("Null found in sequence");
            default:
                throw new InvalidOperationException("Unrecognized type");
        }
    }
    return sum;
}
```

- `case 0:` はおなじみの定数パターンです。
- `case IEnumerable<int> childSequence:` は型パターンです。
- `case int n when n > 0:` は `when` 条件が追加された型パターンです。
- `case null:` は null パターンです。
- `default:` はおなじみの既定ケースです。

パターン マッチングの詳細については、[C# のパターン マッチング](../pattern-matching.md)を参照してください。

## <a name="ref-locals-and-returns"></a>ref ローカル変数と戻り値

この機能により、他の場所に定義されている変数への参照を使用したり返したりするアルゴリズムが実現します。 1 つの例として、大規模なマトリックスを使用していて、特定の特性を持つ 1 つの場所を探します。 次のメソッドでは、マトリックスのそのストレージに**参照**が返されます。

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

戻り値を `ref` として宣言し、次のコードのように、マトリックスでその値を変更できます。

[!code-csharp[AssignRefReturn](~/samples/snippets/csharp/new-in-7/Program.cs#AssignRefReturn "Assign ref return")]

C# 言語には、`ref` ローカル変数と戻り値の誤用を防ぐ規則がいくつかあります。

- メソッド シグネチャと、メソッド内のすべての `ref` ステートメントに `return` キーワードを追加する必要があります。
  - それにより、メソッド全体でメソッドは参照渡しで返すことになります。
- `ref return` は値の変数か `ref` 変数に割り当てることができます。
  - 呼び出し元により、戻り値がコピーされるかどうかが制御されます。 戻り値を割り当てるとき、`ref` 修飾子を省略すると、呼び出し元はストレージの参照ではなく、値のコピーを求めることになります。
- 標準的なメソッドの戻り値を `ref` ローカル変数に割り当てることはできません。
  - したがって、`ref int i = sequence.Count();` のようなステートメントは使用できません。
- 有効期間がメソッドの実行期間を超えない変数に `ref` を返すことはできません。
  - つまり、ローカル変数または類似のスコープの変数への参照を返すことはできません。
- `ref` ローカル変数と戻り値は、非同期メソッドと共に使用することはできません。
  - コンパイラは、非同期メソッドが戻るときに、参照先の変数が、最終的な値に設定されているかどうかを認識できません。

ref ローカル変数および ref 戻り値の追加により、値のコピーを回避したり、逆参照操作を複数回実行したりすることで、より効率的なアルゴリズムを実現できます。

戻り値に `ref` を追加することは、[ソース互換性がある変更](version-update-considerations.md#source-compatible-changes)です。 既存のコードはコンパイルされますが、参照戻り値は割り当て時にコピーされます。 呼び出し元は、戻り値を参照として格納するために、戻り値の記憶域を `ref` ローカル変数に更新する必要があります。

詳しくは、[ref](../language-reference/keywords/ref.md) キーワードに関する記事をご覧ください。

## <a name="local-functions"></a>ローカル関数

クラスの多くの設計には、1 つの場所からのみ呼び出されるメソッドが含まれます。 このような追加のプライベート メソッドを使用することで、各メソッドのサイズを小さくし、その焦点を絞ることができます。 *ローカル関数*を使用すると、別のメソッドのコンテキスト内でメソッドを宣言することができます。 ローカル関数のおかげで、クラスを読み取る際に、ローカル メソッドはそれ自体が宣言されているコンテキストからしか呼び出されないことが、簡単にわかります。

ローカル関数には、パブリック反復子メソッドとパブリック非同期メソッドという 2 つの一般的なユース ケースがあります。 どちらの種類のメソッドも、プログラマーが期待するよりも遅くエラーを報告するコードを生成します。 反復子メソッドの場合、例外が検出されるのは、返されたシーケンスを列挙するコードを呼び出した場合のみです。 非同期メソッドの場合、例外が検出されるのは、返された `Task` が待機状態になったときのみです。 次の例では、ローカル関数を使用し、反復子の実装からパラメーター検証を分ける動作を確認できます。

[!code-csharp[22_IteratorMethodLocal](~/samples/snippets/csharp/new-in-7/Iterator.cs#IteratorMethodLocal "Iterator method with local function")]

同じ手法を `async` メソッドで使用すると、引数の検証で発生する例外が非同期操作の開始前にスローされることを保証できます。

[!code-csharp[TaskExample](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

> [!NOTE]
> ローカル関数によってサポートされる設計の中には、"*ラムダ式*" を使用して実現できるものもあります。 詳細については、[ローカル関数とラムダ式](../local-functions-vs-lambdas.md)に関するページをご覧ください。

## <a name="more-expression-bodied-members"></a>式形式のメンバーの追加

C# 6 では、メンバー関数の[式形式のメンバー](csharp-6.md#expression-bodied-function-members)と読み取り専用プロパティが導入されました。 C# 7.0 では、式として実装できる許可されたメンバーが拡張されます。 C# 7.0 では、"*コンストラクター*"、"*ファイナライザー*"、`get` アクセサー、および `set` アクセサーを "*プロパティ*" と "*インデクサー*" に実装できます。 それぞれの例を次のコードに示します。

[!code-csharp[ExpressionBodiedMembers](~/samples/snippets/csharp/new-in-7/expressionmembers.cs#ExpressionBodiedEverything "new expression-bodied members")]

> [!NOTE]
> この例ではファイナライザーは必要ありませんが、その構文を紹介するために示しています。 アンマネージ リソースを解放する必要がない限り、クラスにファイナライザーを実装しないでください。 また、アンマネージ リソースを直接管理する代わりに、<xref:System.Runtime.InteropServices.SafeHandle> クラスの使用を検討する必要もあります。

式形式のメンバーに関するこのような新しい場所は、C# 言語にとって重要なマイルストーンを表します。これらの機能は、オープンソースの [Roslyn](https://github.com/dotnet/Roslyn) プロジェクトに従事しているコミュニティ メンバーによって実装されました。

メソッドを式のようなメンバーに変更することは、[バイナリ互換性がある変更](version-update-considerations.md#binary-compatible-changes)です。

## <a name="throw-expressions"></a>throw 式

C# では、`throw` は常にステートメントでした。 `throw` は式ではなくステートメントであるため、使用できない C# コンストラクトがありました。 これには、条件式、null 結合式、および一部のラムダ式が含まれます。 式形式のメンバーが追加されたことにより、さらに多くの場所で `throw` 式が役に立つようになりました。 C# 7.0 では、このようなコンストラクトを記述できるように、[*throw 式*](../language-reference/keywords/throw.md#the-throw-expression)が導入されています。

この導入により、式ベースのコードをたくさん記述することが簡単になります。 エラー チェックのためにステートメントを追加する必要はありません。

## <a name="generalized-async-return-types"></a>一般化された async の戻り値の型

非同期メソッドから `Task` オブジェクトを返すと、特定のパスでパフォーマンスのボトルネックが発生する可能性があります。 `Task` は参照型です。したがって、これを使うことは、オブジェクトを割り当てることを意味します。 `async` 修飾子で宣言されたメソッドがキャッシュされた結果を返すか、同期的に完了する場合、追加の割り当ては、コードのパフォーマンスが重要なセクションにおいて大きな時間コストにつながります。 厳密なループ処理でこのような割り当てが発生した場合、コストがかかる場合があります。

新しい言語機能では、非同期メソッドの戻り値の型が `Task`、`Task<T>`、`void` に限定されません。 返される型は引き続き非同期パターンを満たす必要があります。つまり、`GetAwaiter` メソッドはアクセス可能である必要があります。 1 つの具体的な例として、この新しい言語機能を使用するために .NET に `ValueTask` 型が追加されました。

[!code-csharp[UsingValueTask](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#UsingValueTask "Using ValueTask")]

> [!NOTE]
> [ 型を使用するには、NuGet パッケージ `System.Threading.Tasks.Extensions`](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/)<xref:System.Threading.Tasks.ValueTask%601> を追加する必要があります。

この機能強化は、ライブラリの作成時、パフォーマンス クリティカルなコードに `Task` を割り当てることを回避する目的で非常に便利です。

## <a name="numeric-literal-syntax-improvements"></a>数値リテラルの構文の改善

数値定数を読み間違えると、コードを初めて読むときにコードを理解するのが難しくなる場合があります。 ビット マスクやその他の記号を用いた値では誤解を招きやすくなります。 C# 7.0 では、使用目的に応じて最も読みやすい形式で数値を記述しできるように、*バイナリ リテラル*と*桁区切り文字*という 2 つの新機能が導入されました。

ビット マスクを作成しているときや、数値をバイナリで表現するとコードが最も読みやすくなる場合は、数字をバイナリで記述します。

[!code-csharp[ThousandSeparators](~/samples/snippets/csharp/new-in-7/Program.cs#ThousandSeparators "Thousands separators")]

定数の先頭にある `0b` は、数値が 2 進数として記述されていることを示します。 バイナリ数値は非常に長くなる場合があるため、ビット パターンが見やすくなるように、桁区切り記号として `_` が導入されました。上の画像のバイナリ定数で確認できます。 桁区切り記号は定数のどこにでも置くことができます。 10 進数の場合は、3 桁の区切り記号として使用するのが一般的です。

[!code-csharp[LargeIntegers](~/samples/snippets/csharp/new-in-7/Program.cs#LargeIntegers "Large integer")]

桁区切り記号は、`decimal` 型、`float` 型、`double` 型でも使用できます。

[!code-csharp[OtherConstants](~/samples/snippets/csharp/new-in-7/Program.cs#OtherConstants "non-integral constants")]

これらをまとめると、数値定数をさらに見やすい状態で宣言できます。
