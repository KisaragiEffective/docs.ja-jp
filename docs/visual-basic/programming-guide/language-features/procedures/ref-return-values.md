---
title: Ref 戻り値
ms.date: 04/28/2017
helpviewer_keywords:
- variables [Visual Basic]
- ref return values [Visual Basic]
- ref returns [Visual Basic]
ms.assetid: 5ef0cc69-eb3a-4a67-92a2-78585f223cb5
ms.openlocfilehash: 2d2a302a899fbde549161469f281d3e580bcb71f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352537"
---
# <a name="support-for-reference-return-values-visual-basic"></a>参照戻り値のサポート (Visual Basic)

C# 7.0 以降の言語でC#は、*参照戻り値*がサポートされています。 参照戻り値を理解する方法の1つは、メソッドへの参照によって渡される引数の逆であることです。 参照によって渡された引数が変更されると、変更は呼び出し元の変数の値に反映されます。 メソッドが呼び出し元に参照戻り値を提供すると、呼び出し元によって参照戻り値に加えられた変更が、呼び出されたメソッドのデータに反映されます。

Visual Basic では、参照戻り値を使用してメソッドを作成することはできませんが、参照戻り値を使用することはできます。 言い換えると、参照戻り値を指定してメソッドを呼び出し、その戻り値を変更することができます。また、参照戻り値への変更が呼び出されたメソッドのデータに反映されます。

## <a name="modifying-the-ref-return-value-directly"></a>Ref 戻り値の直接変更

常に成功し `ByRef` パラメーターを持たないメソッドの場合は、参照戻り値を直接変更できます。 これを行うには、参照戻り値を返す式に新しい値を代入します。

次C#の例では、内部値をインクリメントし、それを参照戻り値として返す `NumericValue.IncrementValue` メソッドを定義します。

[!code-csharp[Ref-Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/procedures/ref-returns1.cs)]

次の Visual Basic 例では、呼び出し元によって参照戻り値が変更されます。 `NumericValue.IncrementValue` メソッドの呼び出しを含む行では、メソッドに値が割り当てられないことに注意してください。 代わりに、メソッドによって返される参照戻り値に値を割り当てます。

[!code-vb[Ref-Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/procedures/use-ref-returns1.vb)]

## <a name="using-a-helper-method"></a>ヘルパーメソッドの使用

それ以外の場合は、メソッド呼び出しの参照戻り値を直接変更することは必ずしも望ましいとは限りません。 たとえば、文字列を返す検索メソッドは、常に一致するとは限りません。 その場合は、検索が成功した場合にのみ、参照の戻り値を変更します。

次C#の例は、このシナリオを示しています。 ここでは、でC#記述された `Sentence` クラスに、指定した部分文字列で始まる文の次の単語を検索する `FindNext` メソッドが含まれています。 文字列は参照戻り値として返され、参照によりメソッドに渡される `Boolean` 変数は検索が成功したかどうかを示します。 参照戻り値は、呼び出し元が戻り値を読み取ることができないことを示します。また、変更することもできます。この変更は、`Sentence` クラスに内部的に含まれるデータに反映されます。

[!code-csharp[Ref-Return](../../../../../samples/snippets/visualbasic/getting-started/ref-returns.cs)]

この場合、参照戻り値を直接変更しても信頼できません。これは、メソッドの呼び出しで一致が検出されず、文の最初の単語が返される可能性があるためです。 その場合、呼び出し元は誤って文の最初の単語を変更します。 これは、呼び出し元が `null` (または Visual Basic で `Nothing`) を返すことによって阻止される可能性があります。 ただし、その場合は、値が `Nothing` 文字列を変更しようとすると、<xref:System.NullReferenceException>がスローされます。 呼び出し元が <xref:System.String.Empty?displayProperty=nameWithType>を返すことによってもを回避できる場合がありますが、そのためには、値が <xref:System.String.Empty?displayProperty=nameWithType>である文字列変数を呼び出し元で定義する必要があります。 呼び出し元はこの文字列を変更することができますが、変更した文字列には、`Sentence` クラスによって格納されている文の単語との関係がないため、変更自体は目的がありません。

このシナリオを処理する最善の方法は、参照によって参照の戻り値をヘルパーメソッドに渡すことです。 ヘルパーメソッドには、メソッド呼び出しが成功したかどうかを判断するロジックが含まれています。成功した場合は、参照戻り値を変更します。 次の例では、可能な実装を示します。

[!code-vb[Ref-Return](../../../../../samples/snippets/visualbasic/getting-started/ref-return-helper.vb#1)]

## <a name="see-also"></a>関連項目

- [引数の値渡しと参照渡し](passing-arguments-by-value-and-by-reference.md)
- [Visual Basic におけるプロシージャ](index.md)
