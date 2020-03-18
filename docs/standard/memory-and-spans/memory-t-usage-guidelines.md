---
title: Memory<T> と Span<T> の使用ガイドライン
ms.date: 10/01/2018
helpviewer_keywords:
- Memory&lt;T&gt; and Span&lt;T&gt; best practices
- using Memory&lt;T&gt; and Span&lt;T&gt;
ms.openlocfilehash: 0a614f628faa98be778c627573e4dddc462c9107
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73121962"
---
# <a name="memoryt-and-spant-usage-guidelines"></a>Memory\<T> と Span\<T> の使用ガイドライン

.NET Core には、任意の連続したメモリ領域を表す多くの型があります。 .NET Core 2.0 では <xref:System.Span%601> と <xref:System.ReadOnlySpan%601> が導入されました。これらはマネージド メモリまたはアンマネージド メモリでサポートできる軽量のメモリ バッファーです。 このような型はスタック上にのみ格納できるため、非同期メソッドの呼び出しを含む多くのシナリオには適していません。 .NET Core 2.1 では、<xref:System.Memory%601>、<xref:System.ReadOnlyMemory%601>、<xref:System.Buffers.IMemoryOwner%601>、<xref:System.Buffers.MemoryPool%601> など、さらに多くの型が追加されています。 <xref:System.Span%601> と同様に、<xref:System.Memory%601> とそれに関連する型は、マネージド メモリとアンマネージド メモリの両方でサポートできます。 <xref:System.Span%601> とは異なり、<xref:System.Memory%601> はマネージド ヒープ上に格納できます。

<xref:System.Span%601> と <xref:System.Memory%601> は、どちらもパイプラインで使用できる構造化データのバッファーです。 つまり、データの一部または全部をパイプライン内のコンポーネントに効率的に渡すことができるように設計されています。そのため、データを処理し、必要に応じてバッファーを変更できます。 <xref:System.Memory%601> とその関連する型には、複数のコンポーネントまたは複数のスレッドからアクセスできるため、開発者が標準的な使用ガイドラインに従って堅牢なコードを作成することが重要です。

## <a name="owners-consumers-and-lifetime-management"></a>所有者、コンシューマー、有効期間管理

バッファーは API 間で渡すことができ、複数のスレッドからバッファーにアクセスされることがあるため、有効期間管理を考慮することが重要です。 次の 3 つの中心的な概念があります。

- **所有権**。 バッファー インスタンスの所有者は、バッファーが使用されなくなったときにバッファーを破棄することを含め、有効期間の管理を担当します。 すべてのバッファーの所有者は 1 つです。 通常、所有者とは、バッファーを作成したコンポーネント、またはファクトリーからバッファーを受け取ったコンポーネントです。 所有権は譲渡することもできます。**コンポーネント A** はバッファーの制御を**コンポーネント B** に譲渡することができます。その時点で**コンポーネント A** はバッファーを使用できなくなり、**コンポーネント B** が、使用されなくなったバッファーの破棄を担当します。

- **消費**。 バッファー インスタンスのコンシューマーは、読み取り、場合によっては書き込みでバッファー インスタンスを使用できます。 何らかの外部の同期メカニズムが提供されていない限り、バッファーは同時に持つことができるコンシューマーは 1 つです。 バッファーのアクティブなコンシューマーは必ずしもバッファーの所有者ではない点に注意してください。

- **リース**。 リースは、特定のコンポーネントがバッファーの消費者になることができる時間の長さです。

これら 3 つの概念を示す疑似コード例を次に示します。 これには、型 <xref:System.Char> の <xref:System.Memory%601> バッファーをインスタンス化し、`WriteInt32ToBuffer` メソッドを呼び出して整数の文字列表現をバッファーに書き込み、次に `DisplayBufferToConsole` メソッドを呼び出してバッファーの値を表示する `Main` メソッドが含まれます。

