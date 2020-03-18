---
title: ポリモーフィズム - C# プログラミング ガイド
ms.date: 02/08/2020
helpviewer_keywords:
- C# language, polymorphism
- polymorphism [C#]
ms.assetid: 086af969-29a5-4ce8-a993-0b7d53839dab
ms.openlocfilehash: 58980bd0d70d8a778cdb208f56d31ee8465871a4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170170"
---
# <a name="polymorphism-c-programming-guide"></a>ポリモーフィズム (C# プログラミング ガイド)

ポリモーフィズムは、カプセル化と継承に次ぐ、オブジェクト指向プログラミングの第 3 の柱と言われることがよくあります。 ポリモーフィズムは、ギリシャ語で "多形" を意味し、次の 2 つの側面を持っています。
  
- メソッド パラメーター、コレクション、配列などに渡された派生クラスのオブジェクトは、実行時に基底クラスのオブジェクトとして扱われることがあります。 このポリモーフィズムが発生すると、オブジェクトの宣言された型はその実行時の型と同じではなくなります。
- 基底クラスでは、"[virtual](../../language-reference/keywords/virtual.md) *メソッド*" を定義して実行できます。派生クラスでそれを[オーバーライド](../../language-reference/keywords/override.md)すると、独自の定義と実装を提供できます。 実行時には、クライアント コードがメソッドを呼び出したとき、CLR によってオブジェクトの実行時の型が検索され、仮想メソッドのオーバーライドが呼び出されます。 ソース コード内で、基底クラスでメソッドを呼び出し、そのメソッドの派生クラス版を実行することができます。

仮想メソッドを使用すると、関連するオブジェクトのグループを同一の方法で扱うことができます。 たとえば、描画サーフェイスにさまざまな種類の図形を作成できる描画アプリケーションがあるとします。 コンパイル時には、ユーザーがどのような種類の図形を作成するかわかりません。 しかし、アプリケーションでは、作成されたさまざまな種類の図形を追跡し、ユーザーのマウス操作に応じて更新する必要があります。 ポリモーフィズムを使用すると、2 つの基本的な手順でこの問題を解決できます。

1. 各図形クラスが共通の基底クラスから派生するようなクラス階層を作成します。
1. 仮想メソッドを使用して、基底クラスの 1 つのメソッドを呼び出すことで、派生クラスの適切なメソッドが呼び出されるようにします。

まず、`Shape` という基底クラスと、`Rectangle`、`Circle`、`Triangle` などの派生クラスを作成します。 `Shape` クラスで `Draw` という仮想メソッドを定義し、各派生クラスでそれをオーバーライドして、そのクラスが表す特定の図形を描画します。 `List<Shape>` オブジェクトを作成し、`Circle`、`Triangle`、`Rectangle` をそれに追加します。

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#PolymorphismOverview)]

描画サーフェイスを更新するには、[foreach](../../language-reference/keywords/foreach-in.md) ループを使用してリストを反復処理し、リスト内の各 `Shape` オブジェクトの `Draw` メソッドを呼び出します。 リスト内の各オブジェクトの宣言された型は `Shape` ですが、呼び出されるのは実行時の型 (それぞれの派生クラスでオーバーライドされたメソッド) になります。

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UsePolymorphism)]

C# では、すべての型がポリモーフィックです。これは、ユーザー定義型を含むすべての型が <xref:System.Object> から派生するためです。  

## <a name="polymorphism-overview"></a>ポリモーフィズムの概要

### <a name="virtual-members"></a>仮想メンバー

基底クラスから派生クラスを継承すると、派生クラスでは、基底クラスのすべてのメソッド、フィールド、プロパティ、イベントが継承されます。 派生クラスを設計するとき、仮想メソッドの動作についてはさまざまな選択肢があります。

- 派生クラスでは基底クラスの仮想メンバーがオーバーライドされ、新しい動作を定義できます。
- 派生クラスでは最も近い基底クラス メソッドがオーバーライドされることなく継承され、既存の動作が維持されますが、さらに派生したクラスではメソッドをオーバーライドできるようになります。
- 派生クラスでは、基底クラスの実装を隠ぺいするメンバーの非仮想実装を新しく定義できます。

派生クラスが基底クラスのメンバーをオーバーライドできるのは、基底クラスのメンバーが [virtual](../../language-reference/keywords/virtual.md) または [abstract](../../language-reference/keywords/abstract.md) として宣言されている場合だけです。 派生メンバーでは、[override](../../language-reference/keywords/override.md) キーワードを使用して、そのメソッドが仮想呼び出しに加わることを明示的に示す必要があります。 次にコード例を示します。

