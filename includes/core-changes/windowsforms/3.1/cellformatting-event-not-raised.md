---
ms.openlocfilehash: add3ff8faed2e7fab245e5b6f1b9158b7bdd06f5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74567373"
---
### <a name="cellformatting-event-not-raised-if-tooltip-is-shown"></a>ヒントが表示されていると CellFormatting が発生しない

マウスでポイントしたときと、キーボードで選択したときに、<xref:System.Windows.Forms.DataGridView> にセルのテキストとエラーのヒントが表示されるようになりました。 ヒントが表示されている場合、<xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType> イベントは発生しません。

#### <a name="change-description"></a>変更の説明

.NET Core 3.1 より前、<xref:System.Windows.Forms.DataGridView> プロパティが <xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A> に設定されている `true` では、セルにマウスでポイントしたときにセルのテキストとエラーのヒントが表示されていました。 セルをキーボード (たとえば、Tab キー、ショートカット キー、矢印ナビゲーションなどを使用して) から選択したときはヒントが表示されませんでした。 ユーザーがセルを編集したときに <xref:System.Windows.Forms.DataGridView> がまだ編集モードだった場合に <xref:System.Windows.Forms.DataGridViewCell.ToolTipText> プロパティが設定されていないセルをポイントすると、セルのテキストをセルに表示する書式設定をするための <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントが発生しました。

.NET Core 3.1 以降では、アクセシビリティの標準を満たすため、<xref:System.Windows.Forms.DataGridView> プロパティが <xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A> に設定されている `true` には、セルがポイントされたときだけでなく、キーボードを使用して選択されたときにも、セルのテキストとエラーのヒントが表示されます。 この変更の結果、<xref:System.Windows.Forms.DataGridView.CellFormatting> が編集モードの間に  *プロパティが設定されていないセルをポイントしても、* イベントは発生<xref:System.Windows.Forms.DataGridViewCell.ToolTipText>しません<xref:System.Windows.Forms.DataGridView>。 イベントが発生しないのは、ポイントされたセルの内容が、セルに表示される代わりにヒントとして表示されるためです。

#### <a name="version-introduced"></a>導入されたバージョン

3.1

#### <a name="recommended-action"></a>推奨アクション

<xref:System.Windows.Forms.DataGridView.CellFormatting> が編集モードのときに、<xref:System.Windows.Forms.DataGridView> イベントに依存するすべてのコードをリファクターします。

#### <a name="category"></a>カテゴリ

Windows フォーム

#### <a name="affected-apis"></a>影響を受ける API

API 分析では検出できません。

<!-- 

### Affected APIs

- Not detectable via API analysis.

-->
