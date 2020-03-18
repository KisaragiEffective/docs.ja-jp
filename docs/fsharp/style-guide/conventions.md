---
title: F# のコーディング規則
description: コードを記述F#するときの一般的なガイドラインと表現方法について説明します。
ms.date: 01/15/2020
ms.openlocfilehash: ca86bcf714d2fb4ee5f173ee54ba12c317f9abe7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737827"
---
# <a name="f-coding-conventions"></a>F# のコーディング規則

次の規則は、大規模F#なコードベースでの作業経験からのものです。 [優れF#たコードの5つの原則](index.md#five-principles-of-good-f-code)は、各推奨事項の基礎となります。 これらは[ F#コンポーネントのデザインガイドライン](component-design-guidelines.md)に関連していますが、 F#ライブラリなどのコンポーネントだけでなく、任意のコードに適用できます。

## <a name="organizing-code"></a>コードの整理

F#コードを整理するための主な方法として、モジュールと名前空間の2つの機能があります。 これらは似ていますが、次の点が異なります。

* 名前空間は、.NET 名前空間としてコンパイルされます。 モジュールは静的クラスとしてコンパイルされます。
* 名前空間は常に最上位レベルです。 モジュールはトップレベルで、他のモジュール内で入れ子にすることができます。
* 名前空間は、複数のファイルにまたがることができます。 モジュールはできません。
* モジュールは、`[<RequireQualifiedAccess>]` と `[<AutoOpen>]`で装飾できます。

次のガイドラインは、これらを使用してコードを整理する際に役立ちます。

### <a name="prefer-namespaces-at-the-top-level"></a>最上位レベルで名前空間を優先する

パブリックに使用できるコードでは、最上位レベルのモジュールに対して名前空間が優先されます。 これらは .NET 名前空間としてコンパイルされるためC# 、では問題ありません。

```fsharp
// Good!
namespace MyCode

type MyClass() =
    ...
```

トップレベルモジュールの使用は、からF#のみ呼び出された場合には異なる場合C#がありますが、コンシューマーにとっては、`MyCode` モジュールで `MyClass` を修飾する必要があるため、呼び出し元が驚かれる可能性があります。

```fsharp
// Bad!
module MyCode

type MyClass() =
    ...
```

### <a name="carefully-apply-autoopen"></a>慎重に `[<AutoOpen>]` を適用する

`[<AutoOpen>]` コンストラクトは、呼び出し元が使用できるもののスコープを汚染ことができます。また、その結果は "マジック" になります。 これは適切な方法ではありません。 このルールの例外はF#コアライブラリ自体です (ただし、これは少しの議論でもあります)。

ただし、パブリック api のヘルパー機能があり、パブリック api とは別に整理する必要がある場合に便利です。

```fsharp
module MyAPI =
    [<AutoOpen>]
    module private Helpers =
        let helper1 x y z =
            ...

    let myFunction1 x =
        let y = ...
        let z = ...

        helper1 x y z
```

これにより、呼び出しのたびにヘルパーを完全に修飾することなく、関数のパブリック API から実装の詳細を明確に分離できます。

さらに、拡張メソッドおよび式ビルダーを名前空間レベルで公開することは、`[<AutoOpen>]`でも使用できます。

### <a name="use-requirequalifiedaccess-whenever-names-could-conflict-or-you-feel-it-helps-with-readability"></a>名前が競合する場合や、読みやすさを向上させる場合は、`[<RequireQualifiedAccess>]` を使用する

`[<RequireQualifiedAccess>]` 属性をモジュールに追加すると、モジュールが開かれていないこと、およびモジュールの要素への参照に明示的な修飾アクセスが必要であることが示されます。 たとえば、`Microsoft.FSharp.Collections.List` モジュールにはこの属性があります。

これは、モジュールの関数と値の名前が、他のモジュールの名前と競合する可能性がある場合に便利です。 修飾されたアクセスを必要とすると、ライブラリの長期的な保守性と発展性を大幅に向上させることができます。

```fsharp
[<RequireQualifiedAccess>]
module StringTokenization =
    let parse s = ...

...

let s = getAString()
let parsed = StringTokenization.parse s // Must qualify to use 'parse'
```

### <a name="sort-open-statements-topologically"></a>`open` ステートメントの並べ替え位相的

でF#は、`open` ステートメントを含め、宣言の順序が重要になります。 これはとC#は異なります。 `using` および `using static` の効果は、ファイル内のステートメントの順序に依存しません。

でF#は、スコープ内で開かれた要素は、既に存在する他の要素をシャドウできます。 これは、`open` ステートメントの順序を変更すると、コードの意味が変更される可能性があることを意味します。 その結果、すべての `open` ステートメント (英数字順など) の任意の並べ替えを行うことは推奨されません。ようを使用すると、必要に応じて異なる動作が生成されます。

