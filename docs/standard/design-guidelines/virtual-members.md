---
title: 仮想メンバー
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- overridable members
- virtual members
- members [.NET Framework], virtual
ms.assetid: 8ff4eb97-0364-43ec-8a02-934b5cd94d19
ms.openlocfilehash: 8ed519a01162056151d8ae6398c0d06495911afd
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743531"
---
# <a name="virtual-members"></a>仮想メンバー
仮想メンバーをオーバーライドして、サブクラスの動作を変更することができます。 これらの関数は、提供される機能拡張に関してコールバックとよく似ていますが、実行のパフォーマンスとメモリの消費に関しては優れています。 また、仮想メンバーは、特別な種類の既存の型 (特殊化) を作成する必要があるシナリオでは、より自然に感じられます。

 仮想メンバーは、コールバックとイベントよりもパフォーマンスが優れていますが、非仮想メソッドよりもパフォーマンスは高くありません。

 仮想メンバーの主な欠点は、仮想メンバーの動作は、コンパイル時にのみ変更できることです。 コールバックの動作は、実行時に変更できます。

 仮想メンバーは、コールバック (またはコールバックを超える可能性があります) のように、設計、テスト、および保守にコストがかかります。これは、仮想メンバーへの呼び出しを予測不可能な方法でオーバーライドし、任意のコードを実行できるためです。 また、仮想メンバーのコントラクトを明確に定義するために、通常はより多くの労力が必要になります。そのため、これらを設計および文書化するコストが高くなります。

 仮想メンバーの設計、テスト、および保守に関連するすべてのコストを把握している場合を除き、メンバーを仮想メンバーにしないでください ❌ します。

 仮想メンバーは、互換性を損なうことなく変更することができます。 また、仮想メンバーへの呼び出しはインライン化されないため、ほとんどの場合、仮想メンバー以外のメンバーよりも低速になります。

 ✔️、必要な機能拡張に限定することを検討してください。

 ✔️は、仮想メンバーに対してパブリックアクセシビリティよりも保護されたアクセシビリティを優先します。 パブリックメンバーは、保護された仮想メンバーを呼び出すことによって、拡張機能を提供する必要があります (必要な場合)。

 クラスのパブリックメンバーは、そのクラスの直接のコンシューマーに適切な一連の機能を提供する必要があります。 仮想メンバーはサブクラスでオーバーライドされるように設計されており、保護されたアクセシビリティは、すべての仮想拡張ポイントを使用できる場所にスコープを設定するための優れた方法です。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [機能拡張のデザイン](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
