---
title: Newtonsoft.Json から System.Text.Json への移行
author: tdykstra
ms.author: tdykstra
no-loc:
- System.Text.Json
- Newtonsoft.Json
ms.date: 01/10/2020
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: e0a6912c10baa0be4a8ef9f6536948ae27f235c7
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159560"
---
# <a name="how-to-migrate-from-newtonsoftjson-to-systemtextjson"></a>Newtonsoft. Json から system.string に移行する方法

この記事では、 [Newtonsoft. Json](https://www.newtonsoft.com/json)から <xref:System.Text.Json>に移行する方法について説明します。

`System.Text.Json` は、主にパフォーマンス、セキュリティ、および標準への準拠に焦点を当てています。 既定の動作にはいくつかの重要な違いがあり、`Newtonsoft.Json`との機能の同等性はありません。 一部のシナリオでは、`System.Text.Json` に組み込まれている機能はありませんが、推奨される回避策があります。 他のシナリオでは、回避策は現実的ではありません。 アプリケーションが不足している機能に依存している場合は、[問題](https://github.com/dotnet/runtime/issues/new)を報告して、シナリオのサポートが追加されるかどうかを確認することを検討してください。

<!-- For information about which features might be added in future releases, see the [Roadmap](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md). [Restore this when the roadmap is updated.]-->

この記事では、<xref:System.Text.Json.JsonSerializer> API の使用方法について説明しますが、<xref:System.Text.Json.JsonDocument> (ドキュメントオブジェクトモデルまたは DOM を表す)、<xref:System.Text.Json.Utf8JsonReader>、<xref:System.Text.Json.Utf8JsonWriter> の型の使用方法に関するガイダンスも含まれています。

## <a name="table-of-differences-between-newtonsoftjson-and-systemtextjson"></a>Newtonsoft. Json と system.string の違いの表

次の表に、`Newtonsoft.Json` の機能と同等の `System.Text.Json` を示します。 同等のものは、次のカテゴリに分類されます。

* 組み込み機能でサポートされています。 `System.Text.Json` と同様の動作を得るには、属性またはグローバルオプションの使用が必要になることがあります。
* サポートされていません。回避策があります。 この回避策は、`Newtonsoft.Json` の機能に完全なパリティを提供しない可能性がある[カスタムコンバーター](system-text-json-converters-how-to.md)です。 これらの一部については、例としてサンプルコードを示しています。 これらの `Newtonsoft.Json` 機能に依存している場合、移行には .NET オブジェクトモデルまたはその他のコード変更を変更する必要があります。
* サポートされていません。回避策は実用的でも可能でもありません。 これらの `Newtonsoft.Json` 機能に依存している場合は、大幅な変更を行わずに移行することはできません。

| Newtonsoft. Json 機能                               | 対応する system.string |
|-------------------------------------------------------|-----------------------------|
| 既定による大文字と小文字を区別しない逆シリアル化           | [Propertynamecaseinsensitive グローバル設定](#case-insensitive-deserialization)を✔️ |
| Camel 形式のプロパティ名                             | [Propertynamingpolicy グローバル設定](system-text-json-how-to.md#use-camel-case-for-all-json-property-names)の✔️ |
| 最小文字エスケープ                            | [厳密な文字のエスケープを✔️、構成可能](#minimal-character-escaping) |
| `NullValueHandling.Ignore` グローバル設定             | ✔️ [Ignorenullvalues グローバルオプション](system-text-json-how-to.md#exclude-all-null-value-properties) |
| コメントを許可する                                        | ✔️ [ReadCommentHandling グローバル設定](#comments) |
| 末尾のコンマを許可する                                 | ✔️ [AllowTrailingCommas グローバル設定](#trailing-commas) |
| カスタムコンバーターの登録                         | [優先順位の✔️順序が異なります](#converter-registration-precedence) |
| 既定では最大の深さはありません                           | ✔️[既定の最大深度64、構成可能](#maximum-depth) |
| さまざまな種類のサポート                    | [一部の型 ⚠️ カスタムコンバーターが必要です](#types-without-built-in-support) |
| 文字列を数値として逆シリアル化する                        | ⚠️[サポートされていません。回避策、サンプル](#quoted-numbers) |
| 文字列以外のキーを使用した `Dictionary` の逆シリアル化          | ⚠️[サポートされていません。回避策、サンプル](#dictionary-with-non-string-key) |
| ポリモーフィックシリアル化                             | ⚠️[サポートされていません。回避策、サンプル](#polymorphic-serialization) |
| ポリモーフィック逆シリアル化                           | ⚠️[サポートされていません。回避策、サンプル](#polymorphic-deserialization) |
| 推論された型を `object` プロパティに逆シリアル化する      | ⚠️[サポートされていません。回避策、サンプル](#deserialization-of-object-properties) |
| JSON `null` リテラルを null 非許容型に逆シリアル化する | ⚠️[サポートされていません。回避策、サンプル](#deserialize-null-to-non-nullable-type) |
| 変更できないクラスと構造体への逆シリアル化          | ⚠️[サポートされていません。回避策、サンプル](#deserialize-to-immutable-classes-and-structs) |
| `[JsonConstructor]` 属性                         | ⚠️[サポートされていません。回避策、サンプル](#specify-constructor-to-use) |
| `[JsonProperty]` 属性の `Required` 設定        | ⚠️[サポートされていません。回避策、サンプル](#required-properties) |
| `[JsonProperty]` 属性の `NullValueHandling` 設定 | ⚠️[サポートされていません。回避策、サンプル](#conditionally-ignore-a-property)  |
| `[JsonProperty]` 属性の `DefaultValueHandling` 設定 | ⚠️[サポートされていません。回避策、サンプル](#conditionally-ignore-a-property)  |
| `DefaultValueHandling` グローバル設定                 | ⚠️[サポートされていません。回避策、サンプル](#conditionally-ignore-a-property) |
| プロパティを除外する `DefaultContractResolver`       | ⚠️[サポートされていません。回避策、サンプル](#conditionally-ignore-a-property) |
| `DateTimeZoneHandling`、`DateFormatString` 設定   | ⚠️[サポートされていません。回避策、サンプル](#specify-date-format) |
| 関数                                             | ⚠️[サポートされていません。回避策、サンプル](#callbacks) |
| パブリックフィールドと非パブリックフィールドのサポート              | ⚠️[サポートされていません。回避策](#public-and-non-public-fields) |
| 内部およびプライベートのプロパティ setter と getter のサポート | ⚠️[サポートされていません。回避策](#internal-and-private-property-setters-and-getters) |
| `JsonConvert.PopulateObject` メソッド                   | ⚠️[サポートされていません。回避策](#populate-existing-objects) |
| `ObjectCreationHandling` グローバル設定               | ⚠️[サポートされていません。回避策](#reuse-rather-than-replace-properties) |
| Setter を使用せずにコレクションに追加する                    | ⚠️[サポートされていません。回避策](#add-to-collections-without-setters) |
| `PreserveReferencesHandling` グローバル設定           | ❌[サポートされていません](#preserve-object-references-and-handle-loops) |
| `ReferenceLoopHandling` グローバル設定                | ❌[サポートされていません](#preserve-object-references-and-handle-loops) |
| `System.Runtime.Serialization` 属性のサポート | ❌[サポートされていません](#systemruntimeserialization-attributes) |
| `MissingMemberHandling` グローバル設定                | ❌[サポートされていません](#missingmemberhandling) |
| 引用符なしでプロパティ名を許可する                   | ❌[サポートされていません](#json-strings-property-names-and-string-values) |
| 文字列値の前後に単一引用符を使用する              | ❌[サポートされていません](#json-strings-property-names-and-string-values) |
| 文字列プロパティに文字列以外の JSON 値を許可する    | ❌[サポートされていません](#non-string-values-for-string-properties) |

これは `Newtonsoft.Json` の機能の完全な一覧ではありません。 この一覧には、 [GitHub の問題](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Text.Json)または[stackoverflow](https://stackoverflow.com/questions/tagged/system.text.json)の投稿で要求された多くのシナリオが含まれています。 現在サンプルコードが含まれていないシナリオの1つに対応する回避策を実装し、ソリューションを共有する場合は、このページの [[フィードバック] セクション](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to#feedback)で**このページ**を選択します。 GitHub の問題が作成され、このページの下部に一覧表示されます。

## <a name="differences-in-default-jsonserializer-behavior-compared-to-newtonsoftjson"></a>既定の JsonSerializer の動作が Newtonsoft. Json と比較した場合の相違点

<xref:System.Text.Json> は既定で厳密であり、決定的な動作を重視して、呼び出し元の代わりに推測や解釈を回避します。 ライブラリは、このようにして、パフォーマンスとセキュリティのために意図的に設計されています。 既定では、`Newtonsoft.Json` は柔軟です。 設計におけるこの基本的な違いは、既定の動作の次のような具体的な違いの多くによるものです。

### <a name="case-insensitive-deserialization"></a>大文字と小文字を区別しない逆シリアル化

逆シリアル化中に、`Newtonsoft.Json` は、既定では大文字と小文字を区別せずにプロパティ名が一致します。 <xref:System.Text.Json> の既定値では大文字と小文字が区別され、完全に一致するため、パフォーマンスが向上します。 大文字と小文字を区別しない照合を実行する方法については、「[大文字と小文字](system-text-json-how-to.md#case-insensitive-property-matching)を区別しないプロパティの一致

ASP.NET Core を使用して `System.Text.Json` 間接的に使用している場合は、`Newtonsoft.Json`のような動作を取得するために何も行う必要はありません。 ASP.NET Core は、`System.Text.Json`を使用するときに、camel 形式の[プロパティ名](system-text-json-how-to.md#use-camel-case-for-all-json-property-names)と大文字と小文字を区別しない一致の設定を指定します。

### <a name="minimal-character-escaping"></a>最小文字エスケープ

シリアル化時には、文字をエスケープせずに文字を使用することを、`Newtonsoft.Json` が比較的制限されています。 つまり、`xxxx` が文字のコードポイントである `\uxxxx` には置き換えられません。 エスケープされた場所では、文字の前に `\` を出力します (たとえば、`"` が `\"`になります)。 <xref:System.Text.Json> は、クロスサイトスクリプティング (XSS) 攻撃または情報漏えい攻撃に対する多層防御を提供するために、既定でより多くの文字をエスケープし、6文字のシーケンスを使用してこれを行います。 `System.Text.Json` は、既定では ASCII 以外の文字をすべてエスケープするため、`Newtonsoft.Json`で `StringEscapeHandling.EscapeNonAscii` を使用している場合は何もする必要はありません。 また `System.Text.Json` は、既定で HTML を区別する文字もエスケープします。 既定の `System.Text.Json` 動作をオーバーライドする方法については、「[文字エンコードをカスタマイズ](system-text-json-how-to.md#customize-character-encoding)する」を参照してください。

### <a name="comments"></a>説明

逆シリアル化中、`Newtonsoft.Json` は JSON 内のコメントを既定で無視します。 既定 <xref:System.Text.Json> は、 [RFC 8259](https://tools.ietf.org/html/rfc8259)仕様に含まれていないため、コメントの例外をスローします。 コメントを許可する方法の詳細については、「[コメントと末尾のコンマを許可](system-text-json-how-to.md#allow-comments-and-trailing-commas)する」を参照してください。

### <a name="trailing-commas"></a>末尾のコンマ

逆シリアル化中に、`Newtonsoft.Json` は既定で末尾のコンマを無視します。 また、複数の末尾のコンマ (`[{"Color":"Red"},{"Color":"Green"},,]`など) も無視されます。 <xref:System.Text.Json> 既定では、 [RFC 8259](https://tools.ietf.org/html/rfc8259)仕様では許可されていないため、末尾のコンマの例外をスローします。 `System.Text.Json` 使用する方法の詳細については、「[コメントと末尾のコンマを許可](system-text-json-how-to.md#allow-comments-and-trailing-commas)する」を参照してください。 複数の末尾にコンマを使用することはできません。

### <a name="converter-registration-precedence"></a>コンバーターの登録の優先順位

カスタムコンバーターの `Newtonsoft.Json` 登録の優先順位は次のとおりです。

* プロパティの属性
* 型の属性
* [コンバーター](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializerSettings_Converters.htm)コレクション

この順序は、`Converters` コレクション内のカスタムコンバーターが、型レベルで属性を適用することによって登録されるコンバーターによってオーバーライドされることを意味します。 これらの登録はどちらも、プロパティレベルで属性によってオーバーライドされます。

カスタムコンバーターの <xref:System.Text.Json> 登録の優先順位は次のとおり異なります。

* プロパティの属性
* <xref:System.Text.Json.JsonSerializerOptions.Converters> コレクション
* 型の属性

ここでの違いは、`Converters` コレクションのカスタムコンバーターが型レベルの属性をオーバーライドすることです。 この優先順位の背後にある意図は、実行時の変更によってデザイン時の選択肢がオーバーライドされることです。 優先順位を変更する方法はありません。

カスタムコンバーターの登録の詳細については、「[カスタムコンバーターの登録](system-text-json-converters-how-to.md#register-a-custom-converter)」を参照してください。

### <a name="maximum-depth"></a>最大の深さ

`Newtonsoft.Json` には、既定では最大の深さが制限されていません。 <xref:System.Text.Json> の既定の制限は64であり、<xref:System.Text.Json.JsonSerializerOptions.MaxDepth?displayProperty=nameWithType>を設定することによって構成できます。

### <a name="json-strings-property-names-and-string-values"></a>JSON 文字列 (プロパティ名と文字列値)

逆シリアル化中に、`Newtonsoft.Json` は、二重引用符、単一引用符、引用符なしで囲まれたプロパティ名を受け取ります。 二重引用符または単一引用符で囲まれた文字列値を受け取ります。 たとえば、`Newtonsoft.Json` は次の JSON を受け入れます。

```json
{
  "name1": "value",
  'name2': "value",
  name3: 'value'
}
```

`System.Text.Json` は、プロパティ名と文字列値を二重引用符でのみ受け入れます。これは、 [RFC 8259](https://tools.ietf.org/html/rfc8259)仕様ではその形式が必要であり、有効な JSON と見なされる唯一の形式であるためです。

単一引用符で囲まれた値は、次のメッセージと共に[Jsonexception](xref:System.Text.Json.JsonException)になります。

```
''' is an invalid start of a value.
```

### <a name="non-string-values-for-string-properties"></a>文字列プロパティの文字列以外の値

`Newtonsoft.Json` は、文字列型のプロパティへの逆シリアル化のために、数値やリテラル `true` や `false`などの文字列以外の値を受け入れます。 次の例では、`Newtonsoft.Json`、次のクラスへの逆シリアル化が正常に行われています。

```json
{
  "String1": 1,
  "String2": true,
  "String3": false
}
```

```csharp
public class ExampleClass
{
    public string String1 { get; set; }
    public string String2 { get; set; }
    public string String3 { get; set; }
}
```

`System.Text.Json` は、文字列以外の値を文字列プロパティに逆シリアル化しません。 文字列フィールドに対して受け取った文字列以外の値を指定すると、 [Jsonexception](xref:System.Text.Json.JsonException)が次のメッセージと共に返されます。

```
The JSON value could not be converted to System.String.
```

## <a name="scenarios-using-jsonserializer-that-require-workarounds"></a>回避策を必要とする JsonSerializer の使用シナリオ

次のシナリオは、組み込み機能ではサポートされていませんが、回避策があります。 この回避策は、`Newtonsoft.Json` の機能に完全なパリティを提供しない可能性がある[カスタムコンバーター](system-text-json-converters-how-to.md)です。 これらの一部については、例としてサンプルコードを示しています。 これらの `Newtonsoft.Json` 機能に依存している場合、移行には .NET オブジェクトモデルまたはその他のコード変更を変更する必要があります。

### <a name="types-without-built-in-support"></a>組み込みサポートのない型

<xref:System.Text.Json> には、次の種類の組み込みサポートが用意されていません。

* <xref:System.Data.DataTable> と関連する型
* F#[判別共用体](../../fsharp/language-reference/discriminated-unions.md)、[レコードの種類](../../fsharp/language-reference/records.md)、[匿名レコードの種類](../../fsharp/language-reference/anonymous-records.md)などの型。
* <xref:System.Dynamic.ExpandoObject>
* <xref:System.TimeZoneInfo>
* <xref:System.Numerics.BigInteger>
* <xref:System.TimeSpan>
* <xref:System.DBNull>
* <xref:System.Type>
* <xref:System.ValueTuple> とそれに関連付けられているジェネリック型

組み込みのサポートがない型に対しては、カスタムコンバーターを実装できます。

### <a name="quoted-numbers"></a>引用符で囲まれた数値

`Newtonsoft.Json` は、(引用符で囲まれた) JSON 文字列によって表される数値をシリアル化または逆シリアル化できます。 たとえば、`{"DegreesCelsius":23}`ではなく、`{"DegreesCelsius":"23"}` を受け入れることができます。 <xref:System.Text.Json>でこの動作を有効にするには、次の例のようなカスタムコンバーターを実装します。 コンバーターは、`long`として定義されたプロパティを処理します。

* JSON 文字列としてシリアル化されます。
* このメソッドは、逆シリアル化中に、JSON の数値と数値を引用符で囲みます。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/LongToStringConverter.cs)]

個々の `long` プロパティの[属性を使用](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property)するか、<xref:System.Text.Json.JsonSerializerOptions.Converters> コレクションに[コンバーターを追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)して、このカスタムコンバーターを登録します。

### <a name="dictionary-with-non-string-key"></a>文字列以外のキーを含むディクショナリ

`Newtonsoft.Json` は `Dictionary<TKey, TValue>`型のコレクションをサポートしています。 <xref:System.Text.Json> でのディクショナリコレクションの組み込みサポートは、`Dictionary<string, TValue>`に制限されています。 つまり、キーは文字列である必要があります。

キーとして整数またはその他の型の辞書をサポートするには、「[カスタムコンバーターの記述方法](system-text-json-converters-how-to.md#support-dictionary-with-non-string-key)」の例のようなコンバーターを作成します。

### <a name="polymorphic-serialization"></a>ポリモーフィックシリアル化

`Newtonsoft.Json` は、自動的にポリモーフィックシリアル化を行います。 <xref:System.Text.Json>の制限されたポリモーフィックシリアル化機能については、「[派生クラスのプロパティのシリアル化](system-text-json-how-to.md#serialize-properties-of-derived-classes)」を参照してください。

ここで説明する回避策は、`object`型として派生クラスを含む可能性のあるプロパティを定義することです。 これが不可能な場合は、「[カスタムコンバーターを記述する方法](system-text-json-converters-how-to.md#support-polymorphic-deserialization)」の例のように、継承の種類の階層全体に対して `Write` メソッドを使用してコンバーターを作成することもできます。

### <a name="polymorphic-deserialization"></a>ポリモーフィック逆シリアル化

`Newtonsoft.Json` には、シリアル化中に JSON に型名のメタデータを追加する `TypeNameHandling` 設定があります。 このメタデータは、ポリモーフィックな逆シリアル化を行うために逆シリアル化するときに使用します。 <xref:System.Text.Json> は、ポリモーフィックな逆シリアル化ではなく、限られた範囲の[ポリモーフィックなシリアル化](system-text-json-how-to.md#serialize-properties-of-derived-classes)を実行できます。

ポリモーフィックな逆シリアル化をサポートするには、「[カスタムコンバーターの記述方法](system-text-json-converters-how-to.md#support-polymorphic-deserialization)」の例のようなコンバーターを作成します。

### <a name="deserialization-of-object-properties"></a>オブジェクトプロパティの逆シリアル化

<xref:System.Object>に逆シリアル化 `Newtonsoft.Json` と、次のようになります。

* JSON ペイロード (`null`以外) のプリミティブ値の型を推測し、格納されている `string`、`long`、`double`、`boolean`、または `DateTime` をボックス化されたオブジェクトとして返します。 *プリミティブ値*は、json 数値、文字列、`true`、`false`、`null`などの1つの json 値です。
* JSON ペイロード内の複合値の `JObject` または `JArray` を返します。 *複合値*は、中かっこ (`{}`) 内の JSON キーと値のペアのコレクション、または角かっこ (`[]`) 内の値のリストです。 中かっこ内または角かっこ内のプロパティと値には、追加のプロパティまたは値を含めることができます。
* ペイロードに `null` JSON リテラルが含まれている場合は、null 参照を返します。

<xref:System.Text.Json> は、<xref:System.Object>に逆シリアル化するときに、プリミティブ値と複合値の両方にボックス化された `JsonElement` を格納します。次に例を示します。

* `object` プロパティ。
* `object` ディクショナリ値。
* `object` 配列値。
* ルート `object`。

ただし、`System.Text.Json` は `Newtonsoft.Json` と同じ `null` を扱い、ペイロードに `null` JSON リテラルが含まれている場合は null 参照を返します。

`object` プロパティの型の推定を実装するには、「[カスタムコンバーターの記述方法](system-text-json-converters-how-to.md#deserialize-inferred-types-to-object-properties)」の例のようなコンバーターを作成します。

### <a name="deserialize-null-to-non-nullable-type"></a>Null 非許容型に null を逆シリアル化する

`Newtonsoft.Json` は、次のシナリオでは例外をスローしません。

* `NullValueHandling` は `Ignore`に設定されます。
* 逆シリアル化中に、JSON に null 非許容型の null 値が含まれています。

同じシナリオでは、<xref:System.Text.Json> によって例外がスローされます。 (対応する null 処理設定は <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType>です)。

対象の型を所有している場合は、問題のプロパティを null 許容にすることをお勧めします (たとえば、`int` を `int?`に変更します)。

もう1つの回避策は、型のコンバーターを作成することです。たとえば、次の例では `DateTimeOffset` 型の null 値を処理します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DateTimeOffsetNullHandlingConverter.cs)]

このカスタムコンバーターを登録するには[、プロパティの属性を使用](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property)するか、<xref:System.Text.Json.JsonSerializerOptions.Converters> コレクションに[コンバーターを追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)します。

**注:** 上記のコンバーターは、既定値を指定する POCOs の `Newtonsoft.Json` とは**異なる方法で null 値を処理**します。 たとえば、次のコードがターゲットオブジェクトを表しているとします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithDefault)]

また、前のコンバーターを使用して、次の JSON が逆シリアル化されたとします。

```json
{
  "Date": null,
  "TemperatureCelsius": 25,
  "Summary": null
}
```

逆シリアル化の後、`Date` プロパティに 1/1/0001 (`default(DateTimeOffset)`) が設定されます。つまり、コンストラクターで設定された値が上書きされます。 同じ POCO と JSON を指定すると、`Newtonsoft.Json` 逆シリアル化によって `Date` プロパティに1/1/2001 が残されます。

### <a name="deserialize-to-immutable-classes-and-structs"></a>変更できないクラスと構造体への逆シリアル化

`Newtonsoft.Json` は、パラメーターを持つコンストラクターを使用できるため、変更できないクラスおよび構造体に逆シリアル化できます。 <xref:System.Text.Json> では、パラメーターなしのパブリックコンストラクターのみがサポートされます。 回避策として、カスタムコンバーターでパラメーターを使用してコンストラクターを呼び出すことができます。

次に、複数のコンストラクターパラメーターを持つ変更できない構造体を示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ImmutablePoint.cs#ImmutablePoint)]

この構造体をシリアル化および逆シリアル化するコンバーターを次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ImmutablePointConverter.cs)]

<xref:System.Text.Json.JsonSerializerOptions.Converters> コレクションにコンバーターを[追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)して、このカスタムコンバーターを登録します。

オープンな汎用プロパティを処理する同様のコンバーターの例については、[キーと値のペアの組み込みのコンバーター](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters/JsonValueConverterKeyValuePair.cs)を参照してください。

### <a name="specify-constructor-to-use"></a>使用するコンストラクターの指定

`Newtonsoft.Json` `[JsonConstructor]` 属性では、POCO に逆シリアル化するときに呼び出すコンストラクターを指定できます。 <xref:System.Text.Json> では、パラメーターなしのコンストラクターのみがサポートされます。 回避策として、カスタムコンバーターで必要なコンストラクターを呼び出すことができます。 変更できない[クラスと構造体への逆シリアル](#deserialize-to-immutable-classes-and-structs)化の例を参照してください。

### <a name="required-properties"></a>必須プロパティ

`Newtonsoft.Json`では、`[JsonProperty]` 属性に `Required` を設定することによって、プロパティが必須であることを指定します。 必須とマークされたプロパティの JSON で値が受信されない場合、`Newtonsoft.Json` は例外をスローします。

ターゲット型のプロパティの1つに対して値が受信されない場合、<xref:System.Text.Json> は例外をスローしません。 たとえば、`WeatherForecast` クラスがある場合は、次のようになります。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

次の JSON は、エラーなしで逆シリアル化されます。

```json
{
    "TemperatureCelsius": 25,
    "Summary": "Hot"
}
```

JSON に `Date` プロパティがない場合に逆シリアル化が失敗するようにするには、カスタムコンバーターを実装します。 次のサンプルコンバーターコードは、逆シリアル化の完了後に `Date` プロパティが設定されていない場合に例外をスローします。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastRequiredPropertyConverter.cs)]

[POCO クラスの属性を使用](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type)するか、<xref:System.Text.Json.JsonSerializerOptions.Converters> コレクションにコンバーターを[追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)して、このカスタムコンバーターを登録します。

このパターンに従う場合は、<xref:System.Text.Json.JsonSerializer.Serialize%2A> または <xref:System.Text.Json.JsonSerializer.Deserialize%2A>を再帰的に呼び出すときに options オブジェクトを渡しないでください。 Options オブジェクトには、<xref:System.Text.Json.JsonSerializerOptions.Converters%2A> コレクションが含まれています。 `Serialize` または `Deserialize`に渡すと、カスタムコンバーターはそれ自体を呼び出し、無限ループを行い、stack overflow 例外が発生します。 既定のオプションを使用できない場合は、必要な設定を使用してオプションの新しいインスタンスを作成します。 新しいインスタンスが個別にキャッシュされるため、この方法は遅くなります。

上記のコンバーターコードは、簡略化された例です。 属性 ( [[Jsonignore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute)やカスタムエンコーダーなど) を処理する必要がある場合は、追加のロジックが必要になります。 また、この例のコードでは、コンストラクターで既定値が設定されているプロパティを処理しません。 このアプローチでは、次のシナリオは区別されません。

* JSON にプロパティがありません。
* Null 非許容型のプロパティは JSON に存在しますが、値は、`int`の0など、型の既定値です。
* Null 許容型のプロパティが JSON に存在しますが、値は null です。

### <a name="conditionally-ignore-a-property"></a>条件付きでプロパティを無視する

`Newtonsoft.Json` には、シリアル化または逆シリアル化のプロパティを条件付きで無視するいくつかの方法があります。

* `DefaultContractResolver` を使用すると、任意の条件に基づいて追加または除外するプロパティを選択できます。
* `JsonSerializerSettings` の `NullValueHandling` と `DefaultValueHandling` の設定を使用すると、すべての null 値または既定値のプロパティを無視するように指定できます。
* `[JsonProperty]` 属性の `NullValueHandling` と `DefaultValueHandling` の設定を使用すると、null または既定値に設定されている場合に無視する必要がある個々のプロパティを指定できます。

<xref:System.Text.Json> には、シリアル化中にプロパティを省略する次の方法が用意されています。

* プロパティの[[Jsonignore]](system-text-json-how-to.md#exclude-individual-properties)属性を指定すると、シリアル化中にプロパティが JSON から除外されます。
* [Ignorenullvalues](system-text-json-how-to.md#exclude-all-null-value-properties)グローバルオプションを使用すると、すべての null 値プロパティを除外できます。
* [Ignorereadonlyproperties](system-text-json-how-to.md#exclude-all-read-only-properties)グローバルオプションを使用すると、すべての読み取り専用プロパティを除外できます。

これらのオプションでは、次のことはでき**ません**。

* 型の既定値を持つすべてのプロパティを無視します。
* 型の既定値を持つ選択されたプロパティを無視します。
* 選択したプロパティの値が null の場合は無視します。
* 実行時に評価された任意の条件に基づいて、選択したプロパティを無視します。

この機能については、カスタムコンバーターを作成できます。 このアプローチを示すサンプル POCO とカスタムコンバーターを次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastRuntimeIgnoreConverter.cs)]

コンバーターは、値が null、空の文字列、または "N/A" の場合、`Summary` プロパティをシリアル化から省略します。

[クラスの属性を使用](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type)するか、<xref:System.Text.Json.JsonSerializerOptions.Converters> コレクションにコンバーターを[追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)して、このカスタムコンバーターを登録します。

この方法では、次の場合に追加のロジックが必要になります。

* POCO には、複合プロパティが含まれています。
* `[JsonIgnore]` などの属性や、カスタムエンコーダーなどのオプションを処理する必要があります。

### <a name="specify-date-format"></a>日付の形式を指定する

`Newtonsoft.Json` には、`DateTime` と `DateTimeOffset` 型のプロパティをシリアル化および逆シリアル化する方法を制御するためのいくつかの方法が用意されています。

* `DateTimeZoneHandling` 設定は、すべての `DateTime` 値を UTC 日付としてシリアル化するために使用できます。
* `DateFormatString` 設定と `DateTime` コンバーターを使用して、日付文字列の形式をカスタマイズできます。

<xref:System.Text.Json>では、サポートが組み込まれている唯一の形式は ISO 8601-1:2019 です。これは広く採用されており、明確で、ラウンドトリップが正確に行われるためです。 他の形式を使用するには、カスタムコンバーターを作成します。 詳細については、「system.string[での DateTime と DateTimeOffset のサポート](../datetime/system-text-json-support.md)」を参照してください。

### <a name="callbacks"></a>関数

`Newtonsoft.Json` では、シリアル化または逆シリアル化プロセスの複数のポイントでカスタムコードを実行できます。

* OnDeserializing シリアル化 (オブジェクトの逆シリアル化の開始時)
* OnDeserialized シリアル化 (オブジェクトの逆シリアル化が完了したとき)
* OnSerializing (オブジェクトのシリアル化の開始時)
* OnSerialized 化 (オブジェクトのシリアル化が完了したとき)

<xref:System.Text.Json>では、カスタムコンバーターを記述してコールバックをシミュレートできます。 次の例は、POCO のカスタムコンバーターを示しています。 コンバーターには、`Newtonsoft.Json` コールバックに対応する各ポイントにメッセージを表示するコードが含まれています。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastCallbacksConverter.cs)]

[クラスの属性を使用](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type)するか、<xref:[!OP.NO-LOC(System.Text.Json)].JsonSerializerOptions.Converters> コレクションにコンバーターを[追加](system-text-json-converters-how-to.md#registration-sample---converters-collection)して、このカスタムコンバーターを登録します。

前のサンプルに続くカスタムコンバーターを使用する場合は、次の手順を実行します。

* `OnDeserializing` コードには、新しい POCO インスタンスへのアクセス権がありません。 逆シリアル化の開始時に新しい POCO インスタンスを操作するには、そのコードを POCO コンストラクターに配置します。
* `Serialize` または `Deserialize`を再帰的に呼び出すときは、options オブジェクトを渡しないでください。 Options オブジェクトには、`Converters` コレクションが含まれています。 `Serialize` または `Deserialize`に渡すと、コンバーターが使用され、無限ループが発生してスタックオーバーフロー例外が発生します。

### <a name="public-and-non-public-fields"></a>パブリックフィールドと非パブリックフィールド

`Newtonsoft.Json` は、フィールドおよびプロパティをシリアル化および逆シリアル化できます。 <xref:System.Text.Json> は、パブリックプロパティに対してのみ機能します。 カスタムコンバーターは、この機能を提供できます。

### <a name="internal-and-private-property-setters-and-getters"></a>内部およびプライベートのプロパティ setter と getter

`Newtonsoft.Json` は、`JsonProperty` 属性を使用して、プライベートと内部のプロパティセッターおよび getter を使用できます。 <xref:System.Text.Json> では、パブリック setter のみがサポートされます。 カスタムコンバーターは、この機能を提供できます。

### <a name="populate-existing-objects"></a>既存のオブジェクトの設定

`Newtonsoft.Json` の `JsonConvert.PopulateObject` メソッドは、新しいインスタンスを作成するのではなく、クラスの既存のインスタンスに JSON ドキュメントを逆シリアル化します。 <xref:System.Text.Json> は常に、既定のパブリックパラメーターなしのコンストラクターを使用して、ターゲット型の新しいインスタンスを作成します。 カスタムコンバーターは、既存のインスタンスに逆シリアル化できます。

### <a name="reuse-rather-than-replace-properties"></a>プロパティの置換ではなく再利用

`Newtonsoft.Json` `ObjectCreationHandling` 設定を使用すると、逆シリアル化中に、プロパティ内のオブジェクトを置き換えるのではなく再利用するように指定できます。 <xref:System.Text.Json> は常にプロパティ内のオブジェクトを置き換えます。  カスタムコンバーターは、この機能を提供できます。

### <a name="add-to-collections-without-setters"></a>Setter を使用せずにコレクションに追加する

逆シリアル化時には、プロパティに setter がない場合でも、`Newtonsoft.Json` によってオブジェクトがコレクションに追加されます。 <xref:System.Text.Json> は、setter を持たないプロパティを無視します。 カスタムコンバーターは、この機能を提供できます。

## <a name="scenarios-that-jsonserializer-currently-doesnt-support"></a>現在 JsonSerializer がサポートしていないシナリオ

次のシナリオでは、回避策は実用的でも可能でもありません。 これらの `Newtonsoft.Json` 機能に依存している場合は、大幅な変更を行わずに移行することはできません。

### <a name="preserve-object-references-and-handle-loops"></a>オブジェクト参照の保持とループの処理

既定では、`Newtonsoft.Json` は値によってシリアル化されます。 たとえば、オブジェクトに同じ `Person` オブジェクトへの参照を含む2つのプロパティが含まれている場合、その `Person` オブジェクトのプロパティの値は JSON で重複します。

`Newtonsoft.Json` には、参照によってシリアル化できる `JsonSerializerSettings` に `PreserveReferencesHandling` の設定があります。

* 識別子メタデータは、最初の `Person` オブジェクト用に作成された JSON に追加されます。
* 2番目の `Person` オブジェクト用に作成された JSON には、プロパティ値ではなく、その識別子への参照が含まれています。

`Newtonsoft.Json` には、例外をスローするのではなく、循環参照を無視できるようにする `ReferenceLoopHandling` 設定もあります。

<xref:System.Text.Json> は、値によるシリアル化のみをサポートし、循環参照の例外をスローします。

### <a name="systemruntimeserialization-attributes"></a>System.object のシリアル化属性

<xref:System.Text.Json> は、`DataMemberAttribute` や `IgnoreDataMemberAttribute`など、`System.Runtime.Serialization` 名前空間の属性をサポートしていません。

### <a name="octal-numbers"></a>8進数の数値

`Newtonsoft.Json` は、先頭に0を持つ数値を8進数として扱います。 [RFC 8259](https://tools.ietf.org/html/rfc8259)仕様では許可されていないため、<xref:System.Text.Json> で先頭に0を使用することはできません。

### <a name="missingmemberhandling"></a>MissingMemberHandling

JSON に対象の型に存在しないプロパティが含まれている場合は、逆シリアル化中に例外をスローするように `Newtonsoft.Json` を構成できます。 <xref:System.Text.Json> では、 [[Jsonextensiondata] 属性](system-text-json-how-to.md#handle-overflow-json)を使用する場合を除き、JSON の追加のプロパティは無視されます。 見つからないメンバー機能に対する回避策はありません。

### <a name="tracewriter"></a>TraceWriter

`Newtonsoft.Json` を使用すると、シリアル化または逆シリアル化によって生成されるログを表示するために `TraceWriter` を使用してデバッグを行うことができます。 <xref:System.Text.Json> はログ記録を行いません。

## <a name="jsondocument-and-jsonelement-compared-to-jtoken-like-jobject-jarray"></a>JsonDocument および Jsondocument と JToken (Jtoken、Jtoken など) の比較

<xref:System.Text.Json.JsonDocument?displayProperty=fullName> は、既存の JSON ペイロードから**読み取り**専用のドキュメントオブジェクトモデル (DOM) を解析し、構築する機能を提供します。 DOM は、JSON ペイロード内のデータへのランダムアクセスを提供します。 ペイロードを構成する JSON 要素には、<xref:System.Text.Json.JsonElement> の種類を使用してアクセスできます。 `JsonElement` 型は、JSON テキストを共通の .NET 型に変換するための Api を提供します。 `JsonDocument` は <xref:System.Text.Json.JsonDocument.RootElement> プロパティを公開します。

### <a name="jsondocument-is-idisposable"></a>JsonDocument が IDisposable です

`JsonDocument` は、プールされたバッファーにデータのインメモリビューを構築します。 したがって、`JObject` または `Newtonsoft.Json`からの `JArray` とは異なり、`JsonDocument` 型は `IDisposable` を実装し、using ブロック内で使用する必要があります。

有効期間の所有権を転送し、責任を呼び出し元に破棄する場合にのみ、API から `JsonDocument` を返します。 ほとんどのシナリオでは、これは必要ありません。 呼び出し元が JSON ドキュメント全体を処理する必要がある場合は、<xref:System.Text.Json.JsonDocument.RootElement%2A>の <xref:System.Text.Json.JsonElement.Clone%2A> を返します。これは <xref:System.Text.Json.JsonElement>です。 呼び出し元が JSON ドキュメント内の特定の要素を操作する必要がある場合は、その <xref:System.Text.Json.JsonElement>の <xref:System.Text.Json.JsonElement.Clone%2A> を返します。 `Clone`を作成せずに `RootElement` またはサブ要素を直接返した場合、呼び出し元は、それを所有する `JsonDocument` が破棄された後に、返された `JsonElement` にアクセスすることはできません。

次に、`Clone`の作成を要求する例を示します。

```csharp
public JsonElement LookAndLoad(JsonElement source)
{
    string json = File.ReadAllText(source.GetProperty("fileName").GetString());

    using (JsonDocument doc = JsonDocument.Parse(json))
    {
        return doc.RootElement.Clone();
    }
}
```

前のコードでは、`fileName` プロパティを含む `JsonElement` が必要です。 JSON ファイルが開き、`JsonDocument`が作成されます。 メソッドは、呼び出し元がドキュメント全体を処理することを前提としているため、`RootElement`の `Clone` が返されます。

`JsonElement` を受け取り、サブ要素を返す場合は、サブ要素の `Clone` を返す必要はありません。 呼び出し元は、渡された `JsonElement` が属している `JsonDocument` を維持する役割を担います。 次に例を示します。

```csharp
public JsonElement ReturnFileName(JsonElement source)
{
   return source.GetProperty("fileName");
}
```

### <a name="jsondocument-is-read-only"></a>JsonDocument は読み取り専用です

<xref:System.Text.Json> DOM は、JSON 要素の追加、削除、または変更を行うことはできません。 このようにパフォーマンスを向上させ、共通の JSON ペイロードサイズ (つまり < 1 MB) を解析するための割り当てを減らすことができます。 現在のシナリオで変更可能な DOM を使用している場合は、次のいずれかの回避策を実行できます。

* `JsonDocument` を最初から作成する (つまり、既存の JSON ペイロードを `Parse` メソッドに渡すことなく) には、`Utf8JsonWriter` を使用して JSON テキストを書き込み、それからの出力を解析して新しい `JsonDocument`を作成します。
* 既存の `JsonDocument`を変更するには、JSON テキストの書き込み、書き込み中の変更、および新しい `JsonDocument`を作成するための出力の解析に使用します。
* `JObject.Merge` または `JContainer.Merge` Api と同等の既存の JSON ドキュメントを `Newtonsoft.Json`からマージする方法については、 [GitHub の問題](https://github.com/dotnet/corefx/issues/42466#issuecomment-570475853)を参照してください。

### <a name="jsonelement-is-a-union-struct"></a>JsonElement は union 構造体です

`JsonDocument` は、`RootElement` を <xref:System.Text.Json.JsonElement>型のプロパティとして公開します。これは、任意の JSON 要素を含む共用体の構造体型です。 `Newtonsoft.Json` は、`JObject`、`JArray`、`JToken`などの専用の階層型を使用します。 `JsonElement` は検索と列挙を行うことができ、`JsonElement` を使用して JSON 要素を .NET 型に具体化できます。

### <a name="how-to-search-a-jsondocument-and-jsonelement-for-sub-elements"></a>サブ要素の JsonDocument と Jsondocument を検索する方法

`JObject` または `Newtonsoft.Json` の `JArray` を使用した JSON トークンの検索は、一部の辞書で参照されているため、比較的高速になる傾向があります。 比較すると、`JsonElement` の検索にはプロパティのシーケンシャル検索が必要になるため、比較的低速になります (たとえば、`TryGetProperty`を使用する場合)。 <xref:System.Text.Json> は、検索時間ではなく、初期の解析時間を最小限に抑えるように設計されています。 そのため、`JsonDocument` オブジェクトを検索するときのパフォーマンスを最適化するには、次の方法を使用します。

* 独自のインデックス作成やループを実行するのではなく、組み込みの列挙子 (<xref:System.Text.Json.JsonElement.EnumerateArray%2A> と <xref:System.Text.Json.JsonElement.EnumerateObject%2A>) を使用します。
* `RootElement`を使用して、すべてのプロパティを通して `JsonDocument` 全体で順次検索を実行しないでください。 代わりに、JSON データの既知の構造に基づいて入れ子になった JSON オブジェクトを検索します。 たとえば、`Student` オブジェクトで `Grade` のプロパティを探している場合は、`JsonElement` のプロパティを検索するすべての `Grade` オブジェクトを検索するのではなく、`Student` オブジェクトをループし、それぞれの `Grade` の値を取得します。 後者を実行すると、同じデータに対して不要なパスが返されます。

コード例については、「[データへのアクセスに JsonDocument を使用](system-text-json-how-to.md#use-jsondocument-for-access-to-data)する」を参照してください。

## <a name="utf8jsonreader-compared-to-jsontextreader"></a>Utf8JsonReader と JsonTextReader の比較

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> は、UTF-8 でエンコードされた JSON テキストの高パフォーマンス、低割り当て、前方参照専用のリーダーであり、 [ReadOnlySpan\<byte >](xref:System.ReadOnlySpan%601)または[ReadOnlySequence\<バイト >](xref:System.Buffers.ReadOnlySequence%601)から読み取ります。 `Utf8JsonReader` は、カスタムパーサーとデシリアライザーを構築するために使用できる低レベルの型です。

以下のセクションでは、`Utf8JsonReader`を使用するための推奨されるプログラミングパターンについて説明します。

### <a name="utf8jsonreader-is-a-ref-struct"></a>Utf8JsonReader は ref 構造体です。

`Utf8JsonReader` 型は*ref 構造体*であるため、いくつかの[制限](../../csharp/language-reference/keywords/ref.md#ref-struct-types)があります。 たとえば、ref 構造体以外のクラスまたは構造体のフィールドとして格納することはできません。 高パフォーマンスを実現するには、この型が `ref struct` である必要があります。これは、それ自体が ref 構造体である入力[ReadOnlySpan\<バイト >](xref:System.ReadOnlySpan%601)をキャッシュする必要があるためです。 また、この型は状態を保持しているため変更可能です。 したがって、値ではなく、 **ref で渡す必要**があります。 値渡しで渡すと、構造体のコピーが生成され、状態の変更が呼び出し元から参照できなくなります。 これは、`Newtonsoft.Json` `JsonTextReader` がクラスであるため、`Newtonsoft.Json` とは異なります。 Ref 構造体の使用方法の詳細については、「[安全C#で効率的なコードを記述](../../csharp/write-safe-efficient-code.md)する」を参照してください。

### <a name="read-utf-8-text"></a>UTF-8 テキストの読み取り

`Utf8JsonReader`の使用中に最大限のパフォーマンスを実現するには、utf-16 文字列としてではなく、既に UTF-8 テキストとしてエンコードされている JSON ペイロードを読み取ります。 コード例については、「 [Utf8JsonReader を使用してデータをフィルター処理](system-text-json-how-to.md#filter-data-using-utf8jsonreader)する」を参照してください。

### <a name="read-with-a-stream-or-pipereader"></a>ストリームまたは PipeReader で読み取ります。

`Utf8JsonReader` は、UTF-8 でエンコードされた[ReadOnlySpan\<バイト >](xref:System.ReadOnlySpan%601)または[ReadOnlySequence\<バイト >](xref:System.Buffers.ReadOnlySequence%601) (<xref:System.IO.Pipelines.PipeReader>からの読み取りの結果) からの読み取りをサポートします。

同期読み取りの場合は、ストリームの末尾がバイト配列になるまで JSON ペイロードを読み取り、それをリーダーに渡すことができます。 (UTF-16 としてエンコードされた) 文字列から読み取るには、<xref:System.Text.Encoding.UTF8>を呼び出します。<xref:System.Text.Encoding.GetBytes%2A> まず、文字列を UTF-8 でエンコードされたバイト配列にトランスコードします。 その後、それを `Utf8JsonReader`に渡します。

`Utf8JsonReader` は入力を JSON テキストと見なしているため、UTF-8 バイトオーダーマーク (BOM) は無効な JSON と見なされます。 呼び出し元は、データをリーダーに渡す前にフィルター処理する必要があります。

コード例については、「 [Use Utf8JsonReader](system-text-json-how-to.md#use-utf8jsonreader)」を参照してください。

### <a name="read-with-multi-segment-readonlysequence"></a>マルチセグメント ReadOnlySequence を使用した読み取り

JSON 入力が[ReadOnlySpan\<byte >](xref:System.ReadOnlySpan%601)の場合、読み取りループを進めるときに、リーダーの `ValueSpan` プロパティから各 json 要素にアクセスできます。 ただし、入力が[ReadOnlySequence\<byte >](xref:System.Buffers.ReadOnlySequence%601) (<xref:System.IO.Pipelines.PipeReader>からの読み取りの結果) である場合、一部の JSON 要素によって、`ReadOnlySequence<byte>` オブジェクトの複数のセグメントがまたがるされる可能性があります。 これらの要素には、連続したメモリブロック内の <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> からアクセスすることはできません。 代わりに、入力として複数セグメントの `ReadOnlySequence<byte>` がある場合は、リーダーの <xref:System.Text.Json.Utf8JsonReader.HasValueSequence%2A> プロパティをポーリングして、現在の JSON 要素にアクセスする方法を確認します。 推奨されるパターンを次に示します。

```csharp
while (reader.Read())
{
    switch (reader.TokenType)
    {
        // ...
        ReadOnlySpan<byte> jsonElement = reader.HasValueSequence ?
            reader.ValueSequence.ToArray() :
            reader.ValueSpan;
        // ...
    }
}
```

### <a name="use-valuetextequals-for-property-name-lookups"></a>プロパティ名の参照に ValueTextEquals を使用する

プロパティ名の参照に <xref:System.MemoryExtensions.SequenceEqual%2A> を呼び出すことによってバイト単位の比較を実行する場合は、<xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> を使用しないでください。 このメソッドは JSON でエスケープされた文字を unescapes するため、代わりに <xref:System.Text.Json.Utf8JsonReader.ValueTextEquals%2A> を呼び出します。 "Name" という名前のプロパティを検索する方法の例を次に示します。

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ValueTextEqualsExample.cs?name=SnippetDefineUtf8Var)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ValueTextEqualsExample.cs?name=SnippetUseUtf8Var&highlight=11)]

### <a name="read-null-values-into-nullable-value-types"></a>Null 値が許容される値型に null 値を読み取ります。

`Newtonsoft.Json` には、`TokenType` を返すことによって `Null` `bool?`を処理する `ReadAsBoolean`など、<xref:System.Nullable%601>を返す Api が用意されています。 組み込みの `System.Text.Json` Api は、null 非許容の値型のみを返します。 たとえば、<xref:System.Text.Json.Utf8JsonReader.GetBoolean%2A?displayProperty=nameWithType> は `bool`を返します。 JSON で `Null` が見つかった場合、例外がスローされます。 次の例では、null を処理する2つの方法を示しています。 null 許容値型を返し、既定値を返すことによって1つです。

```csharp
public bool? ReadAsNullableBoolean()
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return null;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

```csharp
public bool ReadAsBoolean(bool defaultValue)
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return defaultValue;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

### <a name="multi-targeting"></a>マルチターゲット

特定のターゲットフレームワークに対して `Newtonsoft.Json` を引き続き使用する必要がある場合は、複数のターゲットを持つことができ、2つの実装があります。 ただし、これは簡単ではなく、一部の `#ifdefs` とソースの複製が必要になります。 できるだけ多くのコードを共有する方法の1つとして、`Utf8JsonReader` と `Newtonsoft.Json` `JsonTextReader`の `ref struct` ラッパーを作成します。 このラッパーは、動作の違いを特定しながら、公開された領域を統合します。 これにより、変更を主に型の構造に分離し、新しい型を参照渡しで渡すことができます。 このパターン[は、次](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/)のようになります。

* [UnifiedJsonReader.JsonTextReader.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.JsonTextReader.cs)
* [UnifiedJsonReader.Utf8JsonReader.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.Utf8JsonReader.cs)

## <a name="utf8jsonwriter-compared-to-jsontextwriter"></a>Utf8JsonWriter と JsonTextWriter の比較

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> は、`String`、`Int32`、`DateTime`などの一般的な .NET 型から、UTF-8 でエンコードされた JSON テキストを書き込むための高パフォーマンスの方法です。 ライターは、カスタムシリアライザーを構築するために使用できる低レベルの型です。

以下のセクションでは、`Utf8JsonWriter`を使用するための推奨されるプログラミングパターンについて説明します。

### <a name="write-with-utf-8-text"></a>UTF-8 テキストを使用した書き込み

`Utf8JsonWriter`の使用中に最大限のパフォーマンスを実現するには、utf-16 文字列としてではなく、既に UTF-8 テキストとしてエンコードされている JSON ペイロードを書き込みます。 <xref:System.Text.Json.JsonEncodedText> を使用して、既知の文字列プロパティの名前と値をスタティックとしてキャッシュおよび事前エンコードし、UTF-16 文字列リテラルを使用するのではなく、ライターに渡します。 これは、キャッシュし、UTF-8 バイト配列を使用するよりも高速です。

この方法は、カスタムエスケープを行う必要がある場合にも機能します。 `System.Text.Json` では、文字列の書き込み中にエスケープを無効にすることはできません。 ただし、独自のカスタム <xref:System.Text.Encodings.Web.JavaScriptEncoder> をライターにオプションとして渡すことも、`JavascriptEncoder` を使用してエスケープを行う独自の `JsonEncodedText` を作成してから、文字列の代わりに `JsonEncodedText` を書き込むこともできます。 詳細については、「[文字エンコードをカスタマイズ](system-text-json-how-to.md#customize-character-encoding)する」を参照してください。

### <a name="write-raw-values"></a>生の値の書き込み

`Newtonsoft.Json` `WriteRawValue` メソッドは、値が必要な場合に生の JSON を書き込みます。 <xref:System.Text.Json> には同等の機能はありませんが、有効な JSON だけが書き込まれるようにするための回避策を次に示します。

```csharp
using JsonDocument doc = JsonDocument.Parse(string);
doc.WriteTo(writer);
```

### <a name="customize-character-escaping"></a>文字エスケープのカスタマイズ

`JsonTextWriter` の[StringEscapeHandling](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_StringEscapeHandling.htm)設定には、ASCII 以外の文字**または**HTML 文字をすべてエスケープするオプションが用意されています。 既定では、`Utf8JsonWriter` は、ASCII 以外の文字**と**HTML 文字をすべてエスケープします。 このエスケープは、多層防御のセキュリティ上の理由で行われます。 別のエスケープポリシーを指定するには、<xref:System.Text.Encodings.Web.JavaScriptEncoder> を作成し、<xref:System.Text.Json.JsonWriterOptions.Encoder?displayProperty=nameWithType>を設定します。 詳細については、「[文字エンコードをカスタマイズ](system-text-json-how-to.md#customize-character-encoding)する」を参照してください。

### <a name="customize-json-format"></a>JSON 形式のカスタマイズ

`JsonTextWriter` には、次の設定が含まれており、`Utf8JsonWriter` に相当するものはありません。

* [インデント](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_Indentation.htm)-インデントする文字数を指定します。 `Utf8JsonWriter` 常に2文字のインデントが行われます。
* [IndentChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_IndentChar.htm) -インデントに使用する文字を指定します。  `Utf8JsonWriter` は常に空白文字を使用します。
* [QuoteChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteChar.htm) -文字列値を囲むために使用する文字を指定します。  `Utf8JsonWriter` は常に二重引用符を使用します。
* [QuoteName](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteName.htm) -プロパティ名を引用符で囲むかどうかを指定します。  `Utf8JsonWriter` は常に引用符で囲みます。

これらの方法で `Utf8JsonWriter` によって生成される JSON をカスタマイズできる回避策はありません。

### <a name="write-null-values"></a>Null 値の書き込み

`Utf8JsonWriter`を使用して null 値を書き込むには、次のように呼び出します。

* 値として null を持つキーと値のペアを書き込む <xref:System.Text.Json.Utf8JsonWriter.WriteNull%2A> ます。
* <xref:System.Text.Json.Utf8JsonWriter.WriteNullValue%2A>、JSON 配列の要素として null を書き込むことができます。

文字列プロパティの場合、文字列が null の場合、<xref:System.Text.Json.Utf8JsonWriter.WriteString%2A> と <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A> は `WriteNull` と `WriteNullValue`に相当します。

### <a name="write-timespan-uri-or-char-values"></a>Timespan、Uri、または char 値の書き込み

`JsonTextWriter` には、 [TimeSpan](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_18.htm)、 [Uri](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_22.htm)、および[char](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_3.htm)値の `WriteValue` メソッドが用意されています。 `Utf8JsonWriter` には、同等のメソッドがありません。 代わりに、これらの値を文字列として書式設定し (たとえば `ToString()`を呼び出し)、<xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A>を呼び出します。

### <a name="multi-targeting"></a>マルチターゲット

特定のターゲットフレームワークに対して `Newtonsoft.Json` を引き続き使用する必要がある場合は、複数のターゲットを持つことができ、2つの実装があります。 ただし、これは簡単ではなく、一部の `#ifdefs` とソースの複製が必要になります。 できるだけ多くのコードを共有する方法の1つは、`Utf8JsonWriter` のラッパーを作成し、`JsonTextWriter`を `Newtonsoft` することです。 このラッパーは、動作の違いを特定しながら、公開された領域を統合します。 これにより、主に型の構造に変更を分離できます。 次[のよう](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/)にします。

* [UnifiedJsonWriter.JsonTextWriter.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.JsonTextWriter.cs)
* [UnifiedJsonWriter.Utf8JsonWriter.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.Utf8JsonWriter.cs)

## <a name="additional-resources"></a>その他のリソース

<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)[Restore this when the roadmap is updated.]-->
* [System.Text.Json の概要](system-text-json-overview.md)
* [System.Text.Json の使用方法](system-text-json-how-to.md)
* [カスタムコンバーターを記述する方法](system-text-json-converters-how-to.md)
* [System.Text.Json での DateTime と DateTimeOffset のサポート](../datetime/system-text-json-support.md)
* [System.Text.Json API リファレンス](xref:System.Text.Json)
* [System.Text.Json。シリアル化 API リファレンス](xref:System.Text.Json.Serialization)
