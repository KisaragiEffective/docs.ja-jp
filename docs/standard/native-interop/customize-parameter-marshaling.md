---
title: パラメーターのマーシャリングのカスタマイズ - .NET
description: .NET でパラメーターをネイティブ表現にマーシャリングする方法をカスタマイズする手順について説明します。
ms.date: 01/18/2019
ms.openlocfilehash: ff646ad942cf051ce90cd75b24c8562e536182d9
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159612"
---
# <a name="customizing-parameter-marshaling"></a>パラメーターのマーシャリングのカスタマイズ

.NET ランタイムの既定のパラメーター マーシャリング動作が意図したとおりに動作しない場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 属性を使用してパラメーターのマーシャリング方法をカスタマイズできます。

## <a name="customizing-string-parameters"></a>文字列パラメーターのカスタマイズ

.NET には、文字列をマーシャリングするためのさまざまな形式があります。 これらのメソッドは、C スタイルの文字列と Windows 中心の文字列の形式に関する別のセクションに分割されています。

### <a name="c-style-strings"></a>C スタイルの文字列

これらの各形式では、null で終わる文字列をネイティブ コードに渡します。 これらはネイティブ文字列のエンコードが異なります。

| `System.Runtime.InteropServices.UnmanagedType` 値 | エンコード |
|------------------------------------------------------|----------|
| LPStr | ANSI |
| LPUTF8Str | UTF-8 |
| LPWStr | UTF-16 |
| LPTStr | UTF-16 |

<xref:System.Runtime.InteropServices.UnmanagedType.VBByRefStr?displayProperty=nameWithType> 形式は少し異なります。 `LPWStr` と同様に、文字列を UTF-16 でエンコードされたネイティブの C スタイルの文字列にマーシャリングします。 ただし、マネージド シグネチャは参照で文字列を渡し、一致するネイティブ シグネチャは値で文字列を受け取ります。 この違いがあるため、`StringBuilder` を使用しなくても、文字列を値で受け取り、その場で変更するネイティブ API を使用できます。 一致しないネイティブ シグネチャとマネージド シグネチャは混乱しやすいため、手動でこの形式を使用することはお勧めしません。

### <a name="windows-centric-string-formats"></a>Windows 中心の文字列形式

COM または OLE インターフェイスとやり取りするときに、ネイティブ関数が文字列を `BSTR` 引数として受け取ることに気づかれるのではないでしょうか。 <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> アンマネージド型を使用して、文字列を `BSTR` としてマーシャリングすることができます。

WinRT API とやり取りしている場合は、<xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 形式を使用して文字列を `HSTRING` としてマーシャリングできます。

## <a name="customizing-array-parameters"></a>配列パラメーターのカスタマイズ

.NET には、配列パラメーターをマーシャリングする方法が複数用意されています。 C スタイルの配列を受け取る API を呼び出す場合は、<xref:System.Runtime.InteropServices.UnmanagedType.LPArray?displayProperty=nameWithType> アンマネージド型を使用します。 配列内の値にカスタマイズされたマーシャリングが必要な場合は、そのために <xref:System.Runtime.InteropServices.MarshalAsAttribute.ArraySubType> 属性に対して `[MarshalAs]` フィールドを使用できます。

COM API を使用している場合は、おそらく配列パラメーターを `SAFEARRAY*` としてマーシャリングする必要があります。 そのためには、<xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> アンマネージド型を使用できます。 `SAFEARRAY` の要素の既定の種類については、[`object` フィールドのカスタマイズ](./customize-struct-marshaling.md#marshaling-systemobjects)の表を参照してください。 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> および <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> フィールドを使用すると、`SAFEARRAY` の正確な要素の種類をカスタマイズできます。

## <a name="customizing-boolean-or-decimal-parameters"></a>ブールまたは 10 進数パラメーターのカスタマイズ

ブールまたは 10 進数パラメーターのマーシャリングについては、「[構造体のマーシャリングのカスタマイズ](customize-struct-marshaling.md)」を参照してください。

## <a name="customizing-object-parameters-windows-only"></a>オブジェクト パラメーターのカスタマイズ (Windows のみ)

Windows 上の .NET ランタイムには、オブジェクト パラメーターをネイティブ コードにマーシャリングするさまざまな方法が用意されています。

### <a name="marshaling-as-specific-com-interfaces"></a>特定の COM インターフェイスとしてのマーシャリング

API が COM オブジェクトへのポインターを受け取る場合、`UnmanagedType` 型のパラメーターに次の `object` 形式のいずれかを使用して、以下のような特定のインターフェイスとしてマーシャリングするように .NET に指示できます。

- `IUnknown`
- `IDispatch`
- `IInspectable`

さらに、型が `[ComVisible(true)]` とマークされている場合、または `object` 型をマーシャリングする場合は、<xref:System.Runtime.InteropServices.UnmanagedType.Interface?displayProperty=nameWithType> 形式を使用して、オブジェクトをその型の COM ビューの COM 呼び出し可能ラッパーとしてマーシャリングできます。

### <a name="marshaling-to-a-variant"></a>`VARIANT` へのマーシャリング

ネイティブ API が Win32 `VARIANT` を受け取る場合、<xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> パラメーターに `object` 形式を使用してオブジェクトを `VARIANT` としてマーシャリングすることができます。 .NET 型と [ 型の間のマッピングについては、`object`](customize-struct-marshaling.md#marshaling-systemobjects) フィールドのカスタマイズ`VARIANT`に関するドキュメントを参照してください。

### <a name="custom-marshalers"></a>カスタム マーシャラー

ネイティブ COM インターフェイスを別のマネージ型にプロジェクションする場合は、`UnmanagedType.CustomMarshaler` 形式と <xref:System.Runtime.InteropServices.ICustomMarshaler> の実装を使用して独自のカスタム マーシャリング コードを用意できます。
