---
title: public キーワード - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- public
- public_CSharpKeyword
helpviewer_keywords:
- public keyword [C#]
ms.assetid: 0ae45d16-a551-4b74-9845-57208de1328e
ms.openlocfilehash: 19906d7fd0f7d41ef9e4cdaf951c77825e0bbead
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713171"
---
# <a name="public-c-reference"></a>public (C# リファレンス)

`public` キーワードは、型と型のメンバーを示すアクセス修飾子です。 パブリック アクセスは、最も制限の少ないアクセス レベルです。 次の例のように、パブリック メンバーへのアクセスには制限がありません。

```csharp
class SampleClass
{
    public int x; // No access restrictions.
}
```

詳しくは、「[アクセス修飾子](../../programming-guide/classes-and-structs/access-modifiers.md)」および「[アクセシビリティ レベル](accessibility-levels.md)」をご覧ください。

## <a name="example"></a>例

次の例では、2 つのクラス (`PointTest` と `MainClass`) が宣言されています。 `x` のパブリック メンバー `y` および `PointTest` は、`MainClass` から直接アクセスされます。

[!code-csharp[csrefKeywordsModifiers#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#13)]

`public` アクセス レベルを [private](private.md) または [protected](protected.md) に変更すると、次のエラー メッセージが表示されます。

'PointTest.y' はアクセスできない保護レベルになっています。

## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語仕様](~/_csharplang/spec/basic-concepts.md#declared-accessibility)」の[宣言されたアクセシビリティ](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [アクセス修飾子](../../programming-guide/classes-and-structs/access-modifiers.md)
- [C# のキーワード](index.md)
- [アクセス修飾子](access-modifiers.md)
- [アクセシビリティ レベル](accessibility-levels.md)
- [修飾子](index.md)
- [private](private.md)
- [protected](protected.md)
- [internal](internal.md)