```csharp
using System;

class Program
{
    // Write 'value' as a human-readable string to the output buffer.
    void WriteInt32ToBuffer(int value, Buffer buffer);

    // Display the contents of the buffer to the console.
    void DisplayBufferToConsole(Buffer buffer);

    // Application code
    static void Main()
    {
        var buffer = CreateBuffer();
        try
        {
            int value = Int32.Parse(Console.ReadLine());
            WriteInt32ToBuffer(value, buffer);
            DisplayBufferToConsole(buffer);
        }
        finally
        {
            buffer.Destroy();
        }
    }
}
```

`Main` メソッドによってバッファー (この場合は <xref:System.Span%601> インスタンス) が作成され、その所有者も作成されます。 そのため、`Main` には使用されなくなったバッファーを破棄する責任があります。 この処理は、バッファーの <xref:System.Span%601.Clear?displayProperty=nameWithType> メソッドを呼び出して実行します (実際には、この <xref:System.Span%601.Clear> メソッドによってバッファーのメモリがクリアされます。<xref:System.Span%601> 構造体には、実際にバッファーを破棄するメソッドはありません)。

バッファーには `WriteInt32ToBuffer` と `DisplayBufferToConsole` という 2 つのコンシューマーがあります。 同時に存在するコンシューマーは 1 つのみであり (最初は `WriteInt32ToBuffer`、次に `DisplayBufferToConsole`)、どちらのコンシューマーもバッファーを所有していません。 この文脈における "コンシューマー" は、バッファーの読み取り専用ビューを意味していない点にも注意してください。バッファーの読み取り/書き込みビューがある場合、コンシューマーは `WriteInt32ToBuffer` と同様にバッファーの内容を変更できます。

メソッド呼び出しの開始からメソッドから返されるまでの間に、`WriteInt32ToBuffer` メソッドはバッファー上にリースを持ちます (消費することができます)。 同様に、`DisplayBufferToConsole` は実行中にバッファー上にリースを持ち、メソッドがアンワインドするとリースは解放されます (リース管理のための API はありません。"リース" は概念的なものです)。

## <a name="memoryt-and-the-ownerconsumer-model"></a>Memory\<T> と所有者/コンシューマー モデル

