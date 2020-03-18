---
ms.openlocfilehash: bfe406161ac754124a2cc38c68a80c3b9fb2c7f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858422"
---
### <a name="persian-calendar-now-uses-the-hijri-solar-algorithm"></a>ペルシャ暦でイスラム暦の太陽アルゴリズムが使用されるようになった

|   |   |
|---|---|
|説明|.NET Framework 4.6 以降では、<xref:System.Globalization.PersianCalendar?displayProperty=name> クラスでイスラム暦の太陽アルゴリズムが使用されます。 <xref:System.Globalization.PersianCalendar?displayProperty=name> と他のカレンダーの間で日付を変換すると、.NET Framework 4.6 以降では、1800 年より前か 2023 年 (グレゴリオ暦) よりも後の日付について、わずかに異なる結果になることがあります。また、<xref:System.Globalization.PersianCalendar.MinSupportedDateTime?displayProperty=nameWithType> は <code>March 22, 0622</code> ではなく <code>March 21, 0622</code> になります。|
|提案される解決策|.NET Framework 4.6 で PersianCalendar を使用するときには、一部の早い日付または遅い日付に若干の差が生じる場合があることに注意してください。 また、異なるバージョンの .NET Framework で実行する可能性のあるプロセス間で日付をシリアル化するときには、PersianCalendar の日付文字列として格納しないでください (これらの値が異なる場合があるため)。|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Globalization.PersianCalendar?displayProperty=nameWithType></li></ul>|
