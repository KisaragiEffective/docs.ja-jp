---
title: パラメーターと引数
description: パラメーターをF#定義し、関数、メソッド、およびプロパティに引数を渡すための言語サポートについて説明します。
ms.date: 12/04/2019
ms.openlocfilehash: b234ef939128e7cf09d35f9580d4d5010d7dc639
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837130"
---
# <a name="parameters-and-arguments"></a>パラメーターと引数

このトピックでは、パラメーターを定義し、関数、メソッド、およびプロパティに引数を渡すための言語サポートについて説明します。 これには、参照渡しの方法に関する情報と、可変個の引数を受け取ることができるメソッドを定義して使用する方法についての情報が含まれます。

## <a name="parameters-and-arguments"></a>パラメーターと引数

Term*パラメーター*を使用して、指定される値の名前を記述します。 Term*引数*は、各パラメーターに指定された値に使用されます。

パラメーターは、組またはカリー化形式、またはその2つの組み合わせで指定できます。 引数は、明示的なパラメーター名を使用して渡すことができます。 メソッドのパラメーターは、省略可能として指定し、既定値を指定できます。

## <a name="parameter-patterns"></a>パラメーターパターン

関数およびメソッドに渡すパラメーターは、通常、スペースで区切られたパターンです。 つまり、原則として、関数またはメンバーのパラメーターリストでは、「 [Match 式](match-expressions.md)」で説明されているパターンのいずれかを使用できます。

メソッドは、通常、引数を渡す組形式を使用します。 これにより、他の .NET 言語の観点から、.NET メソッドで引数が渡される方法と組の形式が一致するため、より明確な結果が得られます。

カリー化形式は、`let` バインディングを使用して作成された関数で最もよく使用されます。

次の擬似コードは、組とカリー化引数の例を示しています。

```fsharp
// Tuple form.
member this.SomeMethod(param1, param2) = ...
// Curried form.
let function1 param1 param2 = ...
```

一部の引数が組に含まれていて、一部がでない場合は、結合されたフォームを使用できます。

```fsharp
let function2 param1 (param2a, param2b) param3 = ...
```

パラメーターリストで他のパターンを使用することもできますが、パラメーターパターンがすべての可能な入力と一致しない場合は、実行時に不完全な一致が発生する可能性があります。 引数の値がパラメーターリストで指定されているパターンと一致しない場合、例外 `MatchFailureException` が生成されます。 パラメーターパターンで不完全な一致が許可されている場合、コンパイラは警告を発行します。 少なくとも1つの他のパターンはパラメーターリストに便利で、ワイルドカードパターンです。 指定された引数を無視するだけの場合は、パラメーターリストでワイルドカードパターンを使用します。 次のコードは、引数リストでワイルドカードパターンを使用する方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3801.fs)]

ワイルドカードパターンは、次のコードのように、通常は文字列配列として指定されているコマンドライン引数を必要としない場合に、プログラムへのメインエントリポイントなど、渡された引数が不要な場合に便利です。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3802.fs)]

引数で使用されるその他のパターンとして、`as` パターン、および判別共用体とアクティブパターンに関連付けられた識別子パターンがあります。 次のように、単一ケース判別共用体パターンを使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3803.fs)]

出力は次のとおりです。

```console
Data begins at 0 and ends at 4 in string Et tu, Brute?
Et tu
```

アクティブパターンは、次の例のように、引数を目的の形式に変換するときなどに、パラメーターとして役立ちます。

```fsharp
type Point = { x : float; y : float }

let (| Polar |) { x = x; y = y} =
    ( sqrt (x*x + y*y), System.Math.Atan (y/ x) )

let radius (Polar(r, _)) = r
let angle (Polar(_, theta)) = theta
```

次のコード行に示すように、`as` パターンを使用すると、一致した値をローカル値として格納できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3805.fs)]

場合によって使用されるもう1つのパターンは、関数の本体として、暗黙的な引数に対してパターン一致を直ちに実行するラムダ式を指定することによって、最後の引数を無名のままにする関数です。 例として、次のコード行があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3804.fs)]

このコードは、ジェネリックリストを受け取り、リストが空の場合は `true` を返し、それ以外の場合は `false` を返す関数を定義します。 このような手法を使用すると、コードが読みにくくなる可能性があります。

場合によっては、不完全な一致を含むパターンが役に立つことがあります。たとえば、プログラムのリストに3つの要素しかないことがわかっている場合は、パラメーターリストで次のようなパターンを使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3806.fs)]

一致しないパターンを使用すると、簡単なプロトタイプ作成やその他の一時的な使用に適しています。 コンパイラは、このようなコードに対して警告を発行します。 このようなパターンでは、可能なすべての入力の一般的なケースに対応できないため、コンポーネント Api には適していません。

## <a name="named-arguments"></a>名前付き引数

メソッドの引数は、コンマで区切られた引数リスト内の位置によって指定できます。また、名前を指定してメソッドに明示的に渡すこともできます。その後、等号と渡される値を指定します。 名前を指定して指定した場合は、宣言で使用されているものとは異なる順序で表示されます。

名前付き引数を使用すると、メソッドパラメーターの並べ替えなど、API の特定の種類の変更にコードを読みやすくし、適応性を高めることができます。

名前付き引数は、メソッドに対してのみ許可されます。 `let`バインド関数、関数値、またはラムダ式には使用できません。

名前付き引数の使用方法を次のコード例に示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3807.fs)]

クラスコンストラクターの呼び出しでは、名前付き引数と同様の構文を使用して、クラスのプロパティの値を設定できます。 次の例は、この構文を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3506.fs)]

