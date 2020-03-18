---
title: do - C# リファレンス
ms.date: 05/28/2018
f1_keywords:
- do_CSharpKeyword
- do
helpviewer_keywords:
- do keyword [C#]
ms.assetid: 50725f79-9ba6-4898-aa78-6e331568a1bb
ms.openlocfilehash: 38224ce70c19ff67ad80b99d3da52155849f1341
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713597"
---
# <a name="do-c-reference"></a>do (C# リファレンス)

`do` ステートメントでは、指定されたブール式が `true` と評価される間、ステートメントまたはステートメント ブロックが実行されます。 ループの各実行の後に式が評価されるため、`do-while` ループは 1 回以上実行されます。 [while](while.md) ループは、これとは異なり、0 回以上実行されます。

`do` ステートメント ブロック内の任意の位置で、[break](break.md) ステートメントを使用してループを抜けることができます。

`while`continue[ ステートメントを使用すると、](continue.md) 式の評価に直接ステップ実行できます。 式の評価が `true` の場合、ループの最初のステートメントから実行が続行されます。 それ以外の場合、実行は、ループの後の最初のステートメントから続行されます。

また、`do-while`goto[、](goto.md)return[、](return.md)throw[ ステートメントのいずれかを使って ](throw.md) ループを終了することもできます。

## <a name="example"></a>例

`do` ステートメントの使用方法を次の例に示します。 **[実行]** を選択して、コード例を実行します。 その後に、コードを変更し、もう一度実行することができます。

[!code-csharp-interactive[do loop example](~/samples/snippets/csharp/keywords/IterationKeywordsExamples.cs#4)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/statements.md#the-do-statement)」の [do ステートメント](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [while ステートメント](while.md)
