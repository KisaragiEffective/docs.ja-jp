---
title: 引数としての配列の受け渡し - C# プログラミング ガイド
ms.date: 07/05/2018
helpviewer_keywords:
- arrays [C#], passing as arguments
ms.assetid: f3a0971e-c87c-4a1f-8262-bc0a3b712772
ms.openlocfilehash: 2e53008910a9062ada25680eb4b8e54a225fd226
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705692"
---
# <a name="passing-arrays-as-arguments-c-programming-guide"></a>引数としての配列の受け渡し (C# プログラミング ガイド)

配列は、引数としてメソッド パラメーターに渡すことができます。 配列は参照型であるため、メソッドは要素の値を変更できます。

## <a name="passing-single-dimensional-arrays-as-arguments"></a>1 次元配列を引数として渡す

初期化された 1 次元配列をメソッドに渡すことができます。 たとえば、次のステートメントは、配列を print メソッドに送信します。

[!code-csharp[csProgGuideArrays#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#34)]

次のコードは、print メソッドの実装の一部を示しています。

[!code-csharp[csProgGuideArrays#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#33)]

次の例に示すように、一度に新しい配列を初期化して渡すことができます。

[!code-csharp[CsProgGuideArrays#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#35)]

### <a name="example"></a>例

次の例では、文字列の配列が初期化され、引数として文字列の `DisplayArray` メソッドに渡されます。 このメソッドは、配列の要素を表示します。 次に、`ChangeArray` メソッドで配列の要素を反転させた後、`ChangeArrayElements` メソッドで配列の最初の 3 つの要素を変更します。 各メソッドから戻った後、`DisplayArray` メソッドで、配列を値で渡すと配列要素の変更が禁止されないことを示します。

[!code-csharp[csProgGuideArrays#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/ArrayExample.cs)]

## <a name="passing-multidimensional-arrays-as-arguments"></a>多次元配列を引数として渡す

1 次元配列を渡すのと同じ方法で、初期化された多次元配列をメソッドに渡します。

[!code-csharp[csProgGuideArrays#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#41)]

次のコードに、2 次元配列を引数として受け取る print メソッドの宣言の一部を示します。

[!code-csharp[csProgGuideArrays#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#36)]

次の例に示すように、一度に新しい配列を初期化して渡すことができます。

[!code-csharp[csProgGuideArrays#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#32)]

### <a name="example"></a>例

次の例では、整数の 2 次元配列が初期化され、`Print2DArray` メソッドに渡されます。 このメソッドは、配列の要素を表示します。

[!code-csharp[csProgGuideArrays#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#31)]

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [配列](index.md)
- [1 次元配列](single-dimensional-arrays.md)
- [多次元配列](multidimensional-arrays.md)
- [ジャグ配列](jagged-arrays.md)
