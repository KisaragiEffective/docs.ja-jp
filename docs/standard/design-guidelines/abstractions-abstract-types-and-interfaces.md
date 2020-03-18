---
title: 抽象化 (抽象型およびインターフェイス)
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- interfaces [.NET Framework], abstract
- abstract interfaces [.NET Framework]
- abstract types [.NET Framework]
- types [.NET Framework], abstract
ms.assetid: 0a632bc7-9b03-44ee-8842-c82f88672a45
ms.openlocfilehash: 6a4f511af72aad916d367153090504e2a8e11cb8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741818"
---
# <a name="abstractions-abstract-types-and-interfaces"></a>抽象化 (抽象型およびインターフェイス)
抽象はコントラクトを記述する型ですが、コントラクトの完全な実装は提供しません。 抽象化は通常、抽象クラスまたは抽象インターフェイスとして実装され、適切に定義された一連の参照ドキュメントに含まれており、コントラクトを実装する型の必要なセマンティクスについて説明します。 .NET Framework の最も重要な抽象化には、<xref:System.IO.Stream>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Object>があります。

 抽象化のコントラクトをサポートする具象型を実装し、抽象化を使用するフレームワーク Api でこの具象型を使用することによって、フレームワークを拡張できます。

 時間のテストに耐えられる意味のある有用な抽象化は、設計が非常に困難です。 主な難しさは、メンバーの適切なセットを取得することです。これは、それ以上ではありません。 抽象化に含まれるメンバーが多すぎると、の実装が困難になるか、または不可能になります。 約束された機能のメンバーが少なすぎると、多くの興味深いシナリオでは役に立たなくなります。

 フレームワークの抽象化が多すぎると、フレームワークのユーザビリティにも悪影響を及ぼします。 多くの場合、抽象化を理解することは、具象実装の大きな画像や、抽象化で動作する Api にどのように適合するかを理解することでは非常に困難です。 また、抽象化とそのメンバーの名前は必ずしも抽象的なものです。そのため、多くの場合、使用状況のより広範なコンテキストを理解する必要がありません。

 ただし、抽象化によって、他の拡張メカニズムが一致しないことが多い非常に強力な拡張性が提供されます。 これらは、プラグイン、制御の反転 (IoC)、パイプラインなど、さまざまなアーキテクチャパターンの中核になっています。 また、フレームワークのテスト性をテストするうえで非常に重要です。 優れた抽象化によって、単体テストの目的で、高い依存関係をスタブ化できます。 要約すると、抽象化は、最新のオブジェクト指向フレームワークによって検出された結果を処理します。

 抽象化を使用するいくつかの具象実装と Api を開発してテストしている場合を除き、抽象化を提供しない ❌ ます。

 抽象化を設計するときは、抽象クラスとインターフェイスの間で慎重に選択する✔️ます。

 ✔️抽象化の具象実装の参照テストを提供することを検討してください。 このようなテストでは、実装でコントラクトが正しく実装されているかどうかをテストすることができます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [機能拡張のデザイン](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
