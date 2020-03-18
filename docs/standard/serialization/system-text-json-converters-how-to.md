---
title: JSON シリアル化のカスタムコンバーターを記述する方法-.NET
ms.date: 01/10/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
- converters
ms.openlocfilehash: 310967f39c3aa7a46d79087bcbf0cb016f7d7284
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159573"
---
# <a name="how-to-write-custom-converters-for-json-serialization-marshalling-in-net"></a>.NET で JSON シリアル化 (マーシャリング) のカスタムコンバーターを記述する方法

この記事では、<xref:[!OP.NO-LOC(System.Text.Json)]> 名前空間に用意されている JSON シリアル化クラスのカスタムコンバーターを作成する方法について説明します。 `[!OP.NO-LOC(System.Text.Json)]`の概要については、「 [.net で JSON をシリアル化および逆シリアル化する方法](system-text-json-how-to.md)」を参照してください。

*コンバーター*は、オブジェクトまたは値を JSON との間で変換するクラスです。 `[!OP.NO-LOC(System.Text.Json)]` 名前空間には、JavaScript プリミティブにマップされるほとんどのプリミティブ型用の組み込みのコンバーターがあります。 カスタムコンバーターは、次のように記述できます。

* 組み込みのコンバーターの既定の動作をオーバーライドする場合は。 たとえば、既定の ISO 8601-1:2019 形式ではなく、`DateTime` 値を mm/dd/yyyy 形式で表すようにすることができます。
* カスタム値型をサポートする場合は。 たとえば、`PhoneNumber` 構造体などです。

また、カスタムコンバーターを作成して、現在のリリースに含まれていない機能を使用して `[!OP.NO-LOC(System.Text.Json)]` をカスタマイズまたは拡張することもできます。 この記事の後半では、次のシナリオについて説明します。

* 推論された[型をオブジェクトのプロパティに逆シリアル](#deserialize-inferred-types-to-object-properties)化します。
* [文字列以外のキーを使用した辞書をサポート](#support-dictionary-with-non-string-key)します。
* [ポリモーフィック逆シリアル化をサポート](#support-polymorphic-deserialization)します。

## <a name="custom-converter-patterns"></a>カスタムコンバーターパターン

カスタムコンバーターを作成するには、基本パターンとファクトリパターンという2つのパターンがあります。 ファクトリパターンは、型 `Enum` またはオープンジェネリックを処理するコンバーター用です。 基本パターンは、非ジェネリックおよびクローズジェネリック型用です。  たとえば、次の型のコンバーターには、ファクトリパターンが必要です。

* `Dictionary<TKey, TValue>`
* `Enum`
* `List<T>`

基本的なパターンによって処理できる型の例を次に示します。

* `Dictionary<int, string>`
* `WeekdaysEnum`
* `List<DateTimeOffset>`
* `DateTime`
* `Int32`

基本パターンでは、1つの型を処理できるクラスが作成されます。 ファクトリパターンは、実行時に特定の型が必要であることを判断し、適切なコンバーターを動的に作成するクラスを作成します。

## <a name="sample-basic-converter"></a>基本的なコンバーターのサンプル

次のサンプルは、既存のデータ型の既定のシリアル化をオーバーライドするコンバーターです。 コンバーターでは、`DateTimeOffset` プロパティに mm/dd/yyyy 形式が使用されます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DateTimeOffsetConverter.cs)]

## <a name="sample-factory-pattern-converter"></a>サンプルファクトリパターンコンバーター

次のコードは、`Dictionary<Enum,TValue>`で動作するカスタムコンバーターを示しています。 このコードは、最初のジェネリック型パラメーターが `Enum`、2番目のジェネリック型パラメーターが開いているため、ファクトリパターンに従います。 `CanConvert` メソッドは、2つのジェネリックパラメーターを持つ `Dictionary` に対してのみ `true` を返します。最初のパラメーターは `Enum` 型です。 内部コンバーターは、`TValue`に対して実行時に提供されるいずれかの型を処理する既存のコンバーターを取得します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DictionaryTKeyEnumTValueConverter.cs)]

