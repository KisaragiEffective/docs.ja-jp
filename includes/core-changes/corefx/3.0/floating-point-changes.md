---
ms.openlocfilehash: ce4f09908b1025e8e5a0380c9bf035c6b0db479a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74568154"
---
### <a name="floating-point-formatting-and-parsing-behavior-changed"></a>浮動小数点の書式設定と解析の動作が変更されました

(<xref:System.Double> および <xref:System.Single> 型による) 浮動小数点の解析と書式設定の動作が IEEE に準拠するようになりました。

#### <a name="change-description"></a>変更の説明

.NET Core 2.2 以前のバージョンでは、<xref:System.Double.ToString%2A?displayProperty=nameWithType> および <xref:System.Single.ToString%2A?displayProperty=nameWithType> を使用した書式設定と <xref:System.Double.Parse%2A?displayProperty=nameWithType>、<xref:System.Double.TryParse%2A?displayProperty=nameWithType>、<xref:System.Single.Parse%2A?displayProperty=nameWithType>、および <xref:System.Single.TryParse%2A?displayProperty=nameWithType> を使用した解析は IEEE に準拠していません。 その結果、任意のサポートされている標準またはカスタムの書式設定文字列について、値が往復することは保証できません。 一部の入力では、書式設定された値を解析しようとして失敗する場合や、解析された値は元の値と同じではない場合があります。

.NET Core 3.0 以降、解析および書式設定操作は IEEE 754 に準拠しています。 これにより、.NET での浮動小数点型の動作が、C# などの IEEE 準拠言語の動作と一致するようになります。 詳細については、「[Floating-point parsing and formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)」(.NET Core 3.0 での浮動小数点の解析と書式設定の強化) に関するブログ記事を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

「[Floating-point parsing and formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)」(.NET Core 3.0 での浮動小数点の解析と書式設定の強化) に関するブログ記事の「Potential impact to existing code」(既存のコードに対して考えられる影響) セクションでは、.NET Core 2.2 アプリケーションと比較して動作の変化が見られた場合はコードの変更を提案しています。通常、これには、異なる標準またはカスタムの書式設定文字列を使用して目的の動作を強制することが含まれます。 一部の結果は、以前は正しくなかった場合、回避策がない場合があります。

#### <a name="category"></a>カテゴリ

CoreFx

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Double.ToString%2A?displayProperty=nameWithType>
- <xref:System.Single.ToString%2A?displayProperty=nameWithType>
- <xref:System.Double.Parse%2A?displayProperty=nameWithType>
- <xref:System.Double.TryParse%2A?displayProperty=nameWithType>
- <xref:System.Single.Parse%2A?displayProperty=nameWithType>
- <xref:System.Single.TryParse%2A?displayProperty=nameWithType>

<!-- 

### Affected APIs

- `Overload:System.Double.ToString`
- `Overload:System.Single.ToString`
- `Overload:System.Double.Parse`
- `Overload:System.Double.TryParse`
- `Overload:System.Single.Parse`
- `Overload:System.Single.TryParse`

-->
