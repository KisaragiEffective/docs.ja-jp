---
title: throw - C# リファレンス
ms.date: 03/02/2015
f1_keywords:
- throw
- throw_CSharpKeyword
helpviewer_keywords:
- throw statement [C#]
- throw expression [C#]
- throw keyword [C#]
ms.assetid: 5ac4feef-4b1a-4c61-aeb4-61d549e5dd42
ms.openlocfilehash: 04d3138e3390627355b4b2d4e25c6b00248cec1a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398113"
---
# <a name="throw-c-reference"></a>throw (C# リファレンス)

プログラムの実行中に例外が発生したことを通知します。  
  
## <a name="remarks"></a>解説

`throw` の構文は次のとおりです。

```csharp
throw [e];
```

ここで `e` は <xref:System.Exception?displayProperty=nameWithType> から派生したクラスのインスタンスです。 次の例では、`throw` という名前のメソッドに渡された引数が内部配列の有効なインデックスに対応していない場合に、<xref:System.IndexOutOfRangeException> ステートメントを使用して `GetNumber` をスローします。

[!code-csharp[csrefKeyword#1](~/samples/snippets/csharp/language-reference/keywords/throw/throw-1.cs#1)]

その後、メソッドの呼び出し元が `try-catch` ブロックまたは `try-catch-finally` ブロックを使用して、スローされた例外を処理します。 次の例では、`GetNumber` メソッドによってスローされた例外を処理します。

[!code-csharp[csrefKeyword#2](~/samples/snippets/csharp/language-reference/keywords/throw/throw-1.cs#2)]

## <a name="re-throwing-an-exception"></a>例外の再スロー

`throw` を `catch` ブロックで使用すると、`catch` ブロックで処理された例外を再スローすることもできます。  この場合、`throw` は例外オペランドを使用しません。 これは、メソッドが呼び出し元から他のライブラリ メソッドに引数を渡し、そのライブラリ メソッドが、呼び出し元に渡す必要がある例外をスローするときに最も役立ちます。 たとえば、次の例では、初期化されていない文字列の最初の文字を取得しようとしたときにスローされた <xref:System.NullReferenceException> を再スローします。

[!code-csharp[csrefKeyword#3](~/samples/snippets/csharp/language-reference/keywords/throw/throw-3.cs#3)]

> [!IMPORTANT]
> `throw e` ブロックで `catch` 構文を使用すると、呼び出し元に渡す新しい例外をインスタンス化することもできます。 この場合、<xref:System.Exception.StackTrace> プロパティから使用できる、元の例外のスタック トレースが保持されません。

## <a name="the-throw-expression"></a>`throw` 式

C# 7.0 以降、`throw` は、式およびステートメントとして使用できます。 これにより、以前サポートされていなかったコンテキストでの例外のスローが可能になります。 チェックの内容は次のとおりです

- [条件演算子](../operators/conditional-operator.md)。 次の例では、`throw` 式を使用して、メソッドに空の文字列配列が渡された場合に <xref:System.ArgumentException> をスローします。 C# 7.0 より前では、このロジックが `if`/`else` ステートメントで使用されている必要があります。

   [!code-csharp[csrefKeyword#4](~/samples/snippets/csharp/language-reference/keywords/throw/conditional.cs#1)]

- [ull 合体演算子](../operators/null-coalescing-operator.md)。 次の例では、null 合体演算子と共に `throw` 式を使用して、`Name` プロパティに割り当てられた文字列が `null` の場合に例外をスローします。

   [!code-csharp[csrefKeyword#5](~/samples/snippets/csharp/language-reference/keywords/throw/coalescing.cs#1)]

- 式形式の[ラムダ](../../programming-guide/statements-expressions-operators/lambda-expressions.md)またはメソッド。 次の例では、<xref:System.InvalidCastException> 値への変換がサポートされていないため <xref:System.DateTime> をスローする、式形式のメソッドを示しています。

   [!code-csharp[csrefKeyword#6](~/samples/snippets/csharp/language-reference/keywords/throw/exp-bodied.cs#1)]

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [try-catch](try-catch.md)
- [C# のキーワード](index.md)
- [方法: 例外を明示的にスローする](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