上記のコードは、この記事の後半で説明する[文字列以外のキーを使用したサポート辞書](#support-dictionary-with-non-string-key)に示されているものと同じです。

## <a name="steps-to-follow-the-basic-pattern"></a>基本的なパターンに従う手順

次の手順では、基本的なパターンに従ってコンバーターを作成する方法について説明します。

* `T` がシリアル化および逆シリアル化される型である <xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverter%601> から派生するクラスを作成します。
* `Read` メソッドをオーバーライドして受信 JSON を逆シリアル化し、型 `T`に変換します。 メソッドに渡された <xref:[!OP.NO-LOC(System.Text.Json)].Utf8JsonReader> を使用して、JSON を読み取ります。
* `Write` メソッドをオーバーライドして、`T`型の受信オブジェクトをシリアル化します。 メソッドに渡された <xref:[!OP.NO-LOC(System.Text.Json)].Utf8JsonWriter> を使用して JSON を記述します。
* `CanConvert` メソッドは、必要な場合にのみオーバーライドします。 変換する型が `T`型である場合、既定の実装は `true` を返します。 したがって、型 `T` だけをサポートするコンバーターは、このメソッドをオーバーライドする必要はありません。 このメソッドをオーバーライドする必要があるコンバーターの例については、この記事で後述する「[ポリモーフィックな逆シリアル化](#support-polymorphic-deserialization)」を参照してください。

[組み込みのコンバーターソースコード](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/[!OP.NO-LOC(System.Text.Json)]/src/[!OP.NO-LOC(System/Text/Json)]/Serialization/Converters/)は、カスタムコンバーターを作成するための参照の実装として参照できます。

## <a name="steps-to-follow-the-factory-pattern"></a>ファクトリパターンに従う手順

次の手順では、ファクトリパターンに従ってコンバーターを作成する方法について説明します。

* <xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterFactory> から派生するクラスを作成します。
* 変換する型が、コンバーターが処理できる型である場合に true を返すように `CanConvert` メソッドをオーバーライドします。 たとえば、コンバーターが `List<T>` 用の場合、`List<int>`、`List<string>`、および `List<DateTime>`だけを処理できます。
* `CreateConverter` メソッドをオーバーライドして、実行時に提供される変換の種類を処理するコンバータークラスのインスタンスを返します。
* `CreateConverter` メソッドによってインスタンス化されるコンバータークラスを作成します。

オブジェクトを文字列との間で変換するコードはすべての型で同じではないため、オープンジェネリックにはファクトリパターンが必要です。 オープンジェネリック型 (`List<T>`など) のコンバーターは、背後にあるクローズジェネリック型 (`List<DateTime>`など) のコンバーターを作成する必要があります。 コードは、コンバーターが処理できる各閉じられたジェネリック型を処理するように記述する必要があります。

`Enum` 型はオープンジェネリック型に似ています。 `Enum` のコンバーターは、シーンの背後にある特定の `Enum` (`WeekdaysEnum`など) のコンバーターを作成する必要があります。

## <a name="error-handling"></a>エラー処理

エラー処理コードで例外をスローする必要がある場合は、メッセージを使用せずに <xref:[!OP.NO-LOC(System.Text.Json)].JsonException> をスローすることを検討してください。 この例外の種類では、エラーの原因となった JSON の部分へのパスを含むメッセージが自動的に作成されます。 たとえば、ステートメント `throw new JsonException();` では、次の例のようなエラーメッセージが生成されます。

```
Unhandled exception. [!OP.NO-LOC(System.Text.Json)].JsonException:
The JSON value could not be converted to System.Object.
Path: $.Date | LineNumber: 1 | BytePositionInLine: 37.
```

メッセージ (`throw new JsonException("Error occurred")`など) を指定した場合でも、例外は <xref:[!OP.NO-LOC(System.Text.Json)].JsonException.Path> プロパティのパスを提供します。

## <a name="register-a-custom-converter"></a>カスタムコンバーターを登録する

カスタムコンバーターを*登録*して、`Serialize` および `Deserialize` メソッドで使用されるようにします。 次のいずれかの方法を選択します。

* コンバータークラスのインスタンスを <xref:[!OP.NO-LOC(System.Text.Json)].JsonSerializerOptions.Converters?displayProperty=nameWithType> コレクションに追加します。
* カスタムコンバーターを必要とするプロパティに[[jsonconverter]](xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterAttribute)属性を適用します。
* [[Jsonconverter]](xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterAttribute)属性をカスタム値型を表すクラスまたは構造体に適用します。

## <a name="registration-sample---converters-collection"></a>登録サンプル-コンバーターコレクション

<xref:System.DateTimeOffset>型のプロパティの既定値 <xref:System.ComponentModel.DateTimeOffsetConverter> する例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithConvertersCollection.cs?name=SnippetSerialize)]

次の型のインスタンスをシリアル化するとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

カスタムコンバーターが使用されたことを示す JSON 出力の例を次に示します。

