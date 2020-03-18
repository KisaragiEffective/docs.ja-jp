---
title: ネイティブ相互運用性のベスト プラクティス - .NET
description: .NET でネイティブ コンポーネントとやり取りするためのベスト プラクティスについて説明します。
ms.date: 01/18/2019
ms.openlocfilehash: 9486256b815856c0c283f5fe231be3d35d6e8f00
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742749"
---
# <a name="native-interoperability-best-practices"></a>ネイティブ相互運用性のベスト プラクティス

.NET には、ネイティブ相互運用性コードをカスタマイズするさまざまな方法が用意されています。 この記事には、Microsoft の .NET チームがネイティブ相互運用性のために従うガイダンスが含まれています。

## <a name="general-guidance"></a>一般的なガイダンス

このセクションのガイダンスは、すべての相互運用シナリオに適用されます。

- メソッドとパラメーターには、呼び出すネイティブメソッドと同じ名前付けと大文字小文字を使用✔️ます。
- ✔️定数値には、同じ名前と大文字小文字の使用を検討してください。
- ネイティブ型に最も近い型にマップされる .NET 型を使用✔️ます。 たとえば、C# では、ネイティブ型が `uint` の場合は `unsigned int` を使用します。
- ✔️は、必要な動作が既定の動作と異なる場合にのみ `[In]` と `[Out]` 属性を使用します。
- ✔️ <xref:System.Buffers.ArrayPool%601?displayProperty=nameWithType> を使用して、ネイティブ配列バッファーをプールすることを検討してください。
- ネイティブライブラリと同じ名前で大文字と小文字を区別するクラスで P/Invoke 宣言をラップする✔️ます。
  - これにより、`[DllImport]` 属性で C# `nameof` 言語機能を使用してネイティブ ライブラリの名前を渡し、ネイティブ ライブラリの名前のスペルを間違えないようにすることができます。

## <a name="dllimport-attribute-settings"></a>dllImport 属性の設定

| 設定 | Default | 推奨 | 詳細 |
|---------|---------|----------------|---------|
| <xref:System.Runtime.InteropServices.DllImportAttribute.PreserveSig>   | `true` |  既定値のままにします  | これが明示的に false に設定されている場合、失敗した HRESULT の戻り値は例外になります (そして結果として定義内の戻り値は null になります)。|
| <xref:System.Runtime.InteropServices.DllImportAttribute.SetLastError> | `false`  | API によって異なります  | API が GetLastError を使用し、Marshal.GetLastWin32Error を使用して値を取得する場合は、true に設定します。 API がエラーがあるという条件を設定している場合は、誤って上書きされないように他の呼び出しを行う前にエラーを取得します。|
| <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> | `CharSet.None` (`CharSet.Ansi` の動作にフォールバックします)  | 定義内に文字列または文字が存在する場合は、明示的に `CharSet.Unicode` または `CharSet.Ansi` を使用します。 | これは、文字列のマーシャリング動作と `ExactSpelling` の場合の `false` の動作を指定します。 Unix では `CharSet.Ansi` は実際には UTF8 である点に注意してください。 "_ほとんど_" の場合、Windows では Unicode が使用され、Unix では UTF8 が使用されます。 詳細については、[文字セットのドキュメント](./charset.md)を参照してください。 |
| <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling> | `false` | `true`             | ランタイムで `CharSet` 設定の値に応じてサフィックスが "A" または "W" (`CharSet.Ansi` の場合は "A"、`CharSet.Unicode` の場合は "W") の代替の関数名が検索されないときに、これを true に設定し、わずかなパフォーマンス上のメリットを得ます。 |

## <a name="string-parameters"></a>文字列パラメーター

文字セットが Unicode の場合、または引数が `[MarshalAs(UnmanagedType.LPWSTR)]` として明示的にマークされて_おり_、文字列が (`ref` または `out`ではなく) 値によって渡された場合、文字列は (コピーではなく) ネイティブコードによって直接固定されて使用されます。

文字列の ANSI 処理が明示的に必要ではない限り、必ず `[DllImport]` を `Charset.Unicode` としてマークしてください。

❌ `[Out] string` パラメーターを使用しません。 文字列がインターン処理された文字列で、文字列パラメーターが `[Out]` 属性の値で渡された場合、ランタイムが不安定になる可能性があります。 文字列のインターン処理の詳細については、<xref:System.String.Intern%2A?displayProperty=nameWithType> のドキュメントを参照してください。

`StringBuilder` パラメーターを ❌ しないようにします。 `StringBuilder` のマーシャリングによって "*常に*" ネイティブ バッファー コピーが作成されます。 そのため、非常に非能率的になる場合もあります。 その典型的なシナリオとして、文字列を受け取る Windows API を呼び出す場合があります。

