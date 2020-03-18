---
title: .NET での型の書式設定
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data formatting [.NET Framework]
- dates [.NET Framework], formatting
- date formatting [.NET Framework]
- number formatting [.NET Framework]
- ToString method
- custom cultural settings [.NET Framework]
- numbers [.NET Framework], formatting
- formatting strings [.NET Framework]
- time [.NET Framework], formatting
- currency [.NET Framework], formatting
- types [.NET Framework], formatting
- format specifiers [.NET Framework]
- times [.NET Framework], formatting
- culture [.NET Framework], formatting
- formatting [.NET Framework], types supported
- base types [.NET Framework], formatting
- custom formatting [.NET Framework]
- strings [.NET Framework], formatting
ms.assetid: 0d1364da-5b30-4d42-8e6b-03378343343f
ms.openlocfilehash: a1f4d9107427140bcfa6b49bc8a850432fb204f7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75348253"
---
# <a name="format-types-in-net"></a>.NET での型の書式設定

書式設定とはクラス、構造体、または列挙値のインスタンスを文字列形式に変換するプロセスのことで、多くの場合、変換した文字列をユーザーに表示したり、逆シリアル化して元のデータ型を復元したりするために行います。 この変換には次のような問題がある場合があります。

- 値の内部での格納方法に、ユーザーが望む表示方法が反映されない場合がある。 たとえば、電話番号が 8009999999 という形式で格納されることがあります。これではユーザーにはわかりにくいため、 代わりに 800-999-9999 と表示する必要があります。 数値をこのように書式指定する例については、「 [カスタム書式指定文字列](#custom-format-strings) 」を参照してください。

- オブジェクトから文字列形式への変換が直観的に理解しづらい場合がある。 たとえば、Temperature オブジェクトや Person オブジェクトの文字列形式がどのようになるのか、明確ではありません。 Temperature オブジェクトをさまざまな方法で書式指定する例については、「 [標準書式指定文字列](#standard-format-strings) 」を参照してください。

- 通常、カルチャ依存の書式指定が必要になる。 たとえば、通貨値を数字で表すアプリケーションでは、数値文字列にカルチャ別の通貨記号、桁区切り記号 (ほとんどのカルチャでは 1000 単位)、および小数点を含める必要があります。 例については、「[書式プロバイダーによるカルチャに依存した書式設定](#culture-sensitive-formatting-with-format-providers)」セクションを参照してください。

- アプリケーションによっては、同じ値をさまざまな方法で表示する必要がある。 たとえば、列挙型のメンバーを表すために、その名前の文字列形式を表示する場合や、基になる値を表示する場合が考えられます。 <xref:System.DayOfWeek> 列挙体のメンバーをさまざまな方法で書式指定する例については、「 [標準書式指定文字列](#standard-format-strings) 」を参照してください。

> [!NOTE]
> 書式設定は型の値を文字列形式に変換します。 解析は書式設定の逆の操作で、 文字列形式からデータ型のインスタンスを作成します。 他のデータ型への文字列の変換については、[文字列の解析](../../../docs/standard/base-types/parsing-strings.md)に関するページを参照してください。

.NET は書式設定機能が充実しているため、開発者はこうした要件を満たすことができます。

## <a name="formatting-in-net"></a>.NET での書式設定

基本的な書式設定の方式は、<xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドによって既定として実装されます。このメソッドについては、このトピックの「[ToString メソッドを使用した既定の書式設定](#default-formatting-using-the-tostring-method) 」のセクションを参照してください。 ただし、.NET には、この既定の書式設定機能を変更および拡張する方法がいくつかあります。 次に例を示します。

- <xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドをオーバーライドして、オブジェクトの値のカスタム文字列形式を定義する方法。 詳細については、このトピックの後の「[ToString メソッドのオーバーライド](#override-the-tostring-method)」セクションを参照してください。

- オブジェクトの値の文字列形式に複数の形式を持たせる書式指定子を定義する方法。 たとえば、次のステートメントでは "X" 書式指定子を使用して整数を 16 進値の文字列形式に変換します。

     [!code-csharp[Conceptual.Formatting.Overview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#3)]
     [!code-vb[Conceptual.Formatting.Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#3)]

     書式指定子の詳細については、「 [ToString メソッドと書式指定文字列](#the-tostring-method-and-format-strings) 」のセクションを参照してください。

- 書式プロバイダーを使用して、特定のカルチャの書式指定規則を利用する方法。 たとえば、次のステートメントでは en-US カルチャの書式指定規則を使用して通貨値を表示します。

     [!code-csharp[Conceptual.Formatting.Overview#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#10)]
     [!code-vb[Conceptual.Formatting.Overview#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#10)]

     書式プロバイダーを使用した書式設定の詳細については、[書式プロバイダー](#culture-sensitive-formatting-with-format-providers)に関するセクションを参照してください。

- <xref:System.IFormattable> インターフェイスを実装して、 <xref:System.Convert> クラスによる文字列変換と複合書式指定の両方をサポートする方法。 詳細については、「 [IFormattable インターフェイス](#the-iformattable-interface) 」のセクションを参照してください。

- 複合書式指定を使用して、値の文字列形式を大きな文字列に埋め込む方法。 詳細については、「 [複合書式指定](#composite-formatting) 」のセクションを参照してください。

- <xref:System.ICustomFormatter> および <xref:System.IFormatProvider> を実装して、完全なカスタム書式設定ソリューションを提供する方法。 詳細については、「 [ICustomFormatter を使用したカスタム書式設定](#custom-formatting-with-icustomformatter) 」のセクションを参照してください。

以降のセクションでは、オブジェクトを文字列形式に変換するこれらの方法について説明します。

## <a name="default-formatting-using-the-tostring-method"></a>ToString メソッドを使用した既定の書式設定

<xref:System.Object?displayProperty=nameWithType> から派生したすべての型は、既定で型の名前を返す、パラメーターなしの `ToString` メソッドを自動的に継承します。 既定の `ToString` メソッドの例を次に示します。 このコード例では、実装を持たない `Automobile` という名前のクラスを定義します。 このクラスがインスタンス化され、 `ToString` メソッドが呼び出されると、その型の名前が表示されます。 サンプルでは、 `ToString` メソッドが明示的に呼び出されないことに注意してください。 <xref:System.Console.WriteLine%28System.Object%29?displayProperty=nameWithType> メソッドは引数として渡されたオブジェクトの `ToString` メソッドを暗黙的に呼び出します。

[!code-csharp[Conceptual.Formatting.Overview#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/default1.cs#1)]
[!code-vb[Conceptual.Formatting.Overview#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/default1.vb#1)]

> [!WARNING]
> Windows 8 以降、Windows ランタイムには、既定の書式指定をサポートする単一のメソッド [IStringable.ToString](xref:Windows.Foundation.IStringable.ToString%2A) を備えた <xref:Windows.Foundation.IStringable> インターフェイスが含まれています。 ただし、マネージド型では `IStringable` インターフェイスを実装しないことをお勧めします。 詳細については、<xref:System.Object.ToString%2A?displayProperty=nameWithType> リファレンス ページの「Windows ランタイムと `IStringable` インターフェイス」セクションを参照してください。

インターフェイス以外の型はすべて <xref:System.Object>から派生するため、この機能はカスタムのクラスまたは構造体に自動的に提供されます。 ただし、既定の `ToString` メソッドで提供される機能には制限があります。型を識別しますが、型のインスタンスに関する情報を指定すると、エラーになります。 それ自体に関する情報を提供するオブジェクトの文字列形式を提供するには、 `ToString` メソッドをオーバーライドする必要があります。

> [!NOTE]
> 構造体は、 <xref:System.ValueType>から派生した <xref:System.Object>を継承します。 <xref:System.ValueType> は <xref:System.Object.ToString%2A?displayProperty=nameWithType>をオーバーライドしますが、その実装は同じです。

## <a name="override-the-tostring-method"></a>ToString メソッドのオーバーライド

型の名前の表示は用途が限定され、型のコンシューマー側でインスタンスを別のインスタンスと区別することはできません。 ただし、 `ToString` メソッドをオーバーライドして、もっと役に立つオブジェクトの値の形式を作成することができます。 次の例では `Temperature` オブジェクトを定義し、その `ToString` メソッドをオーバーライドして、温度を摂氏で表示します。

[!code-csharp[Conceptual.Formatting.Overview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/overrides1.cs#2)]
[!code-vb[Conceptual.Formatting.Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/overrides1.vb#2)]

.NET では、各プリミティブ値型の `ToString` メソッドは、名前の代わりにオブジェクトの値を表示するようにオーバーライドされています。 各プリミティブ型のオーバーライドを次の表に示します。 オーバーライドされているメソッドのほとんどは `ToString` メソッドの別のオーバーロードを呼び出し、それにその型の一般書式を定義する "G" 書式指定子と、現在のカルチャを表す <xref:System.IFormatProvider> オブジェクトを渡します。

|種類|ToString のオーバーライド|
|----------|-----------------------|
|<xref:System.Boolean>|<xref:System.Boolean.TrueString?displayProperty=nameWithType> または <xref:System.Boolean.FalseString?displayProperty=nameWithType>を返します。|
|<xref:System.Byte>|`Byte.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Byte> 値の書式設定をします。|
|<xref:System.Char>|文字を文字列として返します。|
|<xref:System.DateTime>|`DateTime.ToString("G", DatetimeFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて日付と時刻の値の書式設定をします。|
|<xref:System.Decimal>|`Decimal.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Decimal> 値の書式設定をします。|
|<xref:System.Double>|`Double.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Double> 値の書式設定をします。|
|<xref:System.Int16>|`Int16.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Int16> 値の書式設定をします。|
|<xref:System.Int32>|`Int32.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Int32> 値の書式設定をします。|
|<xref:System.Int64>|`Int64.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Int64> 値の書式設定をします。|
|<xref:System.SByte>|`SByte.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.SByte> 値の書式設定をします。|
|<xref:System.Single>|`Single.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.Single> 値の書式設定をします。|
|<xref:System.UInt16>|`UInt16.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.UInt16> 値の書式設定をします。|
|<xref:System.UInt32>|`UInt32.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.UInt32> 値の書式設定をします。|
|<xref:System.UInt64>|`UInt64.ToString("G", NumberFormatInfo.CurrentInfo)` を呼び出して、現在のカルチャに合わせて <xref:System.UInt64> 値の書式設定をします。|

## <a name="the-tostring-method-and-format-strings"></a>ToString メソッドと書式指定文字列

既定の `ToString` メソッドや `ToString` のオーバーライドの使用は、オブジェクトの文字列形式が 1 つの場合に適しています。 しかし、多くの場合オブジェクトの値には複数の形式があります。 たとえば、温度は華氏、摂氏、またはケルビンで表現できます。 また、整数値 10 は 10、10.0、1.0e01、$10.00 などの多くの方法で表すことができます。

.NET では、書式指定文字列を使用することで、1 つの値に複数の文字列形式を持たせることができます。 書式指定文字列とは定義済みの書式指定子を 1 つ以上含む文字列です。書式指定子とは、 `ToString` メソッドによるその出力の書式設定方法を定義する 1 文字または文字グループです。 書式指定文字列はパラメーターとしてオブジェクトの `ToString` メソッドに渡され、オブジェクトの値の文字列形式の表示方法を決定します。

.NET では、すべての数値型、日付/時刻型、および列挙型で、定義済みの一連の書式指定子をサポートしています。 書式指定文字列を使用して、アプリケーションで定義されたデータ型の文字列形式を複数定義することもできます。

### <a name="standard-format-strings"></a>標準の書式指定文字列

標準書式指定文字列には、適用先のオブジェクトの文字列形式を定義する英字の単一の書式指定子と、結果文字列に表示される桁数に影響するオプションの精度指定子が含まれます。 精度指定子が省略されるかサポートされていない場合、標準書式指定子は標準書式指定文字列と同じになります。

.NET では、すべての数値型、すべての日付/時刻型、およびすべての列挙型に対して一連の標準書式指定子が定義されています。 たとえば、それらのカテゴリごとに、対応する型の値の一般的な文字列形式を定義する "G" 標準書式指定子がサポートされています。

列挙型の標準書式指定文字列は、値の文字列形式を直接制御します。 列挙値の `ToString` メソッドに渡された書式指定文字列によって、文字列名 ("G" 書式指定子および "F" 書式指定子)、基になる整数値 ("D" 書式指定子)、または 16 進値 ("X" 書式指定子) のどの形式で値を表示するかが決定します。 次のコード例では、標準書式指定文字列を使用して、 <xref:System.DayOfWeek> 列挙値の書式を設定する方法を示しています。

[!code-csharp[Conceptual.Formatting.Overview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/standard1.cs#4)]
[!code-vb[Conceptual.Formatting.Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/standard1.vb#4)]

列挙型書式指定文字列については、「 [列挙型書式指定文字列](../../../docs/standard/base-types/enumeration-format-strings.md)」を参照してください。

数値型の標準書式指定文字列は、通常、表示される桁数が 1 つ以上のプロパティ値によって制御される結果文字列を定義します。 たとえば、"C" 書式指定子は数字を通貨値として書式設定します。 唯一のパラメーターとして "C" 書式指定子を渡して `ToString` メソッドを呼び出した場合、現在のカルチャの <xref:System.Globalization.NumberFormatInfo> オブジェクトの次のプロパティ値を使用して数値の文字列形式を定義します。

- <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A> プロパティ。現在のカルチャの通貨記号を指定します。

- <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> プロパティまたは <xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A> プロパティ。次の情報を特定する整数を返します。

  - 通貨記号の位置。

  - 負の値を表すために、先頭の負の符号、末尾の負の符号、またはかっこのどれを使用するか。

  - 数値と通貨記号の間にスペース文字を表示するかどうか。

- <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A> プロパティ。結果文字列の小数点以下の桁数を定義します。

- <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A> プロパティ。結果文字列の小数点の記号を定義します。

- <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A> プロパティ。桁区切り記号を定義します。

- <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A> プロパティ。整数部の各グループの桁数を定義します。

- <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A> プロパティ。かっこを使用せずに負の値を表す場合に結果文字列で使用する負の符号を決定します。

さらに、数値書式指定文字列には、精度指定子が含まれる場合があります。 この指定子の意味は一緒に使用される書式指定文字列によって異なりますが、通常は、結果文字列に表示される合計桁数か小数点以下の桁数を示します。 たとえば、次の例では、"X4" の標準数値文字列と精度指定子を使用して、4 桁の 16 進数から成る文字列値を作成します。

[!code-csharp[Conceptual.Formatting.Overview#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/precisionspecifier1.cs#6)]
[!code-vb[Conceptual.Formatting.Overview#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/precisionspecifier1.vb#6)]

標準の数値書式指定文字列の詳細については、「 [標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)」を参照してください。

日付と時刻の値の標準書式指定文字列は、特定の <xref:System.Globalization.DateTimeFormatInfo> プロパティに格納されているカスタム書式指定文字列のエイリアスです。 たとえば、"D" 書式指定子を渡して日付と時刻の値の `ToString` メソッドを呼び出すと、現在のカルチャの <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> プロパティに格納されているカスタム書式指定文字列を使用して日付と時刻が表示されます (カスタム書式指定文字列の詳細については、[次のセクション](#custom-format-strings)を参照してください)。この関係を次の例に示します。

[!code-csharp[Conceptual.Formatting.Overview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/alias1.cs#5)]
[!code-vb[Conceptual.Formatting.Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/alias1.vb#5)]

標準の日時書式指定文字列の詳細については、「 [標準の日時形式文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)」を参照してください。

また、標準書式指定文字列を使用して、オブジェクトの `ToString(String)` メソッドによって生成された、アプリケーション定義のオブジェクトの文字列形式を定義することもできます。 オブジェクトでサポートする特定の標準書式指定子を定義したり、それらで大文字と小文字を区別するかしないかを決定したりすることができます。 `ToString(String)` メソッドの実装で、以下がサポートされます。

- オブジェクトの一般的な書式または共通の書式を表す "G" 書式指定子。 オブジェクトの `ToString` メソッドのパラメーターなしのオーバーロードで、その `ToString(String)` オーバーロードを呼び出し、それに "G" 標準書式指定文字列を渡します。

- null 参照 (Visual Basic の場合は`Nothing` ) に相当する書式指定子。 null 参照に相当する書式指定子は "G" 書式指定子と同等に扱う必要があります。

たとえば、 `Temperature` クラスの場合、内部的には温度を摂氏で格納し、書式指定子を使用して `Temperature` オブジェクトの値を摂氏、華氏、およびケルビンで表すことができます。 具体的な例を次に示します。

[!code-csharp[Conceptual.Formatting.Overview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/appstandard1.cs#7)]
[!code-vb[Conceptual.Formatting.Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/appstandard1.vb#7)]

### <a name="custom-format-strings"></a>カスタム書式指定文字列

.NET では、標準書式指定文字列のほかに、数値および日付と時刻の値の両方のカスタム書式指定文字列が定義されています。 カスタム書式指定文字列は、値の文字列形式を定義する 1 つ以上のカスタム書式指定子で構成されます。 たとえば、"yyyy/mm/dd hh:mm:ss.ffff t zzz" というカスタム日時書式指定文字列の場合、en-US カルチャでは日付が "2008/11/15 07:45:00.0000 P -08:00" という文字列形式に変換されます。 また、"0000" というカスタム書式指定文字列の場合、整数値 12 は "0012" に変換されます。 カスタム書式指定文字列の一覧については、「 [カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md) 」および「 [カスタム数値形式文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)」を参照してください。

書式指定文字列が単一のカスタム書式指定子で構成される場合は、標準書式指定子と混同しないように、書式指定子の前にパーセント (%) 記号を付ける必要があります。 次の例では、"M" カスタム書式指定子を使用して、特定の日付の月を表す 1 桁または 2 桁の数値を表示します。

[!code-csharp[Conceptual.Formatting.Overview#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/singlecustom1.cs#8)]
[!code-vb[Conceptual.Formatting.Overview#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/singlecustom1.vb#8)]

日付および時刻の値の標準書式指定文字列の多くは、 <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのプロパティによって定義されているカスタム書式指定文字列のエイリアスです。 また、カスタム書式指定文字列を使用することで、数値または日付と時刻の値に対して、柔軟にアプリケーション定義の書式を指定できます。 複数のカスタム書式指定子を 1 つのカスタム書式指定文字列に結合することによって、数値および日付と時刻の値の両方に対するカスタムの結果文字列を独自に定義することができます。 次の例では、月の名前、日付、および年の後に、かっこで囲んで曜日を表示するカスタム書式指定文字列を定義しています。

[!code-csharp[Conceptual.Formatting.Overview#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/custom1.cs#9)]
[!code-vb[Conceptual.Formatting.Overview#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/custom1.vb#9)]

次の例では、 <xref:System.Int64> 値を米国で標準的な 7 桁の電話番号を市外局番と共に表示するカスタム書式指定文字列を定義します。

[!code-csharp[Conceptual.Formatting.Overview#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/telnumber1.cs#21)]
[!code-vb[Conceptual.Formatting.Overview#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/telnumber1.vb#21)]

一般には、アプリケーション定義の型に対する書式設定のほとんどのニーズに標準書式指定文字列を使用して対応できますが、カスタム書式指定子を定義して型の書式を設定することもできます。

### <a name="format-strings-and-net-types"></a>書式指定文字列と .NET 型

すべての数値型 (つまり、<xref:System.Byte>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64>、および <xref:System.Numerics.BigInteger> 型)、<xref:System.DateTime>、<xref:System.DateTimeOffset>、<xref:System.TimeSpan>、<xref:System.Guid>、すべての列挙型が、書式指定文字列による書式設定に対応しています。 各型でサポートされている特定の書式指定文字列については、次のトピックを参照してください。

|Title|定義|
|-----------|----------------|
|[Standard Numeric Format Strings](../../../docs/standard/base-types/standard-numeric-format-strings.md)|数値に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム数値形式文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)|数値に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[標準の日時形式文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)|<xref:System.DateTime> と <xref:System.DateTimeOffset> 値に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)|<xref:System.DateTime> と <xref:System.DateTimeOffset> 値に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[標準の時間間隔書式指定文字列](../../../docs/standard/base-types/standard-timespan-format-strings.md)|時間間隔に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム時間間隔書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)|時間間隔に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[Enumeration Format Strings](../../../docs/standard/base-types/enumeration-format-strings.md)|列挙型の文字列形式を作成するために使用される標準書式指定文字列について説明します。|
|<xref:System.Guid.ToString%28System.String%29?displayProperty=nameWithType>|<xref:System.Guid> 値の標準的書式指定文字列について説明します。|

## <a name="culture-sensitive-formatting-with-format-providers"></a>書式プロバイダーによるカルチャに依存した書式設定

書式指定子を利用することでオブジェクトの書式をカスタマイズできますが、多くの場合、意味のあるオブジェクトの文字列形式を生成するには追加の書式設定情報が必要です。 たとえば、"C" 標準書式指定文字列または "$ #,#.00" などのカスタム書式指定文字列を使用して数字を通貨値として書式設定する場合、少なくとも、正しい通貨記号、桁区切り記号、および小数点記号についての情報を書式設定された文字列に含めることができる必要があります。 .NET では、<xref:System.IFormatProvider> インターフェイスによって、この追加の書式設定情報を利用できるようにします。このインターフェイスは、数値型および日付/時刻型の `ToString` メソッドの 1 つ以上のオーバーロードに対するパラメーターとして提供されます。 <xref:System.IFormatProvider> の実装は .NET で使用され、カルチャ固有の書式指定をサポートします。 それぞれ異なるカルチャを示す 3 つの <xref:System.IFormatProvider> オブジェクトを使用してオブジェクトの書式を設定した場合に、その文字列形式がどのように変化するかを次の例に示します。

[!code-csharp[Conceptual.Formatting.Overview#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformatprovider1.cs#11)]
[!code-vb[Conceptual.Formatting.Overview#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformatprovider1.vb#11)]

<xref:System.IFormatProvider> インターフェイスには、 <xref:System.IFormatProvider.GetFormat%28System.Type%29>という 1 つのメソッドが含まれています。このメソッドには、書式設定情報を提供するオブジェクトの型を指定する 1 つのパラメーターがあります。 このメソッドがその型のオブジェクトを提供できる場合は、それが返されます。 それ以外の場合は、null 参照 (Visual Basic の場合は`Nothing` ) が返されます。

<xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> はコールバック メソッドです。 `ToString` パラメーターを含む <xref:System.IFormatProvider> メソッド オーバーロードを呼び出すと、その <xref:System.IFormatProvider.GetFormat%2A> オブジェクトの <xref:System.IFormatProvider> メソッドが呼び出されます。 <xref:System.IFormatProvider.GetFormat%2A> メソッドは、 `formatType` パラメーターで指定された、必要な書式設定情報を提供するオブジェクトを `ToString` メソッドに返します。

数多くの書式指定メソッドや文字列変換メソッドに <xref:System.IFormatProvider>型のパラメーターが含まれていますが、多くの場合、メソッドを呼び出すときはパラメーターの値は無視されます。 <xref:System.Type> メソッドに渡される <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> オブジェクトのパラメーターと型を使用する書式指定メソッドの一部を次の表に示します。

|メソッド|`formatType` パラメーターの型|
|------------|------------------------------------|
|数値型の`ToString` メソッド|<xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>|
|日付/時刻型の`ToString` メソッド|<xref:System.Globalization.DateTimeFormatInfo?displayProperty=nameWithType>|
|<xref:System.String.Format%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|

> [!NOTE]
> 数値型および日付/時刻型の `ToString` メソッドはオーバーロードされますが、 <xref:System.IFormatProvider> パラメーターが含まれるのはそのうちの一部のオーバーロードだけです。 メソッドに <xref:System.IFormatProvider>型のパラメーターがない場合は、代わりに <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> プロパティによって返されるオブジェクトが渡されます。 たとえば、既定の <xref:System.Int32.ToString?displayProperty=nameWithType> メソッドの呼び出しの場合は、最終的には `Int32.ToString("G", System.Globalization.CultureInfo.CurrentCulture)`のようなメソッド呼び出しになります。

.NET には、<xref:System.IFormatProvider> を実装する次の 3 つのクラスが用意されています。

- <xref:System.Globalization.DateTimeFormatInfo>。このクラスは、特定のカルチャの日付と時刻の値に対する書式設定情報を提供します。 対応する <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> の実装では、単にそれ自身のインスタンスが返されます。

- <xref:System.Globalization.NumberFormatInfo>。このクラスは、特定のカルチャの数値に対する書式設定情報を提供します。 対応する <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> の実装では、単にそれ自身のインスタンスが返されます。

- <xref:System.Globalization.CultureInfo>。 対応する <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> の実装では、数値に対する書式設定情報を提供する <xref:System.Globalization.NumberFormatInfo> オブジェクトか、日付と時刻の値に対する書式設定情報を提供する <xref:System.Globalization.DateTimeFormatInfo> オブジェクトのいずれかが返されます。

また、これらのクラスのうちのいずれかを置き換える独自の書式プロバイダーを実装できます。 ただし、実装の <xref:System.IFormatProvider.GetFormat%2A> メソッドは、書式設定情報を `ToString` メソッドに渡す場合、前の表に示した型のオブジェクトを返す必要があります。

### <a name="culture-sensitive-formatting-of-numeric-values"></a>数値のカルチャに依存した書式設定

既定では、数値の書式指定はカルチャに依存します。 書式指定メソッドを呼び出すときにカルチャを指定しない場合は、現在のスレッド カルチャの書式指定規則が使用されます。 次に示す例では、現在のスレッド カルチャを 4 回変更した後に、 <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType> メソッドを呼び出します。 各ケースでは、結果の文字列は、現在のカルチャの書式指定規則を反映します。 これは、各数値型の `ToString` メソッドへの呼び出しを、 `ToString(String)` メソッドと `ToString(String, IFormatProvider)` メソッドがラップするためです。

[!code-csharp[Conceptual.Formatting.Overview#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific3.cs#19)]
[!code-vb[Conceptual.Formatting.Overview#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific3.vb#19)]

また、 `ToString` パラメーターを持つ `provider` オーバーロードを呼び出して、次のどちらかを渡すことにより、特定カルチャの数値を書式指定することもできます。

- 使用される書式指定規則のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクト。 その <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> メソッドは、 <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType> プロパティの値を返します。このプロパティは、数値にカルチャ固有の書式指定情報を提供する <xref:System.Globalization.NumberFormatInfo> オブジェクトです。

- 使用されるカルチャ固有の書式指定規則を定義する <xref:System.Globalization.NumberFormatInfo> オブジェクト。 その <xref:System.Globalization.NumberFormatInfo.GetFormat%2A> メソッドでは、それ自身のインスタンスが返されます。

次の例では、浮動小数点数を書式指定する際に、英語 (米国) と英語 (英国) のカルチャおよびフランス語とロシア語のニュートラル カルチャを表す <xref:System.Globalization.NumberFormatInfo> オブジェクトを使用します。

[!code-csharp[Conceptual.Formatting.Overview#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific4.cs#20)]
[!code-vb[Conceptual.Formatting.Overview#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific4.vb#20)]

### <a name="culture-sensitive-formatting-of-date-and-time-values"></a>日付と時刻の値のカルチャに依存した書式設定

既定では、日時の値の書式指定はカルチャに依存します。 書式指定メソッドを呼び出すときにカルチャを指定しない場合は、現在のスレッド カルチャの書式指定規則が使用されます。 次に示す例では、現在のスレッド カルチャを 4 回変更した後に、 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> メソッドを呼び出します。 各ケースでは、結果の文字列は、現在のカルチャの書式指定規則を反映します。 これは、 <xref:System.DateTime.ToString?displayProperty=nameWithType>、 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType>、 <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType>、 <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> の各メソッドが、 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドおよび <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドへの呼び出しをラップするためです。

[!code-csharp[Conceptual.Formatting.Overview#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific1.cs#17)]
[!code-vb[Conceptual.Formatting.Overview#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific1.vb#17)]

<xref:System.DateTime.ToString%2A?displayProperty=nameWithType> パラメーターを持つ <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> または `provider` オーバーロードを呼び出して、次のどちらかを渡すことにより、特定カルチャの日時の値を書式指定することもできます。

- 使用される書式指定規則のカルチャを表す <xref:System.Globalization.CultureInfo> オブジェクト。 その <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> メソッドは、 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> プロパティの値を返します。このプロパティは、日時の値にカルチャ固有の書式指定情報を提供する <xref:System.Globalization.DateTimeFormatInfo> オブジェクトです。

- 使用されるカルチャ固有の書式指定規則を定義する <xref:System.Globalization.DateTimeFormatInfo> オブジェクト。 その <xref:System.Globalization.DateTimeFormatInfo.GetFormat%2A> メソッドでは、それ自身のインスタンスが返されます。

次の例では、日付を書式指定する際に、英語 (米国) と英語 (英国) のカルチャおよびフランス語とロシア語のニュートラル カルチャを表す <xref:System.Globalization.DateTimeFormatInfo> オブジェクトを使用します。

[!code-csharp[Conceptual.Formatting.Overview#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific2.cs#18)]
[!code-vb[Conceptual.Formatting.Overview#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific2.vb#18)]

## <a name="the-iformattable-interface"></a>IFormattable インターフェイス

通常、書式指定文字列および `ToString` パラメーターを使用して <xref:System.IFormatProvider> メソッドをオーバーロードする型は、 <xref:System.IFormattable> インターフェイスも実装します。 このインターフェイスには、 <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>という単一のメンバーがあります。このメンバーには、パラメーターとして書式指定文字列と書式プロバイダーの両方が含まれています。

アプリケーション定義のクラスに <xref:System.IFormattable> インターフェイスを実装した場合、2 つの利点があります。

- <xref:System.Convert> クラスによる文字列変換がサポートされます。 <xref:System.Convert.ToString%28System.Object%29?displayProperty=nameWithType> メソッドおよび <xref:System.Convert.ToString%28System.Object%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドを呼び出すと、自動的に <xref:System.IFormattable> の実装が呼び出されます。

- 複合書式指定がサポートされます。 書式指定文字列を含む書式指定項目を使用してカスタムの型の書式を設定する場合に、共通言語ランタイムによって自動的に <xref:System.IFormattable> の実装が呼び出され、それに書式指定文字列が渡されます。 <xref:System.String.Format%2A?displayProperty=nameWithType> メソッドや <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>メソッドを使用した複合書式指定の詳細については、「 [複合書式指定](#composite-formatting) 」のセクションを参照してください。

次の例では、 `Temperature` インターフェイスを実装する <xref:System.IFormattable> クラスを定義しています。 このクラスでは、温度を摂氏で表示するために "C" 書式指定子または "G" 書式指定子、華氏で表示するために "F" 書式指定子、ケルビンで表示するために "K" 書式指定子をそれぞれサポートしています。

[!code-csharp[Conceptual.Formatting.Overview#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#12)]
[!code-vb[Conceptual.Formatting.Overview#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#12)]

次の例では、 `Temperature` オブジェクトをインスタンス化しています。 その後、 <xref:System.Convert.ToString%2A> メソッドを呼び出し、いくつかの複合書式指定文字列を使用して `Temperature` オブジェクトのさまざまな文字列形式を取得します。 さらに、それらの各メソッド呼び出しで、 <xref:System.IFormattable> クラスの `Temperature` 実装を呼び出しています。

[!code-csharp[Conceptual.Formatting.Overview#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#13)]
[!code-vb[Conceptual.Formatting.Overview#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#13)]

## <a name="composite-formatting"></a>複合書式指定

<xref:System.String.Format%2A?displayProperty=nameWithType> や <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> などの一部のメソッドでは、複合書式指定がサポートされます。 複合書式指定文字列は一種のテンプレートで、0 個以上のオブジェクトの文字列形式が組み込まれた単一の文字列を返します。 各オブジェクトは、インデックス付きの書式指定項目によって、複合書式指定文字列で表現されます。 書式指定項目のインデックスは、それが表すオブジェクトのメソッドのパラメーター リスト内の位置と対応しています。 インデックスは 0 から始まります。 たとえば、 <xref:System.String.Format%2A?displayProperty=nameWithType> メソッドの次のメソッド呼び出しでは、最初の書式指定項目 `{0:D}`は `thatDate`の文字列形式に、2 番目の書式指定項目 `{1}`は `item1`の文字列形式に、3 番目の書式指定項目 `{2:C2}`は `item1.Value`の文字列形式に置き換えられます。

[!code-csharp[Conceptual.Formatting.Overview#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite1.cs#14)]
[!code-vb[Conceptual.Formatting.Overview#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite1.vb#14)]

書式項目をそれに対応するオブジェクトの文字列形式に置換することに加えて、書式項目は以下を制御することもできます。

- オブジェクトを文字列として表現する特定の方法 (オブジェクトが <xref:System.IFormattable> インターフェイスを実装し、書式文字列をサポートする場合)。 これは、 `:` (コロン) 付きの書式項目のインデックスに、有効な書式文字列を続けることによります。 前の例では、日付の値を "d" (短い日付のパターン) 書式文字列 ( `{0:d}`など) を書式設定し、数値を "C2" 書式文字列 ( `{2:C2}` など) で書式設定して数値を 2 桁の小数部を含む 10 進数を持つ通貨値で表していました。

- オブジェクトの文字列形式を含むフィールドの幅、およびそのフィールドの文字列形式の配置。 これは、 `,` (コンマ) 付きの書式項目のインデックスに、フィールドの幅を続けることによります。 フィールドの幅が正の値の場合、文字列はフィールドで右揃えにし、フィールドの幅が負の値の場合、左揃えにします。 次の例では、20 文字のフィールドで日付の値を左揃えにし、11 文字のフィールドで、1 桁の小数部を含む 10 進数値を右揃えにします。

     [!code-csharp[Conceptual.Formatting.Overview#22](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite2.cs#22)]
     [!code-vb[Conceptual.Formatting.Overview#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite2.vb#22)]

     配置文字列コンポーネントと書式文字列コンポーネントの両方が存在する場合、前者は後者の前にきます (たとえば `{0,-20:g}`)。

複合書式指定の詳細については、「 [複合書式指定](../../../docs/standard/base-types/composite-formatting.md)」を参照してください。

## <a name="custom-formatting-with-icustomformatter"></a>ICustomFormatter を使用したカスタム書式設定

<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> および <xref:System.Text.StringBuilder.AppendFormat%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>の 2 つの複合書式指定メソッドには、カスタム書式設定をサポートしている書式プロバイダー パラメーターが含まれています。 これらの書式指定メソッドのいずれかを呼び出すと、書式プロバイダーの <xref:System.Type> メソッドに <xref:System.ICustomFormatter> インターフェイスを表す <xref:System.IFormatProvider.GetFormat%2A> オブジェクトが渡されます。 次に、 <xref:System.IFormatProvider.GetFormat%2A> メソッドによって、カスタム書式設定を提供する <xref:System.ICustomFormatter> の実装が返されます。

<xref:System.ICustomFormatter> インターフェイスには、 <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29>という単一のメソッドがあります。このメソッドは、複合書式指定文字列の書式指定項目ごとに 1 回、複合書式指定メソッドによって自動的に呼び出されます。 <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29> メソッドには、3 つのパラメーターがあります。書式指定項目の `formatString` 引数を表す書式指定文字列、書式を設定するオブジェクト、および書式指定サービスを提供する <xref:System.IFormatProvider> オブジェクトの 3 つです。 通常は、 <xref:System.ICustomFormatter> を実装するクラスでは <xref:System.IFormatProvider>も実装するため、この最後のパラメーターはカスタム書式指定クラス自体への参照になります。 このメソッドは、書式を設定するオブジェクトのカスタム書式の文字列形式を返します。 オブジェクトの書式を設定できない場合は、null 参照 (Visual Basic の場合は`Nothing` ) を返します。

整数値を 2 桁の 16 進値とそれに続く 1 つの空白のシーケンスとして表示する、 <xref:System.ICustomFormatter> という名前の `ByteByByteFormatter` の実装の例を次に示します。

[!code-csharp[Conceptual.Formatting.Overview#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#15)]
[!code-vb[Conceptual.Formatting.Overview#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#15)]

`ByteByByteFormatter` クラスを使用して整数値の書式を設定する例を次に示します。 サンプルでは、 <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> メソッドが 2 回目の <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッド呼び出しで複数回呼び出されることに注意してください。また、 <xref:System.Globalization.NumberFormatInfo> メソッドが "N0" 書式指定文字列を認識せず、null 参照 (Visual Basic の場合は`ByteByByteFormatter.Format` ) を返すため、3 回目のメソッド呼び出しでは既定の`Nothing` ) に相当する書式指定子。

[!code-csharp[Conceptual.Formatting.Overview#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#16)]
[!code-vb[Conceptual.Formatting.Overview#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#16)]

## <a name="related-topics"></a>関連トピック

|Title|定義|
|-----------|----------------|
|[Standard Numeric Format Strings](../../../docs/standard/base-types/standard-numeric-format-strings.md)|数値に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム数値形式文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)|数値に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[Standard Date and Time Format Strings](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)|<xref:System.DateTime> 値に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム日時形式文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)|<xref:System.DateTime> 値に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[標準の時間間隔書式指定文字列](../../../docs/standard/base-types/standard-timespan-format-strings.md)|時間間隔に対して一般的に使用される文字列形式を作成する標準書式指定文字列について説明します。|
|[カスタム時間間隔書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)|時間間隔に対するアプリケーション固有の文字列形式を作成するカスタム書式指定文字列について説明します。|
|[Enumeration Format Strings](../../../docs/standard/base-types/enumeration-format-strings.md)|列挙型の文字列形式を作成するために使用される標準書式指定文字列について説明します。|
|[複合書式指定](../../../docs/standard/base-types/composite-formatting.md)|文字列に 1 つ以上の書式指定された値を埋め込む方法について説明します。 この文字列は、コンソールに表示したり、ストリームに書き込んだりできます。|
|[書式設定操作の実行](../../../docs/standard/base-types/performing-formatting-operations.md)|特定の書式設定操作を行うための手順を説明するトピックの一覧を示します。|
|[文字列の解析](../../../docs/standard/base-types/parsing-strings.md)|オブジェクトの文字列表現によって指定された値にオブジェクトを初期化する方法について説明します。 解析は書式設定の逆の操作です。|

## <a name="reference"></a>関連項目

- <xref:System.IFormattable?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- <xref:System.ICustomFormatter?displayProperty=nameWithType>