詳細については、「[コンストラクター (F#)](https://msdn.microsoft.com/library/2cd0ed07-d214-4125-8317-4f288af99f05)」を参照してください。

## <a name="optional-parameters"></a>省略可能なパラメーター

パラメーター名の前に疑問符を使用して、メソッドの省略可能なパラメーターを指定できます。 省略可能なパラメーターは、 F#オプションの型として解釈されるので、`Some` と `None`で `match` 式を使用して、オプションの型のクエリを実行する通常の方法でクエリを実行できます。 省略可能なパラメーターは、`let` バインディングを使用して作成された関数ではなく、メンバーに対してのみ許可されます。

`?arg=None`、`?arg=Some(3)`、`?arg=arg`などのパラメーター名を使用して、既存の省略可能な値をメソッドに渡すことができます。 これは、オプションの引数を別のメソッドに渡すメソッドを構築する場合に便利です。

関数 `defaultArg`を使用することもできます。これにより、省略可能な引数の既定値が設定できます。 `defaultArg` 関数は、省略可能なパラメーターを最初の引数として受け取り、既定値を2番目の引数として受け取ります。

省略可能なパラメーターの使用例を次に示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3808.fs)]

出力は次のとおりです。

```console
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 4800 Duplex: Half Parity: false
Baud Rate: 300 Duplex: Half Parity: true
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 4800 Duplex: Half Parity: false
```

と Visual Basic の相互C#運用のために、の `[<Optional; DefaultParameterValue<(...)>]` 属性を使用F#して、呼び出し元が引数を省略可能として表示できるようにすることができます。 これは、`MyMethod(int i = 3)`のように、でC#オプションとして引数を省略可能として定義することと同じです。

```fsharp
open System
open System.Runtime.InteropServices
type C =
    static member Foo([<Optional; DefaultParameterValue("Hello world")>] message) =
        printfn "%s" message
```

また、新しいオブジェクトを既定のパラメーター値として指定することもできます。 たとえば、`Foo` のメンバーは、オプションの `CancellationToken` を入力として使用できます。

```fsharp
open System.Threading
open System.Runtime.InteropServices
type C =
    static member Foo([<Optional; DefaultParameterValue(CancellationToken())>] ct: CancellationToken) =
        printfn "%A" ct
```

`DefaultParameterValue` の引数として指定された値は、パラメーターの型と一致している必要があります。 たとえば、次は許可されていません。

```fsharp
type C =
    static member Wrong([<Optional; DefaultParameterValue("string")>] i:int) = ()
```

この場合、コンパイラは警告を生成し、両方の属性を完全に無視します。 `null` 既定値は型指定されている必要があることに注意してください。それ以外の場合は、コンパイラが間違った型 (つまり `[<Optional; DefaultParameterValue(null:obj)>] o:obj`) を推論します。

## <a name="passing-by-reference"></a>渡す (参照渡しで)

参照渡しF#で値を渡すには、マネージポインター型である[byref](byrefs.md)が必要です。 使用する種類に関するガイダンスは次のとおりです。

- ポインターを読み取る必要があるだけの場合は、`inref<'T>` を使用します。
- ポインターへの書き込みのみが必要な場合は、`outref<'T>` を使用します。
- ポインターの読み取りと書き込みの両方を行う必要がある場合は、`byref<'T>` を使用します。

```fsharp
let example1 (x: inref<int>) = printfn "It's %d" x

let example2 (x: outref<int>) = x <- x + 1

let example3 (x: byref<int>) =
    printfn "It'd %d" x
    x <- x + 1

let test () =
    // No need to make it mutable, since it's read-only
    let x = 1
    example1 &x

    // Needs to be mutable, since we write to it
    let mutable y = 2
    example2 &y
    example3 &y // Now 'y' is 3
```

パラメーターはポインターであり、値は変更可能であるため、値に対する変更は、関数の実行後も保持されます。

戻り値としてタプルを使用して、.NET ライブラリメソッドに任意の `out` パラメーターを格納できます。 または、`out` パラメーターを `byref` パラメーターとして扱うこともできます。 両方の方法を次のコード例に示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3810.fs)]

## <a name="parameter-arrays"></a>パラメーター配列

場合によっては、異種型の任意の数のパラメーターを受け取る関数を定義する必要があります。 すべてのオーバーロードされたメソッドを作成して、使用可能なすべての型を考慮することは実用的ではありません。 .NET 実装では、パラメーター配列機能を使用して、このようなメソッドをサポートしています。 シグネチャでパラメーター配列を受け取るメソッドには、任意の数のパラメーターを指定できます。 パラメーターは配列に格納されます。 配列要素の型によって、関数に渡すことができるパラメーターの型が決まります。 要素型として `System.Object` を持つパラメーター配列を定義すると、クライアントコードは任意の型の値を渡すことができます。

でF#は、パラメーター配列はメソッドでのみ定義できます。 これらは、モジュールで定義されているスタンドアロン関数または関数では使用できません。

パラメーター配列は、`ParamArray` 属性を使用して定義します。 `ParamArray` 属性は、最後のパラメーターにのみ適用できます。

次のコードは、パラメーター配列を受け取る .NET メソッドと、パラメーター配列を受け取るメソッドを持つのF#型の定義の両方を呼び出す方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-2/snippet3811.fs)]

プロジェクト内で実行すると、前のコードの出力は次のようになります。

```console
a 1 10 Hello world 1 True
"a"
1
10.0
"Hello world"
1u
true
```

## <a name="see-also"></a>参照

- [メンバー](./members/index.md)
