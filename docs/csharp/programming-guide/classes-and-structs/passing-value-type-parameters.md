---
title: 値型パラメーターの引き渡し - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- method parameters [C#], value types
- parameters [C#], value
ms.assetid: 193ab86f-5f9b-4359-ac29-7cdf8afad3a6
ms.openlocfilehash: 670af18d4b2b356aa33a0a03a29c05f5ba9bf78f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76744499"
---
# <a name="passing-value-type-parameters-c-programming-guide"></a>値型パラメーターの引き渡し (C# プログラミング ガイド)
データへの参照を含む[参照型](../../language-reference/builtin-types/value-types.md)変数とは対照的に、[値型](../../language-reference/keywords/reference-types.md)変数にはデータが直接含まれます。 値型変数を値渡しでメソッドに渡すと、変数のコピーがメソッドに渡されます。 メソッド内部で生じるパラメーターに対する変更の影響は、引数の変数に格納されている元のデータには及びません。 呼び出したメソッドで引数の値を変更する場合は、[ref](../../language-reference/keywords/ref.md) キーワードまたは [out](../../language-reference/keywords/out-parameter-modifier.md) キーワードを使用して、参照によって渡す必要があります。 また、[in](../../language-reference/keywords/in-parameter-modifier.md) キーワードを使用し、参照で値パラメーターを渡してコピーを回避し、同時に値が変更されないようにすることができます。 わかりやすくするために、次の例では `ref` を使用しています。  
  
## <a name="passing-value-types-by-value"></a>値渡しによる値型の引き渡し  
 次の例では、値型のパラメーターを値渡しで渡す方法について説明します。 変数 `n` を、値渡しでメソッド `SquareIt` に渡します。 メソッド内で生じた変更が、変数の元の値に影響することはありません。  
  
 [!code-csharp[csProgGuideParameters#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#3)]  
  
 変数 `n` は値型です。 この変数には、そのデータである値 `5` が含まれます。 `SquareIt` を呼び出すと、`n` の内容がパラメーター `x` にコピーされ、このパラメーターがメソッド内で 2 乗されます。 ただし、`Main` 内の `n` の値は、`SquareIt` メソッドを呼び出した後でも以前の値と同じままです。 メソッド内で生じた変更の影響は、ローカル変数 `x` にのみ及びます。  
  
## <a name="passing-value-types-by-reference"></a>参照渡しによる値型の引き渡し  
 次の例は、`ref` パラメーターとして引数を渡す点を除いて上記の例と同じです。 基になる引数 `n` の値は、メソッド内で `x` が変更されると変わります。  
  
 [!code-csharp[csProgGuideParameters#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#4)]  
  
 この例では、`n` の値が渡されるのではなく、`n` への参照が渡されています。 つまり、パラメーター `x` は [int](../../language-reference/builtin-types/integral-numeric-types.md) ではなく、`int` への参照 (この場合は `n` への参照) となります。 したがって、メソッド内で `x` が 2 乗された場合、`x` の参照先である `n` が実際に2乗されます。  
  
## <a name="swapping-value-types"></a>値型のスワップ  
 引数の値を変更する一般的な例としてスワップ メソッドがあります。この方法では、2 つの変数をメソッドに渡してから、メソッドにより変数の中身を交換します。 スワップ メソッドへの引数の引き渡しは、参照渡しで行う必要があります。 このようにしないと、メソッド内でパラメーターのローカル コピーどうしが交換され、呼び出し元のメソッドでは変更が行われません。 次の例では、整数値が入れ替えられます。  
  
 [!code-csharp[csProgGuideParameters#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#5)]  
  
 `SwapByRef` メソッドを呼び出す場合は、次の例に示すように呼び出しで `ref` キーワードを使用します。  
  
 [!code-csharp[csProgGuideParameters#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#6)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [パラメーターの引き渡し](./passing-parameters.md)
- [参照型パラメーターの引き渡し](./passing-reference-type-parameters.md)
