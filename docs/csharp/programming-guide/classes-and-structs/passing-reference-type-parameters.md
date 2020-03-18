---
title: 参照型パラメーターの引き渡し - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- method parameters [C#], reference types
- parameters [C#], reference
ms.assetid: 9e6eb65c-942e-48ab-920a-b7ba9df4ea20
ms.openlocfilehash: 6fa0e60fafabaa9fb04cdc5d5bf3f9e29490e84f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714713"
---
# <a name="passing-reference-type-parameters-c-programming-guide"></a>参照型パラメーターの引き渡し (C# プログラミング ガイド)
[型参照](../../language-reference/keywords/reference-types.md)の変数には、そのデータは直接含まれず、そのデータへの参照が含まれます。 値で参照型パラメーターを渡す場合、クラス メンバーの値など、参照先オブジェクトに属するデータを変更することができます。 ただし、参照自体の値を変更することはできません。たとえば、同じ参照を使用して、新しいオブジェクトのメモリを割り当て、ブロックの外側で永続化させることはできません。 これを行うには、[ref](../../language-reference/keywords/ref.md) または [out](../../language-reference/keywords/out-parameter-modifier.md) キーワードを使用してパラメーターを渡します。 わかりやすくするために、次の例では `ref` を使用しています。  
  
## <a name="passing-reference-types-by-value"></a>値渡しによる参照型の引き渡し  
 次の例では、参照型のパラメーター `arr` を値渡しで `Change` メソッドに渡す方法について説明します。 このパラメーターは `arr` への参照であるため、配列要素の値を変更できます。 ただし、別のメモリ位置へのパラメーターの再割り当ては、メソッドの内部でのみ機能し、元の変数 `arr` には影響しません。  
  
 [!code-csharp[csProgGuideParameters#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#7)]  
  
 前の例では、`arr` パラメーターなしで参照型の配列 `ref` がメソッドに渡されています。 このような場合は、`arr` を指す参照のコピーがメソッドに渡されます。 出力は、メソッドが配列要素のコンテンツを変更できることを示しています。この場合は `1` から `888` に変更します。 ただし、[ メソッド内で ](../../language-reference/operators/new-operator.md)new`Change` 演算子を使用して新しいメモリ領域を割り当てると、変数 `pArray` が新しい配列を参照します。 このため、この後のすべての変更が、`arr` の内部で作成される元の配列 `Main` に影響しません。 実際、この例では、2 つの配列が作成されます。1 つは `Main` の内部で、もう 1 つは `Change` メソッド内で作成されます。  
  
## <a name="passing-reference-types-by-reference"></a>参照渡しによる参照型の引き渡し  
 次の例は、前の例と同じですが、`ref` キーワードがメソッド ヘッダーと呼び出しに追加されている点が異なります。 メソッドで行われるすべての変更が、呼び出し元のプログラム内の元の変数に影響します。  
  
 [!code-csharp[csProgGuideParameters#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#8)]  
  
 メソッド内で行われるすべての変更は、`Main` の元の配列に影響します。 実際、元の配列は `new` 演算子を使用して再割り当てされます。 したがって、`Change` メソッドを呼び出した後、`arr` へのすべての参照が、`Change` メソッドで作成された 5 つの要素を持つ配列を指します。  
  
## <a name="swapping-two-strings"></a>2 つの文字列のスワップ  
 文字列のスワップは、参照渡しで参照型パラメーターを渡す良い例です。 例では、2 つの文字列 `str1` と `str2` が `Main` で初期化され、`SwapStrings` キーワードで変更されたパラメーターとして `ref` メソッドに渡されます。 2 つの文字列は、メソッド内と `Main` 内でスワップされます。  
  
 [!code-csharp[csProgGuideParameters#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#9)]  
  
 この例では、呼び出し元のプログラム内の変数に影響を与えるため、参照渡しでパラメーターを渡す必要があります。 メソッド ヘッダーと、メソッドの呼び出しの両方から `ref` キーワードを削除すると、呼び出し元プログラムで変更は行われません。  
  
 文字列の詳細については、「[文字列](../../language-reference/builtin-types/reference-types.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [パラメーターの引き渡し](./passing-parameters.md)
- [ref](../../language-reference/keywords/ref.md)
- [in](../../language-reference/keywords/in-parameter-modifier.md)
- [out](../../language-reference/keywords/out.md)
- [参照型](../../language-reference/keywords/reference-types.md)
