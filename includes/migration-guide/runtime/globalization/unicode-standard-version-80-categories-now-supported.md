---
ms.openlocfilehash: efa0efaf40e2e432d477f1659d7bde3abc98241d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857535"
---
### <a name="unicode-standard-version-80-categories-now-supported"></a>Unicode 標準バージョン 8.0 のカテゴリのサポート開始

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 で、Unicode データが Unicode 標準バージョン 6.3 からバージョン 8.0 にアップグレードされました。  .NET Framework 4.6.2 で Unicode 文字カテゴリを要求すると、いくつかの結果が以前の .NET Framework バージョンの結果と一致しない可能性があります。  この変更は、主にチェロキーの音節、新タイ ロ文字の母音記号および声調記号に影響します。|
|提案される解決策|コードを確認し、ハードコーディングされた Unicode 文字カテゴリに依存するロジックを削除/変更します。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Char.GetUnicodeCategory(System.Char)?displayProperty=nameWithType></li><li><xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.Char)?displayProperty=nameWithType></li><li><xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.String,System.Int32)?displayProperty=nameWithType></li></ul>|
