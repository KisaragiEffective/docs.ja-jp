---
title: メソッド - C# ガイド
description: メソッド、メソッド パラメーター、メソッド戻り値の概要
ms.technology: csharp-fundamentals
ms.date: 05/21/2018
ms.assetid: 577a8527-1081-4b36-9b9e-0685b6553c6e
ms.openlocfilehash: f44c83408e884d76eef5e2b5abbca511fbae2a1f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398713"
---
# <a name="methods"></a>メソッド

メソッドは、一連のステートメントが含まれているコード ブロックです。 必要なメソッド引数を指定してプログラムからメソッドを呼び出すと、メソッド内のステートメントが実行されます。 C# では、実行されるすべての命令がメソッドのコンテキストで実行されます。 `Main` メソッドは、すべての C# アプリケーションのエントリ ポイントです。プログラムが開始されると、このメソッドが共通言語ランタイム (CLR) によって呼び出されます。

> [!NOTE]
> このトピックでは、名前付きメソッドについて説明します。 匿名関数については、「[匿名関数](programming-guide/statements-expressions-operators/anonymous-functions.md)」を参照してください。

<a name="signatures"></a>

## <a name="method-signatures"></a>メソッド シグネチャ

メソッドは次の項目を指定することで `class` または `struct` で宣言されます。

- `public` や `private` など、任意のアクセス レベル。 既定値は、`private` です。
- `abstract` や `sealed` など、任意の修飾子。
- メソッドに何も与えられていない場合、戻り値または `void`。
- メソッド名。
- メソッド パラメーター。 メソッド パラメーターはかっこで囲み、各パラメーターをコンマで区切ります。 かっこ内を空にすると、メソッドでパラメーターが不要なことを意味します。

これらのまとまりがメソッド シグネチャとなります。

> [!NOTE]
> メソッドのオーバーロードを可能にするために、メソッドの戻り値の型はメソッドのシグネチャには含まれません。 ただし、デリゲートとそれが指すメソッドの互換性を決定する場合には、メソッドのシグネチャの一部となります。

次の例では、5 つのメソッドを含む `Motorcycle` という名前のクラスを定義します。

