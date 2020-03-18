---
title: .Net を使用してC# JSON をシリアル化および逆シリアル化する方法
ms.date: 01/10/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 8025f84f2425f5b91e08b28ddb24d105d8c4d1a3
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159586"
---
# <a name="how-to-serialize-and-deserialize-marshal-and-unmarshal-json-in-net"></a>.NET で JSON のシリアル化と逆シリアル化 (マーシャリングとマーシャリング解除) を行う方法

この記事では、<xref:System.Text.Json> 名前空間を使用して、JavaScript Object Notation (JSON) との間でシリアル化および逆シリアル化を行う方法について説明します。

指示とサンプルコードは、ライブラリを直接使用します。 [ASP.NET Core](/aspnet/core/)などのフレームワークでは使用されません。

シリアル化のサンプルコードの大部分では、JSON を "見栄えよく" するように `true` に <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> を設定します (ユーザーが読みやすくするためにインデントと空白文字が使用されます)。 実稼働環境で使用する場合は、通常、この設定の `false` の既定値をそのまま使用します。

## <a name="namespaces"></a>名前空間

<xref:System.Text.Json> 名前空間には、すべてのエントリポイントと主要な型が含まれます。 <xref:System.Text.Json.Serialization> 名前空間には、シリアル化と逆シリアル化に固有の高度なシナリオとカスタマイズのための属性と Api が含まれています。 この記事に示されているコード例では、次のいずれかまたは両方の名前空間に `using` ディレクティブが必要です。

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;
```

<xref:System.Runtime.Serialization> 名前空間の属性は、現在 `System.Text.Json`ではサポートされていません。

## <a name="how-to-write-net-objects-to-json-serialize"></a>.NET オブジェクトを JSON に書き込む方法 (シリアル化)

JSON を文字列またはファイルに書き込むには、<xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> メソッドを呼び出します。

次の例では、JSON を文字列として作成します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToString.cs?name=SnippetSerialize)]

次の例では、同期コードを使用して JSON ファイルを作成します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToFile.cs?name=SnippetSerialize)]

次の例では、非同期コードを使用して JSON ファイルを作成します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToFileAsync.cs?name=SnippetSerialize)]

前の例では、シリアル化する型の型の推定を使用します。 `Serialize()` のオーバーロードは、ジェネリック型パラメーターを受け取ります。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToString.cs?name=SnippetSerializeWithGenericParameter)]

### <a name="serialization-example"></a>シリアル化の例

コレクションと入れ子になったクラスを含むクラスの例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPOCOs)]

前の型のインスタンスをシリアル化した JSON 出力は、次の例のようになります。 既定では、JSON 出力が縮小されます。

```json
{"Date":"2019-08-01T00:00:00-07:00","TemperatureCelsius":25,"Summary":"Hot","DatesAvailable":["2019-08-01T00:00:00-07:00","2019-08-02T00:00:00-07:00"],"TemperatureRanges":{"Cold":{"High":20,"Low":-10},"Hot":{"High":60,"Low":20}},"SummaryWords":["Cool","Windy","Humid"]}
```

次の例では、同じ JSON が書式設定されています (空白とインデントで整形されています)。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "TemperatureRanges": {
    "Cold": {
      "High": 20,
      "Low": -10
    },
    "Hot": {
      "High": 60,
      "Low": 20
    }
  },
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

### <a name="serialize-to-utf-8"></a>UTF-8 にシリアル化する

UTF-8 にシリアル化するには、<xref:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes%2A?displayProperty=nameWithType> メソッドを呼び出します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToUtf8.cs?name=SnippetSerialize)]

<xref:System.Text.Json.Utf8JsonWriter> を受け取る <xref:System.Text.Json.JsonSerializer.Serialize%2A> のオーバーロードも使用できます。

UTF-8 へのシリアル化は、文字列ベースのメソッドを使用するよりも約5-10% 高速です。 違いは、バイト (UTF-8) を文字列 (UTF-16) に変換する必要がないためです。

## <a name="serialization-behavior"></a>シリアル化の動作

* 既定では、すべてのパブリックプロパティがシリアル化されます。 [除外するプロパティを指定](#exclude-properties-from-serialization)できます。
* [既定のエンコーダー](xref:System.Text.Encodings.Web.JavaScriptEncoder.Default)は、非 ascii 文字、ascii 範囲内の HTML を区別する文字、および[RFC 8259 JSON 仕様](https://tools.ietf.org/html/rfc8259#section-7)に従ってエスケープされる必要がある文字をエスケープします。
* 既定では、JSON は縮小されています。 [JSON はかなり印刷](#serialize-to-formatted-json)できます。
* 既定では、JSON 名の大文字と小文字の区別は .NET 名と一致します。 [JSON 名の大文字と小文字](#customize-json-names-and-values)の区別をカスタマイズできます。
* 循環参照が検出され、例外がスローされました。
* 現在、フィールドは除外されています。

サポートされている種類は次のとおりです。

* 数値型、文字列、ブール値など、JavaScript プリミティブにマップされる .NET プリミティブ。
* ユーザー定義の[Plain OLD CLR オブジェクト (POCOs)](https://stackoverflow.com/questions/250001/poco-definition)。
* 1次元配列とジャグ配列 (`ArrayName[][]`)。
* `Dictionary<string,TValue>` `TValue` が `object`、`JsonElement`、または POCO です。
* 次の名前空間のコレクション。
  * <xref:System.Collections>
  * <xref:System.Collections.Generic>
  * <xref:System.Collections.Immutable>

[カスタムコンバーターを実装](system-text-json-converters-how-to.md)して、追加の型を処理したり、組み込みのコンバーターでサポートされていない機能を提供したりすることができます。

## <a name="how-to-read-json-into-net-objects-deserialize"></a>JSON を .NET オブジェクトに読み込む方法 (逆シリアル化)

文字列またはファイルから逆シリアル化するには、<xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> メソッドを呼び出します。

次の例では、文字列から JSON を読み取り、[シリアル化の例](#serialization-example)で前に示した `WeatherForecast` クラスのインスタンスを作成します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToString.cs?name=SnippetDeserialize)]

同期コードを使用してファイルから逆シリアル化するには、次の例に示すように、ファイルを文字列に読み取ります。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToFile.cs?name=SnippetDeserialize)]

非同期コードを使用してファイルから逆シリアル化するには、<xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A> メソッドを呼び出します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToFileAsync.cs?name=SnippetDeserialize)]

### <a name="deserialize-from-utf-8"></a>UTF-8 からの逆シリアル化

UTF-8 から逆シリアル化するには、次の例に示すように、`Utf8JsonReader` または `ReadOnlySpan<byte>`を受け取る <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> オーバーロードを呼び出します。 この例では、JSON が jsonUtf8Bytes という名前のバイト配列にあることを前提としています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize1)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize2)]

## <a name="deserialization-behavior"></a>逆シリアル化の動作

* 既定では、プロパティ名の照合では大文字と小文字が区別されます。 [大文字と小文字を区別](#case-insensitive-property-matching)しないように指定できます。
* JSON に読み取り専用プロパティの値が含まれている場合、この値は無視され、例外はスローされません。
* パラメーターなしのコンストラクターを使用しない参照型への逆シリアル化はサポートされていません。
* 変更できないオブジェクトまたは読み取り専用プロパティへの逆シリアル化はサポートされていません。
* 既定では、列挙型は数値としてサポートされています。 [列挙型名を文字列としてシリアル化](#enums-as-strings)できます。
* フィールドはサポートされていません。
* 既定では、JSON のコメントまたは末尾のコンマによって例外がスローされます。 [コメントと末尾のコンマを許可](#allow-comments-and-trailing-commas)できます。
* [既定の最大の深さ](xref:System.Text.Json.JsonReaderOptions.MaxDepth)は64です。

組み込みのコンバーターでサポートされていない機能を提供するために、[カスタムコンバーターを実装](system-text-json-converters-how-to.md)することができます。

## <a name="serialize-to-formatted-json"></a>書式設定された JSON にシリアル化する

JSON 出力を整形するには、<xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> を `true`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripToString.cs?name=SnippetSerializePrettyPrint)]

シリアル化され、整形された JSON 出力の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

## <a name="customize-json-names-and-values"></a>JSON の名前と値をカスタマイズする

既定では、プロパティ名とディクショナリキーは、大文字と小文字を含め、JSON の出力では変更されません。 列挙値は数値として表されます。 このセクションでは、次の方法について説明します。

* [個々のプロパティ名をカスタマイズする](#customize-individual-property-names)
* [すべてのプロパティ名を camel 形式に変換します](#use-camel-case-for-all-json-property-names)
* [カスタムプロパティの名前付けポリシーを実装する](#use-a-custom-json-property-naming-policy)
* [ディクショナリキーを camel 形式に変換する](#camel-case-dictionary-keys)
* [列挙型を文字列および camel 形式に変換する](#enums-as-strings)

JSON プロパティの名前と値の特別な処理を必要とするその他のシナリオでは、[カスタムコンバーターを実装](system-text-json-converters-how-to.md)できます。

### <a name="customize-individual-property-names"></a>個々のプロパティ名をカスタマイズする

個々のプロパティの名前を設定するには、 [[Jsonpropertyname]](xref:System.Text.Json.Serialization.JsonPropertyNameAttribute)属性を使用します。

シリアル化し、結果として得られる JSON の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "Wind": 35
}
```

