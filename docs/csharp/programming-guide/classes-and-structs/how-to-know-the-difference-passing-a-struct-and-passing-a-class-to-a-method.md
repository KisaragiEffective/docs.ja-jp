---
title: メソッドに構造体を渡すこととクラス参照を渡すことの違いを理解する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- structs [C#], passing as method parameter
- passing parameters [C#], structs vs. classes
- methods [C#], passing classes vs. structs
ms.assetid: 9c1313a6-32a8-4ea7-a59f-450f66af628b
ms.openlocfilehash: a280a6df873d7c03c204bc5c86468e7e7298d723
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77673434"
---
# <a name="how-to-know-the-difference-between-passing-a-struct-and-passing-a-class-reference-to-a-method-c-programming-guide"></a>メソッドに構造体を渡すこととクラス参照を渡すことの違いを理解する方法 (C# プログラミング ガイド)
次の例では、メソッドに[構造体](../../language-reference/builtin-types/struct.md)を渡すことと[クラス](../../language-reference/keywords/class.md) インスタンスを渡すことの違いを示します。 この例では、両方の引数 (構造体とクラス インスタンス) が値によって渡され、両方のメソッドが引数の 1 つのフィールドの値を変更します。 ただし、2 つのメソッドの結果は同じではありません。構造体を渡した場合に渡される内容と、クラスのインスタンスを渡した場合に渡される内容が異なるためです。  
  
 構造体は[値型](../../language-reference/builtin-types/value-types.md)であるため、メソッドに[構造体が値によって渡される](./passing-value-type-parameters.md)と、メソッドは構造体引数のコピーを受け取って操作します。 メソッドは、呼び出し側メソッドの元の構造体にはアクセスできないため、どのような場合でもこの構造体を変更することはできません。 メソッドで変更できるのはコピーのみです。  
  
 クラス インスタンスは、値型ではなく、[参照型](../../language-reference/keywords/reference-types.md)です。 メソッドに[参照型が値によって渡される](./passing-reference-type-parameters.md)と、メソッドはクラス インスタンスへの参照のコピーを受け取ります。 つまり、呼び出されたメソッドは、インスタンスのアドレスのコピーを受け取り、呼び出し元のメソッドはインスタンスの元のアドレスを保持します。 呼び出し側メソッドのクラス インスタンスにはアドレスがあり、呼び出されたメソッドのパラメータにはそのアドレスのコピーがあり、両方のアドレスが同じオブジェクトを参照します。 パラメーターにはアドレスのコピーのみが含まれるため、呼び出されたメソッドは呼び出し側メソッドのクラス インスタンスのアドレスを変更できません。 ただし、呼び出されたメソッドはアドレスのコピーを使用して、元のアドレスとアドレスのコピーの両方が参照するクラス メンバーにアクセスできます。 呼び出されたメソッドがクラス メンバーを変更すると、呼び出し側メソッドの元のクラス インスタンスも変更されます。  
  
 次の例の出力はこの違いを示しています。 クラス インスタンスの `willIChange` フィールドの値はメソッド `ClassTaker` の呼び出しによって変更されます。これは、メソッドがパラメーターのアドレスを使用して、クラス インスタンスの指定されたフィールドを検索するためです。 呼び出し側メソッドの構造体の `willIChange` フィールドはメソッド `StructTaker` の呼び出しによって変更されません。これは、引数の値が、そのアドレスのコピーではなく、構造体自体のコピーであるためです。 `StructTaker` はコピーを変更し、そのコピーは、`StructTaker` の呼び出しが完了したときに失われます。  
  
## <a name="example"></a>例  
 [!code-csharp[csProgGuideObjects#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#32)]  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [クラス](./classes.md)
- [構造体型](../../language-reference/builtin-types/struct.md)
- [パラメーターの引き渡し](./passing-parameters.md)
