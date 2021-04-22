---
title: C# 9.0 の新機能 - C# ガイド
description: C# 9.0 で使用できる新しい機能の概要を説明します。
ms.date: 04/07/2021
ms.openlocfilehash: e43ecc02fa7fe1f9a0a76353ce0b6bce001f9dac
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514425"
---
# <a name="whats-new-in-c-90"></a>C# 9.0 の新機能

C# 9.0 によって、C# 言語に次の機能と機能強化が追加されています。

- [レコード](#record-types)
- [init 専用セッター](#init-only-setters)
- [最上位レベルのステートメント](#top-level-statements)
- [パターン マッチングの拡張機能](#pattern-matching-enhancements)
- [パフォーマンスと相互運用](#performance-and-interop)
  - [ネイティブ サイズの整数](~/_csharplang/proposals/csharp-9.0/native-integers.md)
  - [関数ポインター](~/_csharplang/proposals/csharp-9.0/function-pointers.md)
  - [localsinit フラグの出力を抑制する](~/_csharplang/proposals/csharp-9.0/skip-localsinit.md)
- [適合性と完成度の機能](#fit-and-finish-features)
  - [ターゲットにより型指定された `new` 式](~/_csharplang/proposals/csharp-9.0/target-typed-new.md)
  - [`static` 匿名関数](~/_csharplang/proposals/csharp-9.0/static-anonymous-functions.md)
  - [ターゲットにより型指定された条件式](~/_csharplang/proposals/csharp-9.0/target-typed-conditional-expression.md)
  - [共変の戻り値の型](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)
  - [`foreach` ループでの拡張機能 `GetEnumerator` のサポート](~/_csharplang/proposals/csharp-9.0/extension-getenumerator.md)
  - [ラムダ ディスカード パラメーター](~/_csharplang/proposals/csharp-9.0/lambda-discard-parameters.md)
  - [ローカル関数の属性](~/_csharplang/proposals/csharp-9.0/local-function-attributes.md)
- [コード ジェネレーターのサポート](#support-for-code-generators)
  - [モジュールの初期化子](~/_csharplang/proposals/csharp-9.0/module-initializers.md)
  - [部分メソッドの新機能](~/_csharplang/proposals/csharp-9.0/extending-partial-methods.md)

C# 9.0 は **.NET 5** でサポートされています。 詳細については、「[C# 言語のバージョン管理](../language-reference/configure-language-version.md)」を参照してください。

最新の .NET SDK は [.NET のダウンロード ページ](https://dotnet.microsoft.com/download)でダウンロードできます。

## <a name="record-types"></a>レコードの種類

C# 9.0 では、"***レコード型***" が導入されています。 `record` キーワードを使用して、データをカプセル化するための組み込み機能を提供する参照型を定義します。 位置指定パラメーターまたは標準のプロパティ構文を使用して、不変のプロパティを持つレコード型を作成できます。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="PositionalRecord":::
:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="ImmutableRecord":::

また、変更可能なプロパティとフィールドを使用してレコード型を作成することもできます。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="MutableRecord":::

レコードは変更可能ですが、これらは主に不変のデータ モデルをサポートすることを目的としています。 レコード型には次の機能があります。

* [不変プロパティを持つ参照型を作成するための簡潔な構文](#positional-syntax-for-property-definition)
* データ中心の参照型に役立つビヘイビアー:
  * [値の等価性](#value-equality)
  * [非破壊的な変化の簡潔な構文](#nondestructive-mutation)
  * [表示用の組み込みの書式設定](#built-in-formatting-for-display)
* [継承階層のサポート](#inheritance)

[構造型](../language-reference/builtin-types/struct.md)を使用して、値の等価性があり、ビヘイビアーがほとんどない、またはまったくないデータ中心の型を設計することができます。 ただし、比較的大規模なデータ モデルの場合、構造体型にはいくつかの欠点があります。

* 継承はサポートされていません。
* 値の等価性を決定する場合には効率的ではありません。 値型の場合、<xref:System.ValueType.Equals%2A?displayProperty=nameWithType> メソッドによってリフレクションが使用され、すべてのフィールドが検索されます。 レコードの場合、コンパイラによって `Equals` メソッドが生成されます。 実際のところ、レコードでの値の等価性の実装は、多少は高速です。
* すべてのインスタンスにすべてのデータの完全なコピーがあるため、一部のシナリオではより多くのメモリが使用されます。 レコード型は[参照型](../language-reference/builtin-types/reference-types.md)であるため、レコード インスタンスにはデータへの参照のみが含まれます。

### <a name="positional-syntax-for-property-definition"></a>プロパティ定義の位置指定構文

位置指定パラメーターを使用すると、レコードのプロパティを宣言し、インスタンスを作成するときにプロパティ値を初期化できます。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InstantiatePositional":::

プロパティ定義に位置指定構文を使用すると、コンパイラにより、以下が作成されます。

* レコード宣言で指定される各位置指定パラメーターのパブリック init 専用自動実装プロパティ。 [init 専用](../language-reference/keywords/init.md)プロパティは、コンストラクターで、またはプロパティ初期化子を使用して設定できます。
* パラメーターがレコード宣言の位置指定パラメーターと一致するプライマリ コンストラクター。
* レコード宣言で指定された各定位置指定パラメーターの `out` パラメーターを使用する `Deconstruct` メソッド。

詳細については、レコードに関する C# 言語リファレンスの記事の[位置指定構文](../language-reference/builtin-types/record.md#positional-syntax-for-property-definition)に関するセクションを参照してください。

### <a name="immutability"></a>不変性

レコード型は必ずしも不変ではありません。 `readonly` ではない `set` アクセサーとフィールドを使用してプロパティを宣言できます。 ただし、レコードは変更可能ですが、不変のデータ モデルを簡単に作成できます。 位置指定構文を使用して作成したプロパティは変更できません。

不変性は、データ中心の型をスレッドセーフにする必要がある場合、またはハッシュ テーブル内でハッシュ コードを同じにしたい場合に役立ちます。 メソッドに参照によって引数を渡すときに発生するバグを防ぐことができます。また、メソッドによって引数の値が予期せず変更されることがあります。

レコード型に固有の機能は、コンパイラによって合成されたメソッドによって実装されます。このようなメソッドのいずれを使用してオブジェクトの状態を変更しても、不変性は損なわれません。

### <a name="value-equality"></a>値の等価性

値の等価性とは、型が一致し、かつプロパティおよびフィールドの値がすべて一致する場合にレコード型の 2 つの変数が等しいことを意味します。 その他の参照型の場合、等価性は ID を意味します。 つまり、参照型の 2 つの変数は、同じオブジェクトを参照する場合、等しいことになります。

次の例は、レコード型の値が等しいことを示しています。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="Equality":::

`class` 型では、等価性メソッドと演算子を手動でオーバーライドして値の等価性を実現できますが、そのコードの開発とテストには時間がかかり、エラーが発生しやすいものです。 この機能を組み込むと、プロパティまたはフィールドが追加または変更されたときにカスタムのオーバーライド コードの更新を忘れたことで発生するバグを防ぐことができます。

詳細については、レコードに関する C# 言語リファレンスの記事の「[値の等価性](../language-reference/builtin-types/record.md#value-equality)」セクションを参照してください。

### <a name="nondestructive-mutation"></a>非破壊な変化

レコード インスタンスの不変プロパティを変更する必要がある場合は、`with` 式を使用して "*非破壊的な変化*" を実現できます。 `with` 式を使用すると、指定したプロパティとフィールドが変更された、既存のレコード インスタンスのコピーである新しいレコード インスタンスが作成されます。 次の例に示すように、[オブジェクト初期化子](../programming-guide/classes-and-structs/object-and-collection-initializers.md)構文を使用して、変更する値を指定します。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="WithExpressions":::

詳細については、レコードに関する C# 言語リファレンスの記事の「[非破壊な変化](../language-reference/builtin-types/record.md#nondestructive-mutation)」セクションを参照してください。

### <a name="built-in-formatting-for-display"></a>表示用の組み込みの書式設定

レコード型には、パブリック プロパティとフィールドの名前と値を表示する、コンパイラによって生成された <xref:System.Object.ToString%2A> メソッドがあります。 `ToString` メソッドからは、次の形式の文字列が返されます。

> \<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}

参照型の場合、プロパティ値ではなく、プロパティから参照されるオブジェクトの型名が表示されます。 次の例では、配列は参照型であるため、実際の配列要素値ではなく `System.String[]` が表示されます。

```
Person { FirstName = Nancy, LastName = Davolio, ChildNames = System.String[] }
```

詳細については、レコードに関する C# 言語リファレンスの記事の[組み込みの書式設定](../language-reference/builtin-types/record.md#built-in-formatting-for-display)に関するセクションを参照してください。

### <a name="inheritance"></a>継承

レコードは、別のレコードから継承できます。 ただし、レコードはクラスから継承できません。また、クラスはレコードから継承できません。

次の例は、位置指定プロパティ構文を使用した継承を示しています。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="PositionalInheritance":::

2 つのレコード変数が等しくなるには、ランタイム型が等しくなければなりません。 含まれている変数型は異なっていていても構いません。 これを次のコード例に示します。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InheritanceEquality":::

この例では、すべてのインスタンスが同じプロパティであり、同じプロパティ値です。 どちらも `Person` 型の変数ですが、`student == teacher` は、`False` を返します。 一方は `Person` 変数、もう一方は `Student` 変数ですが、`student == student2` は、`True` を返します。

次の例に示すように、派生型と基本データ型の両方のすべてのパブリック プロパティとフィールドが `ToString` の出力に含まれます。

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="ToStringInheritance":::

詳細については、レコードに関する C# 言語リファレンスの記事の「[継承](../language-reference/builtin-types/record.md#inheritance)」セクションを参照してください。

## <a name="init-only-setters"></a>init 専用セッター

"***init 専用セッター***" によって、オブジェクトのメンバーを初期化するための一貫した構文が提供されます。 プロパティ初期化子を使用すると、どの値によってどのプロパティが設定されているかが明確にされます。 欠点は、それらのプロパティが設定可能である必要があることです。 C# 9.0 以降では、プロパティとインデクサーに対して `set` アクセサーの代わりに `init` アクセサーを作成できます。 呼び出し元により、プロパティ初期化子構文を使用して作成式でこれらの値を設定することができますが、構築が完了するとそれらのプロパティは読み取り専用になります。 init 専用セッターによって、状態を変更するためのウィンドウが提供されます。 構築フェーズが終了すると、そのウィンドウは閉じます。 プロパティ初期化子と with 式の完了を含め、すべての初期化の後で、構築フェーズは実質的に終了します。

`init` 専用セッターは、記述する任意の型で宣言できます。 たとえば、次の構造体では、気象監視構造体が定義されています。

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="DeclareWeatherObservation":::

呼び出し元は、プロパティ初期化子構文を使用して値を設定できますが、それでも不変性は維持されます。

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="UseWeatherObservation":::

初期化後に監視を変更しようとすると、コンパイラ エラーが発生します。

```csharp
// Error! CS8852.
now.TemperatureInCelsius = 18;
```

init 専用セッターは、派生クラスから基底クラスのプロパティを設定する場合に便利です。 また、基底クラスのヘルパーを使用して派生プロパティを設定することもできます。 位置指定レコードによって、init 専用セッターを使用してプロパティが宣言されます。 これらのセッターは、with 式で使用されます。 定義する任意の `class`、`struct` または `record` に対して、init 専用セッターを宣言できます。

詳細については、「[init (C# リファレンス)](../language-reference/keywords/init.md)」を参照してください。

## <a name="top-level-statements"></a>最上位レベルのステートメント

"***最上位レベル ステートメント***" により、多くのアプリケーションから不要な手続きが削除されます。 正規の "Hello World!" プログラムについて考えます。

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

何かを行うコード行は 1 つだけです。 最上位レベル ステートメントを使用すると、そのすべての定型句を、`using` ディレクティブと処理を行う 1 行に置き換えることができます。

:::code language="csharp" source="snippets/whats-new-csharp9/Program.cs" ID="TopLevelStatements":::

1 行だけのプログラムが必要な場合は、`using` ディレクティブを削除し、完全修飾型名を使用することができます。

```csharp
System.Console.WriteLine("Hello World!");
```

最上位レベル ステートメントを使用できるのは、アプリケーション内の 1 つのファイルだけです。 コンパイラにより、複数のソース ファイルで最上位レベル ステートメントが検出されると、エラーになります。 また、最上位レベル ステートメントを、宣言されたプログラム エントリ ポイント メソッド (通常は `Main` メソッド) と組み合わせても、エラーになります。 ある程度まで、その 1 つのファイルに、通常は `Program` クラスの `Main` メソッドに含まれるステートメントが含まれているものと考えることができます。  

この機能の最も一般的な用途の 1 つは、教材の作成です。 C# の開発初心者は、正規の "Hello World!" を 1 行または 2 行のコードで作成できます。 余分な手続きは必要ありません。 一方、経験豊富な開発者は、この機能の多くの用途を見つけることができます。 最上位レベル ステートメントを使用すると、Jupyter Notebook で提供されるものと同様の実験用にスクリプトに似たエクスペリエンスを有効にできます。 最上位レベル ステートメントは、小規模なコンソール プログラムとユーティリティに最適です。 [Azure Functions](/azure/azure-functions/) は、最上位レベル ステートメントに最適なユース ケースです。

最も重要なのは、最上位レベル ステートメントでアプリケーションのスコープや複雑さが制限されないことです。 それらのステートメントでは、任意の .NET クラスにアクセスしたり、使用したりできます。 また、コマンド ライン引数や戻り値の使用も制限されません。 最上位レベル ステートメントでは、`args` という名前の文字列の配列にアクセスできます。 最上位レベル ステートメントで整数値が返される場合、その値は、合成された `Main` メソッドからの整数のリターン コードになります。 最上位レベル ステートメントには、非同期式を含めることができます。 その場合、合成されたエントリ ポイントからは `Task` または `Task<int>` が返されます。

詳細については、「[最上位レベルのステートメント (C# プログラミング ガイド)](../programming-guide/main-and-command-args/top-level-statements.md)」を参照してください。

## <a name="pattern-matching-enhancements"></a>パターン マッチングの拡張機能

C# 9 には、新しいパターン マッチングの機能強化が含まれています。

- "***型パターン***" は、変数が型である場合に一致します
- "***かっこで囲まれたパターン***" では、パターンの組み合わせの優先順位が適用または強調されます
- "***接続的 `and` パターン***" では、両方のパターンが一致することが要求されます
- "***離接的 `or` パターン***" では、どちらかのパターンが一致することが要求されます
- "***否定的 `not` パターン***" では、パターンが一致しないことが要求されます
- "***関係パターン***" では、入力が定数より小さい、より大きい、以下、または以上であることが要求されます。

これらのパターンにより、パターンの構文が豊富になります。 次のような例を考えてみてください。

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterPattern":::

省略可能なかっこを使用して、`and` が `or` より優先順位が高いことを明確にします。

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterOrSeparatorPattern":::

最も一般的な使用方法の 1 つは、null チェックの新しい構文です。

```csharp
if (e is not null)
{
    // ...
}
```

これらのパターンのいずれも、パターンが許可される任意のコンテキスト (`is` パターン式、`switch` 式、入れ子になったパターン、`switch` ステートメントの `case` ラベルのパターン) で使用できます。

詳細については、「[パターン (C# リファレンス)](../language-reference/operators/patterns.md)」を参照してください。

詳細については、「[パターン](../language-reference/operators/patterns.md)」の記事の「[リレーショナル パターン](../language-reference/operators/patterns.md#relational-patterns)」と「[論理パターン](../language-reference/operators/patterns.md#logical-patterns)」のセクションを参照してください。

## <a name="performance-and-interop"></a>パフォーマンスと相互運用

3 つの新機能により、高パフォーマンスを必要とするネイティブ相互運用および低レベル ライブラリのサポートが向上します (ネイティブ サイズの整数、関数ポインター、`localsinit` フラグの省略)。

ネイティブ サイズの整数 `nint` と `nuint` は整数型です。 これらは、基になる型 <xref:System.IntPtr?displayProperty=nameWithType> および <xref:System.UIntPtr?displayProperty=nameWithType> によって表されます。 コンパイラによって、これらの型に対する追加の変換と操作がネイティブ int として公開されます。 ネイティブ サイズの整数では、`MaxValue` または `MinValue` のプロパティが定義されます。 これらの値は、ターゲット コンピューターでの整数のネイティブ サイズに依存するため、コンパイル時の定数として表すことはできません。 これらの値は実行時に読み取り専用になります。 `nint` に対する定数値は、[`int.MinValue` .. `int.MaxValue`]. `nuint` に対する定数値は、[`uint.MinValue` .. `uint.MaxValue`]. コンパイラによって、<xref:System.Int32?displayProperty=nameWithType> 型と <xref:System.UInt32?displayProperty=nameWithType> 型を使用するすべての単項演算子と二項演算子に対して、定数の折りたたみが実行されます。 結果が 32 ビットに収まらない場合、演算は実行時に実行され、定数とは見なされません。 ネイティブ サイズの整数を使用すると、整数演算が広く使用されており、最速のパフォーマンスを実現する必要があるシナリオで、パフォーマンスを向上させることができます。 詳細については、[`nint` と `nuint` 型](../language-reference/builtin-types/nint-nuint.md)に関するページを参照してください

関数ポインターでは、IL オペコード `ldftn` および `calli` にアクセスするための簡単な構文が提供されます。 関数ポインターは、新しい `delegate*` 構文を使用して宣言できます。 `delegate*` 型はポインター型です。 `Invoke()` メソッドで `callvirt` が使用されるデリゲートとは異なり、`delegate*` 型の呼び出しでは `calli` が使用されます。 構文的には、呼び出しは同じです。 関数ポインターの呼び出しでは、`managed` の呼び出し規約が使用されます。 `unmanaged` の呼び出し規約が必要であることを宣言するには、`delegate*` 構文の後に `unmanaged` キーワードを追加します。 その他の呼び出し規約は、`delegate*` 宣言の属性を使用して指定できます。 詳細については、「[アンセーフ コードとポインター型](../language-reference/unsafe-code.md)」を参照してください。

最後に、<xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute?displayProperty=nameWithType> を追加することで、`localsinit` フラグを生成しないようコンパイラに指示することができます。 このフラグは、すべてのローカル変数をゼロで初期化するように CLR に指示します。 1\.0 から、`localsinit` フラグが C# に対する既定の動作でした。 しかし、一部のシナリオでは、ゼロによる初期化を追加すると、パフォーマンスに大きく影響する可能性があります。 特に、`stackalloc` を使用する場合です。 そのような場合は、<xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute> を追加できます。 1 つのメソッドまたはプロパティに、または `class`、`struct`、`interface` に、さらにはモジュールに対してさえも、それを追加できます。 この属性は `abstract` メソッドに影響しません。実装用に生成されたコードに影響します。 詳細については、「[`SkipLocalsInit` 属性](../language-reference/attributes/general.md#skiplocalsinit-attribute)」を参照してください。

これらの機能により、一部のシナリオでパフォーマンスを向上させることができます。 導入前と導入後の両方で慎重にベンチマークを行った後でのみ、使用する必要があります。 ネイティブ サイズの整数に関するコードは、複数のターゲット プラットフォームで、異なる整数サイズを使用して、テストする必要があります。 その他の機能には、アンセーフ コードが必要です。

## <a name="fit-and-finish-features"></a>適合性と完成度の機能

他の多くの機能は、コードをより効率的に記述するのに役立ちます。 C# 9.0 では、作成されるオブジェクトの型が既にわかっている場合、[`new` 式](../language-reference/operators/new-operator.md)で型を省略できます。 最も一般的な使用方法は、フィールドの宣言です。

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="WeatherStationField":::

ターゲット型の `new` は、メソッドへの引数として渡す新しいオブジェクトを作成する必要がある場合にも使用できます。 次のようなシグネチャを持つ `ForecastFor()` メソッドについて考えます。

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="ForecastSignature":::

これを、次のように呼び出すことができます。

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="TargetTypeNewArgument":::

この機能のもう 1 つの便利な用途は、init 専用プロパティと組み合わせて、新しいオブジェクトを初期化する場合です。

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="InitWeatherStation":::

`return new();` ステートメントを使用することで、既定のコンストラクターによって作成されたインスタンスを返すことができます。

同様の機能により、[条件式](../language-reference/operators/conditional-operator.md)の対象となる型の解決が向上します。 この変更により、2 つの式の間で暗黙的な変換を行う必要はありませんが、どちらもターゲット型への暗黙的な変換を行うことができます。 多くの場合、この変更に気付くことはありません。 気付くとすれば、以前はキャストを必要としたり、コンパイルされなかったりした一部の条件式が、機能するようになることです。

C# 9.0 以降では、`static` 修飾子を[ラムダ式](../language-reference/operators/lambda-expressions.md)または[匿名メソッド](../language-reference/operators/delegate-operator.md)に追加できます。 静的なラムダ式は、`static` ローカル関数に似ています。静的ラムダまたは匿名メソッドでは、ローカル変数またはインスタンスの状態をキャプチャできません。 `static` 修飾子により、誤って他の変数がキャプチャされることがなくなります。

共変の戻り値の型を使用すると、[override](../language-reference/keywords/override.md) メソッドの戻り値の型を柔軟に指定できます。 override メソッドは、オーバーライドされた基本メソッドの戻り値の型から派生した型を返すことができます。 これは、レコードや、仮想クローンまたはファクトリ メソッドをサポートするその他の型に役立ちます。

また、[`foreach` ループ](../language-reference/keywords/foreach-in.md)によって、それ以外の方法で `foreach` パターンを満たす拡張メソッド `GetEnumerator` が認識され、使用されます。 この変更は、非同期パターンやパターンベースの分解など、他のパターンベースのコンストラクションと `foreach` の間に整合性があることを意味します。 実際、この変更は、あらゆる型に `foreach` サポートを追加できることを意味します。 それの使用は、設計においてオブジェクトの列挙に意味があるときのみに限定してください。

次に、ラムダ式に対するパラメーターとして破棄を使用できます。 このようにすると、引数の名前付けを避けることができ、コンパイラではその使用を避けることができます。 任意の引数に対して `_` を使用します。 詳細については、[ラムダ式](../language-reference/operators/lambda-expressions.md)に関する記事の「[ラムダ式の入力パラメーター](../language-reference/operators/lambda-expressions.md#input-parameters-of-a-lambda-expression)」セクションを参照してください。

ようやく、[ローカル関数](../programming-guide/classes-and-structs/local-functions.md)に属性を適用できるようになりました。 たとえば、[null 許容属性の注釈](../language-reference/attributes/nullable-analysis.md)をローカル関数に適用できます。

## <a name="support-for-code-generators"></a>コード ジェネレーターのサポート

2 つの最終機能では、C# コード ジェネレーターがサポートされています。 C# コード ジェネレーターは、roslyn アナライザーまたはコード修正と同じように記述できるコンポーネントです。 違いは、コード ジェネレーターでは、コードが分析され、コンパイル プロセスの一環として新しいソース コード ファイルが記述されることです。 一般的なコード ジェネレーターでは、属性またはその他の規則がコードで検索されます。

コード ジェネレーターにより、Roslyn 分析 API を使用して属性または他のコード要素が読み取られます。 その情報を基にして、コンパイルに新しいコードが追加されます。 ソース ジェネレーターではコードが追加されるだけで、コンパイル中の既存のコードの変更は許可されていません。

コード ジェネレーターに対して追加された 2 つの機能は、"***部分メソッド構文** _" の拡張機能と、"_*_モジュール初期化子_**" です。 1 つ目は、部分メソッドに対する変更です。 C# 9.0 より前の部分メソッドは `private` ですが、アクセス修飾子を指定することはできず、戻り値は `void` で、`out` パラメーターを持つことはできません。 これらの制限は、メソッドの実装を提供しないと、コンパイラによって部分メソッドのすべての呼び出しが削除されることを意味しました。 C# 9.0 ではこれらの制限はなくなりましたが、部分メソッドの宣言には実装が必要です。 コード ジェネレーターで、その実装を提供できます。 破壊的変更が発生しないよう、コンパイラでは、アクセス修飾子を持たないすべての部分メソッドは、古い規則に従うものと見なされます。 部分メソッドに `private` アクセス修飾子が含まれている場合、その部分メソッドは新しい規則によって制御されます。 詳細については、「[partial メソッド (C# リファレンス)](../language-reference/keywords/partial-method.md)」を参照してください。

コード ジェネレーターの 2 つ目の新機能は、"***モジュール初期化子***" です。 モジュール初期化子は、<xref:System.Runtime.CompilerServices.ModuleInitializerAttribute> 属性が関連付けられているメソッドです。 これらのメソッドは、全体モジュール内の他のフィールド アクセスまたはメソッド呼び出しの前にランタイムによって呼び出されます。 モジュール初期化子メソッドは次のようなものです。

- 静的でなければなりません
- パラメーターなしである必要があります
- void を返す必要があります
- ジェネリック メソッドであってはなりません
- ジェネリック クラスに含まれていてはなりません
- それを含むモジュールからアクセスできる必要があります

最後の項目は事実上、メソッドとそれを含んでいるクラスが internal または public である必要があることを意味します。 メソッドをローカル関数にすることはできません。 詳細については、「[`ModuleInitializer` 属性](../language-reference/attributes/general.md#moduleinitializer-attribute)」を参照してください。