代わりに、[位相的](https://en.wikipedia.org/wiki/Topological_sorting)を並べ替えることをお勧めします。つまり、`open` ステートメントは、システムの_層_が定義されている順序で並べ替えます。 異なるトポロジレイヤー内で英数字の並べ替えを行うこともできます。

例として、 F#コンパイラサービスのパブリック API ファイルのトポロジの並べ替えを次に示します。

```fsharp
namespace Microsoft.FSharp.Compiler.SourceCodeServices

open System
open System.Collections.Generic
open System.Collections.Concurrent
open System.Diagnostics
open System.IO
open System.Reflection
open System.Text

open Microsoft.FSharp.Compiler
open Microsoft.FSharp.Compiler.AbstractIL
open Microsoft.FSharp.Compiler.AbstractIL.Diagnostics
open Microsoft.FSharp.Compiler.AbstractIL.IL
open Microsoft.FSharp.Compiler.AbstractIL.ILBinaryReader
open Microsoft.FSharp.Compiler.AbstractIL.Internal
open Microsoft.FSharp.Compiler.AbstractIL.Internal.Library

open Microsoft.FSharp.Compiler.AccessibilityLogic
open Microsoft.FSharp.Compiler.Ast
open Microsoft.FSharp.Compiler.CompileOps
open Microsoft.FSharp.Compiler.CompileOptions
open Microsoft.FSharp.Compiler.Driver
open Microsoft.FSharp.Compiler.ErrorLogger
open Microsoft.FSharp.Compiler.Infos
open Microsoft.FSharp.Compiler.InfoReader
open Microsoft.FSharp.Compiler.Lexhelp
open Microsoft.FSharp.Compiler.Layout
open Microsoft.FSharp.Compiler.Lib
open Microsoft.FSharp.Compiler.NameResolution
open Microsoft.FSharp.Compiler.PrettyNaming
open Microsoft.FSharp.Compiler.Parser
open Microsoft.FSharp.Compiler.Range
open Microsoft.FSharp.Compiler.Tast
open Microsoft.FSharp.Compiler.Tastops
open Microsoft.FSharp.Compiler.TcGlobals
open Microsoft.FSharp.Compiler.TypeChecker
open Microsoft.FSharp.Compiler.SourceCodeServices.SymbolHelpers

open Internal.Utilities
open Internal.Utilities.Collections
```

1本の改行では、各レイヤーが後で英数字順に並べ替えられるように、トポロジレイヤーが分離されていることに注意してください。 これにより、誤って値をシャドウすることなくコードを整理できます。

## <a name="use-classes-to-contain-values-that-have-side-effects"></a>クラスを使用して副作用のある値を格納する

値の初期化には、データベースまたはその他のリモートリソースへのコンテキストのインスタンス化など、副作用が生じる場合が多くあります。 モジュール内でこのような処理を初期化し、後続の関数で使用することをお勧めします。

```fsharp
// This is bad!
module MyApi =
    let dep1 = File.ReadAllText "/Users/{your name}/connectionstring.txt"
    let dep2 = Environment.GetEnvironmentVariable "DEP_2"

    let private r = Random()
    let dep3() = r.Next() // Problematic if multiple threads use this

    let function1 arg = doStuffWith dep1 dep2 dep3 arg
    let function2 arg = doSutffWith dep1 dep2 dep3 arg
```

多くの場合、これは次のような理由で不適切です。

まず、アプリケーションの構成は、`dep1` と `dep2`を使用してコードベースにプッシュされます。 これは、より大きなコードベースで維持するのは困難です。

2番目に、静的に初期化されたデータには、コンポーネントが複数のスレッドを使用する場合に、スレッドセーフではない値を含めないでください。 これは、`dep3`によって明確に違反されます。

最後に、モジュールの初期化は、コンパイルユニット全体の静的コンストラクターにコンパイルされます。 このモジュールの let バインド値の初期化でエラーが発生した場合、そのモジュールは、アプリケーションの有効期間全体にわたってキャッシュされる `TypeInitializationException` としてマニフェストになります。 これは診断が困難な場合があります。 通常は、その理由を試みることができる内部例外がありますが、存在しない場合、根本的な原因については何も通知されません。

代わりに、単純なクラスを使用して依存関係を保持するだけです。

```fsharp
type MyParametricApi(dep1, dep2, dep3) =
    member _.Function1 arg1 = doStuffWith dep1 dep2 dep3 arg1
    member _.Function2 arg2 = doStuffWith dep1 dep2 dep3 arg2
```

これにより、次のことが可能になります。

1. API 自体の外部に依存状態をプッシュします。
2. API の外部で構成を実行できるようになりました。
3. 依存する値の初期化でエラーが発生しても、`TypeInitializationException`としてマニフェストになることはありません。
4. API のテストが簡単になりました。

## <a name="error-management"></a>エラー管理

大規模なシステムでのエラー管理は複雑で微妙な的な作業であり、システムがフォールトトレラントで動作しているかどうかを確認するためのシルバーの箇条書きはありません。 次のガイドラインでは、このような困難な領域に移動する際のガイダンスを提供する必要があります。

### <a name="represent-error-cases-and-illegal-state-in-types-intrinsic-to-your-domain"></a>ドメインに固有の型のエラーケースと無効な状態を表します。

[判別共用体](../language-reference/discriminated-unions.md)をF#使用すると、によって、型システム内の問題のあるプログラムの状態を表すことができます。 次に例を示します。

```fsharp
type MoneyWithdrawalResult =
    | Success of amount:decimal
    | InsufficientFunds of balance:decimal
    | CardExpired of DateTime
    | UndisclosedFailure
```

この場合、銀行口座からの送金に失敗する可能性がある既知の方法が3つあります。 各エラーケースは型で表されるため、プログラム全体で安全に処理できます。

```fsharp
let handleWithdrawal amount =
    let w = withdrawMoney amount
    match w with
    | Success am -> printfn "Successfully withdrew %f" am
    | InsufficientFunds balance -> printfn "Failed: balance is %f" balance
    | CardExpired expiredDate -> printfn "Failed: card expired on %O" expiredDate
    | UndisclosedFailure -> printfn "Failed: unknown"
```

一般に、ドメイン内で何らかのエラーが**発生**する可能性のあるさまざまな方法をモデル化できる場合、通常のプログラムフローに加えて、エラー処理コードは処理する必要のあるものとして扱われなくなりました。 これは単に通常のプログラムフローの一部であり、**例外的**とは見なされません。 これには主に次の2つの利点があります。

1. 時間の経過と共にドメインが変化するため、保守が容易になります。
2. エラーケースは、単体テストの方が簡単です。

### <a name="use-exceptions-when-errors-cannot-be-represented-with-types"></a>エラーを型で表すことができない場合に例外を使用する

問題のあるドメインでは、すべてのエラーを表すことはできません。 この種のエラーは本質的に*例外的*であり、でF#例外を発生させてキャッチする機能があります。

まず、[例外のデザインガイドライン](../../standard/design-guidelines/exceptions.md)を読むことをお勧めします。 これらは、にF#も適用できます。

例外を発生させるF#ためにで使用できる主な構成要素は、次の優先順位で考慮する必要があります。

| Function | 構文 | 目的 |
|----------|--------|---------|
| `nullArg` | `nullArg "argumentName"` | 指定された引数名を使用して `System.ArgumentNullException` を発生させます。 |
| `invalidArg` | `invalidArg "argumentName" "message"` | 指定された引数名とメッセージを使用して `System.ArgumentException` を発生させます。 |
| `invalidOp` | `invalidOp "message"` | 指定したメッセージを使用して `System.InvalidOperationException` を発生させます。 |
|`raise`| `raise (ExceptionType("message"))` | 例外をスローするための汎用的な機構。 |
| `failwith` | `failwith "message"` | 指定したメッセージを使用して `System.Exception` を発生させます。 |
| `failwithf` | `failwithf "format string" argForFormatString` | 書式指定文字列とその入力によって決定されたメッセージを使用して、`System.Exception` を発生させます。 |

必要に応じて `ArgumentNullException`、`ArgumentException`、および `InvalidOperationException` をスローするメカニズムとして、`nullArg`、`invalidArg` および `invalidOp` を使用します。

`failwith` 関数と `failwithf` 関数は、特定の例外ではなく基本 `Exception` 型を発生させるため、通常は避ける必要があります。 [例外のデザインガイドライン](../../standard/design-guidelines/exceptions.md)に従って、可能な限りより具体的な例外を生成する必要があります。

### <a name="using-exception-handling-syntax"></a>例外処理構文の使用

F#では、`try...with` 構文を使用した例外パターンがサポートされています。

```fsharp
try
    tryGetFileContents()
with
| :? System.IO.FileNotFoundException as e -> // Do something with it here
| :? System.Security.SecurityException as e -> // Do something with it here
```

パターンマッチングを使用して例外が発生したときに実行する機能を調整することは、コードをクリーンな状態に保つ必要がある場合に少し厄介になることがあります。 このような処理を行う1つの方法として、[アクティブパターン](../language-reference/active-patterns.md)を使用して、エラーの発生したケースを例外自体にグループ化する方法があります。 たとえば、例外がスローされたときに、例外メタデータで重要な情報を囲む API を使用している可能性があります。 アクティブパターン内でキャプチャされた例外の本体にある有用な値をラップ解除し、状況によってはこの値を返すことができます。

### <a name="do-not-use-monadic-error-handling-to-replace-exceptions"></a>Monadic のエラー処理を使用して例外を置き換えることはできません

関数型プログラミングでは、例外がいくつかのタブに表示されます。 実際には、例外は純粋性に違反するため、非常に機能していないと考えるのは安全です。 ただし、これにより、コードを実行する必要がある場合は無視され、実行時エラーが発生する可能性があります。 一般に、望ましくない問題を最小限に抑えるために、ほとんどのものが純粋でも合計でもないことを前提としてコードを記述します。

.NET ランタイムでの関連性と妥当性、および言語間のエコシステム全体に関して、例外の次の主要な長所や側面を考慮することが重要です。

1. これらには詳細な診断情報が含まれており、問題をデバッグするときに非常に便利です。
2. これらは、ランタイムやその他の .NET 言語によってよく理解されています。
3. これらのコードを使用すると、セマンティクスの一部のサブセットをアドホックベースで実装することによって例外を*回避*するコードと比較すると、重要な定型句を減らすことができます。

この3番目のポイントは重要です。 複雑な操作の場合、例外を使用しないと、次のような構造体が処理される可能性があります。

```fsharp
Result<Result<MyType, string>, string list>
```

これは、"stringly 型の" エラーでのパターンマッチングのような脆弱なコードにつながる可能性があります。

```fsharp
let result = doStuff()
match result with
| Ok r -> ...
| Error e ->
    if e.Contains "Error string 1" then ...
    elif e.Contains "Error string 2" then ...
    else ... // Who knows?
```

さらに、"飲み込む" 型を返す "単純な" 関数を希望する場合には、例外が発生する可能性があります。

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with _ -> None
```

残念ながら、`tryReadAllText` は、ファイルシステムで発生する可能性のある多数の要因に基づいて多数の例外をスローすることがあります。このコードでは、実際の環境で実際に問題が発生する可能性がある情報を破棄します。 このコードを結果の型に置き換えると、"stringly" というエラーメッセージの解析に戻ります。

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Ok
    with e -> Error e.Message

let r = tryReadAllText "path-to-file"
match r with
| Ok text -> ...
| Error e ->
    if e.Contains "uh oh, here we go again..." then ...
    else ...
```

例外オブジェクト自体を `Error` コンストラクターに配置すると、関数ではなく、呼び出しサイトで例外の型を適切に処理するだけで済みます。 これを有効にすると、確認済みの例外が効果的に作成されます。これは、API の呼び出し元として扱うことができないことが周知されています。

上記の例の代わりに、*特定*の例外をキャッチし、その例外のコンテキストで意味のある値を返す方法があります。 `tryReadAllText` 関数を次のように変更すると、`None` の意味が大きくなります。

```fsharp
let tryReadAllTextIfPresent (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with :? FileNotFoundException -> None
```

この関数は、キャッチ全体として機能するのではなく、ファイルが見つからなかった場合に適切に処理し、その意味を戻り値に割り当てるようになりました。 この戻り値は、そのエラーケースにマップできますが、コンテキスト情報を破棄したり、コード内のその時点で関連しない可能性のあるケースを呼び出し元に処理したりすることはできません。

`Result<'Success, 'Error>` などの型は、入れ子になっていない基本操作にF#適してい*ます。オプション*の型は、何かが*何かを返す可能性*がある場合に最適です。 ただし、例外の代わりにはなりませんが、例外の置換を試行する場合には使用しないでください。 代わりに、対象の方法で例外およびエラー管理ポリシーの特定の側面に対処するために、慎重に適用する必要があります。

## <a name="partial-application-and-point-free-programming"></a>部分的なアプリケーションとポイントフリープログラミング

F#部分アプリケーションをサポートします。したがって、ポイントフリースタイルでプログラミングするさまざまな方法がサポートされます。 これは、モジュール内でのコードの再利用や何らかの実装に役立ちますが、パブリックに公開するものではありません。 一般に、ポイントフリープログラミングは、それ自体とは関係がありません。そのため、スタイルに専念ない人にとって重要な認知バリアを追加できます。

### <a name="do-not-use-partial-application-and-currying-in-public-apis"></a>パブリック Api で部分的なアプリケーションとカリー化を使用しない

例外がほとんどない場合、パブリック Api で部分的なアプリケーションを使用すると、コンシューマーにとって混乱を招く可能性があります。 通常、コード内のF# `let`バインドされた値は**値**であり、関数の**値**ではありません。 値と関数の値を混在させると、十分な数のコードを exchange に保存することができます。これは特に、`>>` などの演算子と組み合わせて関数を作成する場合に発生します。

### <a name="consider-the-tooling-implications-for-point-free-programming"></a>ポイントフリープログラミングのツールへの影響について検討する

カリー化関数では、引数にラベルが付けられません。 これにはツールの影響があります。 次の2つの関数について考えてみます。

```fsharp
let func name age =
    printfn "My name is %s and I am %d years old!" name age

let funcWithApplication =
    printfn "My name is %s and I am %d years old!"
```

どちらも有効な関数ですが、`funcWithApplication` はカリー化関数です。 エディターでその型をポイントすると、次のように表示されます。

```fsharp
val func : name:string -> age:int -> unit

val funcWithApplication : (string -> int -> unit)
```

呼び出しサイトでは、Visual Studio などのツールのツールヒントには、`string` および `int` 入力型が実際に表している内容についての意味のある情報は記載されていません。

パブリックに使用できる `funcWithApplication` のようなポイントフリーのコードが発生した場合は、ηを完全に展開することをお勧めします。これにより、ツールが引数に対して意味のある名前を取得できるようになります。

さらに、ポイントフリーのコードのデバッグは困難な場合もあります。 デバッグツールは、名前にバインドされた値 (`let` バインドなど) に依存しているので、実行の途中で中間値を検査できます。 コードに検査する値がない場合、デバッグするものはありません。 将来的には、デバッグツールは、以前に実行されたパスに基づいてこれらの値を合成するために発展することがありますが、*潜在的*なデバッグ機能については、お客様のコンテンツを生垣するのは得策ではありません。

### <a name="consider-partial-application-as-a-technique-to-reduce-internal-boilerplate"></a>内部の定型句を減らすための手法として部分的なアプリケーションを検討する

前の点とは対照的に、部分的なアプリケーションは、アプリケーション内の定型句または API の深い内部構造を減らすための優れたツールです。 これは、より複雑な Api の実装を単体テストする場合に役立ちます。これは、多くの場合、定型的な処理は面倒です。 たとえば、次のコードは、このようなフレームワークに外部の依存関係を持たず、関連するオーダーメイド API を習得することなく、ほとんどのモックフレームワークで提供される機能を実現する方法を示しています。

たとえば、次のようなソリューションについて考えてみます。

```
MySolution.sln
|_/ImplementationLogic.fsproj
|_/ImplementationLogic.Tests.fsproj
|_/API.fsproj
```

`ImplementationLogic.fsproj` は次のようなコードを公開する可能性があります。

```fsharp
module Transactions =
    let doTransaction txnContext txnType balance =
        ...

type Transactor(ctx, currentBalance) =
    member _.ExecuteTransaction(txnType) =
        Transactions.doTransaction ctx txtType currentBalance
        ...
```

`ImplementationLogic.Tests.fsproj` での単体テストの `Transactions.doTransaction` は簡単です。

```fsharp
namespace TransactionsTestingUtil

open Transactions

module TransactionsTestable =
    let getTestableTransactionRoutine mockContext = Transactions.doTransaction mockContext
```

モックコンテキストオブジェクトを使用して `doTransaction` を部分的に適用すると、モックコンテキストを毎回構築しなくても、すべての単体テストで関数を呼び出すことができます。

```fsharp
namespace TransactionTests

open Xunit
open TransactionTypes
open TransactionsTestingUtil
open TransactionsTestingUtil.TransactionsTestable

let testableContext =
    { new ITransactionContext with
        member _.TheFirstMember() = ...
        member _.TheSecondMember() = ... }

let transactionRoutine = getTestableTransactionRoutine testableContext

[<Fact>]
let ``Test withdrawal transaction with 0.0 for balance``() =
    let expected = ...
    let actual = transactionRoutine TransactionType.Withdraw 0.0
    Assert.Equal(expected, actual)
```

この手法は、コードベース全体に対して汎用的に適用するべきではありませんが、複雑な内部構造およびそれらの内部の単体テストのために定型句を減らすことをお勧めします。

## <a name="access-control"></a>アクセス制御

F#には、.NET ランタイムで使用できるものから継承された[アクセス制御](../language-reference/access-control.md)のための複数のオプションがあります。 これらは、型では使用できません。関数にも使用できます。

* パブリックに使用できるようにするまでは、`public` 以外の型とメンバーを優先します。 これにより、コンシューマーの数を最小限に抑えることができます。
* すべてのヘルパー機能を `private`保持するよう努めてください。
* ヘルパー関数が多数になる場合は、プライベートモジュールで `[<AutoOpen>]` を使用することを検討してください。

## <a name="type-inference-and-generics"></a>型の推論とジェネリック

型の推定では、大量の定型句を入力することができます。 また、 F#コンパイラにおける自動汎化は、追加の作業をほとんど必要とせずに、より汎用的なコードを作成するのに役立ちます。 ただし、これらの機能は、一般には良好ではありません。

* パブリック Api に明示的な型を使用して引数名にラベルを付けることを検討し、このの型の推定に依存しないでください。

    その理由は、コンパイラではなく、API の形状を制御**する必要があるため**です。 コンパイラは、型を推測する場合でも、適切なジョブを実行できますが、内部的に依存している型が変更されている場合は、API を変更することができます。 これは必要なものになりますが、ほとんどの場合、ダウンストリームのコンシューマーが対処する必要がある、API の変更が中断されます。 代わりに、パブリック API の構造を明示的に制御する場合は、これらの重大な変更を制御できます。 DDD の用語では、これは破損対策レイヤーと考えることができます。

* 汎用引数にわかりやすい名前を付けることを検討してください。

    特定のドメインに固有ではない実際の汎用コードを記述する場合を除き、わかりやすい名前を付けることで、他のプログラマが作業中のドメインを理解するのに役立ちます。 たとえば、ドキュメントデータベースと対話するコンテキストで `'Document` という名前の型パラメーターを使用すると、作業している関数またはメンバーが汎用ドキュメント型を受け入れることができます。

* 汎用型パラメーターには、パラメーターを使用して名前を付けることを検討してください。

    これは .NET での作業を行うための一般的な方法であるため、snake_case またはキャメルケースではなく、パスワードを使用することをお勧めします。

最後に、または大規模なコードベースを初めて使用するF#ユーザーにとっては、自動汎化が常に行われるとは限りません。 汎用のコンポーネントを使用する場合、認識オーバーヘッドが発生します。 さらに、自動的に一般化された関数がさまざまな入力型で使用されていない場合 (そのように使用することを意図している場合のみ)、その時点でジェネリックになるという実際の利点はありません。 記述しているコードが実際にジェネリックになるかどうかを常に考慮してください。

## <a name="performance"></a>パフォーマンス

### <a name="prefer-structs-for-small-data-types"></a>小さいデータ型に対して構造体を優先する

構造体 (値型とも呼ばれます) を使用すると、多くの場合、オブジェクトの割り当てが回避されるため、一部のコードのパフォーマンスが向上します。 ただし、構造体は常に "高速に進む" ボタンではありません。構造体のデータのサイズが16バイトを超えた場合、データをコピーすると、参照型を使用するよりも多くの CPU 時間が消費される可能性があります。

構造体を使用する必要があるかどうかを判断するには、次の条件を考慮してください。

- データのサイズが16バイト以下の場合。
- これらのデータ型の多くが、実行中のプログラムのメモリに常駐している可能性が高い場合。

最初の条件が適用される場合は、通常、構造体を使用する必要があります。 両方とも適用される場合は、ほとんどの場合、構造体を使用する必要があります。 前の条件が適用される場合もありますが、構造体の使用は参照型を使用するよりも適切ではなく、最悪の場合もあります。 ただし、このような変更を行う場合は常に測定することが重要です。また、想定または直感には作用しません。

#### <a name="prefer-struct-tuples-when-grouping-small-value-types"></a>小さい値の型をグループ化するときに構造体の組を優先する

次の2つの関数について考えてみます。

```fsharp
let rec runWithTuple t offset times =
    let offsetValues x y z offset =
        (x + offset, y + offset, z + offset)

    if times <= 0 then
        t
    else
        let (x, y, z) = t
        let r = offsetValues x y z offset
        runWithTuple r offset (times - 1)

let rec runWithStructTuple t offset times =
    let offsetValues x y z offset =
        struct(x + offset, y + offset, z + offset)

    if times <= 0 then
        t
    else
        let struct(x, y, z) = t
        let r = offsetValues x y z offset
        runWithStructTuple r offset (times - 1)
```

これらの関数を[BenchmarkDotNet](https://benchmarkdotnet.org/)のような統計ベンチマークツールでベンチマークする場合は、構造体のタプルを使用する `runWithStructTuple` 関数が40% 高速に実行され、メモリが割り当てられないことがわかります。

ただし、これらの結果は、常に独自のコードには当てはまりません。 関数を `inline`としてマークすると、参照タプルを使用しているコードがいくつかの追加の最適化を受ける可能性があります。または、割り当てられるコードが単に最適化されている可能性があります。 パフォーマンスに懸念がある場合は常に結果を測定し、想定または直感に基づいて動作することはありません。

#### <a name="prefer-struct-records-when-the-data-type-is-small"></a>データ型が小さい場合に構造体レコードを優先する

前述の経験則では、レコードの[ F#種類](../language-reference/records.md)も保持しています。 これらを処理する次のデータ型と関数について考えてみます。

```fsharp
type Point = { X: float; Y: float; Z: float }

[<Struct>]
type SPoint = { X: float; Y: float; Z: float }

let rec processPoint (p: Point) offset times =
    let inline offsetValues (p: Point) offset =
        { p with X = p.X + offset; Y = p.Y + offset; Z = p.Z + offset }

    if times <= 0 then
        p
    else
        let r = offsetValues p offset
        processPoint r offset (times - 1)

let rec processStructPoint (p: SPoint) offset times =
    let inline offsetValues (p: SPoint) offset =
        { p with X = p.X + offset; Y = p.Y + offset; Z = p.Z + offset }

    if times <= 0 then
        p
    else
        let r = offsetValues p offset
        processStructPoint r offset (times - 1)
```

これは前の組コードに似ていますが、今回の例ではレコードとインライン内部関数を使用します。

これらの関数を[BenchmarkDotNet](https://benchmarkdotnet.org/)のような統計ベンチマークツールでベンチマークすると、`processStructPoint` が約60% 高速に実行され、マネージヒープには何も割り当てられません。

#### <a name="prefer-struct-discriminated-unions-when-the-data-type-is-small"></a>データ型が小さい場合に構造体の判別共用体を優先する

構造体の[ F# ](../language-reference/discriminated-unions.md)組とレコードを使用したパフォーマンスに関する前の観察でも、判別共用体が保持されます。 次のコードについて考えてみましょう。

```fsharp
    type Name = Name of string
    
    [<Struct>]
    type SName = SName of string

    let reverseName (Name s) =
        s.ToCharArray()
        |> Array.rev
        |> string
        |> Name

    let structReverseName (SName s) =
        s.ToCharArray()
        |> Array.rev
        |> string
        |> SName
```

ドメインのモデリングには、このような単一ケースの判別共用体を定義するのが一般的です。 これらの関数を[BenchmarkDotNet](https://benchmarkdotnet.org/)のような統計ベンチマークツールでベンチマークする場合、`structReverseName` は小さい文字列の `reverseName` よりも約25% 高速に実行されることがわかります。 大きな文字列の場合は、どちらも同じを実行します。 そのため、この場合は、常に構造体を使用することをお勧めします。 既に説明したように、は常に測定し、想定または直感には作用しません。

前の例では、struct 判別共用体がパフォーマンスを向上させたことが示されていましたが、ドメインをモデル化する場合は、より大きな判別共用体を使用するのが一般的です。 このような大規模なデータ型は、それに対する操作に応じて構造体である場合には、より多くのコピーが関係する可能性があるため、実行できない可能性があります。

### <a name="functional-programming-and-mutation"></a>関数型プログラミングと変異

F#既定では、値は変更できません。これにより、特定のクラスのバグ (特に同時実行性と並列処理を伴うもの) を回避できます。 ただし、場合によっては、実行時間やメモリの割り当てを最適 (または妥当な方法で) 効率よく実現するために、状態のインプレース変化を使用して作業の範囲を実装することをお勧めします。 これは、`mutable` キーワードを使用したでF#のオプトインに使用できます。

の `mutable` を使用F#すると、機能的な純度があると思われるかもしれません。 これは理解しやすいものですが、パフォーマンスの目標に関しては、あらゆる場所で機能的な純度を持つことができます。 妥協点は、呼び出し元が関数を呼び出したときの動作を気にする必要がないように、変異をカプセル化することです。 これにより、パフォーマンスクリティカルなコードのために、変異型の実装に対して機能インターフェイスを記述できます。

#### <a name="wrap-mutable-code-in-immutable-interfaces"></a>変更可能なコードを変更できないインターフェイスにラップする

参照の透明性を目標として、パフォーマンスが重要な関数の変更可能な underbelly を公開しないコードを記述することが重要です。 たとえば、次のコードでは、 F#コアライブラリの `Array.contains` 関数を実装しています。

```fsharp
[<CompiledName("Contains")>]
let inline contains value (array:'T[]) =
    checkNonNull "array" array
    let mutable state = false
    let mutable i = 0
    while not state && i < array.Length do
        state <- value = array.[i]
        i <- i + 1
    state
```

この関数を複数回呼び出しても、基になる配列が変更されることはありません。また、使用中に変更可能な状態を維持する必要もありません。 その中のほぼすべてのコード行で変異が使用されていても、透過的に透過的になります。

#### <a name="consider-encapsulating-mutable-data-in-classes"></a>変更可能なデータをクラスにカプセル化することを検討する

前の例では、変更可能なデータを使用して操作をカプセル化するために単一の関数を使用しています これは、より複雑なデータセットには必ずしも十分ではありません。 次の一連の関数について考えてみます。

```fsharp
open System.Collections.Generic

let addToClosureTable (key, value) (t: Dictionary<_,_>) =
    if not (t.ContainsKey(key)) then
        t.Add(key, value)
    else
        t.[key] <- value

let closureTableCount (t: Dictionary<_,_>) = t.Count

let closureTableContains (key, value) (t: Dictionary<_, HashSet<_>>) =
    match t.TryGetValue(key) with
    | (true, v) -> v.Equals(value)
    | (false, _) -> false
```

このコードはパフォーマンスに優れていますが、呼び出し元が保持する変異ベースのデータ構造を公開しています。 これは、次のように変更できる基になるメンバーを持たないクラスの内部にラップできます。

```fsharp
open System.Collections.Generic

/// The results of computing the LALR(1) closure of an LR(0) kernel
type Closure1Table() =
    let t = Dictionary<Item0, HashSet<TerminalIndex>>()

    member _.Add(key, value) =
        if not (t.ContainsKey(key)) then
            t.Add(key, value)
        else
            t.[key] <- value

    member _.Count = t.Count

    member _.Contains(key, value) =
        match t.TryGetValue(key) with
        | (true, v) -> v.Equals(value)
        | (false, _) -> false
```

`Closure1Table` は、基になる変異ベースのデータ構造をカプセル化するため、呼び出し元が基になるデータ構造を維持することはできません。 クラスは、呼び出し元に詳細情報を公開せずに、変異ベースのデータとルーチンをカプセル化するための強力な方法です。

#### <a name="prefer-let-mutable-to-reference-cells"></a>セルの参照に `let mutable` を優先する

参照セルは、値自体ではなく、値への参照を表す方法です。 パフォーマンスクリティカルなコードには使用できますが、推奨されません。 次の例を確認してください。

```fsharp
let kernels =
    let acc = ref Set.empty

    processWorkList startKernels (fun kernel ->
        if not ((!acc).Contains(kernel)) then
            acc := (!acc).Add(kernel)
        ...)

    !acc |> Seq.toList
```

参照セルを使用すると、後続のすべてのコードで、基になるデータを逆参照して再参照する必要があることが "汚染" になります。 代わりに、次の `let mutable`を検討してください。

```fsharp
let kernels =
    let mutable acc = Set.empty

    processWorkList startKernels (fun kernel ->
        if not (acc.Contains(kernel)) then
            acc <- acc.Add(kernel)
        ...)

    acc |> Seq.toList
```

ラムダ式の途中での1つの変更点以外に、`acc` に触れる他のすべてのコードは、通常の `let`バインドされていない値の使用方法とは異なる方法で実行できます。 これにより、時間の経過と共に簡単に変更できるようになります。

## <a name="object-programming"></a>オブジェクトプログラミング

F#では、オブジェクトとオブジェクト指向 (OO) の概念が完全にサポートされています。 多くの OO 概念は強力で便利ですが、すべての概念を使用するのが理想的であるとは限りません。 次の一覧は、高度なレベルでの OO 特徴のカテゴリに関するガイダンスを提供します。

**さまざまな状況でこれらの機能を使用することを検討してください。**

* ドット表記 (`x.Length`)
* インスタンスのメンバー
* 暗黙的なコンストラクター
* 静的メンバー
* インデクサー表記 (`arr.[x]`)
* 名前付き引数と省略可能な引数
* インターフェイスとインターフェイスの実装

**これらの機能については先にしないでください。問題を解決するのに便利な場合は、慎重に適用してください。**

* メソッドのオーバーロード
* カプセル化された変更可能なデータ
* 型の演算子
* 自動プロパティ
* `IDisposable` と `IEnumerable` の実装
* 型拡張
* events
* 構造体
* デリゲート
* 列挙型

**一般に、これらの機能を使用する必要がない場合は、次のようにしてください。**

* 継承に基づく型の階層と実装の継承
* Null と `Unchecked.defaultof<_>`

### <a name="prefer-composition-over-inheritance"></a>継承を使ってコンポジションを優先する

[継承](https://en.wikipedia.org/wiki/Composition_over_inheritance)を使用したコンポジションは、優れF#たコードが従うことができる長い表現形式です。 基本的な原則は、基底クラスを公開せずに、呼び出し元がその基底クラスから継承して機能を取得する必要があることです。

### <a name="use-object-expressions-to-implement-interfaces-if-you-dont-need-a-class"></a>クラスが不要な場合は、オブジェクト式を使用してインターフェイスを実装する

[オブジェクト式](../language-reference/object-expressions.md)を使用すると、インターフェイスを即座に実装し、実装されたインターフェイスをクラス内ではなく値にバインドできます。 これは特に、インターフェイスを実装_する必要があり、_ 完全なクラスを必要としない場合に便利です。

たとえば、次のコードは、`open` ステートメントがないシンボルを追加した場合にコード修正アクションを提供するために[Ionide](http://ionide.io/)で実行されます。

```fsharp
    let private createProvider () =
        { new CodeActionProvider with
            member this.provideCodeActions(doc, range, context, ct) =
                let diagnostics = context.diagnostics
                let diagnostic = diagnostics |> Seq.tryFind (fun d -> d.message.Contains "Unused open statement")
                let res =
                    match diagnostic with
                    | None -> [||]
                    | Some d ->
                        let line = doc.lineAt d.range.start.line
                        let cmd = createEmpty<Command>
                        cmd.title <- "Remove unused open"
                        cmd.command <- "fsharp.unusedOpenFix"
                        cmd.arguments <- Some ([| doc |> unbox; line.range |> unbox; |] |> ResizeArray)
                        [|cmd |]
                res
                |> ResizeArray
                |> U2.Case1
        }
```

Visual Studio Code API と対話するときにはクラスは不要であるため、オブジェクト式はこのための最適なツールです。 また、テストルーチンを使用してインターフェイスをアドホックにスタブする必要がある場合に、単体テストにも役立ちます。

## <a name="consider-type-abbreviations-to-shorten-signatures"></a>型略称を検討して署名を短縮する

[型略称](../language-reference/type-abbreviations.md)は、関数シグネチャやより複雑な型など、ラベルを別の型に割り当てるのに便利な方法です。 たとえば、次のエイリアスは、ディープラーニングライブラリである[Cntk](https://docs.microsoft.com/cognitive-toolkit/)で計算を定義するために必要なラベルを割り当てます。

```fsharp
open CNTK

// DeviceDescriptor, Variable, and Function all come from CNTK
type Computation = DeviceDescriptor -> Variable -> Function
```

`Computation` 名は、エイリアスとして指定されているシグネチャに一致する任意の関数を示す便利な方法です。 このような型の省略形を使用すると便利で、より簡潔なコードを使用できます。

### <a name="avoid-using-type-abbreviations-to-represent-your-domain"></a>ドメインを表すために型の省略形を使用しない

型の省略形は、関数のシグネチャに名前を付ける場合に便利ですが、他の型を取りすると混乱する可能性があります。 次の略語を考えてみます。

```fsharp
// Does not actually abstract integers.
type BufferSize = int
```

これは、次のような複数の方法で混乱する可能性があります。

* `BufferSize` は抽象化ではありません。これは、整数の別の名前にすぎません。
* `BufferSize` がパブリック API で公開されている場合は、単に `int`だけではなく、誤って解釈される可能性があります。 一般に、ドメイン型には複数の属性があり、`int`のようなプリミティブ型ではありません。 この略語は、このような仮定に違反します。
* `BufferSize` の大文字と小文字の区別は、この型がより多くのデータを保持することを意味します。
* 関数に名前付き引数を指定する場合と比べて、この別名ではわかりやすさが向上していません。
* 省略形はコンパイルされた IL のマニフェストではありません。これは整数であり、このエイリアスはコンパイル時の構造体です。

```fsharp
module Networking =
    ...
    let send data (bufferSize: int) = ...
```

要約すると、型略称の落とし穴は、それらが取りされている型に抽象では**ない**ということです。 前の例では、`BufferSize` は、追加データを含まない、または既に存在する `int` 以外の型システムの利点を備えた `int` にすぎません。

型略称を使用してドメインを表す別の方法として、単一ケース判別共用体を使用する方法があります。 前のサンプルは次のようにモデル化できます。

```fsharp
type BufferSize = BufferSize of int
```

`BufferSize` とその基になる値の観点から動作するコードを記述する場合は、任意の整数を渡すのではなく、1つを構築する必要があります。

```fsharp
module Networking =
    ...
    let send data (BufferSize size) =
    ...
```

これにより、呼び出し元は、関数を呼び出す前に値をラップする `BufferSize` 型を構築する必要があるため、`send` 関数に誤って任意の整数が渡される可能性が低くなります。
