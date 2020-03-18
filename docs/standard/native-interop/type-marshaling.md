---
title: 型のマーシャリング - .NET
description: .NET が型をネイティブ表現にマーシャリングする方法について説明します。
ms.date: 01/18/2019
ms.openlocfilehash: 91b8f3d6cb53fd7a0adea7ea9669e7459e81445f
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706267"
---
# <a name="type-marshaling"></a>型のマーシャリング

**マーシャリング**とは、マネージド コードとネイティブ コードの間でやり取りする必要がある場合に型を変換するプロセスです。

マネージド コードとアンマネージド コード内の型は異なるため、マーシャリングが必要です。 マネージコードでは、たとえば、`String`があり、アンマネージ環境の文字列には Unicode ("ワイド")、非 Unicode、null 終端、ASCII などがあります。既定では、P/Invoke サブシステムは、この記事で説明されている既定の動作に基づいて、適切な処理を試みます。 しかし、追加の制御が必要な状況では、[MarshalAs](xref:System.Runtime.InteropServices.MarshalAsAttribute) 属性を採用して、アンマネージ側で期待する型を指定します。 たとえば、文字列を null で終わる ANSI 文字列として送信させる場合は、次のように指定できます。

```csharp
[DllImport("somenativelibrary.dll")]
static extern int MethodA([MarshalAs(UnmanagedType.LPStr)] string parameter);
```

## <a name="default-rules-for-marshaling-common-types"></a>共通型をマーシャリングする場合の既定の規則

一般的に、ランタイムは、マーシャリングするときにユーザーの必要な作業量が最小限になるように "適切な処理" を実行しようと試みます。 次の表は、パラメーターまたはフィールドで使用されるときに、各型が既定でどのようにマーシャリングされるかを示しています。 次の表がすべてのプラットフォームで正しくなるように、C99/C++ 11 固定幅の整数と文字型が使用されています。 このような型と同じ配置とサイズの要件を持つ任意のネイティブ型を使用できます。

この最初の表は、マーシャリングが P/Invoke とフィールド マーシャリングの両方で同じであるさまざまな型のマッピングを示しています。

| .NET 型 | ネイティブ型  |
|-----------|-------------------------|
| `byte`    | `uint8_t`               |
| `sbyte`   | `int8_t`                |
| `short`   | `int16_t`               |
| `ushort`  | `uint16_t`              |
| `int`     | `int32_t`               |
| `uint`    | `uint32_t`              |
| `long`    | `int64_t`               |
| `ulong`   | `uint64_t`              |
| `char`    | P/Invoke または構造体の `CharSet` に応じて、`char` または `char16_t` のいずれか。 [文字セットのドキュメント](charset.md)を参照してください。 |
| `string`  | P/Invoke または構造体の `CharSet` に応じて、`char*` または `char16_t*` のいずれか。 [文字セットのドキュメント](charset.md)を参照してください。 |
| `System.IntPtr` | `intptr_t`        |
| `System.UIntPtr` | `uintptr_t`      |
| .NET ポインター型 (例: `void*`)  | `void*` |
| `System.Runtime.InteropServices.SafeHandle` の派生型 | `void*` |
| `System.Runtime.InteropServices.CriticalHandle` の派生型 | `void*`          |
| `bool`    | Win32 `BOOL` 型       |
| `decimal` | COM `DECIMAL` 構造体 |
| .NET デリゲート | ネイティブ関数ポインター |
| `System.DateTime` | Win32 `DATE` 型 |
| `System.Guid` | Win32 `GUID` 型 |

パラメーターまたは構造体としてマーシャリングする場合、マーシャリングの一部のカテゴリでは既定が異なります。

| .NET 型 | ネイティブ型 (パラメーター) | ネイティブ型 (フィールド) |
|-----------|-------------------------|---------------------|
| .NET 配列 | 配列要素のネイティブ表現の配列の先頭へのポインター。 | 使用するには `[MarshalAs]` 属性が必要です|
| `LayoutKind` が `Sequential` または `Explicit` のクラス | クラスのネイティブ表現へのポインター | クラスのネイティブ表現 |

次の表には、Windows のみの既定のマーシャリング規則が含まれています。 Windows 以外のプラットフォームでは、このような型をマーシャリングすることはできません。