この属性によって設定されるプロパティ名:

* シリアル化と逆シリアル化の両方の方向で適用されます。
* は、プロパティの名前付けポリシーよりも優先されます。

### <a name="use-camel-case-for-all-json-property-names"></a>すべての JSON プロパティ名にキャメルケースを使用する

すべての JSON プロパティ名にキャメルケースを使用するには、次の例に示すように、<xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> を `JsonNamingPolicy.CamelCase`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundTripCamelCasePropertyNames.cs?name=Serialize)]

シリアル化と JSON 出力の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
  "Wind": 35
}
```

Camel 形式のプロパティの名前付けポリシー:

* シリアル化および逆シリアル化に適用されます。
* は `[JsonPropertyName]` 属性によってオーバーライドされます。 このため、この例の JSON プロパティ名 `Wind` は camel 形式ではありません。

### <a name="use-a-custom-json-property-naming-policy"></a>カスタム JSON プロパティの名前付けポリシーを使用する

カスタム JSON プロパティの名前付けポリシーを使用するには、次の例に示すように、<xref:System.Text.Json.JsonNamingPolicy> から派生するクラスを作成し、<xref:System.Text.Json.JsonNamingPolicy.ConvertName%2A> メソッドをオーバーライドします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/UpperCaseNamingPolicy.cs)]

次に、[<xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType>] プロパティを名前付けポリシークラスのインスタンスに設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripPropertyNamingPolicy.cs?name=SnippetSerialize)]

シリアル化と JSON 出力の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "DATE": "2019-08-01T00:00:00-07:00",
  "TEMPERATURECELSIUS": 25,
  "SUMMARY": "Hot",
  "Wind": 35
}
```

