---
ms.openlocfilehash: 6f8e6d2786d20e055c9bef63891db4d6f88bc64b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901702"
---
### <a name="identity-signinmanager-constructor-accepts-new-parameter"></a>ID: SignInManager コンストラクターは新しいパラメーターを受け入れる

ASP.NET Core 3.0 以降、`SignInManager` コンストラクターに新しい `IUserConfirmation<TUser>` パラメーターが追加されました。 詳細については、[dotnet/aspnetcore#8356](https://github.com/dotnet/aspnetcore/issues/8356) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="reason-for-change"></a>変更理由

変更の動機は、ID での新しいメール/確認フローのサポートを追加するためでした。

#### <a name="recommended-action"></a>推奨アクション

手動で `SignInManager` を構築している場合は、`IUserConfirmation` の実装を提供するか、依存関係の挿入から 1 つを取得して提供します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Identity.SignInManager%601.%23ctor%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

`Overload:Microsoft.AspNetCore.Identity.SignInManager`1.#ctor`

-->
