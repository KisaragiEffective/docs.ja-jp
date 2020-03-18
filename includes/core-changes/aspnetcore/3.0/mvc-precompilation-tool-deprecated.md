---
ms.openlocfilehash: 1e081c9f37fbd7ab754ce44ba89d7aa5cabfc219
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901944"
---
### <a name="mvc-precompilation-tool-deprecated"></a>MVC:プリコンパイル ツールは非推奨です

ASP.NET Core 1.1 では、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` (MVC プリコンパイル ツール) パッケージが導入され、Razor ファイル ( *.cshtml* ファイル) の発行時のコンパイルのサポートが追加されました。 ASP.NET Core 2.1 では、プリコンパイル ツールの機能を拡張するために [Razor SDK](/aspnet/core/razor-pages/sdk?view=aspnetcore-2.1) が導入されました。 Razor SDK では、Razor ファイルのビルドおよび発行時のコンパイルのサポートが追加されました。 SDK では、アプリの起動時間を改善しながら、ビルド時に *の* ファイルの正確性を確認します。 Razor SDK は既定でオンになっており、その使用を開始するためにジェスチャは必要ありません。

ASP.NET Core 3.0 では、ASP.NET Core 1.1 世代の MVC のプリコンパイル ツールが削除されました。 以前のバージョンのパッケージでは、修正プログラムのリリースで重要なバグおよびセキュリティ修正プログラムを引き続き受け取ります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` パッケージは、MVC Razor ビューをプリコンパイルするために使用されました。

#### <a name="new-behavior"></a>新しい動作

Razor SDK では、この機能がネイティブでサポートされます。 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` パッケージが更新されなくなりました。

#### <a name="reason-for-change"></a>変更理由

Razor SDK によって、より多くの機能が提供され、ビルド時に *.cshtml* ファイルの正確性が確認されます。 また、SDK によってアプリの起動時間が改善されます。

#### <a name="recommended-action"></a>推奨アクション

ASP.NET Core 2.1 以降のユーザーの場合は、[Razor SDK](/aspnet/core/razor-pages/sdk?view=aspnetcore-3.0) でのプリコンパイルのネイティブ サポートを使用するように更新します。 バグまたは不足している機能によって Razor SDK への移行が妨げられる場合は、[dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues) で問題を開きます。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

### Affected APIs

Not detectable via API analysis

-->