JSON プロパティの名前付けポリシー:

* シリアル化および逆シリアル化に適用されます。
* は `[JsonPropertyName]` 属性によってオーバーライドされます。 このため、この例の JSON プロパティ名 `Wind` は大文字ではありません。

### <a name="camel-case-dictionary-keys"></a>Camel ケースディクショナリキー

シリアル化するオブジェクトのプロパティが `Dictionary<string,TValue>`型である場合は、`string` キーを camel 形式に変換できます。 これを行うには、次の例に示すように、<xref:System.Text.Json.JsonSerializerOptions.DictionaryKeyPolicy> を `JsonNamingPolicy.CamelCase`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCamelCaseDictionaryKeys.cs?name=SnippetSerialize)]

キーと値の `"ColdMinTemp", 20` ペアを持つ `TemperatureRanges` という名前のディクショナリを使用してオブジェクトをシリアル化すると `"HotMinTemp", 40`、次の例のように JSON 出力が生成されます。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "coldMinTemp": 20,
    "hotMinTemp": 40
  }
}
```

ディクショナリキーの camel ケースの名前付けポリシーは、シリアル化にのみ適用されます。 ディクショナリを逆シリアル化すると、`DictionaryKeyPolicy`に `JsonNamingPolicy.CamelCase` を指定した場合でも、キーが JSON ファイルと一致します。

### <a name="enums-as-strings"></a>列挙 (文字列として)

既定では、列挙型は数値としてシリアル化されます。 列挙型名を文字列としてシリアル化するには、<xref:System.Text.Json.Serialization.JsonStringEnumConverter>を使用します。

たとえば、列挙型を持つ次のクラスをシリアル化する必要があるとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithEnum)]

概要が `Hot`の場合、既定では、シリアル化された JSON の数値は3になります。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": 3
}
```

