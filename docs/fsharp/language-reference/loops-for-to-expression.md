---
title: 'ループ: for...to 式'
description: 詳細についF#ては、to 式は、ループ内でループ変数の値の範囲を反復処理するために使用されます。
ms.date: 05/16/2016
ms.openlocfilehash: 882b6e48dd09efcb57f7ef2f419e68a6c43393a5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283903"
---
# <a name="loops-forto-expression"></a>ループ: for...to 式

`for...to` 式は、ループ内でループ変数の値の範囲を反復処理するために使用されます。

## <a name="syntax"></a>構文

```fsharp
for identifier = start [ to | downto ] finish do
    body-expression
```

## <a name="remarks"></a>コメント

識別子の型は、 *start*および*finish*式の型から推論されます。 これらの式の型は、32ビットの整数である必要があります。

技術的には式でも、`for...to` は命令型プログラミング言語の従来のステートメントに似ています。 *本体式*の戻り値の型は `unit`である必要があります。 次の例では、`for...to` 式のさまざまな用途を示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5101.fs)]

このコードの出力は、次のようになります。

```console
1 2 3 4 5 6 7 8 9 10
10 9 8 7 6 5 4 3 2 1
2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18
```

## <a name="see-also"></a>参照

- [F# 言語リファレンス](index.md)
- [ループ: `for...in` 式](loops-for-in-expression.md)
- [ループ: `while...do` 式](loops-while-do-expression.md)