[!code-csharp[Virtual overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#VirtualMethods)]

フィールドは仮想にできません。仮想にできるのは、メソッド、プロパティ、イベント、インデクサーに限られます。 派生クラスが仮想メンバーをオーバーライドすると、派生クラスのメンバーは、そのクラスのインスタンスが基底クラスのインスタンスとしてアクセスされるときでも呼び出されます。 次にコード例を示します。

[!code-csharp[Virtual overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#VirtualMethods)]

仮想メソッドとプロパティを使用すると、派生クラスは、基底クラスのメソッドの実装を使用せずに基底クラスを拡張できます。 詳細については、「[Override キーワードと New キーワードによるバージョン管理](./versioning-with-the-override-and-new-keywords.md)」を参照してください。 1 つまたは一連のメソッドを定義し、その実装を派生クラスに任せるもう 1 つの方法として、インターフェイスがあります。 詳細については、「[インターフェイス](../interfaces/index.md)」を参照してください。

### <a name="hide-base-class-members-with-new-members"></a>新しいメンバーで基底クラス メンバーを隠ぺいする

基底クラスのメンバーと同じ名前を持つメンバーを派生クラスに与える場合、[new](../../language-reference/keywords/new-modifier.md) キーワードを使用し、基底クラス メンバーを隠ぺいできます。 `new` キーワードは、置き換えられるクラス メンバーの戻り値の型の前に配置します。 次にコード例を示します。

[!code-csharp[New method overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#NewMethods)]

基底クラスのメンバーが隠ぺいされても、派生クラスのインスタンスを基底クラスのインスタンスに型変換することで、クライアント コードからアクセスできます。 次に例を示します。

[!code-csharp[New method overview usage](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UseNewMethods)]

### <a name="prevent-derived-classes-from-overriding-virtual-members"></a>派生クラスで仮想メンバーのオーバーライドを禁止する  

仮想メンバーとそれを最初に宣言したクラスの間で宣言されているクラスの数に関係なく、仮想メンバーは仮想のままになります。 クラス `A` で仮想メンバーが宣言され、クラス `B` が `A` から派生し、クラス `C` が `B` から派生した場合、クラス `C` では仮想メンバーが継承され、そのメンバーのオーバーライドがクラス `B` で宣言されたかどうかに関係なく、仮想メンバーをオーバーライドできます。 次にコード例を示します。

[!code-csharp[Basic hierarchy](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#FirstHierarchy)]

派生クラスでは、オーバーライドを [sealed](../../language-reference/keywords/sealed.md) として宣言することで仮想継承を中止できます。 継承を止めるには、クラス メンバーの宣言で、`override` キーワードの前に `sealed` キーワードを指定する必要があります。 次にコード例を示します。

[!code-csharp[A sealed overridden member](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#SealedOverride)]

上の例では、メソッド `DoWork` は `C` から派生したあらゆるクラスに対して仮想になりません。 `C` のインスタンスの場合、型 `B` や型 `A` に型変換されたとしてでも、依然として仮想となります。 シール メソッドは、次のコード例に示すように、`new` キーワードを使用して派生クラスに置き換えることができます。

[!code-csharp[New method declaration](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#NewDeclaration)]

この場合、型 `D` の変数を利用して `D` で `DoWork` が呼び出されたとき、新しい `DoWork` が呼び出されます。 型 `C`、`B`、`A` の変数が `D` のインスタンスにアクセスする目的で使用される場合、`DoWork` の呼び出しは仮想継承の規則に準拠し、クラス `C` での `DoWork` の実装に呼び出しが転送されます。

### <a name="access-base-class-virtual-members-from-derived-classes"></a>派生クラスから基底クラスの仮想メンバーにアクセスする

メソッドやプロパティを置き換えたり、オーバーライドしたりした派生クラスでは、`base` キーワードを使用して、基底クラスのメソッドやプロパティに引き続きアクセスできます。 次にコード例を示します。

```csharp
public class Base
{
    public virtual void DoWork() {/*...*/ }
}
public class Derived : Base
{
    public override void DoWork()
    {
        //Perform Derived's work here
        //...
        // Call DoWork on base class
        base.DoWork();
    }
}
```

詳細については、「[base](../../language-reference/keywords/base.md)」を参照してください。

> [!NOTE]
> 仮想メンバーの場合、その固有の実装で `base` を使用して、その仮想メンバーの基底クラス実装を呼び出すことをお勧めします。 基底クラスの動作を実行できるようにすることで、派生クラスは、派生クラスに固有の動作を実装することに集中できます。 基底クラス実装を呼び出さない場合は、基底クラスの動作と互換性のある動作を派生クラスで実現する必要があります。

## <a name="in-this-section"></a>このセクションの内容

- [Override キーワードと New キーワードによるバージョン管理](./versioning-with-the-override-and-new-keywords.md)
- [Override キーワードと New キーワードを使用する場合について](./knowing-when-to-use-override-and-new-keywords.md)
- [ToString メソッドをオーバーライドする方法](./how-to-override-the-tostring-method.md)

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [継承](./inheritance.md)
- [抽象クラスとシール クラス、およびクラス メンバー](./abstract-and-sealed-classes-and-class-members.md)
- [メソッド](./methods.md)
- [イベント](../events/index.md)
- [プロパティ](./properties.md)
- [インデクサー](../indexers/index.md)
- [型](../types/index.md)