次のサンプルコードでは、数値ではなく列挙型名をシリアル化し、名前を camel 形式に変換します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripEnumAsString.cs?name=SnippetSerialize)]

結果として得られる JSON は、次の例のようになります。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "hot"
}
```

列挙文字列名も、次の例に示すように逆シリアル化できます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripEnumAsString.cs?name=SnippetDeserialize)]

## <a name="exclude-properties-from-serialization"></a>シリアル化からプロパティを除外する

既定では、すべてのパブリックプロパティがシリアル化されます。 一部のオプションが JSON 出力に表示されないようにするには、いくつかのオプションがあります。 ここでは、を除外する方法について説明します。

* [個々のプロパティ](#exclude-individual-properties)
* [すべての読み取り専用プロパティ](#exclude-all-read-only-properties)
* [すべての null 値プロパティ](#exclude-all-null-value-properties)

### <a name="exclude-individual-properties"></a>個々のプロパティを除外する

個々のプロパティを無視するには、 [[Jsonignore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute)属性を使用します。

シリアル化する型と JSON 出力の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithIgnoreAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
}
```

### <a name="exclude-all-read-only-properties"></a>すべての読み取り専用プロパティを除外する

パブリックゲッターが含まれていてもパブリック setter がない場合、プロパティは読み取り専用です。 すべての読み取り専用プロパティを除外するには、次の例に示すように、<xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyProperties?displayProperty=nameWithType> を `true`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeExcludeReadOnlyProperties.cs?name=SnippetSerialize)]

シリアル化する型と JSON 出力の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithROProperty)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

このオプションは、シリアル化にのみ適用されます。 逆シリアル化中、読み取り専用プロパティは既定では無視されます。

### <a name="exclude-all-null-value-properties"></a>すべての null 値プロパティを除外する

すべての null 値プロパティを除外するには、次の例に示すように、<xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues> プロパティを `true`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeExcludeNullValueProperties.cs?name=SnippetSerialize)]

シリアル化し、JSON 出力を行うオブジェクトの例を次に示します。

|プロパティ |値  |
|---------|---------|
| Date    | 8/1/2019 12:00:00 AM-07:00|
| TemperatureCelsius| 25 |
| まとめ| null|

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25
}
```

この設定は、シリアル化と逆シリアル化に適用されます。 逆シリアル化に対する影響については、「[逆シリアル化するときに null を無視する](#ignore-null-when-deserializing)」を参照してください。

## <a name="customize-character-encoding"></a>文字エンコードのカスタマイズ

既定では、シリアライザーは ASCII 以外のすべての文字をエスケープします。  つまり、`xxxx` が文字の Unicode コードである `\uxxxx` に置き換えます。  たとえば、`Summary` プロパティがキリルжаркоに設定されている場合、次の例に示すように `WeatherForecast` オブジェクトがシリアル化されます。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "\u0436\u0430\u0440\u043A\u043E"
}
```

### <a name="serialize-language-character-sets"></a>言語文字セットのシリアル化

エスケープせずに1つ以上の言語の文字セットをシリアル化するには、次の例に示すように、<xref:System.Text.Encodings.Web.JavaScriptEncoder?displayProperty=fullName>のインスタンスを作成するときに[Unicode 範囲](xref:System.Text.Unicode.UnicodeRanges)を指定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetLanguageSets)]

このコードでは、キリル文字やギリシャ文字はエスケープされません。 `Summary` プロパティがキリルжаркоに設定されている場合、次の例に示すように `WeatherForecast` オブジェクトがシリアル化されます。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жарко"
}
```

エスケープせずにすべての言語セットをシリアル化するには、<xref:System.Text.Unicode.UnicodeRanges.All?displayProperty=nameWithType>を使用します。

### <a name="serialize-specific-characters"></a>特定の文字のシリアル化

別の方法として、エスケープせずにを使用して許可する個々の文字を指定する方法もあります。 次の例では、жаркоの最初の2つの文字のみをシリアル化します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetSelectedCharacters)]

上記のコードによって生成される JSON の例を次に示します。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жа\u0440\u043A\u043E"
}
```

