---
title: try-catch-finally - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- catch-finally_CSharpKeyword
- catch-finally
helpviewer_keywords:
- finally blocks [C#]
- try-catch statement [C#]
ms.assetid: a1b443b0-ff7a-43ab-b835-0cc9bfbd15ca
ms.openlocfilehash: 5d98f6967595c7c32b23ba5422a8d9ca79f7f54c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713041"
---
# <a name="try-catch-finally-c-reference"></a>try-catch-finally (C# リファレンス)

通常、`catch` および `finally` は、`try` ブロックのリソースを取得して使用する場合に、対で記述されます。`catch` ブロックで例外的な状況を処理し、`finally` ブロックでリソースを解放します。

 例外の再スローの使用例を含む詳細については、「[try-catch](try-catch.md)」および[例外のスロー](../../../standard/exceptions/index.md)に関するページをご覧ください。 `finally` ブロックの詳細については、「[try-finally](try-finally.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[csrefKeywordsExceptions#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#1)]  

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/statements.md#the-try-statement)」の「[try ステートメント](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [try、throw、catch ステートメント (C++)](/cpp/cpp/try-throw-and-catch-statements-cpp)
- [throw](throw.md)
- [方法: 例外を明示的にスローする](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
- [using ステートメント](using-statement.md)
