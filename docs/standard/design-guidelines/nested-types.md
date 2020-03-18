---
title: Nested Types
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- types, nested
- public nested types
- type design guidelines, nested types
- nested types
- members [.NET Framework], type
- class library design guidelines [.NET Framework], nested types
ms.assetid: 12feb7f0-b793-4d96-b090-42d6473bab8c
ms.openlocfilehash: dd13116b13ac8e2d7a3af6ef014eb4f393909515
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743705"
---
# <a name="nested-types"></a>Nested Types
入れ子になった型は、外側の型と呼ばれる別の型のスコープ内で定義された型です。 入れ子になった型は、それを囲む型のすべてのメンバーにアクセスできます。 たとえば、外側の型で定義されているプライベートフィールドと、それを囲む型のすべての先祖で定義されている保護されたフィールドにアクセスできます。

 一般に、入れ子になった型は控えめに使用する必要があります。 直接呼び出すべきではないいくつかの理由があります。 一部の開発者は、この概念について完全には理解していません。 たとえば、これらの開発者は、入れ子になった型の変数を宣言する構文で問題が発生する可能性があります。 入れ子になった型もその外側の型と密接に結び付いています。そのため、汎用型には適していません。

 入れ子になった型は、それを囲む型の実装の詳細をモデリングする場合に最適です。 エンドユーザーは、入れ子にされた型の変数を宣言する必要はほとんどなく、入れ子になった型を明示的にインスタンス化する必要はほとんどありません。 たとえば、コレクションの列挙子は、そのコレクションの入れ子にされた型にすることができます。 列挙子は、通常、それを囲む型によってインスタンス化されます。また、多くの言語では foreach ステートメントがサポートされるため、列挙子の変数はエンドユーザーが宣言することはほとんどありません。

 入れ子になった型とその外側の型のリレーションシップが望ましい場合は、入れ子にされた型を使用✔️ます。

 ❌ は、入れ子になったパブリック型を論理グループ化構成体として使用しないでください。このには名前空間を使用します。

 ❌ は、入れ子になった型を公開しないようにします。 唯一の例外は、入れ子にされた型の変数は、サブクラス化やその他の高度なカスタマイズシナリオなど、まれなシナリオでのみ宣言する必要がある場合に限られます。

 型が含まれている型の外で参照される可能性がある場合は、入れ子にされた型を使用しないように ❌ します。

 たとえば、クラスで定義されたメソッドに渡される列挙型は、クラスで入れ子にされた型として定義することはできません。

 入れ子になった型は、クライアントコードでインスタンス化する必要がある場合は ❌ 使用しないでください。  型にパブリックコンストラクターがある場合は、入れ子になっていない可能性があります。

 型がインスタンス化できる場合、型がそれ自体のフレームワーク内に配置されていることを示しているように見えます (作成、操作、外部型を使用せずに破棄できます)。したがって、入れ子にすることはできません。 外部型に関係なく、外側の型の外部で内部型を広く再利用することはできません。

 ❌ は、入れ子にされた型をインターフェイスのメンバーとして定義しないでください。 多くの言語では、このようなコンストラクトはサポートされていません。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [型デザインのガイドライン](../../../docs/standard/design-guidelines/type.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