### <a name="serialize-all-characters"></a>すべての文字をシリアル化する

エスケープを最小限に抑えるには、次の例に示すように <xref:System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping?displayProperty=nameWithType>を使用します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializeCustomEncoding.cs?name=SnippetUnsafeRelaxed)]

> [!CAUTION]
> 既定のエンコーダーと比較して、`UnsafeRelaxedJsonEscaping` エンコーダーでは、エスケープ解除された文字を通過できるようにすることが制限されています。
>
> * `<`、`>`、`&`、`'`など、HTML に依存する文字はエスケープされません。
> * XSS または情報漏えい攻撃 (クライアントとサーバーが*文字セット*に対して無効な場合に発生する可能性があります) に対する追加の多層防御は提供されません。
>
> クライアントが結果のペイロードを UTF-8 でエンコードされた JSON として解釈することがわかっている場合にのみ、unsafe エンコーダーを使用します。 たとえば、サーバーが応答ヘッダー `Content-Type: application/json; charset=utf-8`を送信している場合に使用できます。 未加工の `UnsafeRelaxedJsonEscaping` 出力が HTML ページまたは `<script>` 要素に出力されないようにします。

## <a name="serialize-properties-of-derived-classes"></a>派生クラスのプロパティのシリアル化

ポリモーフィックな型階層のシリアル化はサポートされていません。 たとえば、プロパティがインターフェイスまたは抽象クラスとして定義されている場合、ランタイム型に追加のプロパティがある場合でも、インターフェイスまたは抽象クラスで定義されたプロパティのみがシリアル化されます。 この動作の例外については、このセクションで説明します。

たとえば、`WeatherForecast` クラスと派生クラス `WeatherForecastDerived`があるとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFDerived)]

また、コンパイル時に `Serialize` メソッドの型引数が `WeatherForecast`であるとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializePolymorphic.cs?name=SnippetSerializeDefault)]

このシナリオでは、`weatherForecast` オブジェクトが実際には `WeatherForecastDerived` オブジェクトであっても、`WindSpeed` プロパティはシリアル化されません。 基本クラスのプロパティのみがシリアル化されます。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

この動作は、派生したランタイムによって作成された型でデータが誤って公開されるのを防ぐためのものです。

前の例で派生型のプロパティをシリアル化するには、次のいずれかの方法を使用します。

* 実行時に型を指定できるようにする <xref:System.Text.Json.JsonSerializer.Serialize%2A> のオーバーロードを呼び出します。

  [!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializePolymorphic.cs?name=SnippetSerializeGetType)]

* `object`としてシリアル化するオブジェクトを宣言します。

  [!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializePolymorphic.cs?name=SnippetSerializeObject)]

前の例のシナリオでは、どちらの方法でも、`WindSpeed` プロパティが JSON 出力に含まれるようになっています。

```json
{
  "WindSpeed": 35,
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

> [!IMPORTANT]
> これらの方法では、ルートオブジェクトのプロパティではなく、シリアル化するルートオブジェクトに対してのみ、ポリモーフィックなシリアル化が提供されます。

`object`型として定義すると、下位レベルのオブジェクトのポリモーフィックなシリアル化を取得できます。 たとえば、`WeatherForecast` クラスに `PreviousForecast` という名前のプロパティがあり、`WeatherForecast` または `object`型として定義できるとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPrevious)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithPreviousAsObject)]

`PreviousForecast` プロパティに `WeatherForecastDerived`のインスタンスが含まれている場合は、次のようになります。

* `WeatherForecastWithPrevious` のシリアル化からの JSON 出力には `WindSpeed`**が含まれていません**。
* `WeatherForecastWithPreviousAsObject` をシリアル化するための JSON 出力には、`WindSpeed`**が含まれて**います。

`WeatherForecastWithPreviousAsObject`をシリアル化するには、ルートオブジェクトが派生型である可能性のあるものではないため、`Serialize<object>` または `GetType` を呼び出す必要はありません。 次のコード例では、`Serialize<object>` または `GetType`を呼び出しません。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializePolymorphic.cs?name=SnippetSerializeSecondLevel)]

上記のコードは、`WeatherForecastWithPreviousAsObject`を正しくシリアル化します。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "PreviousForecast": {
    "WindSpeed": 35,
    "Date": "2019-08-01T00:00:00-07:00",
    "TemperatureCelsius": 25,
    "Summary": "Hot"
  }
}
```