「[所有者、コンシューマー、有効期間管理](#owners-consumers-and-lifetime-management)」セクションで説明したように、バッファーには常に所有者がいます。 .NET Core は 2 つの所有権モデルをサポートしています。

- 単一の所有権をサポートするモデル。 バッファーは、その有効期間全体にわたって単一の所有者を持ちます。

- 所有権の譲渡をサポートするモデル。 バッファーの所有権は、元の所有者 (作成者) から別のコンポーネントに譲渡できます。譲渡されたコンポーネントがバッファーの有効期間管理を担当するようになります。 さらにその所有者が所有権を別のコンポーネントに譲渡することもできます。

明示的にバッファーの所有権を管理するには、<xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType> インターフェイスを使用します。 <xref:System.Buffers.IMemoryOwner%601> は両方の所有権モデルをサポートしています。 <xref:System.Buffers.IMemoryOwner%601> の参照を持つコンポーネントがバッファーを所有します。 次の例では、<xref:System.Buffers.IMemoryOwner%601?> インスタンスを使用して <xref:System.Memory%601> バッファーの所有権を反映しています。

[!code-csharp[ownership](~/samples/snippets/standard/buffers/memory-t/owner/owner.cs)]

この例を [`using`](../../csharp/language-reference/keywords/using-statement.md) と記述することもできます。

[!code-csharp[ownership-using](~/samples/snippets/standard/buffers/memory-t/owner-using/owner-using.cs)]

このコードの場合:

- `Main` メソッドは <xref:System.Buffers.IMemoryOwner%601> インスタンスへの参照を保持しているため、`Main` メソッドはバッファーの所有者です。

- `WriteInt32ToBuffer` および `DisplayBufferToConsole` メソッドは、<xref:System.Memory%601> をパブリック API として受け入れます。 そのため、これらはバッファーの消費者です。 また、消費できるのは一度に 1 つのみです。

`WriteInt32ToBuffer` メソッドはバッファーに値を書き込むことが意図されていますが、`DisplayBufferToConsole` メソッドではそうではありません。 これを反映するために、型 <xref:System.ReadOnlyMemory%601> の引数を受け入れておくことができます。 <xref:System.ReadOnlyMemory%601> の詳細については、「[規則 2:バッファーを読み取り専用にする場合は ReadOnlySpan\<T> または ReadOnlyMemory\<T> を使用する](#rule-2)」を参照してください。

### <a name="ownerless-memoryt-instances"></a>"所有者なしの" Memory\<T> インスタンス

<xref:System.Buffers.IMemoryOwner%601> を使用せずに <xref:System.Memory%601> インスタンスを作成できます。 この場合、バッファーの所有権は明示的ではなく暗黙的であり、単一の所有者モデルのみがサポートされます。 これは次の方法で実行できます。

- 次の例のように、<xref:System.Memory%601> コンストラクターのいずれかを直接呼び出し、`T[]` を渡します。

- [String.AsMemory](xref:System.MemoryExtensions.AsMemory(System.String)) 拡張メソッドを呼び出して `ReadOnlyMemory<char>` インスタンスを生成します。

[!code-csharp[ownerless-memory](~/samples/snippets/standard/buffers/memory-t/ownerless/ownerless.cs)]

最初に <xref:System.Memory%601> インスタンスを作成したメソッドは、バッファーの暗黙的な所有者です。 譲渡を支援する <xref:System.Buffers.IMemoryOwner%601> インスタンスはないため、所有権を他のコンポーネントに譲渡することはできません (または、ランタイムのガベージ コレクターがバッファーを所有していて、すべてのメソッドがバッファーを消費するだけであると想像することもできます)。

## <a name="usage-guidelines"></a>使用ガイドライン

メモリ ブロックは所有されていますが、複数のコンポーネントに渡されることが意図されており、その一部は特定のメモリ ブロック上で同時に動作する可能性があるため、<xref:System.Memory%601> と <xref:System.Span%601> の両方を使用するためのガイドラインを確立することが重要です。  ガイドラインが必要な理由は以下のとおりです。

- 所有者がメモリ ブロックを解放した後に、コンポーネントがメモリ ブロックへの参照を保持する可能性があります。

- あるコンポーネントがバッファーに対して動作し、それ同時に他のコンポーネントがそのバッファーに対して動作する可能性があり、そのようなプロセスでは、バッファー内のデータが破損します。

- スタックに割り当てられる <xref:System.Span%601> の性質によってパフォーマンスが最適化され、<xref:System.Span%601> はメモリ ブロック上での操作に適した型になりますが、<xref:System.Span%601> にはいくつかの大きな制限もあります。 <xref:System.Span%601> を使用するタイミングと、<xref:System.Memory%601> を使用するタイミングを把握することが重要です。

以下は、<xref:System.Memory%601> とその関連する型を正しく使用するためのレコメンデーションです。 特に明記しない限り、<xref:System.Memory%601> と <xref:System.Span%601> に適用されるガイダンスは <xref:System.ReadOnlyMemory%601> と <xref:System.ReadOnlySpan%601> にも適用されます。

**規則 1:同期 API の場合、可能であればパラメーターとして Memory\<T> ではなく Span\<T> を使用する。**

<xref:System.Span%601> は <xref:System.Memory%601> よりも汎用性が高く、さまざまな連続するメモリ バッファーを表すことができます。 <xref:System.Span%601> は <xref:System.Memory%601>> よりもパフォーマンスに優れています。 最後に、<xref:System.Memory%601.Span?displayProperty=nameWithType> プロパティを使用して <xref:System.Memory%601> インスタンスを <xref:System.Span%601> に変換することはできますが、Span\<T> から Memory\<T> に変換することはできません。 そのため、呼び出し元が <xref:System.Memory%601> インスタンスを持っていた場合、いずれにしても <xref:System.Span%601> パラメーターを使用してメソッドを呼び出すことができます。

型 <xref:System.Memory%601> ではなく型 <xref:System.Span%601> のパラメーターを使用すると、適切な消費メソッドの実装を記述する場合にも役立ちます。 メソッドのリース時間を過ぎてバッファーにアクセスを試行しないように、コンパイル時のチェックが自動的に実行されます (詳細については後述します)。

完全に同期していても、<xref:System.Span%601> パラメーターではなく <xref:System.Memory%601> パラメーターの使用が必要な場合があります。 おそらく、利用している API は <xref:System.Memory%601> 引数のみを受け入れます。 これは問題ありませんが、<xref:System.Memory%601> を同期的に使用するときに伴うトレードオフに注意してください。

<a name="rule-2" />

**規則 2:バッファーを読み取り専用にする場合は ReadOnlySpan\<T> または ReadOnlyMemory\<T> を使用する。**

前述の例では、`DisplayBufferToConsole` メソッドはバッファーからの読み取りのみを行います。バッファーの内容は変更しません。 メソッドのシグネチャは次のように変更する必要があります。

```csharp
void DisplayBufferToConsole(ReadOnlyMemory<char> buffer);
```

実際、この規則と規則 1 を組み合わせると、さらに改善され、メソッドのシグネチャを次のように書き換えることができます。

```csharp
void DisplayBufferToConsole(ReadOnlySpan<char> buffer);
```

`DisplayBufferToConsole` メソッドは、考えられるほぼすべてのバッファー型 (`T[]`、[stackalloc](../../csharp/language-reference/operators/stackalloc.md) で割り当てられたストレージなど) で動作します。 <xref:System.String> を直接渡すこともできます。

**規則 3:メソッドが Memory\<T> を受け入れて `void` を返した場合、メソッドが返した後に Memory\<T> インスタンスを使用してはならない。**

これは、前述した "リース" の概念に関連しています。 <xref:System.Memory%601> インスタンスに対する void を返すメソッドのリースは、メソッドが開始時に開始され、メソッドの終了時に終了します。 コンソールからの入力に基づいてループで `Log` を呼び出す次の例を考えてみましょう。

[!code-csharp[void-returning](~/samples/snippets/standard/buffers/memory-t/void-returning/void-returning.cs#1)]

`Log` が完全同期メソッドである場合、メモリ インスタンスのアクティブなコンシューマーは常に 1 つのみなので、このコードは予想どおりに動作します。
ただし、代わりに `Log` がこの実装を持っている場合を想像してください。

```csharp
// !!! INCORRECT IMPLEMENTATION !!!
static void Log(ReadOnlyMemory<char> message)
{
    // Run in background so that we don't block the main thread while performing IO.
    Task.Run(() =>
    {
        StreamWriter sw = File.AppendText(@".\input-numbers.dat");
        sw.WriteLine(message);
    });
}
```

この実装では、元のメソッドから返された後も、`Log` はバックグラウンドで <xref:System.Memory%601> インスタンスを使用しようとするため、リースに違反することになります。 `Main` メソッドがバッファーを変更しているときに、`Log` がバッファーを読み込もうとすると、データが破損する可能性があります。

これを解決する方法はいくつかあります。

- 以下の `Log` メソッドの実装のように、`Log` メソッドは `void` ではなく <xref:System.Threading.Tasks.Task> を返す可能性があります。

   [!code-csharp[task-returning](~/samples/snippets/standard/buffers/memory-t/task-returning2/task-returning2.cs#1)]

- `Log` は代わりに次のように実装できます。

   [!code-csharp[defensive-copy](~/samples/snippets/standard/buffers/memory-t/task-returning/task-returning.cs#1)]

**規則 4:メソッドが Memory\<T> を受け入れて Task を返す場合、Task が終了状態に遷移した後に Memory\<T> インスタンスを使用してはならない。**

これは規則 3 の単なる非同期版です。 前の例の `Log` メソッドは、この規則に従うために次のように記述することができます。

[!code-csharp[task-returning-async](~/samples/snippets/standard/buffers/memory-t/void-returning-async/void-returning-async.cs#1)]

ここで、"終了状態" とは、タスクの状態が完了、失敗、またはキャンセル済みに移行することを意味します。 つまり、"終了状態" とは、"スローまたは実行継続の待機を発生させるすべてのもの" を意味します。

このガイダンスは、<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.ValueTask%601>、または同様の型を返すメソッドに適用されます。

**規則 5:コンストラクターがパラメーターとして Memory\<T> を受け入れる場合、構築されたオブジェクトのインスタンス メソッドは Memory\<T> インスタンスの消費者であると見なされる。**

次に例を示します。

```csharp
class OddValueExtractor
{
    public OddValueExtractor(ReadOnlyMemory<int> input);
    public bool TryReadNextOddValue(out int value);
}

void PrintAllOddValues(ReadOnlyMemory<int> input)
{
    var extractor = new OddValueExtractor(input);
    while (extractor.TryReadNextOddValue(out int value))
    {
      Console.WriteLine(value);
    }
}
```

ここで、`OddValueExtractor` コンストラクターは `ReadOnlyMemory<int>` をコンストラクター パラメーターとして受け入れます。そのため、コンストラクター自体が `ReadOnlyMemory<int>` インスタンスのコンシューマーであり、戻り値に対するすべてのインスタンス メソッドも元の `ReadOnlyMemory<int>` インスタンスのコンシューマーです。 つまり、インスタンスが `TryReadNextOddValue` メソッドに直接渡されない場合でも、`TryReadNextOddValue` は `ReadOnlyMemory<int>` インスタンスを消費します。

**規則 6:型に設定できる Memory\<T> 型のプロパティ (または同等のインスタンス メソッド) がある場合、そのオブジェクトに対するインスタンス メソッドは Memory\<T> インスタンスの消費者であると見なされる。**

これは単に規則 5 の一種です。 この規則が存在する理由は、プロパティ セッターまたは同等のメソッドは入力をキャプチャして永続化すると想定されているため、同じオブジェクトに対するインスタンス メソッドがキャプチャした状態を利用する可能性があるためです。

この規則をトリガーする例を次に示します。

```csharp
class Person
{
    // Settable property.
    public Memory<char> FirstName { get; set; }

    // alternatively, equivalent "setter" method
    public SetFirstName(Memory<char> value);

    // alternatively, a public settable field
    public Memory<char> FirstName;
}
```

**規則 7:IMemoryOwner\<T> 参照がある場合、どこかの時点でそれを破棄するか所有権を譲渡する必要がある (両方を実行する必要はない)。**

<xref:System.Memory%601> インスタンスはマネージド メモリまたはアンマネージド メモリのいずれかでサポートされている可能性があるため、<xref:System.Memory%601> インスタンスに対して実行された作業が完了したら、所有者は <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType> を呼び出す必要があります。 または、所有者が <xref:System.Buffers.IMemoryOwner%601> インスタンスの所有権を別のコンポーネントに譲渡することもできます。譲渡の時点で、獲得した側のコンポーネントは適切なタイミングで <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType> を呼び出す責任を負います (詳細については後述します)。

<xref:System.Buffers.MemoryPool%601.Dispose%2A> メソッドの呼び出しに失敗すると、マネージド メモリのリークやその他のパフォーマンス低下が発生する可能性があります。

この規則は、<xref:System.Buffers.MemoryPool%601.Rent%2A?displayProperty=nameWithType> のようなファクトリ メソッドを呼び出すコードにも適用されます。 呼び出し元は、返された <xref:System.Buffers.IMemoryOwner%601> の所有者になり、終了時にインスタンスを破棄する責任を負います。

**規則 8:API サーフェスに IMemoryOwner\<T> パラメーターがある場合、そのインスタンスの所有権を受け入れていることを示す。**

この種類のシグナルを持つインスタンスを受け入れることは、コンポーネントがこのインスタンスの所有権を取得しようとしていることを示します。 規則 7 に従い、コンポーネントは適切な破棄の責任を負うようになります。

<xref:System.Buffers.IMemoryOwner%601> インスタンスの所有権を別のコンポーネントに譲渡したコンポーネントは、メソッド呼び出しの完了後にそのインスタンスを使用できなくなります。

> [!IMPORTANT]
> コンストラクターがパラメーターとして <xref:System.Buffers.IMemoryOwner%601>を受け入れる場合、その型は <xref:System.IDisposable> を実装し、<xref:System.IDisposable.Dispose%2A> メソッドは <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType> を呼び出す必要があります。

**規則 9:同期的 p/invoke メソッドをラップしている場合、API は Span\<T> をパラメーターとして受け入れる必要がある。**

規則 1 に従うと、<xref:System.Span%601> は一般に同期的 API に使用するために適した型です。 次の例のように、[`fixed`](../../csharp/language-reference/keywords/fixed-statement.md) キーワードを介して <xref:System.Span%601> インスタンスを固定できます。

```csharp
using System.Runtime.InteropServices;

[DllImport(...)]
private static extern unsafe int ExportedMethod(byte* pbData, int cbData);

public unsafe int ManagedWrapper(Span<byte> data)
{
    fixed (byte* pbData = &MemoryMarshal.GetReference(data))
    {
        int retVal = ExportedMethod(pbData, data.Length);

        /* error checking retVal goes here */

        return retVal;
    }
}
```

前の例では、入力の範囲が空の場合などに、`pbData` が null になる可能性があります。 `cbData` が 0 であっても、エクスポートされたメソッドで `pbData` を null 以外にする必要がある場合、このメソッドは次のように実装できます。

```csharp
public unsafe int ManagedWrapper(Span<byte> data)
{
    fixed (byte* pbData = &MemoryMarshal.GetReference(data))
    {
        byte dummy = 0;
        int retVal = ExportedMethod((pbData != null) ? pbData : &dummy, data.Length);

        /* error checking retVal goes here */

        return retVal;
    }
}
```

**規則 10:同期的 p/invoke メソッドをラップしている場合、API は Memory\<T> をパラメーターとして受け入れる必要がある。**

非同期操作で [`fixed`](../../csharp/language-reference/keywords/fixed-statement.md) キーワードは使用できないので、インスタンスが表す連続するメモリの種類に関係なく、<xref:System.Memory%601.Pin%2A?displayProperty=nameWithType> メソッドを使用して <xref:System.Memory%601> インスタンスを固定します。 次の例は、この API を使用して非同期の p/invoke 呼び出しを実行する方法を示しています。

```csharp
using System.Runtime.InteropServices;

[UnmanagedFunctionPointer(...)]
private delegate void OnCompletedCallback(IntPtr state, int result);

[DllImport(...)]
private static extern unsafe int ExportedAsyncMethod(byte* pbData, int cbData, IntPtr pState, IntPtr lpfnOnCompletedCallback);

private static readonly IntPtr _callbackPtr = GetCompletionCallbackPointer();

public unsafe Task<int> ManagedWrapperAsync(Memory<byte> data)
{
    // setup
    var tcs = new TaskCompletionSource<int>();
    var state = new MyCompletedCallbackState
    {
        Tcs = tcs
    };
    var pState = (IntPtr)GCHandle.Alloc(state);

    var memoryHandle = data.Pin();
    state.MemoryHandle = memoryHandle;

    // make the call
    int result;
    try
    {
        result = ExportedAsyncMethod((byte*)memoryHandle.Pointer, data.Length, pState, _callbackPtr);
    }
    catch
    {
        ((GCHandle)pState).Free(); // cleanup since callback won't be invoked
        memoryHandle.Dispose();
        throw;
    }

    if (result != PENDING)
    {
        // Operation completed synchronously; invoke callback manually
        // for result processing and cleanup.
        MyCompletedCallbackImplementation(pState, result);
    }

    return tcs.Task;
}

private static void MyCompletedCallbackImplementation(IntPtr state, int result)
{
    GCHandle handle = (GCHandle)state;
    var actualState = (MyCompletedCallbackState)state;
    handle.Free();
    actualState.MemoryHandle.Dispose();

    /* error checking result goes here */

    if (error)
    {
        actualState.Tcs.SetException(...);
    }
    else
    {
        actualState.Tcs.SetResult(result);
    }
}

private static IntPtr GetCompletionCallbackPointer()
{
    OnCompletedCallback callback = MyCompletedCallbackImplementation;
    GCHandle.Alloc(callback); // keep alive for lifetime of application
    return Marshal.GetFunctionPointerForDelegate(callback);
}

private class MyCompletedCallbackState
{
    public TaskCompletionSource<int> Tcs;
    public MemoryHandle MemoryHandle;
}
```

## <a name="see-also"></a>関連項目

- <xref:System.Memory%601?displayProperty=nameWithType>
- <xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType>
- <xref:System.Span%601?displayProperty=nameWithType>
