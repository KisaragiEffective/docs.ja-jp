---
title: + および += 演算子 - C# リファレンス
ms.date: 05/24/2019
f1_keywords:
- +_CSharpKeyword
- +=_CSharpKeyword
helpviewer_keywords:
- addition operator [C#]
- concatenation operator [C#]
- delegate combination [C#]
- + operator [C#]
- addition assignment operator [C#]
- event subscription [C#]
- += operator [C#]
ms.assetid: 93e56486-bb42-43c1-bd43-60af11e64e67
ms.openlocfilehash: cafd07f4b4aefdcc4b43750d61c155fe3d65aa46
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398101"
---
# <a name="-and--operators-c-reference"></a>+ および += 演算子 (C# リファレンス)

`+` 演算子と `+=` 演算子は、組み込みの[整数](../builtin-types/integral-numeric-types.md)および[浮動小数点型](../builtin-types/floating-point-numeric-types.md)の数値型、[文字列](../builtin-types/reference-types.md#the-string-type)型、[デリゲート](../builtin-types/reference-types.md#the-delegate-type)型によってサポートされています。

算術演算子 `+` については、「[算術演算子 (C# リファレンス)](arithmetic-operators.md#unary-plus-and-minus-operators)」の記事の「[単項プラス演算子と単項マイナス演算子](arithmetic-operators.md#addition-operator-)」セクションと「[加算演算子 +](arithmetic-operators.md)」セクションを参照してください。

## <a name="string-concatenation"></a>文字列連結

一方または両方のオペランドが[文字列](../builtin-types/reference-types.md#the-string-type)型の場合、`+` 演算子によってそのオペランドの文字列表現が連結されます。

[!code-csharp-interactive[string concatenation](snippets/AdditionOperator.cs#AddStrings)]

C# 6 以降、[文字列補間](../tokens/interpolated.md)という文字列を書式設定するより便利な方法が提供されています。

[!code-csharp-interactive[string interpolation](snippets/AdditionOperator.cs#UseStringInterpolation)]

## <a name="delegate-combination"></a>デリゲートの組み合わせ

同じ[デリゲート](../builtin-types/reference-types.md#the-delegate-type)型のオペランドの場合、呼び出されると左側のオペランドを呼び出してから右側のオペランドを呼び出す新しいデリゲート インスタンスが `+` 演算子によって返されます。 いずれかのオペランドが `null` の場合、`+` 演算子によって別のオペランドの値が返されます (`null` でもある場合があります)。 次の例では、デリゲートが `+` 演算子と組み合わされるしくみを説明しています。

[!code-csharp-interactive[delegate combination](snippets/AdditionOperator.cs#AddDelegates)]

デリゲートの削除を実行するには、[`-` 演算子](subtraction-operator.md#delegate-removal)を使用します。

デリゲート型の詳細については、[デリゲート](../../programming-guide/delegates/index.md)に関するページを参照してください。

## <a name="addition-assignment-operator-"></a>加算代入演算子 +=

次のような `+=` 演算子を使用する式があるとします

```csharp
x += y
```

上記の式は、次の式と同じです。

```csharp
x = x + y
```

ただし、`x` が評価されるのは 1 回だけです。

`+=` 演算子の使用例を次に示します。

[!code-csharp-interactive[+= examples](snippets/AdditionOperator.cs#AddAndAssign)]

`+=`イベント[をサブスクライブするとき、](../keywords/event.md) 演算子を使用してイベント ハンドラー メソッドを指定することもできます。 詳細については、「[方法: イベント サブスクリプションとサブスクリプションの解除](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)」を参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は [ 演算子を](operator-overloading.md)オーバーロード`+`できます。 2 項 `+` 演算子をオーバーロードすると、`+=` 演算子も暗黙的にオーバーロードされます。 ユーザー定義型では、`+=` 演算子を明示的にオーバーロードできません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#unary-plus-operator)の[単項プラス演算子](~/_csharplang/spec/expressions.md#addition-operator)と[加算演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [複数の文字列を連結する方法](../../how-to/concatenate-multiple-strings.md)
- [イベント](../../programming-guide/events/index.md)
- [算術演算子](arithmetic-operators.md)
- [- および -= 演算子](subtraction-operator.md)