1. 目的の容量の SB を作成します (管理容量を割り当てます) **{1}**
2. Invoke
   1. ネイティブ バッファーを割り当てます **{2}**
   2. `[In]` の場合にコンテンツをコピーします _(`StringBuilder` パラメーターの既定値)_
   3. `[Out]` **{3}** _(`StringBuilder`の既定値も)_ の場合は、新しく割り当てられたマネージ配列にネイティブバッファーをコピーします。
3. `ToString()` でさらに別のマネージド配列を割り当てます **** **

これは、ネイティブ コードから文字列を取得する *{4}* の割り当てです。 これを制限するために最適な方法は、`StringBuilder` を別の呼び出しで再利用することですが、それでも *1* つの割り当てが節約されるだけです。 `ArrayPool` から文字バッファーを使用してキャッシュする方がはるかにお勧めです。以降の呼び出しでは `ToString()` の割り当てのみで済むようになります。

`StringBuilder` に関するもう 1 つの問題は、戻り値のバッファーが常に最初の null までコピーされることです。 渡された文字列が null で終了していない場合、または null 終端文字列が 2 つある場合、よくても P/Invoke は不正確になります。

*を "* 使用する`StringBuilder`" 場合、最後の問題は、相互運用のために常に考慮される非表示の null が容量に "**含まれない**" ことです。ます。 ほとんどの API はバッファー サイズに null が "*含まれる*" ことを想定しているため、これを誤りと考えられることがよくあります。 その結果、無駄な、または不要な割り当てが行われる可能性があります。 さらに、この問題によって、ランタイムでコピーを最小化する `StringBuilder` のマーシャリングを最適化できなくなります。

✔️ `char[]`を `ArrayPool`から使用することを検討してください。

