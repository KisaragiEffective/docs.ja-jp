---
title: System.Buffers - .NET
ms.date: 12/05/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- buffers [.NET]
- I/O [.NET], buffers
author: rick-anderson
ms.author: riande
ms.openlocfilehash: f939164cd56b2fb2feeeb171236b0e1171327e19
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160119"
---
# <a name="work-with-buffers-in-net"></a>.NET でのバッファーの使用

この記事では、複数のバッファーで実行されるデータの読み取りに役立つ型の概要について説明します。 これらは主に、<xref:System.IO.Pipelines.PipeReader> オブジェクトをサポートするために使用されます。

## <a name="ibufferwritert"></a>IBufferWriter\<T\>

<xref:System.Buffers.IBufferWriter%601?displayProperty=fullName> は、バッファーの同期を行う書き込みのためのコントラクトです。 最下位レベルでは、インターフェイスは次のようになります。

- 基本的で、簡単に使用できます。
- <xref:System.Memory%601> または <xref:System.Span%601> へのアクセスが可能です。 `Memory<T>` または `Span<T>` に書き込むことができ、書き込まれた `T` 項目の数を確認できます。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet)]

上記のメソッドでは:

- `IBufferWriter<byte>` を使用して、`GetSpan(5)` から少なくとも 5 バイトのバッファーを要求します。
- 返された `Span<byte>` に、ASCII 文字列 "Hello" のバイトを書き込みます。
- <xref:System.Buffers.IBufferWriter%601> を呼び出して、バッファーに書き込まれたバイト数を示します。

この書き込みメソッドでは、`Memory<T>` によって提供される /`Span<T>``IBufferWriter<T>` バッファーが使用されています。 代わりに、<xref:System.Buffers.BuffersExtensions.Write%2A> 拡張メソッドを使用して既存のバッファーを `IBufferWriter<T>` にコピーすることもできます。 `Write` によって、`GetSpan`/`Advance` を呼び出す処理が適切に実行されます。そのため、書き込み後に `Advance` を呼び出す必要はありません。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet2)]

<xref:System.Buffers.ArrayBufferWriter%601> は、バッキング ストアが単一の隣接した配列である `IBufferWriter<T>` の実装です。

### <a name="ibufferwriter-common-problems"></a>IBufferWriter の一般的な問題

- `GetSpan` および `GetMemory` では、少なくとも要求された量のメモリを持つバッファーを返します。 厳密なバッファー サイズを想定しないでください。
- 連続する呼び出しで同じバッファーまたは同じサイズのバッファーが返される保証はありません。
- さらにデータの書き込みを続行するには、`Advance` を呼び出した後に新しいバッファーを要求する必要があります。 `Advance` を呼び出した後に、前に取得したバッファーに書き込むことはできません。

## <a name="readonlysequencet"></a>ReadOnlySequence\<T\>

![パイプ内のメモリと、その下に読み取り専用メモリのシーケンス位置を示す ReadOnlySequence](media/buffers/ro-sequence.png)

<xref:System.Buffers.ReadOnlySequence%601> は、`T` の隣接したシーケンス、または隣接しないシーケンスを表すことができる構造体です。 これは次のものから構築できます。

1. `T[]` 変数
1. `ReadOnlyMemory<T>` 変数
1. リンク リスト ノード <xref:System.Buffers.ReadOnlySequenceSegment%601> と、シーケンスの開始位置および終了位置を表すインデックスのペア。

最も興味深いのは 3 番目の表現方法です。`ReadOnlySequence<T>` のさまざまな操作に対してパフォーマンスへの影響があるためです。

|表現|操作|複雑さ|
---|---|---|
|`T[]`/`ReadOnlyMemory<T>`|`Length`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`GetPosition(long)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(int, int)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(SequencePostion,  SequencePostion)`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`Length`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`GetPosition(long)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(int, int)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(SequencePostion, SequencePostion)`|`O(1)`|

この混合表現のため、`ReadOnlySequence<T>` では整数ではなく `SequencePosition` としてインデックスが公開されます。 `SequencePosition` は:

- 発生元の `ReadOnlySequence<T>` のインデックスを表す非透過的な値です。
- 整数とオブジェクトの 2 つの部分で構成されます。 これらの 2 つの値によって表されるものは、`ReadOnlySequence<T>` の実装に関連付けられています。

### <a name="access-data"></a>データにアクセスする

