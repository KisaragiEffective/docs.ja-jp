---
ms.openlocfilehash: 3eab49acd3eaa5b6d5802af5f4e6f0fe2699ee97
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937066"
---
### <a name="uselegacyimages-compatibility-switch-not-supported"></a>UseLegacyImages 互換性スイッチはサポートされていません

.NET Framework 4.8 で導入された `Switch.System.Windows.Forms.UseLegacyImages` 互換性スイッチは、.NET Core 3.0 上の Windows フォームではサポートされていません。

#### <a name="change-description"></a>変更の説明

.NET Framework 4.8 以降、`Switch.System.Windows.Forms.UseLegacyImages` 互換性スイッチでは、高 DPI 環境での ClickOnce シナリオで考えられるイメージ スケーリングの問題に対処しています。 `true` に設定してこのスイッチを使用すると、スケールが 100% より大きく設定されている高 DPI ディスプレイで、レガシ イメージのスケーリングを復元できます。 詳細については、GitHub 上の [.NET Framework 4.8 のリリース ノート](https://github.com/microsoft/dotnet/blob/master/releases/net48/dotnet48-changes.md#clickonce)を参照してください。

.NET Core では、`Switch.System.Windows.Forms.UseLegacyImages` スイッチはサポートされていません。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 9

#### <a name="recommended-action"></a>推奨アクション

スイッチを削除します。 そのスイッチはサポートされておらず、代替機能はありません。

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

- なし

<!-- 

### Affected APIs

- Not detectable via API analysis

-->
