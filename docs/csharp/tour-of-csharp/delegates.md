---
title: C# のデリゲート - C# 言語のツアー
description: C# のデリゲートと遅延バインディングについて
ms.date: 02/27/2020
ms.assetid: 3cc27357-3ac2-43a1-aad0-86a77b88f884
ms.openlocfilehash: b1740ddc65dcb0ee8775f4cbaa8356293ea55fae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159170"
---
# <a name="delegates"></a>デリゲート

***デリゲート型***は、特定のパラメーター リストおよび戻り値を使用してメソッドへの参照を表します。 デリゲートを使用すると、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。 デリゲートは、他の言語で検出された関数ポインターの概念に似ています。 ただし、関数ポインターとは異なり、デリゲートはオブジェクト指向で、タイプ セーフです。

次の例では、`Function` という名前のデリゲート型を宣言して使用します。

[!code-csharp[DelegateExample](../../../samples/snippets/csharp/tour/delegates/Program.cs#L3-L37)]

`Function` デリゲート型のインスタンスは、`double` 引数を取得して `double` 値を返す任意のメソッドを参照できます。 `Apply` メソッドは、指定された関数を `double[]` の要素に適用し、`double[]` を結果とともに返します。 `Main` メソッドでは、`Apply` は `double[]` に 3 つの異なる関数を適用するために使用されます。

デリゲートは、静的メソッド (前述の例の `Square` や `Math.Sin` など) またはインスタンス メソッド (前述の例の `m.Multiply` など) のいずれかを参照できます。 インスタンス メソッドを参照するデリゲートはまた、特定のオブジェクトを参照し、インスタンス メソッドがデリゲートから呼び出されると、そのオブジェクトは呼び出しで `this` になります。

宣言時に作成される "インライン メソッド" である匿名関数を使用してデリゲートを作成することもできます。 匿名関数では、周囲のメソッドのローカル変数を確認できます。 次の例では、クラスは作成されません。

[!code-csharp[LambdaExample](../../../samples/snippets/csharp/tour/delegates/Program.cs#L44-L44)]

デリゲートによって、参照されるメソッドのクラスは把握されることも必要とされることもありません。重要なのは、参照されるメソッドがデリゲートと同じパラメーターおよび戻り値の型を持つという点だけです。

>[!div class="step-by-step"]
>[前へ](interfaces.md)
>[次へ](attributes.md)
