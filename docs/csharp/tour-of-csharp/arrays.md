---
title: C# の配列 - C# 言語のツアー
description: 配列は、C# 言語において最も基本的なコレクション型です。
ms.date: 02/27/2020
ms.assetid: a440704c-9e88-4c75-97dd-bfe30ca0fb97
ms.openlocfilehash: 3e045c0933a21beab6958c7851546ba6e0b55ef9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159196"
---
# <a name="arrays"></a>配列

***配列***はデータ構造で、算出されたインデックスを介してアクセスされる多くの変数を含みます。 配列に含まれる変数は配列の***要素***とも呼ばれ、すべて同じ型であり、この型は配列の***要素型***と呼ばれます。

配列型は参照型で、配列変数の宣言は、配列インスタンスへの参照の領域を確保します。 実際の配列インスタンスは、new 演算子を使用して実行時に動的に作成されます。 この操作で新しい配列インスタンスの***長さ***が指定され、それはインスタンスの有効期間中は固定です。 配列の要素のインデックスは、`0` から `Length - 1` までです。 `new` 演算子は配列の要素を自動的に既定値に初期化します。たとえば、すべての数値型はゼロ、すべての参照型は `null` です。

次の例では、`int` 要素の配列を作成し、その配列を初期化し、配列の内容を出力します。

[!code-csharp[ArraySample](../../../samples/snippets/csharp/tour/arrays/Program.cs#L3-L18)]

この例は ***1 次元配列***を作成し、操作対象とします。 C# はさらに***多次元配列***をサポートします。 配列型の次元数は、配列型の***ランク***とも呼ばれ、配列型の角かっこ内に記述されたコンマの数に 1 を追加したものです。 次の例では、1 次元、2 次元、および 3 次元の配列をそれぞれ割り当てます。

[!code-csharp[ArrayRank](../../../samples/snippets/csharp/tour/arrays/Program.cs#L24-L26)]

`a1` 配列は 10 の要素、`a2` 配列は 50 (10 × 5) の要素、`a3` 配列は 100 (10 × 5 × 2) の要素を含みます。
配列の要素型には、配列型を含む任意の型を指定できます。 配列型の要素を持つ配列は、***ジャグ配列***と呼ばれることがあります。要素の配列の長さがすべて同じである必要がないからです。 次の例では、`int` の配列の配列を割り当てます。

[!code-csharp[ArrayAllocation](../../../samples/snippets/csharp/tour/arrays/Program.cs#L31-L34)]

最初の行で 3 つの要素を持つ配列を作成しますが、各々は `int[]` 型で `null` 初期値を持ちます。 それ以降の行では、3 つの要素を、それぞれ異なる長さの配列インスタンスへの参照で初期化します。

new 演算子では、配列要素の初期値を、区切り記号 `{` および `}` のあいだに記述された式の一覧である***配列初期化子***を使用して指定することができます。 次の例は、`int[]` を割り当て 3 つの要素で初期化します。

[!code-csharp[ArrayInitialization](../../../samples/snippets/csharp/tour/arrays/Program.cs#L39-L39)]

配列の長さは、{ と } の間にある式の数から推論されます。 ローカル変数およびフィールド宣言をさらに短縮して、配列型の記述を省略することができます。

[!code-csharp[ArrayInitialization](../../../samples/snippets/csharp/tour/arrays/Program.cs#L44-L44)]

前述の例はどちらも、次のコードと同等です。

[!code-csharp[ArrayAssignment](../../../samples/snippets/csharp/tour/arrays/Program.cs#L49-L53)]

>[!div class="step-by-step"]
>[前へ](classes-and-objects.md)
>[次へ](interfaces.md)
