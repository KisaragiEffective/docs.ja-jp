---
title: 安全で効率的な C# コードを記述する
description: C# 言語に対する最新の機能強化により、以前はアンセーフ コードに関連付けられていたようなパフォーマンスの検証可能なセーフ コードを記述することができます。
ms.date: 10/23/2018
ms.technology: csharp-advanced-concepts
ms.custom: mvc
ms.openlocfilehash: d4a7916b80e15c7f00fa0a7da213ed0593e0959d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78239977"
---
# <a name="write-safe-and-efficient-c-code"></a>安全で効率的な C# コードを記述する

C# の新しい機能により、よりよいパフォーマンスの検証可能なセーフ コードを記述できます。 これらの手法を慎重に適用した場合、アンセーフ コードが必要なシナリオが少なくなります。 これらの機能により、メソッドの引数およびメソッドの戻り値として、これまでより簡単に値の型への参照を使用できるようになります。 これらの手法を安全に使用すると、値の型のコピーが最小限に抑えられます。 値の型を使用することで、割り当てとガベージ コレクション パスの数が最小限になります。

この記事にあるサンプル コードの多くでは、C# 7.2 で追加された機能が使用されています。 そのような機能を使用するには、C# 7.2 以降を使用するようにプロジェクトを構成する必要があります。 言語バージョンを設定する方法の詳細については、[言語バージョンの構成](language-reference/configure-language-version.md)に関する記事を参照してください。

この記事では、効率的なリソース管理の手法に焦点を当てます。 値の型を利用する利点の 1 つは、多くの場合にヒープ割り当てが回避されることです。 欠点は、値でコピーされるということです。 このトレードオフは、大量のデータを操作するアルゴリズムの最適化を難しくします。 C# 7.2 の新しい言語機能では、値の型への参照を使用して安全で効率的なコードを作成できるメカニズムが提供されます。 これらの機能を賢く使って、割り当てとコピー操作の両方を最小限に抑えます。 この記事では、これらの新しい機能について説明します。

この記事では、以下のリソース管理手法に焦点を当てます。

