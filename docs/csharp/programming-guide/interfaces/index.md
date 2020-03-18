---
title: インターフェイス - C# プログラミング ガイド
ms.date: 02/20/2020
helpviewer_keywords:
- interfaces [C#]
- C# language, interfaces
ms.assetid: 2feda177-ce11-432d-81b4-d50f5f35fd37
ms.openlocfilehash: f4ee269f41e79562c113a7627816f797b083095e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79157079"
---
# <a name="interfaces-c-programming-guide"></a>インターフェイス (C# プログラミング ガイド)

インターフェイスには、非抽象[クラス](../../language-reference/keywords/class.md)または[構造体](../../language-reference/builtin-types/struct.md)で実装する必要がある、関連する機能のグループに対する定義が含まれます。 インターフェイスを使用すると、実装が必要な `static` メソッドを定義することができます。 インターフェイスでは、宣言されたインスタンス メンバーの一部またはすべてに既定の実装を提供できます。 インターフェイスでは、フィールド、自動実装プロパティ、プロパティに似たイベントなどのインスタンス データを宣言できません。

インターフェイスを使用すると、たとえば、クラス内の複数のソースからの動作を含めることができます。 C# ではクラスの複数の継承がサポートされないため、この機能は重要です。 また、構造体の継承をシミュレートする場合はインターフェイスを使用する必要があります。これは、実際に別の構造体またはクラスから継承することができないためです。

インターフェイスを定義するには、次の例に示すように、[interface](../../language-reference/keywords/interface.md) キーワードを使用します。

[!code-csharp[Equatable](~/samples/snippets/csharp/objectoriented/interfaces.cs#Equatable)]

インターフェイスの名前を、有効な C# の[識別子名](../inside-a-program/identifier-names.md)にする必要があります。 慣例により、インターフェイス名は大文字の `I` で始めます。

<xref:System.IEquatable%601> インターフェイスを実装するすべてのクラスまたは構造体は、インターフェイスで指定されたシグネチャに一致する <xref:System.IEquatable%601.Equals%2A> メソッドの定義を含む必要があります。 したがって、`IEquatable<T>` を実装するクラスが `Equals` メソッドを含むと想定したうえで、これを使用してクラスの 1 つのインスタンスが同じクラスの別のインスタンスと等しいかどうかを判定できます。

`IEquatable<T>` の定義は `Equals` の実装を提供しません。 クラスまたは構造体には複数のインターフェイスを実装できます。ただし、クラスは 1 つのクラスからのみ継承できます。

抽象クラスの詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](../classes-and-structs/abstract-and-sealed-classes-and-class-members.md)」を参照してください。

インターフェイスには、instance メソッド、プロパティ、イベント、インデクサー、またはこれらの 4 種類のメンバーの任意の組み合わせを含めることができます。 インターフェイスには、静的コンストラクター、フィールド、定数、または演算子を含めることができます。 例へのリンクについては、「[関連項目](./index.md#BKMK_RelatedSections)」を参照してください。 インターフェイスには、インスタンス フィールド、インスタンス コンストラクター、またはファイナライザーを含めることができません。 インターフェイス メンバーは、既定でパブリックです。

インターフェイスのメンバーを実装するには、実装するクラスの対応するメンバーがパブリックかつ非静的であり、インターフェイスのメンバーと同じ名前およびシグネチャを持つ必要があります。

クラスまたは構造体でインターフェイスを実装する場合、インターフェイスによって宣言され、既定の実装が提供されないすべてのメンバーについて、クラスまたは構造体で実装を提供する必要があります。 ただし、基本クラスでインターフェイスが実装される場合、その基本クラスから派生するすべてのクラスはその実装を継承します。

<xref:System.IEquatable%601> インターフェイスを実装する例を次に示します。 実装するクラスの `Car` は、<xref:System.IEquatable%601.Equals%2A> メソッドの実装を提供する必要があります。

[!code-csharp[ImplementEquatable](~/samples/snippets/csharp/objectoriented/interfaces.cs#ImplementEquatable)]

クラスのプロパティとインデクサーでは、インターフェイスに定義されているプロパティまたはインデクサーの追加のアクセサーを定義できます。 たとえば、インターフェイスで [get](../../language-reference/keywords/get.md) アクセサーを持つプロパティを宣言するとします。 このインターフェイスを実装するクラスでは、`get` アクセサーと [set](../../language-reference/keywords/set.md) アクセサーの両方を持つ同じプロパティを宣言できます。 ただし、プロパティまたはインデクサーで明示的な実装を使用する場合は、これらのアクセサーが一致する必要があります。 明示的な実装の詳細については、「[明示的なインターフェイス実装](explicit-interface-implementation.md)」および「[インターフェイスのプロパティ](../classes-and-structs/interface-properties.md)」を参照してください。

インターフェイスは、1 つ以上のインターフェイスから継承できます。 派生インターフェイスは、その基本インターフェイスからメンバーを継承します。 派生インターフェイスを実装するクラスでは、派生インターフェイスの基底インターフェイスのすべてのメンバーを含め、派生インターフェイスのすべてのメンバーを実装する必要があります。 そのクラスは、暗黙的に派生インターフェイスまたはその基底インターフェイスのいずれかに変換されます。 クラスは、継承する基底クラス、または他のインターフェイスが継承するインターフェイスを介して、インターフェイスを複数回含めることができます。 ただし、クラスでインターフェイスの実装を提供できるのは 1 回のみであり、それもクラスでクラスの定義の一部としてインターフェイスを宣言する場合 (`class ClassName : InterfaceName`) に限られます。 インターフェイスを実装する基本クラスを継承することによってインターフェイスを継承する場合、基本クラスは、そのインターフェイスのメンバーの実装を提供します。 ただし、派生クラスでは、継承された実装を使用する代わりに、任意の仮想インターフェイスのメンバーを再実装できます。 インターフェイスでメソッドの既定の実装を宣言すると、そのインターフェイスを実装したクラスにはその実装が継承されます。 インターフェイスで定義された実装は仮想であり、実装クラスを使ってその実装をオーバーライドできます。

また、基本クラスでは、仮想メンバーを使用して、インターフェイスのメンバーを実装することもできます。 その場合、派生クラスでは、仮想メンバーをオーバーライドすることでインターフェイスの動作を変更できます。 仮想メンバーの詳細については、「[ポリモーフィズム](../classes-and-structs/polymorphism.md)」を参照してください。

## <a name="interfaces-summary"></a>インターフェイスの概要

インターフェイスは、次の特性を持ちます。

- 通常、インターフェイスは抽象メンバーのみを含む抽象基底クラスに似ています。 インターフェイスを実装するすべてのクラスまたは構造体では、そのすべてのメンバーを実装する必要があります。 必要に応じて、そのメンバーの一部またはすべての既定の実装をインターフェイスで定義できます。
- インターフェイスを直接インスタンス化することはできません。 そのメンバーは、インターフェイスを実装する任意のクラスまたは構造体によって実装されます。
- クラスまたは構造体は、複数のインターフェイスを実装できます。 クラスは、基本クラスを継承する一方で、1 つまたは複数のインターフェイスを実装できます。

## <a name="BKMK_RelatedSections"></a> 関連セクション

- [インターフェイスのプロパティ](../classes-and-structs/interface-properties.md)  
- [インターフェイスのインデクサー](../indexers/indexers-in-interfaces.md)  
- [インターフェイス イベントを実装する方法](../events/how-to-implement-interface-events.md)
- [クラスと構造体](../classes-and-structs/index.md)  
- [継承](../classes-and-structs/inheritance.md)  
- [メソッド](../classes-and-structs/methods.md)  
- [ポリモーフィズム](../classes-and-structs/polymorphism.md)  
- [抽象クラスとシール クラス、およびクラス メンバー](../classes-and-structs/abstract-and-sealed-classes-and-class-members.md)  
- [プロパティ](../classes-and-structs/properties.md)  
- [イベント](../events/index.md)  
- [インデクサー](../indexers/index.md)  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [継承](../classes-and-structs/inheritance.md)
- [識別子名](../inside-a-program/identifier-names.md)
