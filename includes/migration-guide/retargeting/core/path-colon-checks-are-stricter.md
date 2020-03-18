---
ms.openlocfilehash: a51738fa75ba2dd4574549fce2570df8231c4cae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859255"
---
### <a name="path-colon-checks-are-stricter"></a>パスのコロン チェックがより厳密になった

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 では、以前はサポートされていなかったパスをサポートするために (長さと形式の両方で) 数多くの変更が加えられました。 適切なドライブ区切り (コロン) 構文の確認がより正確に行われました。その結果、それ以前は許容されていた、ごく少数のパス API の一部の URI パスがブロックされました。|
|提案される解決策|影響を受ける API に URI を渡す場合、まず、文字列を正規のパスに変更します。<ul><li>URL から手動でスキームを削除します (たとえば、URL から <code>file://</code> を削除します)。</li><li><xref:System.Uri> クラスに URI を渡し、<xref:System.Uri.LocalPath> を使用します。</li></ul>あるいは、<code>Switch.System.IO.UseLegacyPathHandling</code> AppContext スイッチを true に設定し、新しいパス正規化を無効にできます。|
|スコープ|エッジ|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.IO.Path.GetDirectoryName(System.String)?displayProperty=nameWithType></li><li><xref:System.IO.Path.GetPathRoot(System.String)?displayProperty=nameWithType></li></ul>|
