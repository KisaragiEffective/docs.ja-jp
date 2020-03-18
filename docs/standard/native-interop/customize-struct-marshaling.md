---
title: 構造体のマーシャリングのカスタマイズ - .NET
description: .NET で構造体をネイティブ表現にマーシャリングする方法をカスタマイズする手順について説明します。
ms.date: 01/18/2019
dev_langs:
- csharp
- cpp
ms.openlocfilehash: 7f8d1ad93633d6feef9c3c6f5d19aad52105968c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741533"
---
# <a name="customizing-structure-marshaling"></a>構造体のマーシャリングのカスタマイズ

構造体の既定のマーシャリング規則が、必要な規則とは異なる場合があります。 .NET ランタイムには、構造体のレイアウトやフィールドのマーシャリング方法をカスタマイズできる拡張ポイントがいくつか用意されています。

## <a name="customizing-structure-layout"></a>構造体レイアウトのカスタマイズ

.NET には <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 属性と <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> 列挙型が用意されており、フィールドをメモリに配置する方法をカスタマイズできます。 次のガイダンスを使用すると、一般的な問題を回避できます。

✔️は、可能な限り `LayoutKind.Sequential` の使用を検討してください。

✔️は、ネイティブ構造体に共用体などの明示的なレイアウトがある場合にのみ、マーシャリングで `LayoutKind.Explicit` を使用します。

.NET Core 3.0 より前のランタイムを対象にする必要がある場合は、Windows 以外のプラットフォームで構造体をマーシャリングするときに `LayoutKind.Explicit` を使用しないように ❌ します。 3\.0 より前の .NET Core ランタイムでは、Intel または AMD 64 ビットの Windows 以外のシステムでは、明示的な構造を値でネイティブ関数に渡すことがサポートされていません。 ただし、ランタイムはすべてのプラットフォーム上で明示的な構造体の参照渡しをサポートしています。

## <a name="customizing-boolean-field-marshaling"></a>ブール値フィールドのマーシャリングのカスタマイズ

ネイティブ コードには、さまざまなブール表現があります。 Windows だけでも、ブール値を表現する方法が 3 つあります。 ランタイムは構造体のネイティブ定義を認識しないので、ランタイムに可能な最善の処理は、ブール値のマーシャリング方法を推測することです。 .NET ランタイムには、ブール値フィールドのマーシャリング方法を示す方法が用意されています。 以下の例は、.NET `bool` をさまざまなネイティブ ブール型にマーシャリングする方法を示しています。

次の例に示すように、ブール値は既定でネイティブの 4 バイト Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) 値としてマーシャリングされます。

