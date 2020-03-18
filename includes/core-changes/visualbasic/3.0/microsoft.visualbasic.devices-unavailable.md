---
ms.openlocfilehash: 7f528510e4158dad71280a7b1f269a231b8c60f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76116337"
---
### <a name="types-in-microsoftvisualbasicdevices-namespace-not-available"></a>Microsoft.VisualBasic.Devices 名前空間の型は使用できません

<xref:Microsoft.VisualBasic.Devices?displayProperty=fullName> 名前空間の型は使用できません。

#### <a name="version-introduced"></a>導入されたバージョン

.NET Core 3.0 Preview 8

#### <a name="change-description"></a>変更の説明

<xref:Microsoft.VisualBasic.Devices?displayProperty=fullName> 名前空間の型は、一部の .NET Core 3.0 プレビュー リリースで使用できました。 それらは、.NET Core 3.0 Preview 9 以降では使用できなくなりました。

これらの型は、今後のリリースでの不要なアセンブリの依存関係や破壊的変更を回避するために削除されました。

#### <a name="recommended-action"></a>推奨アクション

コードが <xref:Microsoft.VisualBasic.Devices> 型とそのメンバーの使用に依存している場合は、.NET クラス ライブラリ内の対応する型またはメンバーを使用できる可能性があります。 たとえば、<xref:Microsoft.VisualBasic.Devices.Clock?displayProperty=nameWithType> クラスと同等の機能は <xref:System.DateTime?displayProperty=nameWithType> および <xref:System.Environment?displayProperty=nameWithType> 型によって提供されており、<xref:Microsoft.VisualBasic.Devices.Ports?displayProperty=nameWithType> クラスと同等の機能は、<xref:System.IO.Ports?displayProperty=nameWithType> 名前空間の型によって提供されています。

#### <a name="category"></a>カテゴリ

Visual Basic

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.VisualBasic.Devices?displayProperty=fullName>

<!--

### Affected APIs

- `N:Microsoft.VisualBasic.Devices`

-->
