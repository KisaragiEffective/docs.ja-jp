---
ms.openlocfilehash: c8f44ae1a500ed240dbff7d9a2c1479af368b7f1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394133"
---
### <a name="identity-default-bootstrap-version-of-ui-changed"></a>ID:UI の既定の Bootstrap のバージョンが変更された

ASP.NET Core 3.0 以降、ID UI では既定で Bootstrap のバージョン 4 が使用されています。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` メソッドの呼び出しは `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);` と同じでした

#### <a name="new-behavior"></a>新しい動作

`services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` メソッドの呼び出しは `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap4);` と同じです

#### <a name="reason-for-change"></a>変更理由

Bootstrap 4 は ASP.NET Core 3.0 の期間中にリリースされました。

#### <a name="recommended-action"></a>推奨アクション

既定の ID UI を使用し、次の例に示すように `Startup.ConfigureServices` にそれを追加している場合、この変更による影響を受けます。

```csharp
services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();
```

次のいずれかのアクションを実行します。

- [移行ガイド](https://getbootstrap.com/docs/4.0/migration)を使用して、Bootstrap 4 を使用するようにアプリを移行します。
- Bootstrap 3 の使用を強制するように、`Startup.ConfigureServices` を更新します。 次に例を示します。

    ```csharp
    services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);
    ```

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
