---
title: '@ - C# リファレンス'
ms.date: 02/09/2017
f1_keywords:
- '@_CSharpKeyword'
- '@'
helpviewer_keywords:
- '@ special character [C#]'
- '@ language element [C#]'
ms.assetid: 89bc7e53-85f5-478a-866d-1cca003c4e8c
ms.openlocfilehash: a3446eceb0d3c415e36ea1d2c7d8d6d34f65350d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712417"
---
# <a name="-c-reference"></a>@ (C# リファレンス)

特殊文字 `@` は、逐語的識別子として機能します。 次のように使用できます。

1. C# のキーワードを識別子として使用できるようにする。 コード要素のプレフィックスとして `@` 文字を使用すると、その要素はC# のキーワードではなく、識別子としてコンパイラに解釈されます。 次の例では、`@` 文字を使用して、`for` ループで使用する `for` という識別子を定義しています。

   [!code-csharp[verbatim1](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#1)]

1. 文字列リテラルを逐語的に解釈することを示す。 このインスタンス内の `@` 文字は、*逐語的文字列リテラル*を定義します。 単純なエスケープ シーケンス (バック スラッシュの `"\\"` など)、16 進数のエスケープ シーケンス (大文字 A の `"\x0041"` など)、Unicode のエスケープ シーケンス (大文字 A の `"\u0041"` など) は、リテラルに解釈されます。 引用符のエスケープ シーケンス (`""`) だけは、リテラルに解釈することはできません。一重引用符が生成されます。 また、逐語的な[補間文字列](interpolated.md)の場合、中かっこエスケープ シーケンス (`{{` と `}}`) は文字どおり解釈されません。単一の中かっこ文字が生成されます。 次の例では、2 つの同じファイル パスを定義しています。一方は通常の文字列リテラルを使用して、もう一方は 逐語的文字列リテラルを使用して定義しています。 これは、逐語的文字列リテラルの一般的な用途の 1 つです。

   [!code-csharp[verbatim2](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#2)]

   次の例は、同じ文字シーケンスを通常の文字列リテラルと 逐語的文字列リテラルで定義した場合の結果を示したものです。

   [!code-csharp[verbatim3](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#3)]

1. 名前の競合がある場合に、コンパイラが属性を区別できるようにする。 属性は <xref:System.Attribute> の派生クラスです。 通常、その型の名前には **Attribute** サフィックスが含まれます。これは、コンパイラがその規則を強制していない場合でも同様です。 そのため属性は、完全な型名 (たとえば、`[InfoAttribute]`) か、短縮名 (たとえば、`[Info]`) によってコード内から参照できます。 ただし、短縮された 2 つの属性型名が同じである場合、一方の型名に **Attribute** サフィックスが含まれていて、もう一方に含まれていないと、名前の競合が発生します。 たとえば、次のコードでは、`Info` と `InfoAttribute` のどちらの属性が `Example` クラスに適用されるかをコンパイラが判断できないため、コンパイルが失敗します。 詳細については、[CS1614](../compiler-messages/cs1614.md) を参照してください。

   [!code-csharp[verbatim4](../../../../samples/snippets/csharp/language-reference/keywords/verbatim2.cs#1)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# の特殊文字](./index.md)
