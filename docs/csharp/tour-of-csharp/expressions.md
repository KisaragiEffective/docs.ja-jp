---
title: C# の式 - C# 言語のツアー
description: 式、オペランド、および演算子は、C# 言語の構成要素です
ms.date: 02/27/2020
ms.assetid: 20d5eb10-7381-47b9-ad90-f1cc895aa27e
ms.openlocfilehash: 209b5da01cd7539f2bd97023f40fd149910b6f1d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159157"
---
# <a name="expressions"></a>式

"*式*" は "*オペランド*" と "*演算子*" で構成されます。 式の演算子は、オペランドに適用する演算を表します。 演算子の例として、`+`、`-`、`*`、`/`、および `new` などがあります。 オペランドの例としては、リテラル、フィールド、ローカル変数、式などがあります。

複数の演算子を含む式の場合、演算子の*優先順位*によって各々の演算子が評価される順序が決定されます。 たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。

1 つのオペランドが同じ優先順位を持つ 2 つの演算子の間で発生した場合、演算子の*結合性*によって演算が実行される順序が決定されます。

* 代入演算子および null 合体演算子を除くすべてのバイナリ演算子は、"*左からの結合*"、つまり演算は左から右に実行されます。 たとえば、 `x + y + z` は `(x + y) + z`と評価されます。
* 代入演算子、null 合体 `??` および `??=` 演算子、および条件演算子 `?:` は、"*右からの結合*"、つまり演算は右から左に実行されます。 たとえば、 `x = y = z` は `x = (y = z)`と評価されます。

優先順位と結合性は、かっこを使用して制御することができます。 たとえば、`x + y * z` は最初に `y` と `z` を掛け、そして結果を `x` に足しますが、`(x + y) * z` では最初に `x` と `y` を足してから `z` を掛けます。

ほとんどの演算子は[*オーバーロード*](../language-reference/operators/operator-overloading.md)できます。 演算子をオーバーロードすると、ユーザー定義演算子の実装を、1 つまたは両方のオペランドがユーザー定義のクラスまたは構造体型である演算子に指定することができます。

C# では、[算術](../language-reference/operators/arithmetic-operators.md)、[論理](../language-reference/operators/boolean-logical-operators.md)、[ビットごとやシフト](../language-reference/operators/bitwise-and-shift-operators.md)の演算に加えて、[等値](../language-reference/operators/equality-operators.md)や[順序](../language-reference/operators/comparison-operators.md)の比較を実行するための多数の演算子を提供しています。

優先度順に並べられた C# 演算子の完全な一覧については、「[C# 演算子](../language-reference/operators/index.md)」を参照してください。

> [!div class="step-by-step"]
> [前へ](types-and-variables.md)
> [次へ](statements.md)
