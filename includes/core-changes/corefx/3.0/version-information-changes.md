---
ms.openlocfilehash: 5612ebce67946e22aaeeba861115ce4f8967e1f5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75344454"
---
### <a name="apis-that-report-version-now-report-product-and-not-file-version"></a>バージョンをレポートする API が、ファイル バージョンではなく製品をレポートするようになりました

.NET Core のバージョンを返す API の多くは、ファイル バージョンではなく製品バージョンを返すようになりました。

#### <a name="change-description"></a>変更の説明

.NET Core 2.2 およびそれ以前のバージョンでは、<xref:System.Environment.Version?displayProperty=nameWithType>、<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> などのメソッドと、.NET Core アセンブリの [ファイルのプロパティ] ダイアログにファイルのバージョンが反映されています。 .NET Core 3.0 以降では、製品のバージョンが反映されています。

次の図は、.NET Core 2.2 (左側) と .NET Core 3.0 (右側) の *System.Runtime.dll* アセンブリのバージョン情報の違いを示しています。これは、**Windows Explorer** [ファイルのプロパティ] ダイアログに表示されます。

![製品バージョン情報の相違点](~/docs/images/core-changes/corefx/version-information-changes/file-details.png)

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

[なし] : この変更により、機能性ではなく、バージョン検出を直感的に行う必要があります。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Environment.Version?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType>

<!--

### Affected APIs

- `P:System.Environment.Version`
- `P:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription`

-->