[!code-csharp[csSnippets.Methods#40](../../samples/snippets/csharp/concepts/methods/methods40.cs#40)]

`Motorcycle` クラスにオーバーロードされたクラス `Drive` が含まれていることに注意してください。 2 つのメソッドの名前が同じであり、パラメーターの種類で識別する必要があります。

<a name="invocation"></a>

## <a name="method-invocation"></a>メソッドの呼び出し

メソッドは*インスタンス*または*静的*になります。 インスタンス メソッドを呼び出すには、オブジェクトをインスタンス化し、そのオブジェクトでメソッドを呼び出す必要があります。インスタンス メソッドはこのインスタンスとそのデータを操作します。 メソッドが属する型の名前を参照して静的メソッドを呼び出します。静的メソッドではインスタンス データは操作できません。 オブジェクト インスタンス経由で静的メソッドを呼び出そうとすると、コンパイラ エラーが発生します。

メソッドを呼び出すことは、フィールドにアクセスするのと似ています。 オブジェクトの名前 (インスタンス メソッドを呼び出す場合) または型の名前 (`static` メソッドを呼び出す場合) の後ろに、期間、メソッドの名前、かっこを追加します。 引数はかっこの中に記述し、コンマで区切ります。

メソッド定義には、必要なパラメーターの名前と型を指定します。 呼び出し元からメソッドを呼び出すとき、各パラメーターに引数と呼ばれる具体的な値を指定します。 引数にはパラメーター型との互換性が必要ですが、呼び出し元のコードで引数名を使用する場合、引数名がメソッドで定義されるパラメーター名と同じである必要はありません。 次の例では、`Square` メソッドに型が `int` で *i* という名前のパラメーターが 1 つ含まれています。 最初のメソッド呼び出しでは、型が `Square` で `int`num*という名前の変数が* メソッドに渡されます。2 つ目のメソッド呼び出しでは数値定数が、3 つ目のメソッド呼び出しでは式が渡されます。

[!code-csharp[csSnippets.Methods#74](../../samples/snippets/csharp/concepts/methods/params74.cs#74)]

メソッド呼び出しの最も一般的な形式では、位置引数が使用されます。これはメソッド パラメーターと同じ順序で引数を指定するものです。 そのため、`Motorcycle` クラスのメソッドは次の例のように呼び出されます。 たとえば、`Drive` メソッドの呼び出しには 2 つの引数が含まれます。この 2 つの引数は、メソッドの構文の 2 つのパラメーターに対応しています。 1 つ目は `miles` パラメーターの値になります。2 つ目は `speed` パラメーターの値になります。

[!code-csharp[csSnippets.Methods#41](../../samples/snippets/csharp/concepts/methods/methods40.cs#41)]

メソッドを呼び出すとき、位置引数の代わりに*名前付き引数*を使用することもできます。 名前付き引数を使用するとき、パラメーター名に続けてコロン (":") と引数を指定します。 必要なすべての引数が存在する限り、メソッドの引数の順序は問われません。 次の例では、名前付き引数を使用して `TestMotorcycle.Drive` メソッドを呼び出しています。 この例では、メソッドのパラメーター リストとは反対の順序で名前付き引数が渡されています。

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/named1.cs#45)]

位置引数と名前付き引数の両方を利用してメソッドを呼び出すことができます。 ただし、名前付き引数の後に位置指定引数を使用することはできません。 次の例では、前の例にあった `TestMotorcycle.Drive` メソッドを呼び出していますが、位置引数が 1 つ、名前付き引数が 1 つ使用されています。

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/named2.cs#46)]

<a name="inherited"></a>

## <a name="inherited-and-overridden-methods"></a>継承されたメソッドとオーバーライドされたメソッド

型に明示的に定義されるメンバーに加え、型は、その基底クラスに定義されているメンバーを継承します。 マネージド型という系統のすべての型は <xref:System.Object> クラスから直接的または間接的に継承するため、すべての型が、<xref:System.Object.Equals(System.Object)>、<xref:System.Object.GetType>、<xref:System.Object.ToString> など、そのメンバーを継承します。 次の例では、`Person` クラスを定義し、2 つの `Person` オブジェクトをインスタンス化し、`Person.Equals` メソッドを呼び出して 2 つのオブジェクトが等しいかどうかを判断します。 ただし、`Equals` メソッドは `Person` クラスに定義されていません。<xref:System.Object> から継承されたものです。

[!code-csharp[csSnippets.Methods#104](../../samples/snippets/csharp/concepts/methods/inherited1.cs#104)]

型は継承されたメンバーをオーバーライドできます。`override` キーワードを使用し、オーバーライドされたメソッドを実装します。 メソッド シグネチャは、オーバーライドされたメソッドのものと同じにする必要があります。 次の例は前の例と似ていますが、<xref:System.Object.Equals(System.Object)> メソッドをオーバーライドしている点が異なります。 (この 2 つのメソッドは一貫性のある結果を提供するため、<xref:System.Object.GetHashCode> メソッドもオーバーライドされます。)

[!code-csharp[csSnippets.Methods#105](../../samples/snippets/csharp/concepts/methods/overridden1.cs#105)]

<a name="passing"></a>

## <a name="passing-parameters"></a>パラメーターを渡す

C# の型は、*値型*と*参照型*のどちらかに区別されます。 組み込みの値型の一覧については、「[型と変数](./tour-of-csharp/types-and-variables.md)」を参照してください。 既定では、値型と参照型の両方が値によりメソッドに渡されます。

<a name="byval"></a>

### <a name="passing-parameters-by-value"></a>パラメーターを値で渡す

値型が値でメソッドに渡されるとき、オブジェクト自体の代わりにオブジェクトのコピーがメソッドに渡されます。 そのため、呼び出されたメソッドでオブジェクトに加えた変更は、コントロールが呼び出し元に戻ったとき、元のオブジェクトで反映されません。

次の例では、値型を値でメソッドに渡します。呼び出されたメソッドが値型の値を変更しようとします。 型 `int` (値型) の変数を定義し、その値を 20 に初期化し、`ModifyValue` という名前のメソッドに値を渡します。このメソッドは変数の値を 30 に変更します。 しかしながら、メソッドが戻ると、変数の値は元のままです。

[!code-csharp[csSnippets.Methods#10](../../samples/snippets/csharp/concepts/methods/byvalue10.cs#10)]

参照型のオブジェクトが値でメソッドに渡されると、オブジェクトへの参照が値で渡されます。 つまり、メソッドは、オブジェクト自体ではなく、オブジェクトの場所を示す引数を受け取ります。 この参照を使用してオブジェクトのメンバーを変更した場合、コントロールが呼び出し元のメソッドに戻ると、オブジェクトで変更が反映されています。 ただし、メソッドに渡されるオブジェクトを置換しても、コントロールが呼び出し元に戻ったとき、元のオブジェクトで反映されません。

次の例では、`SampleRefType` という名前のクラス (参照型) を定義します。 `SampleRefType` オブジェクトをインスタンス化し、その `value` フィールドに 44 を割り当て、`ModifyObject` メソッドにオブジェクトを渡します。 この例は、基本的に前の例と同様に、引数を値でメソッドに渡しています。 ただし、参照型を使用しているため、結果は異なります。 `ModifyObject` の `obj.value` フィールドを変更したことで、`value` メソッドの引数 `rt` の `Main` フィールドも 33 に変更されます。例の出力で確認できます。

[!code-csharp[csSnippets.Methods#42](../../samples/snippets/csharp/concepts/methods/byvalue42.cs#42)]

<a name="byref"></a>

### <a name="passing-parameters-by-reference"></a>パラメーターの参照渡し

メソッドの引数の値を変更し、コントロールが呼び出し元に戻ったときにその変更を反映させるには、参照でパラメーターを渡します。 パラメーターを参照で渡すには、[`ref`](language-reference/keywords/ref.md) または [`out`](language-reference/keywords/out-parameter-modifier.md) キーワードを使用します。 値を参照で渡すことで、コピーを回避することもできますが、[`in`](language-reference/keywords/in-parameter-modifier.md) キーワードを使用して変更を防ぐこともできます。

次の例は前の例とよく似ていますが、値が参照で `ModifyValue` メソッドに渡される点が異なります。 パラメーターの値が `ModifyValue` メソッドで変更されると、コントロールが呼び出し元に戻ったとき、地の変更が反映されます。

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/byref106.cs#106)]

参照型パラメーターを利用する典型的なパターンが変数の値の入れ替えです。 参照でメソッドに 2 つの変数を渡すと、メソッドがその中身を入れ替えます。 次の例では、整数値が入れ替えられます。

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/swap107.cs#107)]

参照型パラメーターを渡すことで、個々の要素またはフィールドの値ではなく、参照自体の値を変更できます。

<a name="paramarray"></a>

### <a name="parameter-arrays"></a>パラメーター配列

メソッドに厳密な数の引数を指定する要件が限定的になることがあります。 `params` キーワードを利用し、パラメーターがパラメーター配列であることを示すことで、可変数の引数でメソッドを呼び出すことができます。 `params` キーワードでタグが付けられたパラメーターは配列型にする必要があり、メソッドのパラメーター リストの最後のパラメーターにする必要があります。

呼び出し元は、次の 3 つの方法のいずれかでメソッドを呼び出すことができます。

- 必要な数の要素を含む、適切な型の配列を渡す。
- 適切な型の引数をコンマで区切った一覧をメソッドに渡す。
- パラメーター配列に引数を指定しない。

次の例は、パラメーター配列からすべての母音を返す `GetVowels` という名前のメソッドを定義します。 `Main` メソッドには、メソッド呼び出しの 3 つ全部の方法が入っています。 呼び出し元は、`params` 修飾子が含まれるパラメーターに引数を指定する必要はありません。 その場合、パラメーターは `null` になります。

[!code-csharp[csSnippets.Methods#75](~/samples/snippets/csharp/concepts/methods/params75.cs#75)]

<a name="optional"></a>

## <a name="optional-parameters-and-arguments"></a>省略可能なパラメーターと引数

メソッド定義では、そのパラメーターが必須であるか、任意であるかを指定できます。 既定では、パラメーターは必須です。 省略可能なパラメーターを指定するには、メソッド定義にパラメーターの既定値を追加します。 メソッドが呼び出されるとき、省略可能なパラメーターに引数が指定されていなければ、既定値が代わりに使用されます。

パラメーターの既定値は、次の種類の式のいずれかで割り当てる必要があります。

- リテラル文字列や数値など、定数。
- `new ValType()` が値型となる、`ValType` 形式の式。 値型の暗黙のパラメーターなしのコンストラクターが呼び出されることに注意してください。それは型の実際のメンバーではありません。
- `default(ValType)` が値型となる、`ValType` 形式の式。

メソッドに必須のパラメーターと省略可能なパラメーターの両方が含まれる場合、省略可能なパラメーターはパラメーター リストの終わりに定義されます (すべての必須パラメーターの後に)。

次の例では、`ExampleMethod` メソッドを定義しています。必須のパラメーターが 1 つ、省略可能なパラメーターが 2 つあります。

[!code-csharp[csSnippets.Methods#21](../../samples/snippets/csharp/concepts/methods/optional1.cs#21)]

省略可能なパラメーターを複数持つメソッドが位置引数で呼び出された場合、呼び出し元は、引数が指定される最初のパラメーターから最後のパラメーターまで、すべての省略可能なパラメーターに引数を指定する必要があります。 たとえば、`ExampleMethod` メソッドの場合、呼び出し元が `description` パラメーターの引数を指定した場合、`optionalInt` パラメーターの引数も指定する必要があります。 `opt.ExampleMethod(2, 2, "Addition of 2 and 2");` は有効なメソッド呼び出しです。`opt.ExampleMethod(2, , "Addition of 2 and 0");` は、"引数がありません" というコンパイラ エラーを発生させます。

メソッドが名前付き引数または位置引数と名前付き引数の組み合わせで呼び出される場合、呼び出し元は、メソッド呼び出しの最後の位置引数の後に続く引数を省略できます。

次の例では、`ExampleMethod` メソッドが 3 回呼び出されます。  最初の 2 つのメソッド呼び出しでは、位置引数が使用されます。 最初の呼び出しではいずれの省略可能なパラメーターも省略され、2 つ目の呼び出しでは最後の引数が省略されます。 3 つ目のメソッドは必須パラメーターの位置引数を指定しますが、名前付き引数を利用して `description` パラメーターに値を指定します。`optionalInt` パラメーターは省略します。

[!code-csharp[csSnippets.Methods#22](../../samples/snippets/csharp/concepts/methods/optional1.cs#22)]

省略可能なパラメーターの使用は、*オーバーロードの解決*に影響を与えます。次のように、特定のオーバーロードをメソッド呼び出しで呼び出すかどうかを C# コンパイラが決定する方法に影響を与えます。

- メソッド、インデクサー、コンストラクターのパラメーターのそれぞれが任意であるか、名前か位置により、呼び出しステートメントの 1 つの引数に対応するとき、その引数がパラメーターの型に変換できる場合、メソッド、インデクサー、コンストラクターが実行の候補になります。
- 複数の候補が見つかった場合、明示的に指定される引数には、優先変換に関するオーバーロード解決の規則が適用されます。 任意のパラメーターの省略された引数は無視されます。
- 2 つの候補が等しく良好であると判断された場合、呼び出しで引数が省略された任意のパラメーターのない候補が優先されます。 これはパラメーターの少ない候補に関するオーバーロード解決の一般優先設定の結果です。

<a name="return"></a>

## <a name="return-values"></a>戻り値

メソッドは、呼び出し元に値を返すことができます。 戻り値の型 (メソッド名の前に記述されている型) が `void`でない場合、メソッドは、`return` キーワードを使用して値を返すことができます。 `return` キーワードに続いて変数、定数、または戻り値の型に一致する値が記述されたステートメントは、その値をメソッドの呼び出し元に返します。 戻り値の型が void 以外のメソッドで値を返すには、 `return` キーワードを使用する必要があります。 また、 `return` キーワードは、メソッドの実行を中止します。

戻り値の型が `void`の場合、値を持たない `return` ステートメントは、メソッドの実行を中止するときに役立ちます。 `return` キーワードを使用しない場合、メソッドは、コード ブロックの最後に到達したときに実行を中止します。

たとえば、次の 2 つのメソッドは、 `return` キーワードを使用して整数を返します。

[!code-csharp[csSnippets.Methods#44](../../samples/snippets/csharp/concepts/methods/return44.cs#44)]

メソッドから返された値を使用する場合、呼び出し元のメソッド内で同じ型の値を使用している場所では、メソッド呼び出し自体を値として使用できます。 戻り値は、変数に代入することもできます。 たとえば、次の 2 つのコードでは、同様の結果が得られます。

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/return44.cs#45)]

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/return44.cs#46)]

この場合、ローカル変数 `result`を使用して値を格納する手順はオプションです。 このローカル変数によってコードの読みやすさが向上することがあります。また、引数の元の値をメソッドのスコープ全体で保持する場合に必要になることがあります。

メソッドで複数の値を返すと便利な場合があります。 C# 7.0 以降では、*タプル型*と*タプル リテラル*を使用してこれを簡単に実行できます。 タプル型は、タプルの要素のデータ型を決定します。 タプル リテラルは、返されたタプルの実際の値を提供します。 次の例では、`(string, string, string, int)` は、`GetPersonalInfo` メソッドにより返されるタプル型を定義します。 式 `(per.FirstName, per.MiddleName, per.LastName, per.Age)` はタプル リテラルです。このメソッドは、`PersonInfo` オブジェクトの名、ミドルネーム、姓、年齢を返します。

```csharp
public (string, string, string, int) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

呼び出し元はそれから、次のようなコードで返されたタプルを利用します。

```csharp
var person = GetPersonalInfo("111111111")
Console.WriteLine("{person.Item1} {person.Item3}: age = {person.Item4}");
```

名前は、タプル型の定義のタプル要素に割り当てることもできます。 次の例は、名前付き要素を使用する `GetPersonalInfo` メソッドの別バージョンです。

```csharp
public (string FName, string MName, string LName, int Age) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

この例の `GetPersonInfo` メソッドの呼び出しは次のように変更できます。

```csharp
var person = GetPersonalInfo("111111111");
Console.WriteLine("{person.FName} {person.LName}: age = {person.Age}");
```

メソッドに引数として配列が渡されるとき、そのメソッドが個々の要素の値を変更する場合、メソッドが配列を返す必要はありません。ただし、見やすいから、値の流れが機能的になるからといった理由で配列を返してもかまいません。  配列を返す必要がないのは、C# ではすべての参照型が値で渡され、配列参照の値がその配列のポインターになるためです。 次の例では、`values` メソッドで行われた `DoubleValues` 配列の内容の変更を、配列の参照があるあらゆるコードで観察できます。

[!code-csharp[csSnippets.Methods#101](../../samples/snippets/csharp/concepts/methods/returnarray1.cs#101)]

<a name="extension"></a>

## <a name="extension-methods"></a>拡張メソッド

通常、既存の型にメソッドを追加する方法が 2 つあります。

- その型のソース コードを変更する。 もちろん、型のソース コードを所有していない場合、これはできません。 また、メソッドをサポートするプライベース データ フィールドも追加した場合、これは互換性に影響する変更になります。
- 派生クラスで新しいメソッドを定義する。 構造体や列挙型など、その他の型の継承を利用し、メソッドをこの方法で追加することはできません。 シール クラスにメソッドを "追加する" こともできません。

拡張メソッドでは、型自体を変更せずに、あるいは継承された型に新しいメソッドを実装せずに、既存の型にメソッドを "追加" できます。 また、拡張メソッドは、それが拡張する型と同じアセンブリに置く必要がありません。 型の定義済みメンバーのように拡張メソッドを呼び出します。

詳細については、「[拡張メソッド](programming-guide/classes-and-structs/extension-methods.md)」を参照してください。

<a name="async"></a>

## <a name="async-methods"></a>非同期メソッド

非同期機能を使用することによって、明示的なコールバックを使用せずに、または複数のメソッドやラムダ式にわたって手動でコードを分割することなく、非同期メソッドを呼び出すことができます。

メソッドに [async](language-reference/keywords/async.md) 修飾子を付けると、そのメソッドで [await](language-reference/operators/await.md) 演算子を使用できます。 コントロールが非同期メソッドの `await` 式に到達すると、待機中のタスクが完了していない場合、コントロールが呼び出し元に戻ります。`await` キーワードが与えられたメソッドの進行は、待機中のタスクが完了するまで中断されます。 タスクが完了すると、メソッドで実行を再開できます。

> [!NOTE]
> 非同期メソッドは、まだ完了していない待機中の最初のオブジェクトに達するか、または非同期メソッドの最後に達すると、呼び出し元に戻ります。

非同期メソッドの戻り値の型としては、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.Task>、`void` を指定できます。 戻り値の型 `void` は主として、戻り値の型 `void` が必要なイベント ハンドラーの定義に使用されます。 `void` を返す非同期メソッドは待機できません。void を返すメソッドの呼び出し元は、このメソッドがスローする例外をキャッチできません。 C# 7.0 以降、非同期メソッドは[タスクと同様の戻り値の型](./whats-new/csharp-7.md#generalized-async-return-types)を持つことができます。

次の例では、`DelayAsync` は、整数を返す return ステートメントのある非同期メソッドです。 非同期メソッドであるため、そのメソッド宣言で戻り値の型 `Task<int>` を指定する必要があります。 戻り値の型が `Task<int>`であるため、次のステートメント `await` に示すように、`DoSomethingAsync` 内の `int result = await delayTask` 式を評価すると整数が生成されます。

[!code-csharp[csSnippets.Methods#102](../../samples/snippets/csharp/concepts/methods/async1.cs#102)]

非同期メソッドで [in](language-reference/keywords/in-parameter-modifier.md)、[ref](language-reference/keywords/ref.md)、[out](language-reference/keywords/out-parameter-modifier.md) パラメーターを宣言することはできませんが、これらのパラメーターを持つメソッドを呼び出すことはできます。

 非同期メソッドの詳細については、「[Async および Await を使用した非同期プログラミング](async.md)」、「[非同期プログラムにおける制御フロー](programming-guide/concepts/async/control-flow-in-async-programs.md)」、「[非同期の戻り値の型](programming-guide/concepts/async/async-return-types.md)」を参照してください。

<a name="expr"></a>

## <a name="expression-bodied-members"></a>式形式のメンバー

メソッドの定義としては、式の結果を即座に返すか、またはメソッドの本文として 1 つのステートメントを含むものが一般的です。  `=>`を使用してこのようなメソッドを定義するための構文ショートカットがあります。

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

メソッドが `void` を返すか、非同期メソッドである場合は、メソッドの本文を (ラムダの場合と同様に) ステートメント式にする必要があります。  プロパティとインデクサーは読み取り専用にする必要があるため、`get` アクセサー キーワードは使用しないでください。

<a name="iterators"></a>

## <a name="iterators"></a>反復子

反復子は、リストや配列など、コレクションに対するカスタム イテレーションを実行します。 反復子は、[yield return](language-reference/keywords/yield.md) ステートメントを使用して、各要素を 1 回に 1 つ返します。 `yield return` ステートメントに到達すると、現在の場所が記録されます。呼び出し元は、シーケンス内の次の要素を要求できます。

反復子の戻り値の型には、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601> を指定できます。

詳細については、「 [反復子](programming-guide/concepts/iterators.md)」を参照してください。

## <a name="see-also"></a>参照

- [アクセス修飾子](language-reference/keywords/access-modifiers.md)
- [静的クラスと静的クラス メンバー](programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [継承](programming-guide/classes-and-structs/inheritance.md)
- [抽象クラスとシール クラス、およびクラス メンバー](programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [params](language-reference/keywords/params.md)
- [out](language-reference/keywords/out-parameter-modifier.md)
- [ref](language-reference/keywords/ref.md)
- [in](language-reference/keywords/in-parameter-modifier.md)
- [パラメーターの引き渡し](programming-guide/classes-and-structs/passing-parameters.md)