`ReadOnlySequence<T>` では、列挙可能な `ReadOnlyMemory<T>` としてデータが公開されます。 基本的な foreach を使用して、各セグメントの列挙を行うことができます。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet3)]

上記のメソッドでは、各セグメントで特定のバイトを検索しています。 各セグメントの `SequencePosition` を追跡する必要がある場合は、<xref:System.Buffers.ReadOnlySequence%601.TryGet%2A?displayProperty=nameWithType> の方が適しています。 次のサンプルでは、整数ではなく `SequencePosition` を返すように前のコードを変更しています。 `SequencePosition` を返すことにより、呼び出し元が特定のインデックスのデータを取得するための 2 回目のスキャンを回避できるという利点があります。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet4)]

`SequencePosition` と `TryGet` の組み合わせは列挙子のように動作します。 position フィールドは、各反復の開始時に、`ReadOnlySequence<T>` 内の各セグメントの開始位置に変更されます。

上記のメソッドは、`ReadOnlySequence<T>` の拡張メソッドとして存在しています。 <xref:System.Buffers.BuffersExtensions.PositionOf%2A> を使用すると、上記のコードを簡略化できます。

```csharp
SequencePosition? FindIndexOf(in ReadOnlySequence<byte> buffer, byte data) => buffer.PositionOf(data);
```

#### <a name="process-a-readonlysequencet"></a>ReadOnlySequence\<T\> の処理

`ReadOnlySequence<T>` の処理は困難な場合があります。シーケンス内の複数のセグメントにまたがってデータが分割されている可能性があるためです。 最適なパフォーマンスを得るには、コードを次の 2 つのパスに分割します。

- 単一セグメントのケースを処理する高速パス。
- セグメント間に分割されたデータを処理する低速パス。

複数のセグメントに分割されたシーケンスのデータを処理するには、いくつかの方法があります。

- [`SequenceReader<T>`](#sequencereadert) を使用する。
- セグメントごとにデータを解析し、解析されたセグメント内の `SequencePosition` とインデックスを追跡する。 これにより不要な割り当てを回避できますが、非効率的になる可能性があります (特に小さなバッファーの場合)。
- `ReadOnlySequence<T>` を隣接した配列にコピーし、それを 1 つのバッファーとして扱う。
  - `ReadOnlySequence<T>` のサイズが小さい場合は、[stackalloc](../../csharp/language-reference/operators/stackalloc.md) 演算子を使用して、スタック割り当てバッファーにデータをコピーすることが適切な場合があります。
  - `ReadOnlySequence<T>` を使用して、プールされた配列に <xref:System.Buffers.ArrayPool%601.Shared%2A?displayProperty=nameWithType> をコピーします。
  - [`ReadOnlySequence<T>.ToArray()`](xref:System.Buffers.BuffersExtensions.ToArray%2A) を使用します。 これは、新しい `T[]` をヒープに割り当てるため、ホット パスでは推奨されません。

次の例では、`ReadOnlySequence<byte>` を処理する一般的なケースをいくつか示しています。

##### <a name="process-binary-data"></a>バイナリ データの処理

次の例では、`ReadOnlySequence<byte>` の先頭から、4 バイトのビッグ エンディアンの整数長を解析しています。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet5)]

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

##### <a name="process-text-data"></a>テキスト データの処理

次に例を示します。

- `\r\n` 内の最初の改行 (`ReadOnlySequence<byte>`) を検索し、out 'line' パラメーターを使用してそれを返します。
- その line をトリミングし、入力バッファーから `\r\n` を除外します。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet6)]

##### <a name="empty-segments"></a>空のセグメント

`ReadOnlySequence<T>` 内に空のセグメントを格納することができます。 セグメントを明示的に列挙するときに、空のセグメントが発生する可能性があります。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet7)]

上記のコードでは、空のセグメントを持つ `ReadOnlySequence<byte>` を作成し、それらの空のセグメントがさまざまな API にどのような影響を与えるかを示しています。

- 空のセグメントを指す `ReadOnlySequence<T>.Slice` を使用した `SequencePosition` では、そのセグメントが保持されます。
- int を使用した `ReadOnlySequence<T>.Slice` では、空のセグメントがスキップされます。
- `ReadOnlySequence<T>` を列挙すると、空のセグメントが列挙されます。

### <a name="potential-problems-with-readonlysequencet-and-sequenceposition"></a>ReadOnlySequence\<T> と SequencePosition に関する潜在的な問題