`object` としてプロパティを定義するのと同じアプローチが、インターフェイスで動作します。 次のインターフェイスと実装があり、実装インスタンスを含むプロパティを使用してクラスをシリアル化するとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/IForecast.cs)]

`Forecasts`のインスタンスをシリアル化する場合、`Tuesday` は `object`として定義されているので、`Tuesday` のみが `WindSpeed` プロパティを表示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/SerializePolymorphic.cs?name=SnippetSerializeInterface)]

次の例は、上記のコードの結果として得られる JSON を示しています。

```json
{
  "Monday": {
    "Date": "2020-01-06T00:00:00-08:00",
    "TemperatureCelsius": 10,
    "Summary": "Cool"
  },
  "Tuesday": {
    "Date": "2020-01-07T00:00:00-08:00",
    "TemperatureCelsius": 11,
    "Summary": "Rainy",
    "WindSpeed": 10
  }
}
```

ポリモーフィックな**シリアル化**の詳細、および**逆シリアル**化の詳細については、「 [newtonsoft. json から system.string に移行する方法](system-text-json-migrate-from-newtonsoft-how-to.md#polymorphic-serialization)」を参照してください。

## <a name="allow-comments-and-trailing-commas"></a>コメントと末尾のコンマを許可する

JSON では、コメントと末尾のコンマは既定で許可されていません。 JSON でコメントを許可するには、<xref:System.Text.Json.JsonSerializerOptions.ReadCommentHandling?displayProperty=nameWithType> プロパティを `JsonCommentHandling.Skip`に設定します。
また、末尾のコンマを許可するには、<xref:System.Text.Json.JsonSerializerOptions.AllowTrailingCommas?displayProperty=nameWithType> プロパティを `true`に設定します。 次の例では、両方を許可する方法を示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DeserializeCommasComments.cs?name=SnippetDeserialize)]

次に、コメントと末尾のコンマを含む JSON の例を示します。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25, // Fahrenheit 77
  "Summary": "Hot", /* Zharko */
}
```

## <a name="case-insensitive-property-matching"></a>大文字と小文字を区別しないプロパティ照合

既定では、逆シリアル化では、JSON とターゲットオブジェクトのプロパティの間で大文字と小文字が区別されるプロパティ名の一致が検索されます。 この動作を変更するには、<xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive?displayProperty=nameWithType> を `true`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DeserializeCaseInsensitive.cs?name=SnippetDeserialize)]

Camel 形式のプロパティ名を持つ JSON の例を次に示します。 Pascal 形式のプロパティ名を持つ次の型に逆シリアル化できます。

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
}
```

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

## <a name="handle-overflow-json"></a>Overflow JSON の処理

逆シリアル化中に、対象の型のプロパティで表されないデータを JSON で受け取ることがあります。 たとえば、対象の型が次のようになっているとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

逆シリアル化される JSON は次のとおりです。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

示されている型に表示されている JSON を逆シリアル化すると、`DatesAvailable` と `SummaryWords` のプロパティがなくなり、失われます。 これらのプロパティなどの追加データをキャプチャするには、 [Jsonextensiondata](xref:System.Text.Json.Serialization.JsonExtensionDataAttribute)属性を `Dictionary<string,object>` 型または `Dictionary<string,JsonElement>`型のプロパティに適用します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithExtensionData)]

前に示した JSON をこのサンプル型に逆シリアル化すると、余分なデータが `ExtensionData` プロパティのキーと値のペアになります。

