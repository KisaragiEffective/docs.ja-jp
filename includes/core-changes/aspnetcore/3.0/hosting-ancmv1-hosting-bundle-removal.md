---
ms.openlocfilehash: 82103d82a6f68c62f3532608718bc71b0ba126bf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901797"
---
### <a name="hosting-aspnetcoremodule-v1-removed-from-windows-hosting-bundle"></a>ホスティング:AspNetCoreModule V1 が Windows ホスティング バンドルから削除されました

ASP.NET Core 3.0 以降では、Windows ホスティング バンドルに AspNetCoreModule (ANCM) V1 が含まれません。

ANCM V2 は、ANCM OutOfProcess との下位互換性があり、ASP.NET Core 3.0 アプリでの使用をお勧めします。

ディスカッションについては、[dotnet/aspnetcore#7095](https://github.com/dotnet/aspnetcore/issues/7095) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

Windows ホスティング バンドルに ANCM V1 が含まれています。

#### <a name="new-behavior"></a>新しい動作

Windows ホスティング バンドルに ANCM V1 が含まれていません。

#### <a name="reason-for-change"></a>変更理由

ANCM V2 は、ANCM OutOfProcess との下位互換性があり、ASP.NET Core 3.0 アプリでの使用をお勧めします。

#### <a name="recommended-action"></a>推奨アクション

ASP.NET Core 3.0 アプリでお使いの ANCM V2 を使用します。

ANCM V1 が必要な場合は、ASP.NET Core 2.1 または 2.2 の Windows ホスティング バンドルを使用してインストールできます。

この変更によって、次のような ASP.NET Core 3.0 アプリが中断されます。

- `<AspNetCoreModuleName>AspNetCoreModule</AspNetCoreModuleName>` での ANCM V1 の使用が明示的に選択されている。
- カスタム *web.config* ファイルに `<add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />` が設定されている。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
