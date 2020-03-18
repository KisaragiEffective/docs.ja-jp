---
title: カスタム数値形式文字列
ms.date: 06/25/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- format strings
- custom numeric format strings
- numbers [.NET Framework], formatting
- format specifiers, numeric
- formatting numbers [.NET Framework]
- format specifiers, custom numeric format strings
ms.assetid: 6f74fd32-6c6b-48ed-8241-3c2b86dea5f4
ms.openlocfilehash: 1eb9c3c189d7bba3a12fdcd0c3d600a66bf819ca
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75348307"
---
# <a name="custom-numeric-format-strings"></a>カスタム数値形式文字列

1 つ以上のカスタム数値指定子で構成されるカスタム数値書式指定文字列を作成して、数値データの書式設定方法を定義できます。 カスタム数値書式指定文字列は、 [標準の数値形式文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)ではない任意の書式指定文字列です。

カスタム数値書式指定文字列は、すべての数値型の `ToString` メソッドの一部のオーバーロードでサポートされています。 たとえば、 <xref:System.Int32.ToString%28System.String%29> 型の <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29> メソッドおよび <xref:System.Int32> メソッドに数値書式指定文字列を指定できます。 カスタム数値書式指定文字列は、.NET の[複合書式指定機能](../../../docs/standard/base-types/composite-formatting.md)でもサポートされています。この機能を使用するメソッドには、`Write` クラスおよび `WriteLine` クラスの一部の <xref:System.Console> メソッドと <xref:System.IO.StreamWriter> メソッド、<xref:System.String.Format%2A?displayProperty=nameWithType> メソッド、<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> メソッドがあります。 [文字列補間](../../csharp/language-reference/tokens/interpolated.md)機能は、カスタム数値書式指定文字列もサポートしています。