|プロパティ |値  |メモ  |
|---------|---------|---------|
| Date    | 8/1/2019 12:00:00 AM-07:00||
| TemperatureCelsius| 0 | 大文字と小文字が区別されない (JSON で`temperatureCelsius`) ため、プロパティが設定されていません。 |
| まとめ | ホット ||
| ExtensionData | temperatureCelsius:25 |大文字と小文字が一致しなかったため、この JSON プロパティは余分で、ディクショナリ内のキーと値のペアになります。|
|| 使用可能な日:<br>  8/1/2019 12:00:00 AM-07:00<br>8/2/2019 12:00:00 AM-07:00 |JSON からの追加のプロパティはキーと値のペアになり、値オブジェクトとして配列が使用されます。|
| |概要語:<br>クール<br>強風<br>Humid |JSON からの追加のプロパティはキーと値のペアになり、値オブジェクトとして配列が使用されます。|

ターゲットオブジェクトがシリアル化されると、拡張データのキーと値のペアは、受信 JSON の場合と同様に JSON プロパティになります。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 0,
  "Summary": "Hot",
  "temperatureCelsius": 25,
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

JSON には `ExtensionData` プロパティ名が表示されないことに注意してください。 この動作により、JSON は、逆シリアル化されない余分なデータを失うことなく、ラウンドトリップを行うことができます。

## <a name="ignore-null-when-deserializing"></a>逆シリアル化時に null を無視する

既定では、JSON のプロパティが null の場合、対象オブジェクト内の対応するプロパティは null に設定されます。 場合によっては、ターゲットプロパティに既定値が設定されていて、null 値を使用して既定値をオーバーライドしないことがあります。

たとえば、次のコードがターゲットオブジェクトを表しているとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithDefault)]

