---
title: unsafe キーワード - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- unsafe_CSharpKeyword
- unsafe
helpviewer_keywords:
- unsafe keyword [C#]
ms.assetid: 7e818009-1c6e-4b9e-b769-3728a01586a0
ms.openlocfilehash: ef98809eae0329c028dfb318c4a437aae4736db1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712989"
---
# <a name="unsafe-c-reference"></a>unsafe (C# リファレンス)

`unsafe` キーワードは、ポインターに関連するすべての操作に必要な、unsafe コンテキストを示します。 詳しくは、「[アンセーフ コードとポインター](../../programming-guide/unsafe-code-pointers/index.md)」をご覧ください。

`unsafe` 修飾子は、型またはメンバーの宣言で使用できます。 そのため、型やメンバーの全体的なテキスト範囲が unsafe コンテキストと見なされます。 たとえば、次に示すのは、`unsafe` 修飾子を使用して宣言されたメソッドです。

```csharp
unsafe static void FastCopy(byte[] src, byte[] dst, int count)
{
    // Unsafe context: can use pointers here.
}
```

unsafe コンテキストのスコープはパラメーター リストからメソッドの末尾までなので、ポインターはパラメーター リストでも使用できます。

```csharp
unsafe static void FastCopy ( byte* ps, byte* pd, int count ) {...}
```

また、unsafe ブロックを使用して、そのブロック内で unsafe コードを使用できるようにすることもできます。 次に例を示します。

```csharp
unsafe
{
    // Unsafe context: can use pointers here.
}
```

アンセーフ コードをコンパイルするには、[`-unsafe`](../compiler-options/unsafe-compiler-option.md) コンパイラ オプションを指定する必要があります。 unsafe コードは、共通言語ランタイムでは検証できません。

## <a name="example"></a>例

[!code-csharp[csrefKeywordsModifiers#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#22)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語の仕様](~/_csharplang/spec/unsafe-code.md)」の[アンセーフ コード](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [fixed ステートメント](fixed-statement.md)
- [アンセーフ コードとポインター](../../programming-guide/unsafe-code-pointers/index.md)
- [固定サイズ バッファー](../../programming-guide/unsafe-code-pointers/fixed-size-buffers.md)
