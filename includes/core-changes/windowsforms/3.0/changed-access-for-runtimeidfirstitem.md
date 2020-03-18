---
ms.openlocfilehash: 5bbbf9075683b0f124e126b661b4ab85011e6c2e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74643929"
---
### <a name="change-of-access-for-accessibleobjectruntimeidfirstitem"></a>AccessibleObject.RuntimeIDFirstItem のアクセスに対する変更

.NET Core 3.0 RC1 以降、`AccessibleObject.RuntimeIDFirstItem` に対するアクセスが `protected` から `internal`に変更されました。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 Preview 4 以降、`AccessibleObject.RuntimeIDFirstItem` フィールドは `protected` でした。 これは .NET Core 3.0 RC1 以降では、.NET Framework のフィールドと同じアクセスである `protected` から `internal` に変更されました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 RC1

#### <a name="recommended-action"></a>推奨アクション

この変更は、開発した .NET Core アプリが、<xref:System.Windows.Forms.AccessibleObject> から派生し、`RuntimeIDFirstItem` フィールドにアクセスする型を使用している場合に影響がある可能性があります。 この場合、次のようにローカル定数を定義してください。

```csharp
const int RuntimeIDFirstItem = 0x2a;
```

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

- API 分析では検出できません。

<!-- 

### Affected APIs

- Not detectable via API analysis.

-->
