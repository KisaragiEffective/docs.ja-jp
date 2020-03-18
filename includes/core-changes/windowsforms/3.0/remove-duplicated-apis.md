---
ms.openlocfilehash: e609b8006846cd202a6a7eeec2529cf1fbb09e7c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937005"
---
### <a name="duplicated-apis-removed-from-windows-forms"></a>Windows フォームからの重複する API の削除

.NET Core 3.0 Preview 4 以降、<xref:System.Windows.Forms?displayProperty=fullName> 名前空間で誤って重複していた多数の API が .NET Core 3.0 RC1 で削除されました。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 Preview 4 には、<xref:System.Windows.Forms?displayProperty=fullName> 名前空間に既にある <xref:System.ComponentModel.Design?displayProperty=fullName> 名前空間の複数の型が、誤って重複しています。 .NET Core 3.0 RC1 以降、重複するこれらの型はなくなります。 次の表に、元の型とその重複する型の一覧を示します。

> [!div class="mx-tdCol2BreakAll"]
> |元の型|重複する型|
> |---|---|
> |<xref:System.ComponentModel.Design.DesignerActionListsChangedEventArgs?displayProperty=fullName>|`System.Windows.Forms.DesignerActionListsChangedEventArgs`|
> |<xref:System.ComponentModel.Design.DesignerActionListsChangedEventHandler?displayProperty=fullName>|`System.Windows.Forms.DesignerActionListsChangedEventHandler`|
> |<xref:System.ComponentModel.Design.DesignerActionListsChangedType?displayProperty=fullName>|`System.Windows.Forms.DesignerActionListsChangedType`|
> |<xref:System.ComponentModel.Design.DesignerActionUIService?displayProperty=fullName>|`System.Windows.Forms.DesignerActionUIService`|
> |<xref:System.ComponentModel.Design.DesignerCommandSet?displayProperty=fullName>|`System.Windows.Forms.DesignerCommandSet`|

#### <a name="version-introduced"></a>導入されたバージョン

3.0 RC1

#### <a name="recommended-action"></a>推奨アクション

元の型を参照するように、表の「**元の型**」列のとおりコードを更新してください。

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

- API 分析では検出できません。

<!--

### Affected APIs

- Not detectable via API analysis.

-->
