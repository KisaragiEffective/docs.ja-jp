---
ms.openlocfilehash: 959f3959c28c7d0159be7a213986345e2865b9a2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394207"
---
### <a name="shared-framework-removed-microsoftaspnetcoreall"></a>共有フレームワーク: Microsoft.AspNetCore.All を削除

ASP.NET Core 3.0 以降では、`Microsoft.AspNetCore.All` メタパッケージと、対応する `Microsoft.AspNetCore.All` 共有フレームワークが生成されなくなりました。 このパッケージは ASP.NET Core 2.2 で使用することができ、ASP.NET Core 2.1 でサービス更新プログラムを引き続き受け取ることができます。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

アプリでは `Microsoft.AspNetCore.All` メタパッケージを使用して、.NET Core 上の `Microsoft.AspNetCore.All` 共有フレームワークをターゲットにすることができました。

#### <a name="new-behavior"></a>新しい動作

.NET Core 3.0 には、`Microsoft.AspNetCore.All` 共有フレームワークは含まれていません。

#### <a name="reason-for-change"></a>変更理由

`Microsoft.AspNetCore.All` メタパッケージには、多数の外部依存関係が含まれていました。

#### <a name="recommended-action"></a>推奨アクション

プロジェクトを移行して、`Microsoft.AspNetCore.App` フレームワークを使用します。 `Microsoft.AspNetCore.All` でこれまで提供されていたコンポーネントは、NuGet で引き続き利用できます。 これらのコンポーネントは、共有フレームワークに含まれるのではなく、アプリと共にデプロイされるようになりました。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