```json
{
  "Date": "08/01/2019",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

次のコードでは、カスタム `DateTimeOffset` コンバーターを使用して逆シリアル化するのと同じアプローチを使用します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithConvertersCollection.cs?name=SnippetDeserialize)]

## <a name="registration-sample---jsonconverter-on-a-property"></a>登録のサンプル-[JsonConverter] プロパティ

次のコードでは、`Date` プロパティのカスタムコンバーターを選択します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithConverterAttribute)]

`WeatherForecastWithConverterAttribute` をシリアル化するコードでは、`JsonSerializeOptions.Converters`を使用する必要はありません。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithAttributeOnProperty.cs?name=SnippetSerialize)]

逆シリアル化するコードでは、`Converters`を使用する必要もありません。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithAttributeOnProperty.cs?name=SnippetDeserialize)]

## <a name="registration-sample---jsonconverter-on-a-type"></a>登録サンプル-[JsonConverter] 型

次に、構造体を作成し、それに `[JsonConverter]` 属性を適用するコードを示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Temperature.cs)]

前の構造体のカスタムコンバーターは次のようになります。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/TemperatureConverter.cs)]

構造体の `[JsonConvert]` 属性は、`Temperature`型のプロパティの既定値としてカスタムコンバーターを登録します。 コンバーターは、シリアル化または逆シリアル化するときに、次の型の `TemperatureCelsius` プロパティで自動的に使用されます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithTemperatureStruct)]

## <a name="converter-registration-precedence"></a>コンバーターの登録の優先順位

シリアル化または逆シリアル化の際には、次の順序で JSON 要素ごとにコンバーターが選択され、最も高い優先度から順に表示されます。

* `[JsonConverter]` プロパティに適用されます。
* `Converters` コレクションに追加されたコンバーター。
* `[JsonConverter]` カスタム値型または POCO に適用されます。

1つの型に対して複数のカスタムコンバーターが `Converters` コレクションに登録されている場合、`CanConvert` に対して true を返す最初のコンバーターが使用されます。

組み込みのコンバーターは、適用可能なカスタムコンバーターが登録されていない場合にのみ選択されます。

## <a name="converter-samples-for-common-scenarios"></a>一般的なシナリオのコンバーターのサンプル

以下のセクションでは、組み込み機能で処理されない一般的なシナリオに対処するコンバーターのサンプルについて説明します。

* [推論された型のオブジェクトプロパティへの逆シリアル化](#deserialize-inferred-types-to-object-properties)
* [文字列以外のキーを使用したサポート辞書](#support-dictionary-with-non-string-key)
* [ポリモーフィック逆シリアル化のサポート](#support-polymorphic-deserialization)

### <a name="deserialize-inferred-types-to-object-properties"></a>推論された型のオブジェクトプロパティへの逆シリアル化

`object`型のプロパティに逆シリアル化すると、`JsonElement` オブジェクトが作成されます。 その理由は、デシリアライザーは、作成する CLR 型を認識しないため、推測しようとしません。 たとえば、JSON プロパティに "true" が含まれている場合、デシリアライザーは、値が `Boolean`であることを推論しません。また、要素に "01/01/2019" がある場合、デシリアライザーは `DateTime`であることを推論しません。

型の推定は不正確になる可能性があります。 デシリアライザーが、小数点のない JSON 番号を `long`として解析する場合、値が最初に `ulong` または `BigInteger`としてシリアル化された場合、範囲外の問題が発生する可能性があります。 数値が最初に `decimal`としてシリアル化された場合、`double` として小数点がある数値を解析すると、精度が失われる可能性があります。

型の推定が必要なシナリオでは、次のコードは `object` プロパティのカスタムコンバーターを示しています。 コードは次のように変換します。

* `true` と `false` `Boolean`
* `long` に10進数を含まない数値
* `double` に10進数が含まれる数値
* `DateTime` する日付
* `string` する文字列
* 他のすべての `JsonElement`

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ObjectToInferredTypesConverter.cs)]

次のコードは、コンバーターを登録します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DeserializeInferredTypesToObject.cs?name=SnippetRegister)]

`object` プロパティを持つ型の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithObjectProperties)]

次の JSON の例には、`DateTime`、`long`、および `string`として逆シリアル化される値が含まれています。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

カスタムコンバーターを使用しない場合、逆シリアル化によって各プロパティに `JsonElement` が挿入されます。

`System.Text.Json.Serialization` 名前空間の[単体テストフォルダー](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)には、`object` プロパティへの逆シリアル化を処理するカスタムコンバーターの例が多数あります。

### <a name="support-dictionary-with-non-string-key"></a>文字列以外のキーを使用したサポート辞書

ディクショナリコレクションの組み込みサポートは `Dictionary<string, TValue>`用です。 つまり、キーは文字列である必要があります。 キーとして整数またはその他の型を持つディクショナリをサポートするには、カスタムコンバーターが必要です。

次のコードは、`Dictionary<Enum,TValue>`で動作するカスタムコンバーターを示しています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DictionaryTKeyEnumTValueConverter.cs)]

次のコードは、コンバーターを登録します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripDictionaryTkeyEnumTValue.cs?name=SnippetRegister)]

コンバーターは、次の `Enum`を使用する次のクラスの `TemperatureRanges` プロパティをシリアル化および逆シリアル化できます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithEnumDictionary)]

シリアル化からの JSON 出力は、次の例のようになります。

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "Cold": 20,
    "Hot": 40
  }
}
```