```csharp
public struct WinBool
{
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

明示的にする場合は、<xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> 値を使用して上記と同じ動作にすることができます。

```csharp
public struct WinBool
{
    [MarshalAs(UnmanagedType.Bool)]
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

以下の `UnmanagedType.U1` 値または `UnmanagedType.I1` 値を使用して、`b` フィールドを 1 バイトのネイティブ `bool` 型としてマーシャリングするようにランタイムに指示できます。

```csharp
public struct CBool
{
    [MarshalAs(UnmanagedType.U1)]
    public bool b;
}
```

```cpp
struct CBool
{
    public bool b;
};
```

Windows では、<xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> 値を使用して、ブール値を 2 バイトの `VARIANT_BOOL` 値にマーシャリングするようにランタイムに指示できます。

```csharp
public struct VariantBool
{
    [MarshalAs(UnmanagedType.VariantBool)]
    public bool b;
}
```

```cpp
struct VariantBool
{
    public VARIANT_BOOL b;
};
```

> [!NOTE]
> `VARIANT_BOOL` は、`VARIANT_TRUE = -1` と `VARIANT_FALSE = 0` が他のほとんどのブール型とは異なります。 さらに、`VARIANT_TRUE` と等しくないすべての値は false と見なされます。

## <a name="customizing-array-field-marshaling"></a>配列フィールドのマーシャリングのカスタマイズ

.NET には、配列のマーシャリングをカスタマイズする方法もいくつか用意されています。

既定では、.NET は要素の連続したリストへのポインターとして配列をマーシャリングします。

```csharp
public struct DefaultArray
{
    public int[] values;
}
```

```cpp
struct DefaultArray
{
    int* values;
};
```

COM API とやり取りする場合は、必要に応じて配列を `SAFEARRAY*` オブジェクトとしてマーシャリングします。 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 値と <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> 値を使用して、配列を `SAFEARRAY*` としてマーシャリングするようにランタイムに指示できます。

```csharp
public struct SafeArrayExample
{
    [MarshalAs(UnmanagedType.SafeArray)]
    public int[] values;
}
```

```cpp
struct SafeArrayExample
{
    SAFEARRAY* values;
};
```

`SAFEARRAY` に含まれる要素の型をカスタマイズする必要がある場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> および <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> フィールドを使用して `SAFEARRAY` の正確な要素の種類をカスタマイズできます。

インプレースで配列をマーシャリングする必要がある場合は、<xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> 値を使用して、インプレースで配列をマーシャリングするようマーシャラーに指示できます。 このマーシャリングを使用するときは、ランタイムが構造体の空間を適切に割り当てられるように、<xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> フィールドに配列内の要素数の値を指定する必要もあります。

```csharp
public struct InPlaceArray
{
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 4)]
    public int[] values;
}
```

```cpp
struct InPlaceArray
{
    int values[4];
};
```

> [!NOTE]
> .NET は、C99 の柔軟な配列のメンバーとして可変長配列フィールドをマーシャリングすることをサポートしていません。

## <a name="customizing-string-field-marshaling"></a>文字列フィールドのマーシャリングのカスタマイズ

.NET には、文字列フィールドをマーシャリングするためのさまざまなカスタマイズも用意されています。

既定では、.NET は文字列を null で終わる文字列へのポインターとしてマーシャリングします。 エンコードは、<xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> の <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> フィールドの値によって決まります。 属性が指定されていない場合、エンコードは既定で ANSI エンコードになります。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char* str;
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

フィールドごとに異なるエンコードを使用する必要がある場合、または単に構造体の定義内でより明示的にする場合は、<xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> 属性に <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> 値または <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 値を使用できます。

```csharp
public struct AnsiString
{
    [MarshalAs(UnmanagedType.LPStr)]
    public string str;
}
```

```cpp
struct AnsiString
{
    char* str;
};
```

```csharp
public struct UnicodeString
{
    [MarshalAs(UnmanagedType.LPWStr)]
    public string str;
}
```

```cpp
struct UnicodeString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

UTF-8 エンコードを使用して文字列をマーシャリングする場合は、<xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> に <xref:System.Runtime.InteropServices.MarshalAsAttribute> 値を使用できます。

```csharp
public struct UTF8String
{
    [MarshalAs(UnmanagedType.LPUTF8Str)]
    public string str;
}
```

```cpp
struct UTF8String
{
    char* str;
};
```

> [!NOTE]
> <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> を使用するには、.NET Framework 4.7 (またはそれ以降のバージョン) または .NET Core 1.1 (またはそれ以降のバージョン) のいずれかが必要です。 .NET Standard 2.0 では使用できません。

COM API を使用している場合は、文字列を `BSTR` としてマーシャリングする必要があります。 <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> 値を使用すると、文字列を `BSTR` としてマーシャリングできます。

```csharp
public struct BString
{
    [MarshalAs(UnmanagedType.BStr)]
    public string str;
}
```

```cpp
struct BString
{
    BSTR str;
};
```

WinRT ベースの API を使用している場合は、文字列を `HSTRING` としてマーシャリングする必要があります。  <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 値を使用すると、文字列を `HSTRING` としてマーシャリングできます。

```csharp
public struct HString
{
    [MarshalAs(UnmanagedType.HString)]
    public string str;
}
```

```cpp
struct BString
{
    HSTRING str;
};
```

API で、構造体でインプレースで文字列を渡す必要がある場合は、<xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> 値を使用できます。 `ByValTStr` でマーシャリングされた文字列のエンコードは、`CharSet` 属性によって決まる点に注意してください。 さらに、文字列長を <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> フィールドで渡す必要があります。

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char str[4];
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t str[4]; // Could also be wchar_t[4] on Windows.
};
```

## <a name="customizing-decimal-field-marshaling"></a>10 進数フィールドのマーシャリングのカスタマイズ

Windows を使用している場合は、ネイティブの [`CY` または `CURRENCY`](/windows/win32/api/wtypes/ns-wtypes-cy~r1) 構造体を使用する API がいくつかあります。 既定で、.NET の `decimal` 型はネイティブの [`DECIMAL`](/windows/win32/api/wtypes/ns-wtypes-decimal~r1) 構造体にマーシャリングされます。 ただし、値が <xref:System.Runtime.InteropServices.MarshalAsAttribute> の <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> を使用して、`decimal` 値をネイティブの `CY` 値に変換するようにマーシャラーに指示することができます。

```csharp
public struct Currency
{
    [MarshalAs(UnmanagedType.Currency)]
    public decimal dec;
}
```

```cpp
struct Currency
{
    CY dec;
};
```

## <a name="marshaling-systemobjects"></a>`System.Object` のマーシャリング

Windows では、`object` 型のフィールドをネイティブ コードにマーシャリングできます。 このようなフィールドは、次の 3 つの型のいずれかにマーシャリングできます。

- [`VARIANT`](/windows/win32/api/oaidl/ns-oaidl-variant)
- [`IUnknown*`](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
- [`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch)

既定では、`object` 型のフィールドはオブジェクトをラップする `IUnknown*` にマーシャリングされます。

```csharp
public struct ObjectDefault
{
    public object obj;
}
```

```cpp
struct ObjectDefault
{
    IUnknown* obj;
};
```

オブジェクト フィールドを `IDispatch*` にマーシャリングする場合は、値が <xref:System.Runtime.InteropServices.MarshalAsAttribute> の <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> を追加します。

```csharp
public struct ObjectDispatch
{
    [MarshalAs(UnmanagedType.IDispatch)]
    public object obj;
}
```

```cpp
struct ObjectDispatch
{
    IDispatch* obj;
};
```

`VARIANT` としてマーシャリングする場合は、値が <xref:System.Runtime.InteropServices.MarshalAsAttribute> の <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> を追加します。

```csharp
public struct ObjectVariant
{
    [MarshalAs(UnmanagedType.Struct)]
    public object obj;
}
```

```cpp
struct ObjectVariant
{
    VARIANT obj;
};
```

次の表は、`obj` フィールドのさまざまなランタイム型が `VARIANT` に格納されるさまざまな型にどのようにマップされるかを示しています。

| .NET 型 | バリアント型 | | .NET 型 | バリアント型 |
|------------|--------------|-|----------|--------------|
|  `byte`  | `VT_UI1` |     | `System.Runtime.InteropServices.BStrWrapper` | `VT_BSTR` |
| `sbyte`  | `VT_I1`  |     | `object`  | `VT_DISPATCH` |
| `short`  | `VT_I2`  |     | `System.Runtime.InteropServices.UnknownWrapper` | `VT_UNKNOWN` |
| `ushort` | `VT_UI2` |     | `System.Runtime.InteropServices.DispatchWrapper` | `VT_DISPATCH` |
| `int`    | `VT_I4`  |     | `System.Reflection.Missing` | `VT_ERROR` |
| `uint`   | `VT_UI4` |     | `(object)null` | `VT_EMPTY` |
| `long`   | `VT_I8`  |     | `bool` | `VT_BOOL` |
| `ulong`  | `VT_UI8` |     | `System.DateTime` | `VT_DATE` |
| `float`  | `VT_R4`  |     | `decimal` | `VT_DECIMAL` |
| `double` | `VT_R8`  |     | `System.Runtime.InteropServices.CurrencyWrapper` | `VT_CURRENCY` |
| `char`   | `VT_UI2` |     | `System.DBNull` | `VT_NULL` |
| `string` | `VT_BSTR`|
