---
title: sealed 修飾子 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- sealed
- sealed_CSharpKeyword
helpviewer_keywords:
- sealed keyword [C#]
ms.assetid: 8e4ed5d3-10be-47db-9488-0da2008e6f3f
ms.openlocfilehash: 686afd5d9d0394bb1a802551b65083732599f384
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713106"
---
# <a name="sealed-c-reference"></a>sealed (C# リファレンス)

`sealed` 修飾子をクラスに適用すると、それ以外のクラスが、そのクラスから継承できなくなります。 次の例では、`B` クラスは `A` クラスを継承しますが、`B` クラスからはどのクラスも継承できなくなります。

```csharp
class A {}
sealed class B : A {}
```

`sealed` 修飾子は、基底クラスの仮想メソッドまたは仮想プロパティをオーバーライドするメソッドやプロパティで使用することもできます。 これにより、クラスの派生が行えるようになり、そのクラスが特定の仮想メソッドまたは仮想プロパティをオーバーライドできなくなります。

## <a name="example"></a>例

次の例では、`Z` は `Y` から継承しますが、`Z` は仮想関数 `F` をオーバーライドできません。この仮想関数は `X` で宣言されており、`Y` でシールされています。

[!code-csharp[csrefKeywordsModifiers#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#16)]

新しいメソッドまたはプロパティをクラスで定義するときに、派生クラスによるオーバーライドを防ぐには、その派生クラスを [virtual](virtual.md) として宣言しないようにします。

[abstract](abstract.md) 修飾子をシール クラスで使用するとエラーになります。抽象メソッドまたは抽象プロパティを実装するクラスでは、抽象クラスを継承する必要があるためです。

`sealed` 修飾子は、メソッドまたはプロパティに適用するときは、常に [override](override.md) と一緒に使用する必要があります。

構造体は暗黙的にシールされるため、継承できません。

詳細については、「[継承](../../programming-guide/classes-and-structs/inheritance.md)」を参照してください。

上記以外の例については、「[抽象クラスとシール クラス、およびクラス メンバー](../../programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[csrefKeywordsModifiers#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#17)]

前の例で、次のステートメントを使用して、シール クラスからの継承を試みたとします。

`class MyDerivedC: SealedClass {}   // Error`

この場合、次のエラー メッセージが表示されます。

`'MyDerivedC': cannot derive from sealed type 'SealedClass'`

## <a name="remarks"></a>解説

クラス、メソッド、またはプロパティをシールするかどうかを判断するには、通常、次の 2 つの点を検討する必要があります。

- クラスをカスタマイズすることで、派生クラスにもたらされる可能性があるメリット。

- 派生クラスがクラスを変更することで、そのクラスが正常に、または期待どおりに機能しなくなる可能性。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [静的クラスと静的クラス メンバー](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [抽象クラスとシール クラス、およびクラス メンバー](../../programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [アクセス修飾子](../../programming-guide/classes-and-structs/access-modifiers.md)
- [修飾子](index.md)
- [override](override.md)
- [virtual](virtual.md)