| .NET 型 | ネイティブ型 (パラメーター) | ネイティブ型 (フィールド) |
|-----------|-------------------------|---------------------|
| `object`  | `VARIANT`               | `IUnknown*`         |
| `System.Array` | COM インターフェイス | 使用するには `[MarshalAs]` 属性が必要です |
| `System.ArgIterator` | `va_list` | 許可しない |
| `System.Collections.IEnumerator` | `IEnumVARIANT*` | 許可しない |
| `System.Collections.IEnumerable` | `IDispatch*` | 許可しない |
| `System.DateTimeOffset` | 1601 年 1 月 1 日午前 0 時以降のティック数を表す `int64_t` || 1601 年 1 月 1 日午前 0 時以降のティック数を表す `int64_t` |

一部の型は、フィールドとしてではなくパラメーターとしてのみマーシャリングできます。 このような型の一覧を次の表に示します。

| .NET 型 | ネイティブ型 (パラメーターのみ) |
|-----------|------------------------------|
| `System.Text.StringBuilder` | P/Invoke の `CharSet` に応じて、`char*` または `char16_t*` のいずれか。  [文字セットのドキュメント](charset.md)を参照してください。 |
| `System.ArgIterator` | `va_list` (Windows x86/x64/arm64 のみ) |
| `System.Runtime.InteropServices.ArrayWithOffset` | `void*` |
| `System.Runtime.InteropServices.HandleRef` | `void*` |

これらの既定値が目的と合わない場合は、パラメーターのマーシャリング方法をカスタマイズできます。 [パラメーターのマーシャリング](customize-parameter-marshaling.md)の記事では、さまざまなパラメーターの型をマーシャリングする方法のカスタマイズ手順について説明しています。

## <a name="default-marshaling-in-com-scenarios"></a>COM シナリオでの既定のマーシャリング

.NET で COM オブジェクトのメソッドを呼び出している場合、.NET ランタイムによって共通の COM セマンティクスを満たすように、既定のマーシャリング ルールが変更されます。 次の表では、COM シナリオで .NET ランタイムによって使用される規則が一覧にされます。

| .NET 型 | ネイティブ型 (COM メソッドの呼び出し) |
|-----------|--------------------------------|
| `bool`    | `VARIANT_BOOL`                 |
| `StringBuilder` | `LPWSTR`                 |
| `string`  | `BSTR`                         |
| デリゲート型 | .NET Framework では `_Delegate*`。 .NET Core では許可されていません。 |
| `System.Drawing.Color` | `OLECOLOR`        |
| .NET 配列 | `SAFEARRAY`                   |
| `string[]` | `BSTR` の `SAFEARRAY`        |

## <a name="marshaling-classes-and-structs"></a>クラスと構造体のマーシャリング

型のマーシャリングの別の側面は、構造体をアンマネージ メソッドに渡す方法です。 たとえば、一部のアンマネージ メソッドでは、パラメーターとして構造体が必要です。 このような場合、環境のマネージド部分に対応する構造体またはクラスを作成し、それをパラメーターとして使用する必要があります。 ただし、クラスを定義するだけでは不十分で、マーシャラーに、クラス内のフィールドをアンマネージ構造体にマップする方法を指示する必要もあります。 ここで `StructLayout` 属性が役立ちます。

```csharp
[DllImport("kernel32.dll")]
static extern void GetSystemTime(SystemTime systemTime);

[StructLayout(LayoutKind.Sequential)]
class SystemTime {
    public ushort Year;
    public ushort Month;
    public ushort DayOfWeek;
    public ushort Day;
    public ushort Hour;
    public ushort Minute;
    public ushort Second;
    public ushort Milsecond;
}

public static void Main(string[] args) {
    SystemTime st = new SystemTime();
    GetSystemTime(st);
    Console.WriteLine(st.Year);
}
```

上のコードは、`GetSystemTime()` 関数を呼び出す簡単な例を示しています。 興味深いビットは 4 行目にあります。 この属性は、他方 (アンマネージ) の側でクラスのフィールドを構造体に順番にマップする必要があることを指定します。 このことは、次の例に示すアンマネージ構造体に対応する必要があるため、フィールドの名前付けは重要でなく、それらの順番だけが重要であることを意味します。

```c
typedef struct _SYSTEMTIME {
  WORD wYear;
  WORD wMonth;
  WORD wDayOfWeek;
  WORD wDay;
  WORD wHour;
  WORD wMinute;
  WORD wSecond;
  WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME;
```

構造体の既定のマーシャリングでは、必要な処理が実行されないことがあります。 「[構造体のマーシャリングのカスタマイズ](./customize-struct-marshaling.md)」の記事では、構造体のマーシャリング方法をカスタマイズする手順が説明されています。
