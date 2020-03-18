---
title: インターフェイス - C# リファレンス
ms.date: 01/17/2020
f1_keywords:
- interface_CSharpKeyword
helpviewer_keywords:
- interface keyword [C#]
ms.assetid: 7da38e81-4f99-4bc5-b07d-c986b687eeba
ms.openlocfilehash: 473f5f8e226f0a144746ac943afcffdccd4777c7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77625853"
---
# <a name="no-loc-textinterface-c-reference"></a>:::no-loc text="interface"::: (C# リファレンス)

インターフェイスによりコントラクトが定義されます。 そのコントラクトを実装する [`class`](class.md) または [`struct`](../builtin-types/struct.md) では、インターフェイスで定義されているメンバーを実装する必要があります。 C# 8.0 以降では、インターフェイスによってメンバーの既定の実装を定義できます。 共通の機能を 1 回で実装する目的で [`static`](static.md) メンバーも定義できます。

次の例の `ImplementationClass` クラスは、`SampleMethod` を返す、パラメーターのない `void` メソッドを実装する必要があります。

使用例を含む詳細については、「[インターフェイス](../../programming-guide/interfaces/index.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[csrefKeywordsTypes#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#14)]

インターフェイスには、名前空間またはクラスのメンバーを指定できます。 インターフェイス宣言には、次のメンバーの宣言を含めることができます (実装なしのシグネチャ)。

- [メソッド](../../programming-guide/classes-and-structs/methods.md)
- [Properties](../../programming-guide/classes-and-structs/using-properties.md)
- [インデクサー](../../programming-guide/indexers/using-indexers.md)
- [イベント](event.md)

これらのメンバー宣言には通常、本文が含まれません。 C# 8.0 以降では、インターフェイス メンバーで本文を宣言できます。 これは*既定の実装*と呼ばれています。 本文のあるメンバーでは、実装がオーバーライドされないクラスおよび構造体に "既定の" 実装を与えることがインターフェイスに許可されます。 また、C# 8.0 以降、インターフェイスには次を含めることができます。

- [定数](const.md)
- [オペレーター](../operators/operator-overloading.md)
- [静的コンストラクター](../../programming-guide/classes-and-structs/constructors.md#static-constructors)。
- [入れ子にされた型](../../programming-guide/classes-and-structs/nested-types.md)
- [静的フィールド、メソッド、プロパティ、インデクサー、イベント](static.md)
- 明示的なインターフェイス実装構文を使用するメンバー宣言。
- 明示的なアクセス修飾子 (既定のアクセスは [`public`](access-modifiers.md))。

インターフェイスには、インスタンスの状態を含めることはできません。 静的フィールドが許可されるようになりましたが、インスタンス フィールドはインターフェイスでは許可されません。 [インスタンスの自動プロパティ](../../programming-guide/classes-and-structs/auto-implemented-properties.md)では暗黙的に隠しフィールドが宣言されることがあり、インターフェイスではサポートされていません。 このルールは、プロパティの宣言に微細な影響を与えます。 インターフェイス宣言では、次のコードによって、`class` や `struct` の場合のように自動実装プロパティが宣言されることはありません。 その代わり、既定の実装が与えられないが、インターフェイスを実装する何らかの型で実装する必要があるプロパティが宣言されます。

```csharp
public interface INamed
{
  public string Name {get; set;}
}
```

インターフェイスは、1 つ以上の基底インターフェイスから継承できます。 あるインターフェイスで基底インターフェイスに実装されている[メソッドがオーバーライドされる](override.md)とき、そのインターフェイスでは[明示的なインターフェイス実装](../../programming-guide/interfaces/explicit-interface-implementation.md)構文を使用する必要があります。

基本型のリストに基底クラスとインターフェイスが含まれる場合は、基底クラスがリストの最初に表示されます。

インターフェイスを実装するクラスは、そのインターフェイスのメンバーを明示的に実装できます。 明示的に実装されているメンバーには、クラス インスタンスではアクセスできません。インターフェイスのインスタンスを使用した場合にのみアクセスできます。 また、既定のインターフェイス メンバーには、インターフェイスのインスタンス経由でのみアクセスできます。

明示的なインターフェイス実装の詳細については、「[明示的なインターフェイスの実装](../../programming-guide/interfaces/explicit-interface-implementation.md)」を参照してください。

## <a name="example"></a>例

ここでは、インターフェイスの実装例を示します。 この例では、インターフェイスにプロパティ宣言が含まれ、クラスに実装が含まれます。 `IPoint` を実装するクラスのインスタンスには、整数プロパティ `x` および `y` が含まれています。

[!code-csharp[csrefKeywordsTypes#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#15)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/interfaces.md)の「[インターフェイス](~/_csharplang/spec/introduction.md)」セクションと [C# 8.0 の既定のインターフェイス メンバー](~/_csharplang/proposals/csharp-8.0/default-interface-methods.md)に関するページを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [参照型](reference-types.md)
- [インターフェイス](../../programming-guide/interfaces/index.md)
- [プロパティの使用](../../programming-guide/classes-and-structs/using-properties.md)
- [インデクサーの使用](../../programming-guide/indexers/using-indexers.md)
- [インターフェイス](../../programming-guide/interfaces/index.md)
