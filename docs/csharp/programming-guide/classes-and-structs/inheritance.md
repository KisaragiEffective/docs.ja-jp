---
title: 継承 - C# プログラミング ガイド
ms.date: 02/07/2020
helpviewer_keywords:
- abstract methods [C#]
- abstract classes [C#]
- inheritance [C#]
- derived classes [C#]
- virtual methods [C#]
- C# language, inheritance
ms.assetid: 81d64ee4-50f9-4d6c-a8dc-257c348d2eea
ms.openlocfilehash: 448b1695a4afc50f4afa20383e5fda280b9f12e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77626660"
---
# <a name="inheritance-c-programming-guide"></a>継承 (C# プログラミング ガイド)

継承は、カプセル化およびポリモーフィズムと共に、オブジェクト指向プログラミングの主要な 3 つの特性の 1 つです。 継承を使用すると、他のクラスで定義されている動作を再利用、拡張、変更して新しいクラスを作成できます。 メンバーが継承される側のクラスを "*基底クラス*" と呼び、メンバーを継承する側のクラスを "*派生クラス*" と呼びます。 派生クラスは、直接の基底クラスを 1 つだけ持つことができます。 ただし、継承は推移的です。 `ClassC` が `ClassB` から派生し、`ClassB` が `ClassA` から派生している場合、`ClassC` では、`ClassB` と `ClassA` で宣言されているメンバーが継承されます。

> [!NOTE]
> 構造体では、継承がサポートされていませんが、インターフェイスを実装することはできます。 詳細については、「[インターフェイス](../interfaces/index.md)」を参照してください。

概念的には、派生クラスは基底クラスから特化したクラスです。 たとえば、基底クラス `Animal` がある場合、`Mammal` という名前の派生クラスと、`Reptile` という名前の別の派生クラスを持つことができます。 `Mammal` は `Animal` であり、`Reptile` も `Animal` ですが、各派生クラスは、基底クラスから別々の特殊化を表します。

インターフェイス宣言では、そのメンバーの既定の実装を定義できます。 そのような実装は、派生インターフェイスと派生インターフェイスを実装するクラスによって継承されます。 既定のインターフェイス メソッドの詳細については、[インターフェイス](../../language-reference/keywords/interface.md)に関する記事の言語リファレンスのセクションをご覧ください。

クラスを別のクラスから派生するように定義すると、派生クラスには、コンストラクターとファイナライザーを除く、基底クラスのすべてのメンバーが暗黙的に引き継がれます。 派生クラスでは、基底クラスのコードを再実装することなく再利用します。 派生クラスでは、メンバーを追加できます。 派生クラスでは、基底クラスの機能が拡張されます。

次の図は、あるビジネス プロセスの作業項目を表す `WorkItem` クラスを示しています。 他のすべてのクラスと同様に、<xref:System.Object?displayProperty=nameWithType> から派生し、そのすべてのメソッドを継承します。 `WorkItem` には、独自のメンバーが 5 つ追加されています。 そのようなメンバーにはコンストラクターが含まれます。コンストラクターは継承されないためです。 `WorkItem` から継承される `ChangeRequest` クラスは、特定の種類の作業項目を表します。 `ChangeRequest` には、`WorkItem` と <xref:System.Object> から継承したメンバーに 2 つのメンバーが追加されます。 独自のコンストラクターを追加する必要があるほか、さらに `originalItemID` も追加されます。 `originalItemID` プロパティを使用すると、`ChangeRequest` インスタンスは、変更要求が適用される元の `WorkItem` と関連付けることができます。

![クラスの継承を示す図](./media/inheritance/class-inheritance-diagram.png)

次の例は、前の図に示したクラスの関係が C# でどのように表現されるかを示しています。 また、`WorkItem` が仮想メソッド <xref:System.Object.ToString%2A?displayProperty=nameWithType> をオーバーライドする方法と、`ChangeRequest` クラスが `WorkItem` によるメソッドの実装を継承する方法も示しています。 最初のブロックでクラスが定義されます。

[!code-csharp[DefineClasses](~/samples/snippets/csharp/objectoriented/inheritance.cs#Classes)]

この次のブロックでは、基底クラスと派生クラスの使用方法が示されます。

[!code-csharp[UseClasses](~/samples/snippets/csharp/objectoriented/inheritance.cs#UseClasses)]

## <a name="abstract-and-virtual-methods"></a>抽象メソッドと仮想メソッド

基底クラスによってメソッドが [`virtual`](../../language-reference/keywords/virtual.md) として宣言されるとき、派生クラスでは、その独自の実装でそのメソッドを[`override`](../../language-reference/keywords/override.md)できます。 基底クラスによってメンバーが [`abstract`](../../language-reference/keywords/abstract.md) として宣言される場合、そのクラスから直接継承されるあらゆる非抽象クラスでそのメソッドをオーバーライドする必要があります。 派生クラス自体が抽象クラスである場合は、抽象メンバーを実装することなく継承します。 抽象メンバーと仮想メンバーは、オブジェクト指向プログラミングの重要な特性の 2 つ目であるポリモーフィズムの基礎です。 詳細については、「[ポリモーフィズム](./polymorphism.md)」を参照してください。

## <a name="abstract-base-classes"></a>抽象基底クラス

[new](../../language-reference/operators/new-operator.md) 演算子を使用して直接インスタンス化されないようにする場合は、クラスを [abstract](../../language-reference/keywords/abstract.md) として宣言できます。 抽象クラスは、新しいクラスがそれから誘導される場合にのみ利用できます。 抽象クラスには、それ自体が abstract として宣言された 1 つ以上のメソッド シグネチャを含めることができます。 これらのシグネチャは、パラメーターと戻り値を指定しますが、実装 (メソッドの本体) は持ちません。 抽象クラスには抽象メンバーを含める必要がありません。ただし、クラスに抽象メンバーが含まれる場合、そのクラス自体を抽象として宣言する必要があります。 それ自体が抽象ではない派生クラスの場合、抽象基底クラスから抽象メソッドを実装する必要があります。 詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](abstract-and-sealed-classes-and-class-members.md)」を参照してください。

## <a name="interfaces"></a>インターフェイス

"*インターフェイス*" は、一連のメンバーを定義する参照型です。 そのインターフェイスを実装するすべてのクラスと構造体で、その一連のメンバーを実装する必要があります。 インターフェイスでは、それらのメンバーの一部または全部に対して既定の実装を定義できます。 クラスは、直接的には 1 つの基底クラスからしか派生できませんが、複数のインターフェイスを実装できます。

インターフェイスは、"is a" 関係を必ずしも持たないクラスに特定の機能を定義する目的で使用されます。 たとえば、<xref:System.IEquatable%601?displayProperty=nameWithType> インターフェイスをクラスまたは構造体によって実装し、特定の型の 2 つのオブジェクトが等しいかどうかを判断できます (もっとも等価性は型で定義されます)。 <xref:System.IEquatable%601> では、基底クラスと派生クラスの間に存在する同じ種類の "is a" 関係 (たとえば、`Mammal` は `Animal` である) を意味しません。 詳細については、「[インターフェイス](../interfaces/index.md)」を参照してください。

## <a name="preventing-further-derivation"></a>後続の派生を禁止する  

クラスでは、それ自体かメンバーを [`sealed`](../../language-reference/keywords/sealed.md) として宣言することで、他のクラスがそのクラスまたはそのメンバーから継承することを禁止できます。 詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](./abstract-and-sealed-classes-and-class-members.md)」を参照してください。

## <a name="derived-class-hiding-of-base-class-members"></a>派生クラスで基底クラス メンバーを隠ぺいする  

派生クラスは、同じ名前とシグネチャでメンバーを宣言することで、基底クラスのメンバーを隠ぺいすることができます。 [`new`](../../language-reference/keywords/new-modifier.md) 修飾子を利用すると、メンバーが基底メンバーのオーバーライドではないことを明示的に示すことができます。 [`new`](../../language-reference/keywords/new-modifier.md) の使用は必須ではありませんが、[`new`](../../language-reference/keywords/new-modifier.md) が使用されていない場合、コンパイラの警告が生成されます。 詳細については、「[Override キーワードと New キーワードによるバージョン管理](./versioning-with-the-override-and-new-keywords.md)」および「[Override キーワードと New キーワードを使用する場合について](./knowing-when-to-use-override-and-new-keywords.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [クラスと構造体](./index.md)
- [class](../../language-reference/keywords/class.md)
