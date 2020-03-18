---
title: C# switch ステートメント
ms.date: 04/09/2019
f1_keywords:
- switch_CSharpKeyword
- switch
- case
- case_CSharpKeyword
helpviewer_keywords:
- switch statement [C#]
- switch keyword [C#]
- case statement [C#]
- default keyword [C#]
ms.assetid: 44bae8b8-8841-4d85-826b-8a94277daecb
ms.openlocfilehash: e5580e81b9175cd95491fdba724bacbffa692a5e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75345387"
---
# <a name="switch-c-reference"></a>switch (C# リファレンス)

`switch` ステートメントは選択ステートメントです。このステートメントは、実行する 1 つの "*switch セクション*" を候補のリストから "*match 式*" によるパターン マッチに基づいて選択します。

[!code-csharp[switch#1](~/samples/snippets/csharp/language-reference/keywords/switch/switch1.cs#1)]

`switch` ステートメントは、1 つの式が 3 つ以上の条件に対してテストされる場合に、[if-else](if-else.md) コンストラクトの代わりとしてよく使用されます。 たとえば、次の `switch` ステートメントは、`Color` 型の変数に 3 つの値のいずれかが含まれているかどうかを確認します。

[!code-csharp[switch#3](~/samples/snippets/csharp/language-reference/keywords/switch/switch3.cs#1)]

これは、`if`-`else` コンストラクトを使用する次の例に相当します。

[!code-csharp[switch#3a](~/samples/snippets/csharp/language-reference/keywords/switch/switch3a.cs#1)]

## <a name="the-match-expression"></a>match 式

match 式は、`case` ラベルのパターンと照合する値を指定します。 構文は次のとおりです。

```csharp
   switch (expr)
```

C# 6 以前では、match 式は、次の型の値を返す必要があります。

- [char](../builtin-types/char.md)。
- [string](../builtin-types/reference-types.md)。
- [bool](../builtin-types/bool.md)。
- [integral](../builtin-types/integral-numeric-types.md) 値。`int` や `long` など。
- [enum](../builtin-types/enum.md)値。

C# 7.0 以降は、match 式は NULL 以外の式にできます。

## <a name="the-switch-section"></a>switch セクション

`switch` ステートメントには、1 つ以上の switch セクションが含まれています。 各 switch セクションには、1 つ以上の "*case ラベル*" (case ラベルまたは default ラベルのいずれか) と、その後に続く 1 つ以上のステートメントのリストが含まれています。 `switch` ステートメントでは、任意の switch セクションに少なくとも 1 つの default ラベルを配置することができます。 次の例に、3 つの switch セクションを持つシンプルな `switch` ステートメントを示します。各セクションには 2 つのステートメントがあります。 2 番目の switch セクションには、`case 2:` ラベルと `case 3:` ラベルが含められています。

`switch` ステートメントには、任意の数の switch セクションを含めることができます。また、次の例に示すように、各セクションに 1 つ以上の case ラベルを含めることができます。 ただし、複数の case ラベルで同じ式を使用することはできません。

[!code-csharp[switch#2](~/samples/snippets/csharp/language-reference/keywords/switch/switch2.cs#1)]

1 つの switch ステートメントでは、1 つの switch セクションのみが実行されます。 C# では 1 つの switch セクションから次のセクションへ実行が連続することが許可されません。 このため、次のコードでは、コンパイラ エラー CS0163:"コントロールは 1 つの case ラベル (\<case label>) から別の case ラベルへフォールスルーすることはできません。"

```csharp
switch (caseSwitch)
{
    // The following switch section causes an error.
    case 1:
        Console.WriteLine("Case 1...");
        // Add a break or other jump statement here.
    case 2:
        Console.WriteLine("... and/or Case 2");
        break;
}
```

この要件は、通常、[break](break.md) ステートメント、[goto](goto.md) ステートメント、または [return](return.md) ステートメントを使用して、switch セクションを明示的に終了することによって満たされます。 ただし、次のコードも有効です。このコードでは、プログラムの制御が `default` switch セクションにフォール スルー (流れ落ちる、case ラベルを超えてコードを実行することが) できないためです。

[!code-csharp[switch#4](~/samples/snippets/csharp/language-reference/keywords/switch/switch4.cs#1)]

match 式に一致する case ラベルが含まれた switch セクションにおけるステートメント リストの実行は、ステートメント リストに沿って最初のステートメントから順に開始され、通常は、`break`、`goto case`、`goto label`、`return`、または`throw` などのジャンプ ステートメントに達するまで続きます。 この時点で、制御は `switch` ステートメントの外側、または他の case ラベルに移動します。 `goto` ステートメントを使用する場合は、制御を constant ラベルに転送する必要があります。 この制約が必要になるのは、非 constant ラベルに制御を転送しようとすると望ましくない副作用 (コード内の意図しない場所に制御を転送してしまったり、無限ループを作成してしまったりなど) が生じる可能性があるためです。

## <a name="case-labels"></a>case ラベル

各 case ラベルで、match 式と比較するためのパターンを指定します (前の例では `caseSwitch` 変数)。 一致すると、**最初の**一致 case を含む switch セクションに制御が移ります。 match 式と一致する case ラベル パターンがない場合は、`default` case ラベルがあれば、制御はそのラベルを含むセクションに移ります。 `default` case がない場合は、どの switch セクションのステートメントも実行されず、制御は `switch` ステートメント外に移ります。

`switch` ステートメントとパターン マッチングの詳細については、「[`switch` ステートメントによるパターン マッチング](#pattern)」を参照してください。

C# 6 でサポートされるのは定数パターンのみで、定数値の繰り返しは許可されません。このため、case ラベルでは相互に排他的な値が定義され、match 式と一致するのは 1 つのパターンだけです。 そのため、`case` ステートメントが表示される順序は重要ではありません。

一方、C# 7.0 では他のパターンがサポートされているため、case ラベルで定義する値が相互に排他的である必要はなく、match 式と一致するパターンが複数存在する可能性があります。 一致するパターンを含む最初の switch セクションのステートメントのみが実行されるので、ここでは、`case` ステートメントが表示される順序が重要になってきます。 C# によって switch セクションが検出され、その switch セクションの case ステートメントが前のステートメントと同じだったり、そのステートメントのサブセットだったりすると、コンパイラ エラー CS8120 "switch case は既に以前のケースで処理されています" が生成されます。

次の例は、相互に排他的でない各種パターンを使用する `switch` ステートメントを示しています。 `case 0:` switch セクションを移動し、そのセクションが `switch` ステートメントの最初のセクションでなくなると、C# によってコンパイラ エラーが生成されます。値がゼロの整数は、整数すべてのサブセットであるためです。これは、`case int val` ステートメントで定義されているパターンです。

[!code-csharp[switch#5](~/samples/snippets/csharp/language-reference/keywords/switch/switch5.cs#1)]

この問題を修正し、コンパイラの警告が表示されないようにするには、次の 2 つのいずれかの方法を使用します。

- switch セクションの順序を変更する。

- `case` ラベルで [when 句](#when) を使用する。

## <a name="the-default-case"></a>`default` case

`default` case では、match 式がどの `case` ラベルとも一致しない場合に実行する switch セクションを指定します。 `default` case を指定しない場合、match 式がどの `case` ラベルとも一致しないと、プログラム フローが `switch` ステートメントにフォール スルーします。

`default` case は、`switch` ステートメントで任意の順序で指定できます。 この case は、ソース コード内での順序に関係なく、すべての `case` ラベルが評価された後、最後に評価されます。

## <a name="a-namepattern--pattern-matching-with-the-switch-statement"></a>`switch` ステートメントによる <a name="pattern" /> パターン マッチング

各 `case` ステートメントで定義されたパターンが match 式と一致した場合に、switch セクションが実行されます。 定数パターンは、すべてのバージョンの C# でサポートされます。 それ以外のパターンは、C# 7.0 以降でサポートされています。

### <a name="constant-pattern"></a>定数パターン

定数パターンでは、match 式が、指定された定数と等しいかどうかがテストされます。 構文は次のとおりです。

```csharp
   case constant:
```

ここで *constant* はテスト対象の値です。 *constant* には、次のいずれかの定数式を指定できます。

- [bool](../builtin-types/bool.md) リテラル。`true` または `false`。
- 任意の [integral](../builtin-types/integral-numeric-types.md) 定数。`int`、`long`、`byte` など。
- 宣言された `const` 変数の名前。
- 列挙定数。
- [char](../builtin-types/char.md) リテラル。
- [string](../builtin-types/reference-types.md) リテラル。

定数式は以下のように評価されます。

- *expr* と *constant* が整数型の場合、式から `true` が返されるかどうか (つまり、`expr == constant` であるかどうか) が C# の等値演算子によって判定されます。

- それ以外の場合、式の値は静的 [Object.Equals(expr, constant)](xref:System.Object.Equals(System.Object,System.Object)) メソッドの呼び出しによって判定されます。

次の例では、定数パターンを使用して、特定の日付が、週末か、週の開始日または最終日か、週の途中かを判断します。 つまり、現在の日付の <xref:System.DateTime.DayOfWeek?displayProperty=nameWithType> プロパティを、<xref:System.DayOfWeek> 列挙のメンバーと照合します。

[!code-csharp[switch#7](~/samples/snippets/csharp/language-reference/keywords/switch/const-pattern.cs#1)]

次の例では、定数パターンを使用して、自動コーヒー メーカーをシミュレートするコンソール アプリケーションのユーザー入力を処理します。

[!code-csharp[switch#6](~/samples/snippets/csharp/language-reference/keywords/switch/switch6.cs)]

### <a name="type-pattern"></a>型パターン

型パターンを使用すると、型の評価と変換を簡潔に記述できます。 `switch` ステートメントと共に使用してパターン マッチングを実行すると、指定された型に式を変換できるかどうかがテストされ、変換できる場合は、その型の変数にキャストされます。 構文は次のとおりです。

```csharp
   case type varname
```

ここで *type* は、*expr* の結果が変換される型の名前、*varname* は、一致した場合に *expr* の結果が変換されるオブジェクトを表しています。 コンパイル時の型 *expr* は、C# 7.1 以降では、ジェネリック型パラメーターにすることができます。

以下のいずれかの条件が true である場合に `case` 式は `true` となります。

- *expr* が *type* と同じ型のインスタンスである。

- *expr* が *type* から派生した型のインスタンスである。 つまり、*expr* の結果を *type* のインスタンスにアップキャストできる。

- *expr* のコンパイル時の型が *type* の基底クラスであり、*expr* の実行時の型が *type* または *type* から派生した型である。 変数の "*コンパイル時の型*" とは、その変数の型宣言で定義されている型です。 変数の "*実行時の型*" とは、その変数に代入されているインスタンスの型です。

- *expr* が、*type* インターフェイスを実装する型のインスタンスである。

case 式が true の場合は、*varname* が確実に割り当てられ、switch セクションにのみローカル スコープが含まれます。

`null` は型と一致しないことに注意してください。 `null` を一致させるには、次の `case` ラベルを使用します。

```csharp
case null:
```

次の例では、型パターンを使用して、さまざまな種類のコレクション型に関する情報を提供します。

[!code-csharp[type-pattern#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern.cs#1)]

次のコードに示すように、`object` の代わりに、コレクションの型を型パラメーターとして使用して、ジェネリック メソッドを作成することができます。

[!code-csharp[type-pattern#3](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern3.cs#1)]

ジェネリック バージョンは、2 つの点で最初のサンプルと異なります。 まず、`null` の case を使用できません。 コンパイラは任意の型 `T` を `object` 以外の型に変換できないため、定数の case は使用できません。 `default` の case だったものは、null 以外の `object` をテストするようになりました。 つまり、`default` の case は `null` のみをテストします。

パターン マッチングを使用しない場合、このコードは次のように記述できます。 型パターン マッチングを使用することにより、変換結果が `null` であるかどうかをテストしたり、キャストを繰り返したりする必要がなくなるため、コードがよりコンパクトで読みやすくなります。

[!code-csharp[type-pattern2#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern2.cs#1)]

## <a name="a-namewhen--the-case-statement-and-the-when-clause"></a><a name="when" />`case`ステートメントおよび `when` 句

C# 7.0 以降では、case ステートメントは相互に排他的である必要がないため、`when` 句を追加して、case ステートメントを true に評価するために満たされなければならない条件を指定できます。 `when` 句には、ブール値を返す任意の式を指定できます。

次の例では、`Shape` 基底クラス、`Shape` から派生する `Rectangle` クラス、および `Rectangle` から派生する `Square` クラスを定義しています。 ここでは `when` 句を使用して、同じ長さと幅が割り当てられている `Rectangle` オブジェクトが、`Square` オブジェクトとしてインスタンス化されていなくても、`ShowShapeInfo` によって確実に `Square` として処理されるようにしています。 このメソッドは、`null` オブジェクトの情報や、面積がゼロの図形の情報を表示しようとしません。

[!code-csharp[when-clause#1](~/samples/snippets/csharp/language-reference/keywords/switch/when-clause.cs#1)]

この例で、`Shape` オブジェクトが `null` かどうかをテストしようとする `when` 句は実行されません。 `null` をテストするための正しい型パターンは `case null:` です。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](/dotnet/csharp/language-reference/language-specification/introduction)に関するページの「[switch ステートメント](~/_csharplang/spec/statements.md#the-switch-statement)」を参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# プログラミング ガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [if-else](if-else.md)
- [パターン一致](../../pattern-matching.md)
