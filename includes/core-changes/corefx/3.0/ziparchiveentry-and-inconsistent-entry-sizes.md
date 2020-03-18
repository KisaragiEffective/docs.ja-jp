---
ms.openlocfilehash: 9520f8c6b6671917f5694bc602293a00e2dab82d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74568215"
---
### <a name="ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes"></a>ZipArchiveEntry による、エントリ サイズに一貫性のないアーカイブ処理の中止

Zip アーカイブは、圧縮されているサイズと圧縮されていないサイズの両方を、中央ディレクトリとローカル ヘッダーに一覧表示します。  また、エントリ データ自体のサイズも示されます。  .NET Core 2.2 以前のバージョンでは、これらの値の一貫性はチェックされませんでした。 .NET Core 3.0 より、これが開始します。

#### <a name="change-description"></a>変更の説明

.NET Core 2.2 以前のバージョンでは、<xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> はローカル ヘッダーが zip ファイルの中央ヘッダーと一致しない場合でも成功します。 圧縮されたストリームの末尾に達するまで、データは圧縮解除されます。これは、その長さが、中央ディレクトリ/ローカル ヘッダーに示されている圧縮されていないファイル サイズを超えている場合も同様です。

.NET Core 3.0 以降では、<xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> メソッドによって、エントリの圧縮されたサイズと圧縮されていないサイズがローカル ヘッダーと中央ヘッダーで一致することが確認されます。  そうでない場合、アーカイブのローカル ヘッダーやデータ記述子に zip ファイルの中央ディレクトリと一致しないサイズが一覧表示されている場合、メソッドによって <xref:System.IO.InvalidDataException> がスローされます。 エントリを読み取るときに、圧縮解除されたデータは、ヘッダーに一覧表示されている圧縮されていないファイル サイズにまで切り詰められます。

この変更は、<xref:System.IO.Compression.ZipArchiveEntry> がそのデータのサイズを正しく表し、そのデータ量だけが読み取られるようにするために行われました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

この問題が発生している zip アーカイブをすべて再パッケージ化します。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.ExtractToDirectory%2A?displayProperty=nameWithType>

<!--

### Affected APIs

`M:System.IO.Compression.ZipArchiveEntry.Open`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A`
`Overload:System.IO.Compression.ZipFile.ExtractToDirectory%2A`

-->
