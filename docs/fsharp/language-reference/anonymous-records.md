---
title: 匿名のレコード
description: データの操作に役立つ言語機能である、コンストラクトを使用して匿名レコードを使用する方法について説明します。
ms.date: 06/12/2019
ms.openlocfilehash: 061fd3279c84b9a3161c687d9392947ee7ce9c83
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453027"
---
# <a name="anonymous-records"></a>匿名のレコード

匿名レコードは、使用前に宣言する必要のない名前付きの値の単純な集計です。 これらは、構造体または参照型として宣言できます。 既定では、これらは参照型です。

## <a name="syntax"></a>構文

次の例は、匿名レコードの構文を示しています。 `[item]` として区切られた項目は省略可能です。

```fsharp
// Construct an anonymous record
let value-name = [struct] {| Label1: Type1; Label2: Type2; ...|}

// Use an anonymous record as a type parameter
let value-name = Type-Name<[struct] {| Label1: Type1; Label2: Type2; ...|}>

// Define a parameter with an anonymous record as input
let function-name (arg-name: [struct] {| Label1: Type1; Label2: Type2; ...|}) ...
```

## <a name="basic-usage"></a>基本的な使用方法

匿名レコードは、インスタンス化のF#前に宣言する必要のないレコードの種類として考えられます。

たとえば、匿名レコードを生成する関数と対話する方法を次に示します。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

