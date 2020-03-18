---
ms.openlocfilehash: db1d09c8c9e606b5327a42977a74a74703282d84
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568100"
---
### <a name="net-core-30-follows-unicode-best-practices-when-replacing-ill-formed-utf-8-byte-sequences"></a>.NET Core 3.0 は不正な形式の UTF-8 バイト シーケンスを置換するときに Unicode のベスト プラクティスに従う

<xref:System.Text.UTF8Encoding> クラスで、バイトを文字にコード変換する操作中に不正な形式の UTF-8 バイトのシーケンスが検出されるとき、そのシーケンスは、出力文字列で '�' (U+FFFD REPLACEMENT CHARACTER) 文字に置き換えられます。 .NET Core 3.0 では、コード変換の操作中にこの置換を実行するための Unicode ベスト プラクティスに従うことで、以前のバージョンの .NET Core と .NET Framework との差別化が行われています。

これは、新しい <xref:System.Text.Unicode.Utf8?displayProperty=nameWithType> 型および <xref:System.Text.Rune?displayProperty=nameWithType> 型の使用を含め、.NET 全体での UTF-8 の処理を向上させようとする、より大きな取り組みの一環です。 <xref:System.Text.UTF8Encoding> 型では、新しく導入された型と一致する出力が生成されるよう、エラー処理のメカニズムが向上しています。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 からは、バイトの文字列へのコード変換は、Unicode のベスト プラクティスに基づいて <xref:System.Text.UTF8Encoding> クラスによって文字列が置換されます。 使用される置換メカニズムについては、[Unicode 標準のバージョン 12.0 のセクション 3.9 (PDF)](https://www.unicode.org/versions/Unicode12.0.0/ch03.pdf) の「_U+FFFD Substitution of Maximal Subparts_」 (最大サブパーツの U+FFFD 置換) という見出しを参照してください。

この動作は、入力バイト シーケンスに不正な形式の UTF-8 データが含まれている場合_のみ_該当します。 また、<xref:System.Text.UTF8Encoding> インスタンスが `throwOnInvalidBytes: true` を使用して構築されている場合 ([UTF8Encoding コンストラクターのドキュメント] を参照)<xref:System.Text.UTF8Encoding.%23ctor(System.Boolean,System.Boolean)>、`UTF8Encoding` インスタンスは U+FFFD 置換を実行せずに無効な入力のスローを続けます。

次に、不正な 3 バイトの入力でのこの変更の影響を示します。

|不正形式の 3 バイトの入力|.NET Core 3.0 以前の出力|.NET Core 3.0 以降の出力|
|---|---|---|
| `[ ED A0 90 ]` | `[ FFFD FFFD ]` (2 文字の出力)| `[ FFFD FFFD FFFD ]` (3 文字の出力)|

この 3 文字の出力は、前にリンクした Unicode 標準の PDF の_表 3-9_ に従って、優先される出力となります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨される操作

開発者側では、何も行う必要はありません。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Text.UTF8Encoding.GetCharCount%2A?displayProperty=nameWithType>
- <xref:System.Text.UTF8Encoding.GetChars%2A?displayProperty=nameWithType>
- <xref:System.Text.UTF8Encoding.GetString(System.Byte[],System.Int32,System.Int32)?displayProperty=nameWithType>

<!--

### Affected APIs

- `Overload:System.Text.UTF8Encoding.GetCharCount`
- `Overload:System.Text.UTF8Encoding.GetChars`
- `M:System.Text.UTF8Encoding.GetString(System.Byte[],System.Int32,System.Int32)`

-->