文字列のマーシャリングの詳細については、「[文字列に対する既定のマーシャリング](../../framework/interop/default-marshaling-for-strings.md)」と「[Customizing string marshalling (文字列のマーシャリングのカスタマイズ)](customize-parameter-marshaling.md#customizing-string-parameters)」を参照してください。

> __Windows 固有__`[Out]` 文字列の場合、CLR では、`UnmanagedType.BSTR`としてマークされている文字列のフリー文字列または `SysStringFree` に対して、既定で `CoTaskMemFree` が使用されます。
> **出力文字列バッファーを持つほとんどの api では、次のようになります。** 渡された文字数には null が含まれている必要があります。 返された値が、渡された文字数より少ない場合、呼び出しは成功し、値は末尾の null を "*除いた*" 文字数になります。 それ以外の場合、カウントは null 文字を "*含む*" バッファーの必要なサイズになります。
>
> - 渡し 5, get 4: 文字列の長さは4文字で、末尾に null を指定します。
> - 渡し 5, get 6: 文字列の長さは5文字で、null を保持するには6文字のバッファーが必要です。
> [Windows の文字列のデータ型](/windows/desktop/Intl/windows-data-types-for-strings)

## <a name="boolean-parameters-and-fields"></a>ブール型のパラメーターとフィールド

ブール値は混乱しやすいものです。 既定では、.NET の `bool` は Windows の `BOOL` にマーシャリングされます。その場合、4 バイト値になります。 一方、C および C++ の `_Bool` 型と `bool` 型は "*シングル*" バイトです。 これは、戻り値の半分が破棄されると、結果のみが変わる "*可能性*" があるので、バグの追跡が困難になる可能性があります。 .NET の `bool` 値を C または C++ の `bool` 型にマーシャリングする方法の詳細については、[ブール値フィールドのマーシャリングのカスタマイズ](customize-struct-marshaling.md#customizing-boolean-field-marshaling)に関するドキュメントを参照してください。

## <a name="guids"></a>GUID

GUID はシグネチャに直接使用できます。 多くの Windows API は、`GUID&` のような `REFIID` 型のエイリアスを受け取ります。 参照渡しするときに、`ref` で渡すか、`[MarshalAs(UnmanagedType.LPStruct)]` 属性を使用して渡すことができます。

| GUID | 参照渡しの GUID |
|------|-------------|
| `KNOWNFOLDERID` | `REFKNOWNFOLDERID` |

❌ `ref` GUID パラメーター以外の `[MarshalAs(UnmanagedType.LPStruct)]` は使用しないでください。

## <a name="blittable-types"></a>blittable 型

blittable 型は、マネージド コードとネイティブ コードで同じビット レベルの表現を持つ型です。 そのため、ネイティブ コードとの間でマーシャリングするために別の形式に変換する必要はなく、これによってパフォーマンスが向上することから、推奨されます。

**blittable 型:**

- `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`single`、`double`
- blittable 型の入れ子になっていない 1 次元配列 (たとえば `int[]`)
- インスタンス フィールドに対して blittable 値型のみを持つ固定レイアウトの構造体とクラス
  - 固定レイアウトには `[StructLayout(LayoutKind.Sequential)]` または `[StructLayout(LayoutKind.Explicit)]` が必要です
  - 構造体は既定で `LayoutKind.Sequential` であり、クラスは `LayoutKind.Auto` です

**blittable ではない:**

- `bool`

**blittable の場合がある:**

- `char`, `string`

blittable 型が参照渡しされると、中間バッファーにコピーされるのではなく、マーシャラーによって単純に固定されます (クラスは本質的に参照渡しされ、構造体は `ref` または `out` と共に使用されるときに参照渡しされます)。

1 次元配列、"`char`または **"**  が指定された `[StructLayout]` で明示的にマークされている型の一部である場合、`CharSet = CharSet.Unicode`は blittable です。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct UnicodeCharStruct
{
    public char c;
}
```

別の型に含まれておらず、`string` でマークされた引数として渡される場合、または `[MarshalAs(UnmanagedType.LPWStr)]` に `[DllImport]` が設定されている場合、`CharSet = CharSet.Unicode` は blittable です。

固定された `GCHandle` を作成しようとすることで、型が blittable かどうかを確認できます。 型が文字列ではない場合、または blittable と見なされる場合、`GCHandle.Alloc` は `ArgumentException` をスローします。

可能な場合は、✔️構造を blittable にすることができます。

詳細については、次を参照してください。

- [Blittable 型と非 Blittable 型](../../framework/interop/blittable-and-non-blittable-types.md)
- [型のマーシャリング](type-marshaling.md)

## <a name="keeping-managed-objects-alive"></a>マネージド オブジェクトのキープ アライブ

`GC.KeepAlive()` で、KeepAlive メソッドがヒットするまでオブジェクトをスコープ内に維持することができます。

[`HandleRef`](xref:System.Runtime.InteropServices.HandleRef) を使用すると、マーシャラーは P/Invoke の期間、オブジェクトの有効性を維持することができます。 メソッドのシグネチャで `IntPtr` の代わりに使用できます。 事実上、`SafeHandle` によってこのクラスは置き換えられるため、代わりに使用するようにします。

[`GCHandle`](xref:System.Runtime.InteropServices.GCHandle) を使用すると、マネージド オブジェクトを固定し、そのオブジェクトへのネイティブ ポインターを取得できます。 次に基本的なパターンを示します。

```csharp
GCHandle handle = GCHandle.Alloc(obj, GCHandleType.Pinned);
IntPtr ptr = handle.AddrOfPinnedObject();
handle.Free();
```

固定は `GCHandle` の既定ではありません。 もう 1 つの主なパターンは、ネイティブ コードを介してマネージド オブジェクトへの参照を渡し、(通常はコールバックを使用して) マネージド コードに戻すことです。 パターンを次に示します。

```csharp
GCHandle handle = GCHandle.Alloc(obj);
SomeNativeEnumerator(callbackDelegate, GCHandle.ToIntPtr(handle));

// In the callback
GCHandle handle = GCHandle.FromIntPtr(param);
object managedObject = handle.Target;

// After the last callback
handle.Free();
```

メモリ リークを防ぐために、`GCHandle` を明示的に解放する必要があることを忘れないでください。

## <a name="common-windows-data-types"></a>一般的な Windows のデータ型

Windows API で一般的に使用されるデータ型と、Windows コードを呼び出すときに使用する C# 型の一覧を次に示します。

次の型は、32 ビット版と 64 ビット版の Windows でサイズは同じですが、名前は異なります。

| 幅 | Windows          | C (Windows)          | C#       | 代替手段                          |
|:------|:-----------------|:---------------------|:---------|:-------------------------------------|
| 32    | `BOOL`           | `int`                | `int`    | `bool`                               |
| 8     | `BOOLEAN`        | `unsigned char`      | `byte`   | `[MarshalAs(UnmanagedType.U1)] bool` |
| 8     | `BYTE`           | `unsigned char`      | `byte`   |                                      |
| 8     | `CHAR`           | `char`               | `sbyte`  |                                      |
| 8     | `UCHAR`          | `unsigned char`      | `byte`   |                                      |
| 16    | `SHORT`          | `short`              | `short`  |                                      |
| 16    | `CSHORT`         | `short`              | `short`  |                                      |
| 16    | `USHORT`         | `unsigned short`     | `ushort` |                                      |
| 16    | `WORD`           | `unsigned short`     | `ushort` |                                      |
| 16    | `ATOM`           | `unsigned short`     | `ushort` |                                      |
| 32    | `INT`            | `int`                | `int`    |                                      |
| 32    | `LONG`           | `long`               | `int`    |                                      |
| 32    | `ULONG`          | `unsigned long`      | `uint`   |                                      |
| 32    | `DWORD`          | `unsigned long`      | `uint`   |                                      |
| 64    | `QWORD`          | `long long`          | `long`   |                                      |
| 64    | `LARGE_INTEGER`  | `long long`          | `long`   |                                      |
| 64    | `LONGLONG`       | `long long`          | `long`   |                                      |
| 64    | `ULONGLONG`      | `unsigned long long` | `ulong`  |                                      |
| 64    | `ULARGE_INTEGER` | `unsigned long long` | `ulong`  |                                      |
| 32    | `HRESULT`        | `long`               | `int`    |                                      |
| 32    | `NTSTATUS`       | `long`               | `int`    |                                      |

ポインターである次の型は、プラットフォームの幅に従います。 このような場合は `IntPtr`/`UIntPtr` を使用します。

| 符号付きのポインター型 (`IntPtr` を使用) | 符号なしのポインター型 (`UIntPtr` を使用) |
|:------------------------------------|:---------------------------------------|
| `HANDLE`                            | `WPARAM`                               |
| `HWND`                              | `UINT_PTR`                             |
| `HINSTANCE`                         | `ULONG_PTR`                            |
| `LPARAM`                            | `SIZE_T`                               |
| `LRESULT`                           |                                        |
| `LONG_PTR`                          |                                        |
| `INT_PTR`                           |                                        |

C `PVOID` である Windows `void*` は `IntPtr` または `UIntPtr` のいずれかとしてマーシャリングできますが、可能であれば `void*` を使用します。

[Windows のデータ型](/windows/desktop/WinProg/windows-data-types)

[データ型の範囲](/cpp/cpp/data-type-ranges)

## <a name="structs"></a>構造体

マネージド構造体はスタックに対して作成され、メソッドから返されるまで削除されません。 定義上、これらは "固定" されます (GC によって移動されません)。 ネイティブ コードが、現在のメソッドの終わりを越えてポインターを使用しない場合は、アンセーフ コード ブロックで単にアドレスを取得することもできます。

blittable 型の構造体は、単純にマーシャリング層から直接使用できるため、はるかに高パフォーマンスです。 できれば構造体を blittable 型にしてください (たとえば、`bool` を避けます)。 詳細については、「[Blittable Types (blittable 型)](#blittable-types)」セクションを参照してください。

構造体が blittable 型の "*場合*"、パフォーマンスを向上するために `sizeof()` ではなく `Marshal.SizeOf<MyStruct>()` を使用してください。 前述のように、固定された `GCHandle` を作成しようとすることで、型が blittable であることを確認できます。 型が文字列ではない場合、または blittable と見なされる場合、`GCHandle.Alloc` は `ArgumentException` をスローします。

定義内の構造体へのポインターは、`ref` で渡すか、`unsafe` と `*` を使用する必要があります。

マネージ構造体は、公式プラットフォームのドキュメントまたはヘッダーで使用されている形と名前にできるだけ近づけるように✔️します。

パフォーマンスを向上さC#せるには、blittable 構造体に `Marshal.SizeOf<MyStruct>()` の代わりに `sizeof()` を使用✔️ます。

`INT_PTR Reserved1[2]` のような配列は、2 つの `IntPtr` フィールド、`Reserved1a` と `Reserved1b` にマーシャリングする必要があります。 ネイティブ配列がプリミティブ型の場合、`fixed` キーワードを使用すると、もう少しわかりやすく記述できます。 たとえば、ネイティブ ヘッダーでは `SYSTEM_PROCESS_INFORMATION` は次のようになります。

```c
typedef struct _SYSTEM_PROCESS_INFORMATION {
    ULONG NextEntryOffset;
    ULONG NumberOfThreads;
    BYTE Reserved1[48];
    UNICODE_STRING ImageName;
...
} SYSTEM_PROCESS_INFORMATION
```

C# では、次のように記述できます。

```csharp
internal unsafe struct SYSTEM_PROCESS_INFORMATION
{
    internal uint NextEntryOffset;
    internal uint NumberOfThreads;
    private fixed byte Reserved1[48];
    internal Interop.UNICODE_STRING ImageName;
    ...
}
```

ただし、固定バッファーに関する問題がいくつかあります。 blittable ではない型の固定バッファーは正しくマーシャリングされないので、インプレース配列は複数の個々のフィールドに展開する必要があります。 さらに、3.0 より前の .NET Framework および .NET Core では、固定バッファー フィールドを含む構造体が blittable 型ではない構造体内に入れ子になっている場合、固定バッファー フィールドはネイティブ コードに正しくマーシャリングされません。
