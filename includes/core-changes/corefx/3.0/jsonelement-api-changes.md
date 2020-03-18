---
ms.openlocfilehash: 79901d0b57816915ca7ea73cfe7fa3d40820434e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74568104"
---
### <a name="jsonelement-api-changes"></a>JsonElement API の変更

.NET Core 3.0 Preview 7 以降、一部の <xref:System.Text.Json.JsonElement> API はより検出しやすく、より使いやすく変更されています。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 Preview 7 では、<xref:System.Text.Json.JsonElement> API は次のようにより検出しやすく、より使いやすく変更されています。

1. すべての `WriteProperty` メソッドのオーバーロードが <xref:System.Text.Json.JsonElement> から削除されています。 これは、次などのコードに影響します。

   ```csharp
   using (JsonDocument doc = JsonDocument.Parse(jsonString))
   {
      JsonElement root = doc.RootElement;
      root.WriteProperty(propertyNameString, writer);
      // ..
      root.WriteProperty(propertyNameSpan, writer);
      // ..
      root.WriteProperty(propertyNameJsonEncoded, writer);
      // ..
      root.WriteProperty(utf8PropertyName, writer);
      //..
   }
   ```

1. `WriteValue` は <xref:System.Text.Json.JsonElement.WriteTo%2A> に名前変更されました。 これは、次などのコードに影響します。

   ```csharp
    using (JsonDocument doc = JsonDocument.Parse(jsonString))
    {
        JsonElement root = doc.RootElement;
        root.WriteValue(writer);
    }
    ```

1. <xref:System.Text.Json.JsonElement.WriteTo%2A> で、それのメソッド パラメーターが <xref:System.ArgumentNullException> の場合に、`null` がスローされるようになりました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 7

#### <a name="recommended-action"></a>推奨アクション

これらの変更の影響をコードが受ける場合、次を行うことができます。

- `WriteProperty` に <xref:System.Text.Json.JsonElement> オーバーロードに対する置換 API はありません。 同じ結果を得るには、代わりに <xref:System.Text.Json.Utf8JsonWriter.WritePropertyName%2A?displayProperty=nameWithType> メソッドと共に <xref:System.Text.Json.JsonElement.WriteTo%2A> オーバーロードの 1 つを呼び出します。 次に例を示します。

   ```csharp
   using (JsonDocument doc = JsonDocument.Parse(jsonString))
   {
       JsonElement root = doc.RootElement;
       writer.WritePropertyName(propertyNameString);
       root.WriteTo(writer);
   }
   ```

- `WriteValue` メソッドの名前を <xref:System.Text.Json.JsonElement.WriteTo(System.Text.Json.Utf8JsonWriter)> に変更します。 このメソッドのパラメーターは変更されません。 たとえば、前のコードは次のように変更します。

   ```csharp
   using (JsonDocument doc = JsonDocument.Parse(jsonString))
   {
       JsonElement root = doc.RootElement;
       root.WriteTo(writer);
   }
   ```

- <xref:System.ArgumentNullException> メソッドへの呼び出しの <xref:System.Text.Json.JsonElement.WriteTo%2A> を処理します。

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Text.Json.JsonElement>
- <xref:System.Text.Json.JsonElement.WriteTo%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Text.Json.JsonElement.WriteProperty`
- `M:System.Text.Json.JsonElement.WriteValue(System.Text.Json.Utf8JsonWriter)`

-->