`ReadOnlySequence<T>`/`SequencePosition` を扱うときには、通常の `ReadOnlySpan<T>`/`ReadOnlyMemory<T>`/`T[]`/`int` に対して、いくつかの通常とは異なる結果が発生します。

- `SequencePosition` は特定の `ReadOnlySequence<T>` の位置マーカーであり、絶対位置ではありません。 これは特定の `ReadOnlySequence<T>` を基準にするため、発生元の `ReadOnlySequence<T>` の外部で使用されても意味がありません。
- `SequencePosition` を使用せずに `ReadOnlySequence<T>` に対する算術演算を行うことはできません。 つまり、`position++` などの基本的な処理は、`ReadOnlySequence<T>.GetPosition(position, 1)` のように記述されます。
- `GetPosition(long)` では、負のインデックスがサポートされて**いません**。 つまり、すべてのセグメントをたどることなく最後から 2 番目の文字を取得することはできません。
- 2 つの `SequencePosition` を比較できないため、次のことが困難になります。
  - ある位置が別の位置より大きいか小さいかを確認する。
  - いくつかの解析アルゴリズムを記述する。
- `ReadOnlySequence<T>` はオブジェクト参照よりも大きいため、可能な場合は [in](../../csharp/language-reference/keywords/in-parameter-modifier.md) または [ref](../../csharp/language-reference/keywords/ref.md) によって渡す必要があります。 `ReadOnlySequence<T>` または `in` によって `ref` を渡すことで、[struct](../../csharp/language-reference/builtin-types/struct.md) のコピーを減らすことができます。
- 空のセグメントは:
  - `ReadOnlySequence<T>` 内で有効です。
  - `ReadOnlySequence<T>.TryGet` メソッドを使った反復処理中に発生する可能性があります。
  - `ReadOnlySequence<T>.Slice()` オブジェクトと共に `SequencePosition` メソッドを使ったシーケンスのスライスで発生する可能性があります。

## <a name="sequencereadert"></a>SequenceReader\<T\>

<xref:System.Buffers.SequenceReader%601>:

- `ReadOnlySequence<T>` の処理を簡略化するために .NET Core 3.0 で導入された新しい型です。
- 単一セグメントの `ReadOnlySequence<T>` と複数セグメントの `ReadOnlySequence<T>` の違いが統合されます。
- 複数のセグメントに分割されている場合でもされていない場合でも、バイナリ データとテキスト データ (`byte` と `char`) を読み取るためのヘルパーが提供されます。

バイナリ データと区切られたデータの両方を処理するための組み込みメソッドが用意されています。 以下のセクションでは、これらの同じメソッドが `SequenceReader<T>` でどのように使用されるかを示します。

### <a name="access-data"></a>データにアクセスする

`SequenceReader<T>` には、`ReadOnlySequence<T>` 内のデータを直接列挙するためのメソッドが用意されています。 次のコードは、一度に `ReadOnlySequence<byte>` ずつ `byte` を処理する例です。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet8)]

`CurrentSpan` を使用すると、現在のセグメントの `Span` が公開されます。これは、このメソッド内で手動で行われたことに似ています。

### <a name="use-position"></a>位置の使用

次のコードでは、`FindIndexOf` を使った `SequenceReader<T>` の実装例を示します。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet9)]

### <a name="process-binary-data"></a>バイナリ データの処理

次の例では、`ReadOnlySequence<byte>` の先頭から、4 バイトのビッグ エンディアンの整数長を解析しています。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet11)]

### <a name="process-text-data"></a>テキスト データの処理

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet10)]

### <a name="sequencereadert-common-problems"></a>SequenceReader\<T\> の一般的な問題

- `SequenceReader<T>` は変更可能な構造体であるため、常に[参照](../../csharp/language-reference/keywords/ref.md)渡しする必要があります。
- `SequenceReader<T>` は [ref struct](../../csharp/language-reference/keywords/ref.md#ref-struct-types) であるため、同期メソッド内でのみ使用でき、フィールドに格納することはできません。 詳細については、「[安全で効率的な C# コードを記述する](../../csharp/write-safe-efficient-code.md)」をご覧ください。
- `SequenceReader<T>` は、順方向専用のリーダーとして使用するために最適化されています。 `Rewind` は、他の `Read`、`Peek`、`IsNext` API を使用しても対処できない小規模なバックアップを目的としています。
