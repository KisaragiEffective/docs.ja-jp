---
ms.openlocfilehash: f5ae4669c85ae4f5d57d88ab55f6e1c758a625a6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79147589"
---
### <a name="jsonencodedtextencode-methods-have-an-additional-javascriptencoder-argument"></a>JsonEncodedText.Encode メソッドに JavaScriptEncoder 引数を追加

.NET Core 3.0 Preview 8 以降、<xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType> メソッドには、オプションの <xref:System.Text.Encodings.Web.JavaScriptEncoder> 引数が含まれます。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 には、新しい型 xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType> が含まれます。 .NET Core 3.0 Preview 8 以降では、<xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType> メソッドのすべてのオーバーロードのシグネチャにオプションの <xref:System.Text.Encodings.Web.JavaScriptEncoder> パラメーターを含むように変更されました。 この変更により、別のエンコーダーまたはカスタムのエンコーダーを使用できるようになりました。

.NET Core 3.0 Preview 7 の `Encode` メソッドのシグネチャは、次のとおりです。

```csharp
namespace System.Text.Json
{
    public readonly struct JsonEncodedText
    {
        public static JsonEncodedText Encode(ReadOnlySpan<byte> utf8Value);
        public static JsonEncodedText Encode(ReadOnlySpan<char> value);
        public static JsonEncodedText Encode(string value);
    }
}
```

.NET Core 3.0 Preview 8 以降のバージョンでは、同じ `Encode` メソッドのシグネチャが次のようになります。

```csharp
namespace System.Text.Json
{
    public readonly struct JsonEncodedText
    {
        public static JsonEncodedText Encode(ReadOnlySpan<byte> utf8Value, JavaScriptEncoder encoder = null);
        public static JsonEncodedText Encode(ReadOnlySpan<char> value, JavaScriptEncoder encoder = null);
        public static JsonEncodedText Encode(string value, JavaScriptEncoder encoder = null);
    }
}
```

#### <a name="version-introduced"></a>導入されたバージョン

.NET Core 3.0 Preview 8

#### <a name="recommended-action"></a>推奨アクション

これはバイナリのみの破壊的変更です。 .NET Core 3.0 Preview 8 以降のバージョンに再コンパイルすると、実行時の問題はすべて修正されます。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan%7BSystem.Byte%7D,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>
- <xref:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan%7BSystem.Char%7D,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>
- <xref:System.Text.Json.JsonEncodedText.Encode(System.String,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>

<!--

### Affected APIs

- `M:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan{System.Byte},System.Text.Encodings.Web.JavaScriptEncoder)`
- `M:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan{System.Char},System.Text.Encodings.Web.JavaScriptEncoder)`
- `M:System.Text.Json.JsonEncodedText.Encode(System.String,System.Text.Encodings.Web.JavaScriptEncoder)`

-->