`System.Text.Json.Serialization` 名前空間の[単体テストフォルダー](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)には、文字列キー以外のディクショナリを処理するカスタムコンバーターの例が多数あります。

### <a name="support-polymorphic-deserialization"></a>ポリモーフィック逆シリアル化のサポート

組み込み機能は、[ポリモーフィックなシリアル化](system-text-json-how-to.md#serialize-properties-of-derived-classes)の範囲を制限しますが、逆シリアル化はサポートされていません。 逆シリアル化を行うには、カスタムコンバーターが必要です。

たとえば、`Person` 抽象基本クラスがあり、`Employee` および `Customer` 派生クラスがあるとします。 ポリモーフィックな逆シリアル化とは、デザイン時に `Person` を逆シリアル化ターゲットとして指定できること、および JSON 内の `Customer` オブジェクトと `Employee` オブジェクトが実行時に正しく逆シリアル化されることを意味します。 逆シリアル化中に、JSON で必要な型を識別する手掛かりを見つける必要があります。 使用できる手掛かりの種類は、シナリオによって異なります。 たとえば、識別子のプロパティを使用できる場合や、特定のプロパティの有無に依存する場合があります。 `System.Text.Json` の現在のリリースでは、ポリモーフィックな逆シリアル化のシナリオを処理する方法を指定する属性が提供されないため、カスタムコンバーターが必要になります。

次のコードは、基底クラス、2つの派生クラス、およびそれらのカスタムコンバーターを示しています。 コンバーターは、ポリモーフィックな逆シリアル化を行うために識別子プロパティを使用します。 型識別子はクラス定義に含まれていませんが、シリアル化中に作成され、逆シリアル化中に読み取られます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Person.cs?name=SnippetPerson)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/PersonConverterWithTypeDiscriminator.cs)]

次のコードは、コンバーターを登録します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripPolymorphic.cs?name=SnippetRegister)]

コンバーターは、同じコンバーターを使用して作成された JSON を逆シリアル化できます。たとえば、次のようになります。

```json
[
  {
    "TypeDiscriminator": 1,
    "CreditLimit": 10000,
    "Name": "John"
  },
  {
    "TypeDiscriminator": 2,
    "OfficeNumber": "555-1234",
    "Name": "Nancy"
  }
]
```

前の例のコンバーターコードは、各プロパティを手動で読み取り、書き込みます。 別の方法として、`Deserialize` または `Serialize` を呼び出して、一部の作業を行うことができます。 例については、[この StackOverflow の投稿](https://stackoverflow.com/a/59744873/12509023)を参照してください。

## <a name="other-custom-converter-samples"></a>その他のカスタムコンバーターのサンプル

[Newtonsoft.Json から System.Text.Jsonへの移行に](system-text-json-migrate-from-newtonsoft-how-to.md)関する記事には、カスタムコンバーターの追加のサンプルが含まれています。

`System.Text.Json.Serialization` ソースコードの[単体テストフォルダー](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)には、次のような他のカスタムコンバーターのサンプルが含まれています。

* [逆シリアル化時に null を0に変換する Int32 コンバーター](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.NullValueType.cs)
* [逆シリアル化時に文字列と数値の両方を使用できる Int32 コンバーター](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Int32.cs)
* [列挙型コンバーター](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Enum.cs)
* [外部データを受け入れる\<T > コンバーターの一覧表示](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.List.cs)
* [コンマで区切られた数値のリストを処理する Long [] コンバーター](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Array.cs)

既存の組み込みコンバーターの動作を変更するコンバーターを作成する必要がある場合は、[既存のコンバーターのソースコード](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters)を取得して、カスタマイズの開始点として機能させることができます。

## <a name="additional-resources"></a>その他のリソース

* [組み込みのコンバーターのソースコード](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters)
* [System.Text.Json での DateTime と DateTimeOffset のサポート](../datetime/system-text-json-support.md)
* [System.Text.Json の概要](system-text-json-overview.md)
* [System.Text.Json の使用方法](system-text-json-how-to.md)
* [Newtonsoft.Json から移行する方法](system-text-json-migrate-from-newtonsoft-how-to.md)
* [System.Text.Json API リファレンス](xref:System.Text.Json)
* [System.Text.Json。シリアル化 API リファレンス](xref:System.Text.Json.Serialization)
<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)-->