次の例では、前の例でを展開して、匿名レコードを入力として受け取る `printCircleStats` 関数を使用します。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let printCircleStats r (stats: {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

入力型と同じ "shape" を持たない匿名レコード型を使用して `printCircleStats` を呼び出すと、コンパイルに失敗します。

```fsharp
printCircleStats r {| Diameter = 2.0; Area = 4.0; MyCircumference = 12.566371 |}
// Two anonymous record types have mismatched sets of field names
// '["Area"; "Circumference"; "Diameter"]' and '["Area"; "Diameter"; "MyCircumference"]'
```

## <a name="struct-anonymous-records"></a>構造体の匿名レコード

匿名レコードは、省略可能な `struct` キーワードを使用して構造体として定義することもできます。 次の例では、構造体の匿名レコードを生成して使用することにより、前のコードを強化します。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    // Note that the keyword comes before the '{| |}' brace pair
    struct {| Area = a; Circumference = c; Diameter = d |}

// the 'struct' keyword also comes before the '{| |}' brace pair when declaring the parameter type
let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

### <a name="structness-inference"></a>Structness の推論

構造体の匿名レコードでは、呼び出しサイトで `struct` キーワードを指定する必要がない "structness 推定" も許可されます。 この例では、`printCircleStats`を呼び出すときに `struct` キーワードの使用を解除します。

```fsharp

let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

printCircleStats r {| Area = 4.0; Circumference = 12.6; Diameter = 12.6 |}
```

逆パターン-入力型が構造体の匿名レコードでない場合に `struct` を指定すると、コンパイルに失敗します。

## <a name="embedding-anonymous-records-within-other-types"></a>他の型の中に匿名レコードを埋め込む

ケースがレコードである[判別共用体](discriminated-unions.md)を宣言すると便利です。 ただし、レコード内のデータが判別共用体と同じ型である場合は、すべての型を相互に再帰的に定義する必要があります。 匿名レコードを使用すると、この制限を回避できます。 パターンが一致する型と関数の例を次に示します。

```fsharp
type FullName = { FirstName: string; LastName: string }

// Note that using a named for Manager and Executive would require mutually recursive definitions.
type Employee =
    | Engineer of FullName
    | Manager of {| Name: FullName; Reports: Employee list |}
    | Executive of {| Name: FullName; Reports: Employee list; Assistant: Employee |}

let getFirstName e =
    match e with
    | Engineer fullName -> fullName.FirstName
    | Manager m -> m.Name.FirstName
    | Executive ex -> ex.Name.FirstName
```

## <a name="copy-and-update-expressions"></a>式のコピーと更新

匿名レコードは[、コピー式および更新式](copy-and-update-record-expressions.md)を使用した構築をサポートします。 たとえば、既存のデータをコピーする匿名レコードの新しいインスタンスを作成する方法を次に示します。

```fsharp
let data = {| X = 1; Y = 2 |}
let data' = {| data with Y = 3 |}
```

ただし、名前付きレコードとは異なり、匿名レコードでは、コピーと更新の式を使用して、まったく異なる形式を構築できます。 次の例では、前の例と同じ匿名レコードを取得し、新しい匿名レコードに展開します。

```fsharp
let data = {| X = 1; Y = 2 |}
let expandedData = {| data with Z = 3 |} // Gives {| X=1; Y=2; Z=3 |}
```

名前付きレコードのインスタンスから匿名レコードを構築することもできます。

```fsharp
type R = { X: int }
let data = { X = 1 }
let data' = {| data with Y = 2 |} // Gives {| X=1; Y=2 |}
```

参照および構造体の匿名レコードとの間でデータをコピーすることもできます。

```fsharp
// Copy data from a reference record into a struct anonymous record
type R1 = { X: int }
let r1 = { X = 1 }

let data1 = struct {| r1 with Y = 1 |}

// Copy data from a struct record into a reference anonymous record
[<Struct>]
type R2 = { X: int }
let r2 = { X = 1 }

let data2 = {| r1 with Y = 1 |}

// Copy the reference anonymous record data into a struct anonymous record
let data3 = struct {| data2 with Z = r2.X |}
```

## <a name="properties-of-anonymous-records"></a>匿名レコードのプロパティ

匿名レコードには、それらの使用方法を十分に理解するために不可欠ないくつかの特性があります。

### <a name="anonymous-records-are-nominal"></a>匿名レコードは公称

匿名レコードは[公称型](https://en.wikipedia.org/wiki/Nominal_type_system)です。 これらは、アップフロント宣言を必要としない名前付き[レコード](records.md)型 (公称でもあります) として考えるのが最適です。

次の例では、2つの匿名レコード宣言を考えてみます。

```fsharp
let x = {| X = 1 |}
let y = {| Y = 1 |}
```

`x` と `y` の値の型が異なり、相互に互換性がありません。 これらは equatable ではなく、比較できません。 これを説明するために、同等の名前付きレコードを考えてみましょう。

```fsharp
type X = { X: int }
type Y = { Y: int }

let x = { X = 1 }
let y = { Y = 1 }
```

型の等価性または比較に関係する場合、匿名レコードの名前付きレコードと比較しても、本質的に異なることはありません。

### <a name="anonymous-records-use-structural-equality-and-comparison"></a>匿名レコードは構造的等価性と比較を使用する

レコードの種類と同様に、匿名レコードは構造的に equatable と比較できます。 これは、すべての構成型が、レコード型などの等値と比較をサポートしている場合にのみ当てはまります。 等値または比較をサポートするには、2つの匿名レコードの "shape" が同じである必要があります。

```fsharp
{| a = 1+1 |} = {| a = 2 |} // true
{| a = 1+1 |} > {| a = 1 |} // true

// error FS0001: Two anonymous record types have mismatched sets of field names '["a"]' and '["a"; "b"]'
{| a = 1 + 1 |} = {| a = 2;  b = 1|}
```

### <a name="anonymous-records-are-serializable"></a>匿名レコードはシリアル化可能

名前付きレコードの場合と同様に、匿名レコードをシリアル化できます。 [Newtonsoft. Json](https://www.nuget.org/packages/Newtonsoft.Json/)を使用した例を次に示します。

```fsharp
open Newtonsoft.Json

let phillip' = {| name="Phillip"; age=28 |}
let philStr = JsonConvert.SerializeObject(phillip') 

let phillip = JsonConvert.DeserializeObject<{|name: string; age: int|}>(philStr)
printfn "Name: %s Age: %d" phillip.name phillip.age
```

匿名レコードは、シリアル化または逆シリアル化された型のドメインを事前に定義しなくても、ネットワーク経由で軽量データを送信する場合に便利です。

### <a name="anonymous-records-interoperate-with-c-anonymous-types"></a>匿名レコードと匿名C#型の相互運用

匿名型を使用する必要がある .net API を使用することができます。 [ C# ](../../csharp/programming-guide/classes-and-structs/anonymous-types.md) C#匿名型は、匿名レコードを使用してと相互運用するのが簡単です。 次の例は、匿名レコードを使用して、匿名型を必要とする[LINQ](../../csharp/programming-guide/concepts/linq/index.md)オーバーロードを呼び出す方法を示しています。

```fsharp
open System.Linq

let names = [ "Ana"; "Felipe"; "Emilia"]
let nameGrouping = names.Select(fun n -> {| Name = n; FirstLetter = n.[0] |})
for ng in nameGrouping do
    printfn "%s has first letter %c" ng.Name ng.FirstLetter
```

.NET で使用される他の多くの Api では、匿名型を渡す必要があります。 匿名レコードは、それらを操作するためのツールです。

## <a name="limitations"></a>制限事項

匿名レコードの使用にはいくつかの制限があります。 設計に固有のものもあれば、変更に対応しているものもあります。

### <a name="limitations-with-pattern-matching"></a>パターン一致に関する制限事項

匿名レコードは、名前付きレコードとは異なり、パターンマッチングをサポートしていません。 次の3つの理由があります。

1. パターンでは、名前付きレコードの種類とは異なり、匿名レコードのすべてのフィールドを考慮する必要があります。 これは、匿名レコードでは構造的サブタイプがサポートされていないためです。これは、標準型です。
2. (1) のため、パターン一致式に追加のパターンを含めることはできません。これは、個別のパターンがそれぞれ異なる匿名レコードの種類を示すためです。
3. (3) のため、匿名レコードパターンは、"ドット" 表記を使用した場合よりも詳細です。

コンテキストが制限されている場合、[パターンマッチングを可能](https://github.com/fsharp/fslang-suggestions/issues/713)にするためのオープン言語の提案があります。

### <a name="limitations-with-mutability"></a>互換性に関する制限事項

現時点では、`mutable` データを含む匿名レコードを定義することはできません。 変更可能なデータを許可する[オープン言語の推奨](https://github.com/fsharp/fslang-suggestions/issues/732)事項があります。

### <a name="limitations-with-struct-anonymous-records"></a>構造体の匿名レコードに関する制限事項

`IsByRefLike` または `IsReadOnly`として構造体の匿名レコードを宣言することはできません。 匿名レコードを `IsByRefLike` および `IsReadOnly` するには、に対する[オープン言語の候補](https://github.com/fsharp/fslang-suggestions/issues/712)があります。
