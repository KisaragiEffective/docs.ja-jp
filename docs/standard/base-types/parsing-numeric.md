---
title: .NET での数値文字列の解析
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, numeric strings
- numeric strings
- enumerations [.NET Framework], parsing strings
- base types, parsing strings
ms.assetid: e39324ee-72e5-42d4-a80d-bf3ee7fc6c59
ms.openlocfilehash: ac44282a06b2b3710d3a9e5390c7a514c1632c3a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127598"
---
# <a name="parsing-numeric-strings-in-net"></a>.NET での数値文字列の解析
すべての数値型には、2 つの静的解析メソッド (`Parse` と `TryParse`) があり、数字の文字列形式を数値型に変換するために使用できます。 これらのメソッドでは、[標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)と[カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)で記述されている書式指定文字列を使用して、生成された文字列を解析できます。 既定では、`Parse` と `TryParse` メソッドは、10 進数の整数を含む文字列を整数値のみに正常に変換することができます。 これらのメソッドは、整数部と小数部、グループ区切り、および小数点記号を含む文字列を浮動小数点値に正常に変換できます。 `Parse` メソッドが `TryParse` を返すのに対して、`false` メソッドは操作が失敗した場合に例外をスローします。  
  
## <a name="parsing-and-format-providers"></a>解析と書式プロバイダー  
 通常、数値の文字列形式はカルチャによって異なります。 通貨記号、グループ (または千単位) 区切り、および小数点記号などの数値文字列の要素は、カルチャによって大きく異なります。 暗黙的または明示的のいずれかの解析メソッドでは、これらのカルチャ固有のバリエーションを認識する書式プロバイダーを使用します。 書式プロバイダーが `Parse` または `TryParse` メソッドの呼び出しで指定されない場合、現在のスレッド カルチャ (<xref:System.Globalization.NumberFormatInfo> プロパティで返された <xref:System.Globalization.NumberFormatInfo.CurrentInfo%2A?displayProperty=nameWithType> オブジェクト) と関連付けられた書式プロバイダーが使用されます。  
  
 書式プロバイダーは、<xref:System.IFormatProvider> 実装によって示されます。 このインターフェイスには、1 つのメンバー (<xref:System.IFormatProvider.GetFormat%2A> メソッド) があり、その 1 つのパラメーターは、書式設定される型を示す <xref:System.Type> オブジェクトです。 このメソッドは、書式情報を示すオブジェクトを返します。 .NET では、数値文字列を解析するために、次の 2 つの <xref:System.IFormatProvider> の実装をサポートします。  
  
- <xref:System.Globalization.CultureInfo> オブジェクト。その <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> メソッドが、カルチャ固有の書式情報を提供する <xref:System.Globalization.NumberFormatInfo> オブジェクトを返します。  
  
