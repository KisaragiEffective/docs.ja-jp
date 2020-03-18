---
title: 等価演算子
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], Equals method
- class library design guidelines [.NET Framework], equality operator
- equality operator (==) [.NET Framework]
- Equals method
- == operator (equality) [.NET Framework]
ms.assetid: bc496a91-fefb-4ce0-ab4c-61f09964119a
ms.openlocfilehash: 34fc8eef5270369419b76899f0dbe1ace106caf6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741694"
---
# <a name="equality-operators"></a>等価演算子
ここでは、等値演算子のオーバーロードと、等値演算子として `operator==` と `operator!=` を参照する方法について説明します。

 ❌ は、等値演算子の1つをオーバーロードしません。

 ✔️ <xref:System.Object.Equals%2A?displayProperty=nameWithType> と等値演算子のセマンティクスと同様のパフォーマンス特性がまったく同じであることを確認します。

 これは、多くの場合、等値演算子がオーバーロードされるときに `Object.Equals` をオーバーライドする必要があることを意味します。

 ❌ は、等値演算子から例外をスローしないようにします。

 たとえば、`NullReferenceException`をスローするのではなく、いずれかの引数が null の場合、false を返します。

## <a name="equality-operators-on-value-types"></a>値型の等値演算子
 等しい場合は、値型に対して等値演算子をオーバーロード✔️ます。

 ほとんどのプログラミング言語では、値型に `operator==` の既定の実装はありません。

## <a name="equality-operators-on-reference-types"></a>参照型の等値演算子
 変更可能な参照型での等値演算子のオーバーロードを回避 ❌。

 多くの言語には、参照型の等価演算子が組み込まれています。 これらの組み込み演算子は、通常、参照の等価性を実装します。多くの開発者は、既定の動作が値の等価性に変更されると驚かれます。

 不変性によって参照の等価性と値の等価性の違いが非常に困難になるため、この問題は変更できない参照型に対しては緩和されます。

 実装が参照の等価性よりも大幅に遅くなる場合は、参照型に対して等値演算子をオーバーロードしないように ❌ します。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [使用方法のガイドライン](../../../docs/standard/design-guidelines/usage-guidelines.md)
