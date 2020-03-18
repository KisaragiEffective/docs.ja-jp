---
title: シール
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- limiting extensibility
- classes [.NET Framework], sealing
- preventing customization
- sealed classes
ms.assetid: cc42267f-bb7a-427a-845e-df97408528d4
ms.openlocfilehash: ddf463e98fb6a9c5ccaae90e9cfc74b5691b13a9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743626"
---
# <a name="sealing"></a>シール
オブジェクト指向フレームワークの機能の1つは、開発者がフレームワークデザイナーによって予期されない方法で拡張およびカスタマイズできることです。 これは、拡張可能な設計の力と危険性の両方です。 フレームワークを設計するときは、必要に応じて拡張機能を慎重に設計し、危険な場合には拡張性を制限することが非常に重要です。

 拡張性を防ぐための強力なメカニズムが封印されています。 クラスまたは個々のメンバーを封印できます。 クラスをシールすると、ユーザーはクラスから継承できなくなります。 メンバーを封印すると、ユーザーは特定のメンバーをオーバーライドできなくなります。

 ❌ は、適切な理由がなくてもクラスを封印しないでください。

 拡張シナリオを考慮することができないため、クラスをシールすることは適切な理由ではありません。 利便性のあるメンバーを追加するなど、さまざまな理由からクラスを継承しようとしているフレームワークユーザー。 ユーザーが型から継承することを望まない明確な理由の例については、「封印されていない[クラス](../../../docs/standard/design-guidelines/unsealed-classes.md)」を参照してください。

 クラスを封印するには、次のような理由があります。

- クラスは静的クラスです。 「[静的クラスの設計](../../../docs/standard/design-guidelines/static-class.md)」を参照してください。

- クラスは、セキュリティに依存する機密情報を継承されたメンバーに格納します。

- クラスは多くの仮想メンバーを継承し、それらを個別にシールするコストは、クラスをシールしたままにする利点よりも大きくなります。

- クラスは、非常に高速なランタイム参照を必要とする属性です。 シールされた属性のパフォーマンスレベルは、封印されていない属性よりも若干高くなります。 「[属性](../../../docs/standard/design-guidelines/attributes.md)」を参照してください。

 ❌ は、シールド型で protected または virtual メンバーを宣言しません。

 定義上、sealed 型をから継承することはできません。 これは、シールされた型のプロテクトメンバーを呼び出すことができず、シール型の仮想メソッドをオーバーライドできないことを意味します。

 オーバーライドするメンバーの封印を検討✔️。

 仮想メンバーの導入によって発生する可能性のある問題 (「[仮想メンバー](../../../docs/standard/design-guidelines/virtual-members.md)」で説明) は、オーバーライドにも適用されます (ただし、若干低い程度)。 上書きを封印すると、継承階層内のその時点から、これらの問題を防ぐことができます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [機能拡張のデザイン](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
- [シールされていないクラス](../../../docs/standard/design-guidelines/unsealed-classes.md)
