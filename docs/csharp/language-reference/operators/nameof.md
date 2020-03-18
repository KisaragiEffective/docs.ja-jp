---
title: nameof 演算子 - C# リファレンス
ms.date: 07/12/2019
f1_keywords:
- nameof_CSharpKeyword
- nameof
helpviewer_keywords:
- nameof operator [C#]
ms.assetid: 33601bf3-cc2c-4496-846d-f9679bccf2a7
ms.openlocfilehash: ffbc801acf61bf72db1c88912dc2142a478fa280
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78846273"
---
# <a name="nameof-operator-c-reference"></a>nameof 演算子 (C# リファレンス)

`nameof` 演算子を使うと、変数、型、またはメンバーの名前を文字列定数として取得できます。

[!code-csharp-interactive[nameof operator](snippets/NameOfOperator.cs#Examples)]

前の例で示されているように、型と名前空間の場合、生成される名前は通常[完全修飾](~/_csharplang/spec/basic-concepts.md#fully-qualified-names)ではありません。

`nameof` 演算子はコンパイル時に評価され、実行時には影響を与えません。

`nameof` 演算子を使って、引数をチェックするコードを保守しやすくすることができます。

[!code-csharp[nameof and argument check](snippets/NameOfOperator.cs#ExceptionMessage)]

`nameof` 演算子は C# 6 以降で使用できます。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#nameof-expressions)の「[Nameof 式](~/_csharplang/spec/introduction.md)」セクションをご覧ください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
