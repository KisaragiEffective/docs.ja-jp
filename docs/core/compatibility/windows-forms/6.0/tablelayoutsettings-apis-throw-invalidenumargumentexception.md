---
title: 破壊的変更:一部の TableLayoutSettings プロパティで InvalidEnumArgumentException がスローされる
description: .NET 6.0 での破壊的変更について学習します。一部の TableLayoutSettings API で、無効な引数に対する InvalidEnumArgumentException がスローされるようになりました。
ms.date: 01/18/2021
ms.openlocfilehash: 8397952e4647347718f11597a100c5d764e7186b
ms.sourcegitcommit: f8cd3ef517ee177c99feed944824c27d208cc0d1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570245"
---
# <a name="selected-tablelayoutsettings-properties-throw-invalidenumargumentexception"></a>選択された TableLayoutSettings プロパティで InvalidEnumArgumentException がスローされる

正しくない値を割り当てようとすると、選択された <xref:System.Windows.Forms.TableLayoutSettings> プロパティで <xref:System.ComponentModel.InvalidEnumArgumentException> がスローされるようになりました。

## <a name="change-description"></a>変更の説明

以前のバージョンの .NET では、正しくない値を割り当てようとすると、これらのプロパティで <xref:System.ArgumentOutOfRangeException> がスローされます。 .NET 6.0 以降では、そのような場合にこれらのプロパティで <xref:System.ComponentModel.InvalidEnumArgumentException> がスローされます。

## <a name="reason-for-change"></a>変更理由

<xref:System.ComponentModel.InvalidEnumArgumentException> のスローは、同様の状況での既存の Windows フォーム API に従っています。 また、この例外をスローすると、開発者のデバッグ エクスペリエンスが向上します。

## <a name="version-introduced"></a>導入されたバージョン

.NET 6.0

## <a name="recommended-action"></a>推奨アクション

- 正しくない値が割り当てられないように、コードを更新します。
- 必要に応じて、これらの API にアクセスするときに <xref:System.ComponentModel.InvalidEnumArgumentException> を処理します。

## <a name="affected-apis"></a>影響を受ける API

次の表に、影響を受けるプロパティを一覧表示します。

| プロパティ | 変更されたバージョン |
|-|-|-|-|
| <xref:System.Windows.Forms.TableLayoutPanel.CellBorderStyle?displayProperty=fullName> | Preview 1 |
| <xref:System.Windows.Forms.TableLayoutPanel.GrowStyle?displayProperty=fullName> | Preview 1 |

<!--

### Affected APIs

- `P:System.Windows.Forms.TableLayoutPanel.CellBorderStyle`
- `P:System.Windows.Forms.TableLayoutPanel.GrowStyle`

### Category

Windows Forms

-->
