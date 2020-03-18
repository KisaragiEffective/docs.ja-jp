---
title: '- および -= 演算子 - C# リファレンス'
ms.date: 05/27/2019
f1_keywords:
- -_CSharpKeyword
- -=_CSharpKeyword
helpviewer_keywords:
- subtraction operator [C#]
- delegate removal [C#]
- '- operator [C#]'
- subtraction assignment operator [C#]
- event unsubscription [C#]
- -= operator [C#]
ms.assetid: 4de7a4fa-c69d-48e6-aff1-3130af970b2d
ms.openlocfilehash: 2017aade92e8d7ad2af7101a107122fa8d7b9e27
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847652"
---
# <a name="--and---operators-c-reference"></a>- および -= 演算子 (C# リファレンス)

`-` 演算子と `-=` 演算子は、組み込みの[整数](../builtin-types/integral-numeric-types.md)および[浮動小数点型](../builtin-types/floating-point-numeric-types.md)の数値型と、[デリゲート](../builtin-types/reference-types.md#the-delegate-type)型によってサポートされています。

算術演算子 `-` については、「[算術演算子 (C# リファレンス)](arithmetic-operators.md#unary-plus-and-minus-operators)」の記事の「[単項プラス演算子と単項マイナス演算子](arithmetic-operators.md#subtraction-operator--)」セクションと「[減算演算子 -](arithmetic-operators.md)」セクションを参照してください。

## <a name="delegate-removal"></a>デリゲートの削除

同じ[デリゲート](../builtin-types/reference-types.md#the-delegate-type)型のオペランドに対しては、`-` 演算子は、次のように計算されるデリゲート インスタンスを返します。

- 両方のオペランドが null 値ではなく、右側のオペランドの呼び出しリストが左側のオペランドの呼び出しリストの適切な連続するサブリストの場合は、演算の結果は右側のオペランドのエントリを左側のオペランドの呼び出しリストから削除することによって取得される新しい呼び出しリストとなります。 右側のオペランドのリストが、左側のオペランドのリストで複数の連続するサブリストと一致する場合、右端の一致するサブリストのみが削除されます。 削除によりリストが空になる場合、結果は `null` になります。

  [!code-csharp-interactive[delegate removal](snippets/SubtractionOperator.cs#DelegateRemoval)]

- 右側のオペランドの呼び出しリストが左側のオペランドの呼び出しリストの適切な連続するサブリストでない場合は、演算結果は左側のオペランドになります。 たとえば、マルチキャストのデリゲートの一部ではないデリゲートを削除しても何も行われず、マルチキャストのデリゲートは変更されません。

  [!code-csharp-interactive[delegate removal with no effect](snippets/SubtractionOperator.cs#DelegateRemovalNoChange)]

  前の例では、デリゲート中に削除デリゲートのインスタンスが比較されることも示しています。 たとえば、同一の[ラムダ式](../../programming-guide/statements-expressions-operators/lambda-expressions.md)の評価から生成されるデリゲートが等しくない、などです。 デリゲートの等値の詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#delegate-equality-operators)の[デリゲートの等値演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

- 左側のオペランドが `null` の場合は、演算結果は `null` になります。 右側のオペランドが `null` の場合は、演算結果は左側のオペランドになります。

  [!code-csharp-interactive[delegate removal and null](snippets/SubtractionOperator.cs#DelegateRemovalAndNull)]

デリゲートを組み合わせるには、[`+` 演算子](addition-operator.md#delegate-combination)を使用します。

デリゲート型の詳細については、[デリゲート](../../programming-guide/delegates/index.md)に関するページを参照してください。

## <a name="subtraction-assignment-operator--"></a>減算代入演算子 -=

次のような `-=` 演算子を使用する式があるとします

```csharp
x -= y
```

上記の式は、次の式と同じです。

```csharp
x = x - y
```

ただし、`x` が評価されるのは 1 回だけです。

`-=` 演算子の使用例を次に示します。

[!code-csharp-interactive[-= examples](snippets/SubtractionOperator.cs#SubtractAndAssign)]

`-=`イベント[から登録を解除するときに、](../keywords/event.md) 演算子を使用して削除するイベント ハンドラー メソッドを指定することもできます。 詳細については、「[イベントのサブスクリプションとサブスクリプション解除を行う方法](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)」を参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

ユーザー定義型は [ 演算子を](operator-overloading.md)オーバーロード`-`できます。 2 項 `-` 演算子をオーバーロードすると、`-=` 演算子も暗黙的にオーバーロードされます。 ユーザー定義型では、`-=` 演算子を明示的にオーバーロードできません。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#unary-minus-operator)の[単項マイナス演算子](~/_csharplang/spec/expressions.md#subtraction-operator)と[減算演算子](~/_csharplang/spec/introduction.md)に関するセクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [イベント](../../programming-guide/events/index.md)
- [算術演算子](arithmetic-operators.md)
- [+ および += 演算子](addition-operator.md)