次の JSON が逆シリアル化されたとします。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": null
}
```

逆シリアル化後、`WeatherForecastWithDefault` オブジェクトの `Summary` プロパティは null になります。

この動作を変更するには、次の例に示すように、<xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType> を `true`に設定します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DeserializeIgnoreNull.cs?name=SnippetDeserialize)]

このオプションを使用すると、逆シリアル化後に、`WeatherForecastWithDefault` オブジェクトの `Summary` プロパティが既定値の "No summary" になります。

JSON 内の Null 値は、有効な場合にのみ無視されます。 Null 非許容値型に対して Null 値を指定すると、例外が発生します。

## <a name="utf8jsonreader-utf8jsonwriter-and-jsondocument"></a>Utf8JsonReader、Utf8JsonWriter、JsonDocument

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> は、UTF-8 でエンコードされた JSON テキストの高パフォーマンス、低割り当て、順方向専用のリーダーであり、`ReadOnlySpan<byte>` または `ReadOnlySequence<byte>`から読み取ります。 `Utf8JsonReader` は、カスタムパーサーとデシリアライザーを構築するために使用できる低レベルの型です。 <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> メソッドは、内部で `Utf8JsonReader` を使用します。

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> は、`String`、`Int32`、`DateTime`などの一般的な .NET 型から、UTF-8 でエンコードされた JSON テキストを書き込むための高パフォーマンスの方法です。 ライターは、カスタムシリアライザーを構築するために使用できる低レベルの型です。 <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> メソッドは、内部で `Utf8JsonWriter` を使用します。

<xref:System.Text.Json.JsonDocument?displayProperty=fullName> は、`Utf8JsonReader`を使用して、読み取り専用ドキュメントオブジェクトモデル (DOM) を構築する機能を提供します。 DOM は、JSON ペイロード内のデータへのランダムアクセスを提供します。 ペイロードを構成する JSON 要素には、<xref:System.Text.Json.JsonElement> の種類を使用してアクセスできます。 `JsonElement` 型は、JSON テキストを共通の .NET 型に変換するための Api と共に配列とオブジェクトの列挙子を提供します。 `JsonDocument` は <xref:System.Text.Json.JsonDocument.RootElement> プロパティを公開します。

以下のセクションでは、これらのツールを使用して JSON の読み取りと書き込みを行う方法について説明します。

## <a name="use-jsondocument-for-access-to-data"></a>データへのアクセスに JsonDocument を使用する

次の例は、JSON 文字列内のデータへのランダムアクセスに <xref:System.Text.Json.JsonDocument> クラスを使用する方法を示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades1)]

上のコードでは以下の操作が行われます。

* は、分析する JSON が `jsonString`という名前の文字列に含まれていると想定します。
* `Grade` プロパティを持つ `Students` 配列内のオブジェクトの平均グレードを計算します。
* 学年のない学生に対して既定のグレード70を割り当てます。
* 各イテレーションで `count` 変数をインクリメントして生徒をカウントします。 別の方法として、次の例に示すように <xref:System.Text.Json.JsonElement.GetArrayLength%2A>を呼び出すこともできます。

  [!code-csharp[](~/samples/snippets/core/system-text-json/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades2)]

このコードで処理される JSON の例を次に示します。

[!code-json[](~/samples/snippets/core/system-text-json/csharp/GradesPrettyPrint.json)]

## <a name="use-jsondocument-to-write-json"></a>JsonDocument を使用した JSON の記述

次の例は、<xref:System.Text.Json.JsonDocument>から JSON を書き込む方法を示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/JsonDocumentWriteJson.cs?name=SnippetSerialize)]

上のコードでは以下の操作が行われます。

* JSON ファイルを読み取り、データを `JsonDocument`に読み込み、書式設定された (非常に印刷された) JSON をファイルに書き込みます。
* <xref:System.Text.Json.JsonDocumentOptions> を使用して、入力 JSON 内のコメントを許可しますが、無視することを指定します。
* 完了すると、はライターで <xref:System.Text.Json.Utf8JsonWriter.Flush%2A> を呼び出します。 別の方法として、破棄されたときにライターを autoflush することもできます。

コード例によって処理される JSON 入力の例を次に示します。

[!code-json[](~/samples/snippets/core/system-text-json/csharp/Grades.json)]

その結果、次のような整形出力の JSON 出力が生成されます。

[!code-json[](~/samples/snippets/core/system-text-json/csharp/GradesPrettyPrint.json)]

## <a name="use-utf8jsonwriter"></a>Utf8JsonWriter を使用する

次の例は、<xref:System.Text.Json.Utf8JsonWriter> クラスの使用方法を示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Utf8WriterToStream.cs?name=SnippetSerialize)]

## <a name="use-utf8jsonreader"></a>Utf8JsonReader を使用する

次の例は、<xref:System.Text.Json.Utf8JsonReader> クラスの使用方法を示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Utf8ReaderFromBytes.cs?name=SnippetDeserialize)]

上記のコードでは、`jsonUtf8` 変数が、UTF-8 としてエンコードされた有効な JSON を含むバイト配列であることを前提としています。

### <a name="filter-data-using-utf8jsonreader"></a>Utf8JsonReader を使用してデータをフィルター処理する

次の例は、ファイルを同期的に読み取り、値を検索する方法を示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Utf8ReaderFromFile.cs)]

上のコードでは以下の操作が行われます。

* JSON にオブジェクトの配列が含まれており、各オブジェクトに文字列型の "name" プロパティが含まれていると仮定します。
* "大学" で終わるオブジェクトと "name" プロパティ値をカウントします。
* は、ファイルが UTF-16 としてエンコードされているものと想定し、UTF-8 にトランスコードします。 UTF-8 としてエンコードされたファイルは、次のコードを使用して、`ReadOnlySpan<byte>`に直接読み取ることができます。

  ```csharp
  ReadOnlySpan<byte> jsonReadOnlySpan = File.ReadAllBytes(fileName);
  ```

  ファイルに UTF-8 バイト順マーク (BOM) が含まれている場合は、リーダーがテキストを受け取るため、バイトを `Utf8JsonReader`に渡す前に削除します。 それ以外の場合、BOM は無効な JSON と見なされ、リーダーは例外をスローします。

上記のコードが読み取ることができる JSON のサンプルを次に示します。 結果として生成される概要メッセージは、"2 ~ 4 個の名前が ' 大学 ' で終わっています" です。

[!code-json[](~/samples/snippets/core/system-text-json/csharp/Universities.json)]

## <a name="additional-resources"></a>その他のリソース

* [System.Text.Json の概要](system-text-json-overview.md)
* [カスタムコンバーターを記述する方法](system-text-json-converters-how-to.md)
* [Newtonsoft.Json から移行する方法](system-text-json-migrate-from-newtonsoft-how-to.md)
* [System.Text.Json での DateTime と DateTimeOffset のサポート](../datetime/system-text-json-support.md)
* [System.Text.Json API リファレンス](xref:System.Text.Json)
<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)-->
