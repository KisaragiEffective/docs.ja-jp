---
title: override 修飾子 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- override
- override_CSharpKeyword
helpviewer_keywords:
- override keyword [C#]
ms.assetid: dd1907a8-acf8-46d3-80b9-c2ca4febada8
ms.openlocfilehash: acad3aa3b196c184132ad1acdf52b18a799b0896
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713249"
---
# <a name="override-c-reference"></a>override (C# リファレンス)

`override` 修飾子は、継承したメソッド、プロパティ、インデクサー、またはイベントの抽象実装または仮想実装を拡張したり修飾したりする際に必要です。

## <a name="example"></a>例

この例では、`Square` クラスが `GetArea` のオーバーライドされる実装を提供する必要があります。これは、`GetArea` が `Shape` 抽象クラスから継承されているためです。

[!code-csharp[csrefKeywordsModifiers#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#1)]

`override` メソッドは、基底クラスから継承されるメンバーの新しい実装を提供します。 `override` 宣言によってオーバーライドされるメソッドを、オーバーライドされる基本メソッドと言います。 オーバーライドされる基本メソッドには、`override` メソッドと同じシグネチャが必要です。 継承については、「[継承](../../programming-guide/classes-and-structs/inheritance.md)」を参照してください。

非仮想メソッドまたは静的メソッドをオーバーライドすることはできません。 オーバーライドされる基本メソッドは、`virtual`、`abstract`、`override` のいずれかである必要があります。

`override` 宣言は、`virtual` メソッドのアクセシビリティを変更できません。 `override` メソッドと `virtual` メソッドの両方に、同じ[アクセス レベル修飾子](access-modifiers.md)が必要です。

`new` メソッドの修飾に、修飾子 `static`、`virtual`、または `override` は使用できません。

オーバーライドするプロパティの宣言では、継承されるプロパティとまったく同じアクセス修飾子、型、および名前を指定する必要があります。オーバーライドされるプロパティは、`virtual`、`abstract`、または `override` である必要があります。

`override` キーワードの使い方の詳細については、「[Override キーワードと New キーワードによるバージョン管理](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)」および「[Override キーワードと New キーワードを使用する場合について](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)」を参照してください。

## <a name="example"></a>例

この例では、`Employee` という基底クラスと、`SalesEmployee` という派生クラスを定義します。 `SalesEmployee` クラスには追加のフィールド `salesbonus` があり、このフィールドを処理に含めるために、`CalculatePay` メソッドをオーバーライドします。

[!code-csharp[csrefKeywordsModifiers#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#9)]

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [継承](../../programming-guide/classes-and-structs/inheritance.md)
- [C# のキーワード](index.md)
- [修飾子](index.md)
- [abstract](abstract.md)
- [virtual](virtual.md)
- [new (修飾子)](new-modifier.md)
- [ポリモーフィズム](../../programming-guide/classes-and-structs/polymorphism.md)