> [!TIP]
> **書式指定ユーティリティ**である .NET Core Windows Forms をダウンロードできます。このアプリケーションを使用すると、書式指定文字列を数値または日付と時刻の値に適用して、結果の文字列を表示できます。 ソース コードは [C#](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs) と [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb) で利用できます。

<a name="table"></a> 次の表に、カスタム数値書式指定子の説明および書式指定子ごとのサンプル出力を示します。 カスタム数値書式指定文字列の使用方法については、「[メモ](#NotesCustomFormatting)」を参照してください。それらを使用する包括的な例については、「[例](#example)」を参照してください。

|書式指定子|name|[説明]|例|
|----------------------|----------|-----------------|--------------|
|"0"|ゼロ プレースホルダー|対応する数字でゼロを置き換えます。置き換えが行われるのは、対応する数字が存在する場合です。それ以外の場合は、結果の文字列にはゼロが表示されます。<br /><br /> 詳細については、「 ["0" カスタム指定子](#Specifier0)」を参照してください。|1234.5678 ("00000") -> 01235<br /><br /> 0.45678 ("0.00", en-US) -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|"#"|桁プレースホルダー|対応する数字で "#" 記号を置き換えます。置き換えが行われるのは、対応する数字が存在する場合です。それ以外の場合は、結果の文字列に数字は表示されません。<br /><br /> 入力文字列の対応する数字が意味を持たない 0 の場合、結果の文字列に数字は表示されません。 たとえば、0003 ("####") -> 3 です。<br /><br /> 詳細については、「 ["#" カスタム指定子](#SpecifierD)」を参照してください。|1234.5678 ("#####") -> 1235<br /><br /> 0.45678 ("#.##", en-US) -> .46<br /><br /> 0.45678 ("#.##", fr-FR) -> ,46|
|"."|小数点|結果の文字列の小数点位置を決定します。<br /><br /> 詳細については、「 ["."カスタム指定子](#SpecifierPt)」を参照してください。|0.45678 ("0.00", en-US) -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|","|桁区切り記号および数値の位取り|桁区切り記号および数値位取り指定子の両方として機能します。 桁区切り記号としては、各グループの間に、ローカライズされた桁区切り記号文字を挿入します。 数値位取り指定子としては、指定されたコンマごとに、数値を 1000 で除算します。<br /><br /> 詳細については、「 ["," カスタム指定子](#SpecifierTh)」を参照してください。|桁区切り記号:<br /><br /> 2147483647 ("##,#", en-US) -> 2,147,483,647<br /><br /> 2147483647 ("##,#", es-ES) -> 2.147.483.647<br /><br /> 位取り指定子:<br /><br /> 2147483647 ("#,#,,", en-US) -> 2,147<br /><br /> 2147483647 ("#,#,,", es-ES) -> 2.147|
|"%"|パーセント プレースホルダー|数値に 100 を乗算し、結果の文字列に、ローカライズされたパーセント記号を挿入します。<br /><br /> 詳細については、「 ["%" カスタム指定子](#SpecifierPct)」を参照してください。|0.3697 ("%#0.00", en-US) -> %36.97<br /><br /> 0.3697 ("%#0.00", el-GR) -> %36,97<br /><br /> 0.3697 ("##.0 %", en-US) -> 37.0 %<br /><br /> 0.3697 ("##.0 %", el-GR) -> 37,0 %|
|"‰"|パーミル プレースホルダー|数値に 1000 を乗算し、結果の文字列にローカライズされたパーミル記号を挿入します。<br /><br /> 詳細については、「 ["‰" カスタム指定子](#SpecifierPerMille)」を参照してください。|0.03697 ("#0.00‰", en-US) -> 36.97‰<br /><br /> 0.03697 ("#0.00‰", ru-RU) -> 36,97‰|
|"E0"<br /><br /> "E+0"<br /><br /> "E-0"<br /><br /> "E0"<br /><br /> "E+0"<br /><br /> "E-0"|指数表記|後に 0 (ゼロ) が 1 つ以上続く場合に、指数表記を使用して結果の書式を設定します。 大文字 "E" と小文字 "e" は、結果の文字列の指数記号を大文字にするか小文字にするかを示します。 "E" 文字または "e" 文字の後に続くゼロの数によって、指数部の最小桁数が決まります。 正符号 (+) は、符号文字が指数部の前に常に挿入されることを示します。 負符号 (-) は、指数部が負の値の場合にだけその前に符号文字が挿入されることを示します。<br /><br /> 詳細については、「 ["E" カスタム指定子と "e" カスタム指定子](#SpecifierExponent)」を参照してください。|987654 ("#0.0e0") -> 98.8e4<br /><br /> 1503.92311 ("0.0##e+00") -> 1.504e+03<br /><br /> 1.8901385E-16 ("0.0e+00") -> 1.9e-16|
|"\\"|エスケープ文字|この文字の次の文字はカスタム書式指定子ではなくリテラルとして解釈されます。<br /><br /> 詳細については、「["\\" エスケープ文字](#SpecifierEscape)」を参照してください。|987654 ("\\###00\\#") -> #987654#|
|'*文字列*'<br /><br /> "*文字列*"|リテラル文字列区切り記号|囲まれた文字列が結果の文字列にそのままコピーされることを示します。<br/><br/>詳細については、「[文字リテラル](#character-literals)」を参照してください。|68 ("# 'degrees'") -> 68 degrees<br /><br /> 68 ("#' degrees'") -> 68 degrees|
|;|セクション区切り記号|正の数値、負の数値、およびゼロの数値に対して、別々の書式指定文字列を使用してセクションを定義します。<br /><br /> 詳細については、「 [";" セクション区切り記号](#SectionSeparator)」を参照してください。|12.345 ("#0.0#;(#0.0#);-\0-") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#);-\0-") -> -0-<br /><br /> -12.345 ("#0.0#;(#0.0#);-\0-") -> (12.35)<br /><br /> 12.345 ("#0.0#;(#0.0#)") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#)") -> 0.0<br /><br /> -12.345 ("#0.0#;(#0.0#)") -> (12.35)|
|その他|上記以外のすべての文字|文字が結果の文字列にそのままコピーされます。<br/><br/>詳細については、「[文字リテラル](#character-literals)」を参照してください。|68 ("# °") -> 68 °|

以降では、それぞれのカスタム数値書式指定子について詳しく説明します。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-partial-note.md)]

<a name="Specifier0"></a>

## <a name="the-0-custom-specifier"></a>"0" カスタム指定子

"0" カスタム書式指定子は、ゼロ プレースホルダー記号として機能します。 書式が設定される値で、書式指定文字列のゼロに対応する位置に数字がある場合には、この数字が結果の文字列にコピーされます。それ以外の場合は、結果の文字列にゼロが表示されます。 整数部の左端のゼロの位置と、小数部の右端のゼロの位置によって、常に結果の文字列に示される桁数が決まります。

指定子が "00" の場合、値は一の位で丸められ、小数点以下のゼロは常に切り捨てられます。 たとえば、"00" を指定して 34.5 を書式設定すると、結果の値は 35 になります。

次の例では、ゼロ プレースホルダーを含むカスタム書式指定文字列を使って書式設定された複数の値を表示します。

[!code-cpp[Formatting.Numeric.Custom#1](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#1)]
[!code-csharp[Formatting.Numeric.Custom#1](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#1)]
[!code-vb[Formatting.Numeric.Custom#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#1)]

[表のトップへ](#table)

<a name="SpecifierD"></a>

## <a name="the--custom-specifier"></a>"#" カスタム指定子

"#" カスタム書式指定子は、桁プレースホルダー記号として機能します。 書式が指定される値で、書式指定文字列の "#" 記号に対応する位置に数字がある場合には、この数字が結果の文字列にコピーされます。 それ以外の場合は、結果の文字列のこの位置には何も格納されません。

この指定子では、文字列の唯一の桁の値がゼロであっても、この桁が有効桁でない場合には、ゼロは表示されません。 表示される数値の有効桁である場合にのみゼロが表示されます。

書式指定文字列が "##" の場合は、値は一の位で丸められ、小数点以下のゼロは常に切り捨てられます。 たとえば、"##" を指定して 34.5 を書式設定すると、結果の値は 35 になります。

次の例では、桁プレースホルダーを含むカスタム書式指定文字列を使って書式設定された複数の値を表示します。

[!code-cpp[Formatting.Numeric.Custom#2](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#2)]
[!code-csharp[Formatting.Numeric.Custom#2](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#2)]
[!code-vb[Formatting.Numeric.Custom#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#2)]

欠落している数字や先頭のゼロをスペースに置き換える結果の文字列を戻すには、次の例に示すように、 [複合書式指定機能](../../../docs/standard/base-types/composite-formatting.md) を使用して、フィールド幅を指定します。

[!code-cpp[Formatting.Numeric.Custom#12](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/SpaceOrDigit1.cpp#12)]
[!code-csharp[Formatting.Numeric.Custom#12](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/SpaceOrDigit1.cs#12)]
[!code-vb[Formatting.Numeric.Custom#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/SpaceOrDigit1.vb#12)]

[表のトップへ](#table)

<a name="SpecifierPt"></a>

## <a name="the--custom-specifier"></a>"." カスタム指定子

"." カスタム書式指定子は、ローカライズされた小数点を結果の文字列に挿入します。 書式指定文字列の 1 番目のピリオドによって、書式設定後の値での小数点の位置が決定します。指定されている他のピリオドは無視されます。

結果の文字列の中で小数点として使用される文字は、ピリオドであるとは限りません。書式設定を制御する <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> オブジェクトの <xref:System.Globalization.NumberFormatInfo> プロパティによって決定されます。

次の例では、結果として得られるさまざまな文字列の小数点位置を、"." 書式指定子を使って定義しています。

[!code-cpp[Formatting.Numeric.Custom#3](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#3)]
[!code-csharp[Formatting.Numeric.Custom#3](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#3)]
[!code-vb[Formatting.Numeric.Custom#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#3)]

[表のトップへ](#table)

<a name="SpecifierTh"></a>

## <a name="the--custom-specifier"></a>"," カスタム指定子

"," 文字は、桁区切り記号および数値位取り指定子の両方として機能します。

- 桁区切り記号: 数値の整数部の桁を書式設定する 2 つの桁プレースホルダー (0 または #) の間に 1 つ以上のコンマが指定されている場合は、出力の整数部分で各数値グループの間に桁区切り記号文字が挿入されます。

  現在の <xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A> オブジェクトの <xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A> プロパティと <xref:System.Globalization.NumberFormatInfo> プロパティによって、桁区切り記号として使用される文字および各数値グループのサイズが決まります。 たとえば、文字列 "#,#" およびインバリアント カルチャを使用して数値 1000 が書式設定される場合は、出力が "1,000" となります。

- 数値位取り指定子: 明示的または暗黙的な小数点のすぐ左側に 1 つ以上のコンマが指定されている場合は、コンマごとに書式設定対象の数値が 1000 で除算されます。 たとえば、"0,," 文字列を使用して数値 1 億が書式設定された場合、出力は "100" となります。

桁区切り記号および数値位取り指定子は、同じ書式指定文字列で使用できます。 たとえば、"#,0,," 文字列およびインバリアント カルチャを使用して 10 億の数値を書式設定した場合、出力は "1,000" となります。

桁区切り記号としてコンマを使用する例を次に示します。

[!code-cpp[Formatting.Numeric.Custom#4](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#4)]
[!code-csharp[Formatting.Numeric.Custom#4](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#4)]
[!code-vb[Formatting.Numeric.Custom#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#4)]

数値の位取り指定子としてコンマを使用する例を次に示します。

[!code-cpp[Formatting.Numeric.Custom#5](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#5)]
[!code-csharp[Formatting.Numeric.Custom#5](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#5)]
[!code-vb[Formatting.Numeric.Custom#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#5)]

[表のトップへ](#table)

<a name="SpecifierPct"></a>

## <a name="the--custom-specifier"></a>"%" カスタム指定子

書式指定文字列にパーセント記号 (%) があると、書式設定前に数値に 100 が乗算されます。 数値では、書式指定文字列の % に対応する位置にローカライズされたパーセント記号が挿入されます。 使用されるパーセント記号は、現在の <xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A> オブジェクトの <xref:System.Globalization.NumberFormatInfo> プロパティによって定義されます。

次の例では、"%" カスタム指定子を含むさまざまなカスタム書式指定文字列を定義します。

[!code-cpp[Formatting.Numeric.Custom#6](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#6)]
[!code-csharp[Formatting.Numeric.Custom#6](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#6)]
[!code-vb[Formatting.Numeric.Custom#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#6)]

[表のトップへ](#table)

<a name="SpecifierPerMille"></a>

## <a name="the--custom-specifier"></a>"‰" カスタム指定子

書式指定文字列にパーミル文字 (‰ または \u2030) があると、書式設定前に数値に 1000 が乗算されます。 返される文字列では、書式指定文字列の ‰ に対応する位置に適切なパーミル記号が挿入されます。 使用されるパーミル文字は、カルチャ固有の書式設定情報を指定するオブジェクトの <xref:System.Globalization.NumberFormatInfo.PerMilleSymbol%2A?displayProperty=nameWithType> プロパティによって定義されます。

次の例では、"‰" カスタム指定子を含むカスタム書式指定文字列を定義します。

[!code-cpp[Formatting.Numeric.Custom#9](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#9)]
[!code-csharp[Formatting.Numeric.Custom#9](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#9)]
[!code-vb[Formatting.Numeric.Custom#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#9)]

[表のトップへ](#table)

<a name="SpecifierExponent"></a>

## <a name="the-e-and-e-custom-specifiers"></a>"E" カスタム指定子と "e" カスタム指定子

書式指定文字列に "E"、"E+"、"E-"、"e"、"e+"、または "e-" が含まれており、これらの文字の直後にゼロが 1 つ以上続く場合には、指数表記を使用して数値の書式が設定されます。また、数値と指数の間に "E" または "e" が挿入されます。 指数表記インジケーターの後に続くゼロの数によって、出力の指数部の最小桁数が決まります。 書式 "E+" または "e+" は、正符号または負符号が指数部の前に常に挿入されることを示します。 書式 "E"、"E-"、"e"、または "e-" は、指数部が負の値の場合にだけその前に符号文字が挿入されることを示します。

次の例では、指数表記の指定子を使用して、さまざまな数値の書式を設定します。

[!code-cpp[Formatting.Numeric.Custom#7](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#7)]
[!code-csharp[Formatting.Numeric.Custom#7](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#7)]
[!code-vb[Formatting.Numeric.Custom#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#7)]

[表のトップへ](#table)

<a name="SpecifierEscape"></a>

## <a name="the--escape-character"></a>"\\" エスケープ文字

書式指定文字列内の "#"、"0"、"."、","、"%"、"‰" の各記号は、リテラル文字ではなく書式指定子として解釈されます。 カスタム書式指定文字列内での位置によっては、大文字および小文字の "E"、および + 記号と - 記号も書式指定子として解釈されます。

文字が書式指定子として解釈されないようにするには、その文字の前に、エスケープ文字の円記号を付けます。 エスケープ文字は、その後に続く文字が、そのまま結果の文字列に含める必要がある文字リテラルであることを示します。

結果の文字列に円記号を含める場合は、円記号をもう 1 つ付加して、円記号自体をエスケープする必要があります (`\\`)。

> [!NOTE]
> C++ コンパイラや C# コンパイラなど、一部のコンパイラでは、同様に、1 つの円記号がエスケープ文字として解釈されることがあります。 書式設定時に文字列が正しく解釈されるようにするには、C# では、逐語的文字列リテラル文字 (@ 文字) を文字列の前に使用します。また、C# および C++ では、円記号の前にもう 1 つ円記号を付ける方法もあります。 両方の方法を次の C# の例に示します。

次の例では、エスケープ文字を使用して、書式設定操作で "#"、"0"、"\\" の各文字がエスケープ文字としても書式指定子としても解釈されないようにします。 この C# の例では、円記号をもう 1 つ付けて、円記号がリテラル文字として解釈されるようにしています。

[!code-cpp[Formatting.Numeric.Custom#11](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/escape1.cpp#11)]
[!code-csharp-interactive[Formatting.Numeric.Custom#11](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/escape1.cs#11)]
[!code-vb[Formatting.Numeric.Custom#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/escape1.vb#11)]

[表のトップへ](#table)

<a name="SectionSeparator"></a>

## <a name="the--section-separator"></a>";" セクション区切り記号

セミコロン (;) は、値が正、負、ゼロのいずれであるかに応じて異なる書式設定を数値に適用する条件付き書式指定子です。 このように数値の内容によって適用する書式を変更するには、カスタム書式指定文字列に、セミコロンで区切ったセクションを最大 3 つまで作成します。 これらのセクションの説明を次に示します。

|セクションの数|[説明]|
|------------------------|-----------------|
|1 つのセクション|書式指定文字列はすべての値に適用されます。|
|2 つのセクション|最初のセクションが正の値とゼロに適用され、2 番目のセクションが負の値に適用されます。<br /><br /> 書式設定対象の数値が負の数値であるが、2 番目のセクションの書式指定に従って丸めた結果ゼロになる場合には、1 番目のセクションの書式に従ってこのゼロが書式設定されます。|
|3 つ|最初のセクションが正の値、2 番目のセクションが負の値、3 番目のセクションがゼロに適用されます。<br /><br /> ゼロ以外の値すべてに 1 番目のセクションが適用される場合には、2 番目のセクションが空になる (セミコロンの間に何も表示されない) ことがあります。<br /><br /> 書式設定対象の数値がゼロ以外の負の数値であるが、1 番目または 2 番目のセクションの書式に従って丸めた結果ゼロになる場合には、3 番目のセクションの書式に従ってこのゼロが書式設定されます。|

セクション区切り記号は、最後の値が書式設定されるときに、数値に関連付けられた既存の書式設定をすべて無視します。 たとえば、セクション区切り記号を使用する場合、負の値はマイナス記号を付けずに表示されます。 最終的に書式設定された値にマイナス記号を付ける場合は、カスタム書式指定子の中に明示的にマイナス記号を含める必要があります。

次の例では、";" 書式指定子を使用して、正数、負数、ゼロの各部分に対し、それぞれ異なる書式を設定します。

[!code-cpp[Formatting.Numeric.Custom#8](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#8)]
[!code-csharp-interactive[Formatting.Numeric.Custom#8](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#8)]
[!code-vb[Formatting.Numeric.Custom#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#8)]

[表のトップへ](#table)

## <a name="character-literals"></a>文字リテラル

カスタム数値書式指定文字列に含まれる書式指定子は、常に書式設定文字として解釈され、決してリテラル文字としては解釈されません。 これには以下の文字が含まれます。

- [0](#Specifier0)
- [\#](#SpecifierD)
- [%](#SpecifierPct)
- [‰](#SpecifierPerMille)
- '
- [\\](#SpecifierEscape)
- [」を参照してください。](#SpecifierPt)
- [、](#SpecifierTh)
- [E または e](#SpecifierExponent)。書式指定文字列の位置によって変わります。

その他の文字はすべて、文字リテラルとして常に解釈され、書式設定操作では、変更されずに結果の文字列に含まれます。  解析操作では、これらは入力文字列の文字と完全に一致している必要があります。比較では大文字小文字を区別します。

次の例は、リテラル文字の単位 (この場合は千単位) の一般的な使用例を示しています。

[!code-csharp-interactive[literal characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal2.cs#1)]
[!code-vb[literal characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal2.vb#1)]

文字が結果の文字列に含まれるように、または入力文字列で正常に解析されるように、文字が書式指定文字としてではなく、リテラル文字として解釈されることを示すには、次の 2 つの方法があります。

- 書式指定文字をエスケープします。 詳細については、["\\" 文字](#SpecifierEscape)のセクションを参照してください。

- リテラル文字列全体を引用符のアポストロフィで囲みます。

次の例では、両方のアプローチを使用して、カスタム数値書式指定文字列に予約文字を含めています。

[!code-csharp-interactive[including reserved characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal1.cs#1)]
[!code-vb[including reserved characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal1.vb#1)]

<a name="NotesCustomFormatting"></a>

## <a name="notes"></a>メモ

### <a name="floating-point-infinities-and-nan"></a>浮動小数点の無限大値と NaN (非数) 値

<xref:System.Single> の浮動小数点型または <xref:System.Double> の浮動小数点型が正の無限大、負の無限大、または NaN (非数) である場合は、書式指定文字列とは関係なく、現在適用可能な <xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A>オブジェクトによって指定される <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A>、 <xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A> 、または <xref:System.Globalization.NumberFormatInfo> の各プロパティの値は、書式設定された文字列となります。

### <a name="control-panel-settings"></a>コントロール パネルの設定

コントロール パネルの **[地域と言語のオプション]** での設定は、書式設定操作によって生成される結果の文字列に影響します。 これらの設定は、現在のスレッド カルチャに関連付けられた <xref:System.Globalization.NumberFormatInfo> オブジェクトを初期化するために使用され、現在のスレッド カルチャから書式設定の制御に使用される値が提供されます。 コンピューターで使用する設定が異なる場合は、生成される文字列も異なります。

また、<xref:System.Globalization.CultureInfo.%23ctor%28System.String%29?displayProperty=nameWithType> コンストラクターを使用して、現在のシステム カルチャと同じカルチャを表す新しい <xref:System.Globalization.CultureInfo> オブジェクトをインスタンス化した場合、コントロール パネルの **[地域と言語のオプション]** 項目で設定されたカスタマイズが新しい <xref:System.Globalization.CultureInfo> オブジェクトに適用されます。 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> コンストラクターを使用すると、システムに対するカスタマイズが反映されない <xref:System.Globalization.CultureInfo> オブジェクトを作成できます。

### <a name="rounding-and-fixed-point-format-strings"></a>丸めと固定小数点の書式指定文字列

固定小数点の書式指定文字列 (つまり指数表記の書式指定文字を含まない書式指定文字列) の場合は、小数点以下の桁数が小数点の右側にある桁プレースホルダーの数と同じである数値に丸められます。 書式指定文字列に小数点が含まれていない場合には、最も近い整数に丸められます。 数値の桁数が、整数部の桁プレースホルダーの数よりも大きい場合には、桁プレースホルダーに収まらない桁が、結果の文字列の 1 番目の桁プレースホルダーの直前にコピーされます。

[表のトップへ](#table)

<a name="example"></a>

## <a name="example"></a>例

2 つのカスタム数値書式指定文字列の例を次に示します。 どちらの場合も、桁プレースホルダー (`#`) によって数値データが表示され、それ以外の文字はすべて結果の文字列にコピーされます。

[!code-cpp[Formatting.Numeric.Custom#10](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/example1.cpp#10)]
[!code-csharp-interactive[Formatting.Numeric.Custom#10](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/example1.cs#10)]
[!code-vb[Formatting.Numeric.Custom#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/example1.vb#10)]

[表のトップへ](#table)

## <a name="see-also"></a>参照

- <xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>
- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
- [標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)
- [方法: 数値に先行するゼロを埋め込む](../../../docs/standard/base-types/how-to-pad-a-number-with-leading-zeros.md)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (C#)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-cs)
- [サンプル: .NET Core WinForms 書式設定ユーティリティ (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/winforms-formatting-utility-vb)
