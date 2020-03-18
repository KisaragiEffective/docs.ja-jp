---
ms.openlocfilehash: 80fc75d0736e2ae17699073a025e79b52b340613
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937067"
---
### <a name="domainupdownuselegacyscrolling-compatibility-switch-not-supported"></a>DomainUpDown.UseLegacyScrolling 互換性スイッチはサポートされていません

.NET Framework 4.7.1 で導入された `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` 互換スイッチは、.NET Core 3.0 上の Windows フォームではサポートされていません。

#### <a name="change-description"></a>変更の説明

.NET Framework 4.7.1 以降では、`Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` 互換スイッチを使用して、開発者が独立した <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> および <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> アクションをオプトアウトできるようになりました。 このスイッチによって、コンテキスト テキストが存在する場合は <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> が無視されるという従来の動作が復元されました。開発者は、コントロールに対して <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> アクションの前に <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> アクションを使用する必要があります。 詳細については、「[\<AppContextSwitchOverrides> 要素](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)」を参照してください。

.NET Core では、`Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` スイッチはサポートされていません。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 9

#### <a name="recommended-action"></a>推奨アクション

スイッチを削除します。 そのスイッチはサポートされておらず、代替機能はありません。

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>

<!-- 

### Affected APIs

- `M:System.Windows.Forms.DomainUpDown.DownButton`
- `M:System.Windows.Forms.DomainUpDown.UpButton`

-->
