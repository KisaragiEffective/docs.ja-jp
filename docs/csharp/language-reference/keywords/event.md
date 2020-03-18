---
title: event - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- event
- remove
- event_CSharpKeyword
- add
helpviewer_keywords:
- event keyword [C#]
ms.assetid: 7858fd85-153b-4259-85d0-6aa13c35f174
ms.openlocfilehash: eb1805ed55921497fea88e6b39989c876ef003d1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713559"
---
# <a name="event-c-reference"></a>event (C# リファレンス)

`event` キーワードを使用し、パブリッシャー クラス内にイベントを宣言します。

## <a name="example"></a>例

次の例では、基になるデリゲート型として <xref:System.EventHandler> を使用するイベントを宣言し、発生させる方法について説明します。 完全なコード例については、「<xref:System.EventHandler%601>.NET Framework ガイドラインに準拠したイベントを発行する方法[」を参照してください。完全なコード例では、ジェネリック ](../../programming-guide/events/how-to-publish-events-that-conform-to-net-framework-guidelines.md) デリゲート型の使用方法と、イベントをサブスクライブして、イベント ハンドラー メソッドを作成する方法も確認できます。

[!code-csharp[csrefKeywordsModifiers#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#7)]

イベントは、宣言元 (パブリッシャー クラス) のクラスまたは構造体内でしか呼び出せない特殊なマルチキャスト デリゲートです。 他のクラスまたは構造体がイベントを受信登録した場合、パブリッシャー クラスがイベントを発生させると、他のクラスまたは構造体のイベント ハンドラー メソッドが呼び出されます。 詳細およびコード例については、「[イベント](../../programming-guide/events/index.md)」および「[デリゲート](../../programming-guide/delegates/index.md)」を参照してください。

イベントは、[public](./public.md)、[private](./private.md)、[protected](./protected.md)、[internal](./internal.md)、[protected internal](./protected-internal.md)、または [private protected](./private-protected.md) としてマークできます。 これらのアクセス修飾子により、クラスのユーザーがイベントにアクセスする方法が定義されます。 詳細については、「[アクセス修飾子](../../programming-guide/classes-and-structs/access-modifiers.md)」を参照してください。

## <a name="keywords-and-events"></a>キーワードとイベント

イベントには次のキーワードが適用されます。

|キーワード|[説明]|詳細情報|
|-------------|-----------------|--------------------------|
|[static](./static.md)|クラスのインスタンスが存在しない場合でも、呼び出し元がいつでもイベントを使用できるようになります。|[静的クラスと静的クラス メンバー](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)|
|[virtual](./virtual.md)|[override](./override.md) キーワードを使用してイベントの動作をオーバーライドすることを派生クラスに許可します。|[継承](../../programming-guide/classes-and-structs/inheritance.md)|
|[sealed](./sealed.md)|派生クラスでは、それが仮想ではなくなったことを指定します。||
|[abstract](./abstract.md)|コンパイラはイベント アクセサー ブロックの `add` と `remove` を生成しません。したがって、派生クラスは固有の実装を提供する必要があります。||

イベントは、[static](./static.md) キーワードを使用して静的イベントとして宣言されることもあります。 その場合、クラスのインスタンスが存在しなくても、呼び出し元がいつでもイベントを使用できるようになります。 詳細については、「[静的クラスと静的クラス メンバー](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)」を参照してください。

イベントは、[virtual](./virtual.md) キーワードを使用して仮想イベントとしてマークできます。 その場合、派生クラスでは、[override](./override.md) キーワードを使用してイベントの動作をオーバーライドできます。 詳細については、「[継承](../../programming-guide/classes-and-structs/inheritance.md)」を参照してください。 仮想イベントをオーバーライドするイベントは、[sealed](./sealed.md) にすることもできます。その場合、派生クラスでは、イベントが仮想でなくなります。 最後に、イベントは [abstract](./abstract.md) として宣言できます。その場合、コンパイラはイベント アクセサー ブロックの `add` と `remove` を生成しません。 したがって、派生クラスごとに固有の実装を提供する必要があります。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](./index.md)
- [add](./add.md)
- [remove](./remove.md)
- [修飾子](index.md)
- [デリゲートを結合する方法 (マルチキャスト デリゲート)](../../programming-guide/delegates/how-to-combine-delegates-multicast-delegates.md)