- <xref:System.Globalization.NumberFormatInfo> オブジェクト。その <xref:System.Globalization.NumberFormatInfo.GetFormat%2A?displayProperty=nameWithType> メソッドがそれ自体を返します。  
  
 次の例では、配列内の各文字列を <xref:System.Double> 値に変換しようとします。 最初に、英語 (米国) カルチャの規則を反映する書式プロバイダーを使用して、文字列を解析しようとします。 この操作が <xref:System.FormatException> をスローする場合、フランス語 (フランス) カルチャの規則を反映する書式プロバイダーを使用して、文字列を解析しようとしています。  
  
 [!code-csharp[Parsing.Numbers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/formatproviders1.cs#1)]
 [!code-vb[Parsing.Numbers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/formatproviders1.vb#1)]  
  
## <a name="parsing-and-numberstyles-values"></a>解析と NumberStyles 値  
 解析操作が処理できるスタイル要素 (空白文字、グループ区切り、小数点の記号など) は、<xref:System.Globalization.NumberStyles> 列挙値によって定義されます。 既定では、整数値を表す文字列は、<xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType> 値を使用して解析されます。これは、数値、先頭と末尾の空白、および先頭の符号のみを許可します。 浮動小数点値を表す文字列は、<xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType> 値と <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> 値の組み合わせを使用して解析されます。この複合スタイルは、先頭と末尾の空白、先頭の符号、小数点記号、グループ区切り、および指数と共に 10 進数を許可します。 `Parse` 型のパラメーターを含む、`TryParse` または <xref:System.Globalization.NumberStyles> メソッドのオーバーロードを呼び出し、1 つ以上の <xref:System.Globalization.NumberStyles> フラグを設定すると、解析操作が成功するように、文字列で示すことができるスタイル要素を制御することができます。  
  
 たとえば、グループ区切りを含む文字列は、<xref:System.Int32> メソッドを使用して、<xref:System.Int32.Parse%28System.String%29?displayProperty=nameWithType> 値に変換することはできません。 ただし、次の例に示すように、<xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> フラグを使用した場合、この変換は成功します。  
  
 [!code-csharp[Parsing.Numbers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/styles1.cs#2)]
 [!code-vb[Parsing.Numbers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/styles1.vb#2)]  
  
> [!WARNING]
> 解析操作は、常に特定のカルチャの書式規則を使用します。 <xref:System.Globalization.CultureInfo> または <xref:System.Globalization.NumberFormatInfo> オブジェクトを渡してカルチャを指定しない場合、現在のスレッドに関連付けられているカルチャが使用されます。  
  
 次の表では、<xref:System.Globalization.NumberStyles> 列挙体のメンバーを一覧し、そのメンバーが解析操作に与える影響について説明します。  
  
|NumberStyles 値|解析する文字列への影響|  
|------------------------|---------------------------------------|  
|<xref:System.Globalization.NumberStyles.None?displayProperty=nameWithType>|数字のみが許可されます。|  
|<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType>|小数点の記号と桁数が許可されます。 整数値の場合、0 のみが小数点の桁数として許可されます。 有効な小数点は <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A?displayProperty=nameWithType> または <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A?displayProperty=nameWithType> パラメーターによって決定されます。|  
|<xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType>|"e" または "E" の文字は、指数表記を示すために使用できます。 詳細については、「<xref:System.Globalization.NumberStyles>」を参照してください。|  
|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>|先頭の空白が許可されます。|  
|<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>|末尾の空白が許可されます。|  
|<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>|正または負の符号には、数字の先頭に追加できます。|  
|<xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>|正または負の符号は、数字の後に続けることができます。|  
|<xref:System.Globalization.NumberStyles.AllowParentheses?displayProperty=nameWithType>|かっこは、負の符号を示すために使用できます。|  
|<xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>|グループ区切りが許可されます。 グループ区切りの文字は、<xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A?displayProperty=nameWithType> または <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A?displayProperty=nameWithType> プロパティによって決定されます。|  
|<xref:System.Globalization.NumberStyles.AllowCurrencySymbol?displayProperty=nameWithType>|通貨記号が許可されます。 通貨記号は <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A?displayProperty=nameWithType> プロパティによって決定されます。|  
|<xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>|解析する文字列は、16 進数として解釈されます。 これには、16 進数の値の 0 ～ 9、A ～ F、a ～ f を含めることができます。 このフラグは、整数値を解析するためにだけに使用できます。|  
  
 さらに、<xref:System.Globalization.NumberStyles> 列挙体は、複数の <xref:System.Globalization.NumberStyles> フラグを含む、次の複合スタイルを指定します。  
  
|複合 NumberStyles 値|数値を含む|  
|----------------------------------|----------------------|  
|<xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType> スタイルが含まれます。 これは、整数値を解析するために使用される既定のスタイルです。|  
|<xref:System.Globalization.NumberStyles.Number?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> スタイルが含まれます。|  
|<xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType> スタイルが含まれます。|  
|<xref:System.Globalization.NumberStyles.Currency?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType> と <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> を除くすべてのスタイルが含まれます。|  
|<xref:System.Globalization.NumberStyles.Any?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> を除くすべてのスタイルが含まれます。|  
|<xref:System.Globalization.NumberStyles.HexNumber?displayProperty=nameWithType>|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> スタイルが含まれます。|  
  
## <a name="parsing-and-unicode-digits"></a>解析と Unicode 数字  
 Unicode 標準では、さまざまな書記体系で数字のコード ポイントを定義します。 たとえば、U+0030 ～ U+0039 のコード ポイントは、0 ～ 9 の基本ラテンの数字を示し、U+09E6 ～ U+09EF のコード ポイントは、0 ～ 9 の数字のベンガル語の数字を示し、U+FF10 ～ U+FF19 のコード ポイントは、0 ～ 9 の全角の数字を示します。 ただし、解析メソッドで認識される数字は、U+0030 ～ U+0039 のコード ポイントの基本ラテンの数字 0 ～ 9 のみです。 数値解析メソッドがその他の数字を含む文字列を渡す場合、メソッドは <xref:System.FormatException> をスローします。  
  
 次の例では、<xref:System.Int32.Parse%2A?displayProperty=nameWithType> メソッドを使用して、異なる書記体系の数字で構成される文字列を解析します。 例の出力に示されているように、基本ラテンの数字を解析する試行は成功しますが、全角、アラビア インド、ベンガル語の数字を解析する試行は失敗します。  
  
 [!code-csharp[Parsing.Numbers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/unicode1.cs#3)]
 [!code-vb[Parsing.Numbers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/unicode1.vb#3)]  
  
## <a name="see-also"></a>参照

- <xref:System.Globalization.NumberStyles>
- [文字列の解析](../../../docs/standard/base-types/parsing-strings.md)
- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