- [`readonly struct`](language-reference/keywords/readonly.md#readonly-struct-example) を宣言して型が**変更不可**であること表し、コンパイラが [`in`](language-reference/keywords/in-parameter-modifier.md) パラメーターを使用するときにコピーを保存できるようにします。
- 型を変更できない場合は、メンバーが状態を変更しないことを示すために、`struct` メンバーに `readonly` を宣言します。
- 戻り値が <xref:System.IntPtr.Size?displayProperty=nameWithType> より大きい `struct` であり、ストレージの有効期間が値を返すメソッドより長い場合に、[`ref readonly`](language-reference/keywords/ref.md#reference-return-values) を使用して戻します。
- `readonly struct` のサイズが <xref:System.IntPtr.Size?displayProperty=nameWithType> より大きいときは、パフォーマンスのため、`in` として渡す必要があります。
- `readonly` 修飾子で宣言されている場合、またはメソッドが構造体の `readonly` メンバーのみを呼び出す場合を除き、`in` パラメーターとして `struct` は渡さないでください。 このガイダンスに違反すると、パフォーマンスが低下し、動作が不明瞭になる場合があります。
- バイトのシーケンスとしてメモリを操作するには、[`ref struct`](language-reference/keywords/ref.md#ref-struct-types) または <xref:System.Span%601> や <xref:System.ReadOnlySpan%601> などの `readonly ref struct` を使用します。

これらの手法では、**参照**と**値**に関する 2 つの相反する目標のバランスを取ることが強要されます。 [参照型](programming-guide/types/index.md#reference-types)の変数では、メモリ内の場所への参照が保持されます。 [値の型](programming-guide/types/index.md#value-types)の変数には、値が直接格納されます。 これらの違いにより、メモリ リソースを管理するために重要となる主な違いが強調されます。 **値の型**は、通常、メソッドに渡されるとき、またはメソッドから戻されるときに、コピーされます。 この動作には、値の型のメンバーを呼び出すときの、`this` の値のコピーが含まれます。 コピーのコストは、型のサイズに関係します。 **参照型**は、マネージド ヒープ上に割り当てられます。 新しいオブジェクトごとに新しく割り当てる必要があり、後でそれを回収する必要があります。 どちらの操作にも時間がかかります。 参照型が引数としてメソッドに渡されるとき、またはメソッドから戻されるときは、参照がコピーされます。

この記事では、次に示す 3 次元の点の構造体を概念の例として使用し、これらの推奨事項を説明します。

```csharp
public struct Point3D
{
    public double X;
    public double Y;
    public double Z;
}
```

別の例では、この概念の異なる実装が使用されます。

## <a name="declare-readonly-structs-for-immutable-value-types"></a>変更不可の値の型用に読み取り専用の構造体を宣言する

`readonly` 修飾子を使用して `struct` を宣言すると、変更不可の型を作成することが意図であることがコンパイラに伝わります。 コンパイラでは、以下の規則に従ってその設計の決定が適用されます。

- すべてのフィールドのメンバーは `readonly` でなければならない
- 自動的に実装されるプロパティも含めて、すべてのプロパティは読み取り専用でなければならない。

これら 2 つの規則は、`readonly struct` のメンバーによってその構造体の状態が変更されないことを保証するのに十分です。 `struct` は変更不可です。 次の例で示すように、`Point3D` 構造体を変更不可の構造体として定義できます。

```csharp
readonly public struct ReadonlyPoint3D
{
    public ReadonlyPoint3D(double x, double y, double z)
    {
        this.X = x;
        this.Y = y;
        this.Z = z;
    }

    public double X { get; }
    public double Y { get; }
    public double Z { get; }
}
```

変更不可の値の型を作成することが設計の意図である場合は常に、この推奨事項に従ってください。 パフォーマンスの向上はすべて付加的なメリットです。 `readonly struct` の機能は、設計の意図を明確に表現することです。

## <a name="declare-readonly-members-when-a-struct-cant-be-immutable"></a>構造体を変更不可にできない場合、読み取り専用メンバーを宣言する

C# 8.0 以降で構造体の型が変更可能な場合、変更を起こさないメンバーを `readonly` に宣言する必要があります。 たとえば、3D ポイントの構造体の変更可能なバリエーションは次のとおりです。

```csharp
public struct Point3D
{
    public Point3D(double x, double y, double z)
    {
        _x = x;
        _y = y;
        _z = z;
    }

    private double _x;
    public double X
    {
        readonly get => _x;
        set => _x = value;
    }

    private double _y;
    public double Y
    {
        readonly get => _y;
        set => _y = value;
    }

    private double _z;
    public double Z
    {
        readonly get => _z;
        set => _z = value;
    }

    public readonly double Distance => Math.Sqrt(X * X + Y * Y + Z * Z);

    public readonly override string ToString() => $"{X}, {Y}, {Z}";
}
```

上の例では、`readonly` 修飾子を適用できる多数の場所を多く示しています (メソッド、プロパティ、およびプロパティ アクセサー)。 自動実装プロパティを使用する場合、コンパイラは読み取り/書き込みプロパティに対し、`get` アクセサーに `readonly` 修飾子を追加します。 コンパイラは、`get` アクセサーのみを持つプロパティに対し、`readonly` 修飾子を、自動実装プロパティの宣言に追加します。

状態が変更不可なメンバーに `readonly` 修飾子を追加すると、2 つの関連する利点があります。 まず、コンパイラによって意図が適用されます。 そのメンバーは構造体の状態を変更することも、`readonly` とマークされていないメンバーにもアクセスできません。 2 つ目には、`readonly` メンバーにアクセスするときに、コンパイラは `in` パラメーターの防御的なコピーを作成しません。 コンパイラは、`readonly` メンバーによって `struct` が変更されないことを保証するため、この最適化を安全に行うことができます。

## <a name="use-ref-readonly-return-statements-for-large-structures-when-possible"></a>可能であれば大きい構造体には `ref readonly return` ステートメントを使用する

返される値が返すメソッドに対してローカルでない場合は、参照渡しで値を返すことができます。 参照渡しで返すということは、構造体ではなく参照のみがコピーされることを意味します。 次の例の `Origin` プロパティでは、返される値がローカル変数であるため、`ref` 返しを使用することはできません。

```csharp
public Point3D Origin => new Point3D(0,0,0);
```

一方、次のプロパティ定義では、返される値が静的メンバーであるため、参照渡しで返すことができます。

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    // Dangerous! returning a mutable reference to internal storage
    public ref Point3D Origin => ref origin;

    // other members removed for space
}
```

呼び出し元によって元の値が変更されては困るので、`ref readonly` で値を返す必要があります。

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    public static ref readonly Point3D Origin => ref origin;

    // other members removed for space
}
```

`ref readonly` を返すと、もっと大きい構造体をコピーしなくて済み、内部データ メンバーの変更不可性を維持することができます。

呼び出しサイトでは、`Origin` プロパティを `ref readonly` または値のどちらとして使用するかを、呼び出し元が選択します。

[!code-csharp[AssignRefReadonly](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#AssignRefReadonly "Assigning a ref readonly")]

先のコードの最初の割り当てでは、`Origin` 定数のコピーが作成され、そのコピーが割り当てられます。 2 つ目は参照を割り当てます。 `readonly` 修飾子は変数の宣言の一部にする必要があります。 それが参照するものは変更できません。 変更を試みると、コンパイル時エラーが発生します。

`originReference` の宣言では、`readonly` 修飾子が必要です。

コンパイラでは、呼び出し元で参照を変更できないように強制されます。 値を直接割り当てようとすると、コンパイル時エラーが生成されます。 ただし、メンバー メソッドによって構造体の状態が変更されるかどうかは、コンパイラでは認識できません。
オブジェクトが変更されないように、コンパイラではコピーが作成され、そのコピーを使用してメンバー参照が呼び出されます。 変更されるとすれば、その防御用のコピーに行われます。

## <a name="apply-the-in-modifier-to-readonly-struct-parameters-larger-than-systemintptrsize"></a>`System.IntPtr.Size` より大きい `readonly struct` パラメーターに `in` 修飾子を適用する

`in` キーワードは、既存の `ref` キーワードと `out` キーワードを補完し、引数を参照で渡します。 `in` キーワードでは、引数を参照で渡すことが指定されますが、呼び出されたメソッドでは値は変更されません。

この追加によって、設計の意図を表すためのボキャブラリが完全に与えられます。
メソッド シグネチャで次の修飾子のいずれも指定しないのであれば、呼び出されたメソッドに渡されるとき、値の型がコピーされます。 これらのどの修飾子を使用しても、変数を参照で渡すことが指定され、コピーが回避されます。 修飾子はそれぞれ、異なる意図を表します。

- `out`:このメソッドでは、このパラメーターとして使用される引数の値が設定されます。
- `ref`:このメソッドでは、このパラメーターとして使用される引数の値が設定されることがあります。
- `in`:このメソッドでは、このパラメーターとして使用される引数の値は変更されません。

`in` 修飾子を追加し、参照で引数を渡し、参照で引数を渡して不必要なコピーを回避する設計の意図を宣言します。 その引数として使用されるオブジェクトを変更することは、意図されていません。

この方法では、多くの場合、<xref:System.IntPtr.Size?displayProperty=nameWithType> より大きい読み取り専用の値の型でのパフォーマンスが向上します。 単純型 (`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`enum` 型) の場合、可能性のあるパフォーマンスの向上は最小限です。 実際には、<xref:System.IntPtr.Size?displayProperty=nameWithType> より小さい型に対して参照渡しを使用すると、パフォーマンスが低下する可能性があります。

次のコードは、3D 空間の 2 点間の距離を計算するメソッドの例です。

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

引数は 2 つの構造で、それぞれに 3 つの倍精度浮動小数点型が含まれます。 倍精度浮動小数点型は 8 バイトです。そのため、各引数は 24 バイトになります。 `in` 修飾子を指定することで、コンピューターのアーキテクチャに基づき、4 バイトまたは 8 バイトの参照をそれらの引数に渡します。 サイズの差はわずかですが、アプリケーションにおいて、多くの異なる値を使用する短いループでこのメソッドを呼び出すと膨れあがります。

`in` 修飾子は、その他の面でも `out` と `ref` を補完します。 `in`、`out`、または `ref` の存在のみが異なるメソッドのオーバーロードは作成できません。 これらの新しいルールは、`out` パラメーターと `ref` パラメーターに常に定義されていた同じ動作を拡張します。 `out` や `ref` 修飾子のように、`in` 修飾子が適用されるため、値の型はボックス化されません。

`in` 修飾子は、メソッド、デリケート、ラムダ、ローカル関数、インデクサー、演算子など、パラメーターを受け取るあらゆるメンバーに適用されることがあります。

`in` パラメーターのもう 1 つの機能として、`in` パラメーターへの引数にリテラル値または定数を使用できます。 また、`ref` パラメーターや `out` パラメーターとは異なり、呼び出しサイトで `in` 修飾子を適用する必要はありません。 次のコードは、`CalculateDistance` メソッドを呼び出す 2 つの例です。 最初のメソッドでは、参照で渡される 2 つのローカル変数が使用されます。 2 つ目のメソッドには、メソッド呼び出しの一部として作成される一時的な変数が含まれます。

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#UseInArgument "Specifying an In argument")]

コンパイラでは、`in` 引数の読み取り専用の性質を強制する方法がいくつかあります。  まず、呼び出されたメソッドは `in` パラメーターに直接割り当てできません。 値が `struct` 型の場合、`in` パラメーターのどのフィールドにも直接割り当てできません。 また、`ref` または `out` 修飾子を使用するメソッドに、`in` パラメーターを渡すことはできません。
これらの規則は、`in` パラメーターのすべてのフィールドに適用されます (ただし、フィールドが `struct` 型でパラメーターも `struct` 型の場合)。 実際、これらの規則は、メンバー アクセスのすべてのレベルで型が `structs` であれば、複数層のメンバー アクセスに適用されます。
コンパイラは `struct` 型を `in` 引数として渡し、その `struct` メンバーが他のメソッドへの引数として使用される場合は読み取り専用変数になるよう強制します。

`in` パラメーターを使用することで、コピーを作成することの潜在的なパフォーマンス コストを回避できます。 メソッド呼び出しのセマンティクスは変更されません。 そのため、呼び出しサイトで `in` 修飾子を指定する必要はありません。 呼び出しサイトで `in` 修飾子を省略すると、次の理由で、引数のコピーを作成することが許可されていることがコンパイラに通知されます。

- 暗黙的な変換は存在するが、引数の型からパラメーターの型への ID 変換が存在しない。
- 引数は式だが、既知のストレージ変数がない。
- `in` の有無によって異なるオーバーロードが存在する。 この場合は、値渡しオーバーロードの方がより適しています。

これらの規則は、既存のコードを読み取り専用の参照引数を使用するように更新するときに役立ちます。 呼び出されるメソッド内で、値渡しパラメーターを使用する任意のインスタンス メソッドを呼び出すことができます。 それらのインスタンスで、`in` パラメーターのコピーが作成されます。 コンパイラは `in` パラメーターに一時的な変数を作成できるため、`in` パラメーターに既定値を指定することもできます。 次のコードでは、2 つ目の点の既定値として原点 (点 0,0) を指定します。

[!code-csharp[InArgumentDefault](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgumentDefault "Specifying defaults for an in parameter")]

コンパイラに読み取り専用引数の参照渡しを強制するには、次のコードに示すように、呼び出しサイトで引数に `in` 修飾子を指定します。

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ExplicitInArgument "Specifying an In argument")]

この動作により、パフォーマンスの向上が可能な大規模なコードベースで、徐々に `in` パラメーターを採用しやすくなります。 最初に、メソッド シグネチャに `in` 修飾子を追加します。 その後、呼び出しサイトで `in` 修飾子を追加し、`readonly struct` 型を作成して、コンパイラに他の場所で `in` パラメーターの防御用コピーを作成しないようにすることができます。

`in` パラメーターの指定は、参照型または数値と併用することもできます。 ただし、いずれの場合も、利点があるとしてもわずかです。

## <a name="never-use-mutable-structs-as-in-in-argument"></a>`in` 引数として変更可能な構造体を使用しない

上で説明した手法では、参照を返し、参照で値を渡すことにより、コピーを避ける方法が説明されています。 これらの手法は、引数の型が `readonly struct` 型として宣言されているときに最善の結果が得られます。 そうでない場合は、多くの状況において、引数の読み取り専用性を強制するために、コンパイラで**防御用コピー**を作成する必要があります。 原点からの 3D の点の距離を計算する以下の例について考えます。

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

`Point3D` 構造体は、読み取り専用の構造体では "*ありません*"。 このメソッドの本体には、6 つの異なるプロパティ アクセス呼び出しがあります。 最初の調査では、これらのアクセスは安全であると思ったかもしれません。 いずれにしても、`get` アクセサーではオブジェクトの状態を変更すべきではありません。 しかし、それを強制する言語規則はありません。 それは、一般的な規則のみです。 すべての型で、内部状態を変更する `get` アクセサーを実装できます。 何らかの言語的保証がないと、メンバーを呼び出す前に、コンパイラで引数の一時コピーを作成する必要があります。 スタック上に一時的なストレージが作成され、引数の値が一時的なストレージにコピーされて、`this` 引数としての各メンバー アクセスに対して値がスタックにコピーされます。 多くの場合、これらのコピーによってパフォーマンスが悪影響を受けるので、引数の型が `readonly struct` ではない場合は、値渡しの方が、読み取り専用参照渡しより速くなります。

代わりに、距離の計算で変更不可の構造体 `ReadonlyPoint3D` が使用されている場合は、一時オブジェクトは必要ありません。

[!code-csharp[readonlyInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ReadOnlyInArgument "Specifying a readonly in argument")]

コンパイラでは、`readonly struct` のメンバーの呼び出しに対してもっと効率的なコードが生成されます。`this` 参照は、レシーバーのコピーではなく、常に、メンバー メソッドに参照で渡される `in` パラメーターです。 この最適化によって、`in` 引数として `readonly struct` を使用するときのコピーが減ります。

null 許容値の型を `in` 引数として渡すことはできません。 <xref:System.Nullable%601> 型は、読み取り専用の構造体として宣言されていません。 つまり、コンパイラは、パラメータ―宣言上で `in` 修飾子を使用してメソッドに渡される null 許容値型の任意の引数に対して、防御用のコピーを生成する必要があります。

GitHub の[サンプル リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/safe-efficient-code/benchmark)の [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) を使用して、パフォーマンスの違いを示すサンプル プログラムを確認できます。 変更可能な構造体の値渡しと参照渡し、および変更不可の構造体の値渡しと参照渡しが比較されています。 変更不可の構造体で参照渡しを使用したときが、最も高速です。

## <a name="use-ref-struct-types-to-work-with-blocks-or-memory-on-a-single-stack-frame"></a>単一のスタック フレームでブロックまたはメモリを操作するために `ref struct` 型を使用する

関連するもう 1 つの言語機能は、単一のスタック フレームに拘束される必要のある値型を宣言する機能です。 この制限により、コンパイラでいくつかの最適化を行うことができます。 この機能の第一の動機は <xref:System.Span%601> と関連構造でした。 <xref:System.Span%601> 型を利用する新規および更新された .NET API を使用することにより、これらの機能強化によるパフォーマンスの向上を実現できます。

[`stackalloc`](language-reference/operators/stackalloc.md) で作成したメモリを使用するとき、あるいは相互運用 API からメモリを使用するとき、同様の要件が求められる場合があります。 そのようなニーズには独自の `ref struct` 型を定義できます。

## <a name="readonly-ref-struct-type"></a>`readonly ref struct` 型

構造体を `readonly ref` として宣言すると、`ref struct` と `readonly struct` の制限の利点と制限が組み合わされます。 読み取り専用スパンによって使用されるメモリは単一のスタック フレームに制限され、読み取り専用スパンによって使用されるメモリは変更できません。

## <a name="conclusions"></a>まとめ

値の型を使用すると、割り当て操作の数が最小限になります。

- 値の型の記憶域は、ローカル変数とメソッド引数に割り当てられたスタックです。
- 他のオブジェクトのメンバーである値の型に対する記憶域は、別の割り当てとしてではなく、そのオブジェクトの一部として割り当てられます。
- 値の型の戻り値に対する記憶域は、割り当てられたスタックです。

それを同じ状況での参照型と比較します。

- 参照型の記憶域は、ローカル変数とメソッド引数に割り当てられたヒープです。 参照は、スタック上に格納されます。
- 他のオブジェクトのメンバーである参照型に対する記憶域は、ヒープ上に別に割り当てられます。 参照は親オブジェクトに格納されます。
- 参照型の戻り値に対する記憶域は、割り当てられたヒープです。 その記憶域に対する参照は、スタック上に格納されます。

割り当てを最小限に抑えることにはトレードオフが伴います。 `struct` のサイズが参照のサイズより大きいと、コピーするメモリの量が増えます。 通常、参照は 32 ビットまたは 64 ビットであり、ターゲット コンピューターの CPU に応じます。

一般に、これらのトレードオフによるパフォーマンスへの影響は最小限です。 ただし、大きい構造体または大きいコレクションでは、パフォーマンスへの影響が大きくなります。 影響は、短いループおよびプログラムに対するホット パスで、大きくなる可能性があります。

C# 言語の以上の拡張機能は、必要なパフォーマンスの達成においてメモリの割り当てを最小限にすることが大きな要因である、パフォーマンス クリティカルなアルゴリズムのために設計されています。 自分が記述するコードではこれらの機能を頻繁に使用することがないかもしれません。 ただし、これらの拡張機能は .NET 全体で採用されています。 これらの機能を利用する API が増えれば、自分で作るアプリケーションのパフォーマンスが向上するでしょう。

## <a name="see-also"></a>関連項目

- [ref キーワード](language-reference/keywords/ref.md)
- [ref 戻り値と ref ローカル変数](programming-guide/classes-and-structs/ref-returns.md)
