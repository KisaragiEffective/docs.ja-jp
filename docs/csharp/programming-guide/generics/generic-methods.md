---
title: ジェネリック メソッド - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], methods
ms.assetid: 673eeea2-4b48-4faa-9c4e-2e89449221b9
ms.openlocfilehash: 5f066ca39c97b70554886e423c37c4ee47d49158
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712196"
---
# <a name="generic-methods-c-programming-guide"></a>ジェネリック メソッド (C# プログラミング ガイド)
ジェネリック メソッドは、次のように型パラメーターで宣言されるメソッドです。  
  
 [!code-csharp[csProgGuideGenerics#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#22)]  
  
 メソッドの呼び出し方法の 1 つを示しているのが次のコード サンプルです。型引数に `int` を利用します。  
  
 [!code-csharp[csProgGuideGenerics#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#23)]  
  
 型引数は省略することもできます。コンパイラが推定します。 次は、前の呼び出しと同じように `Swap` を呼び出します。  
  
 [!code-csharp[csProgGuideGenerics#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#24)]  
  
 型推定の同じ規則が静的メソッドとインスタンス メソッドに適用されます。 コンパイラは、渡されたメソッド引数に基づいて型パラメーターを推定できます。制約や戻り値だけでは型パラメーターを推定できません。 そのため、パラメーターのないメソッドでは型を推定できません。 型はコンパイル時に、コンパイラがオーバーロードされたメソッド シグネチャを解決する前に推定されます。 コンパイラは、同じ名前を共有するすべてのジェネリック メソッドに型の推定ロジックを適用します。 オーバーロード解決手順では、型を推定できたジェネリック メソッドのみがコンパイラに含まれます。  
  
 ジェネリック クラス内では、非ジェネリック メソッドは、次のように、クラスレベルの型パラメーターにアクセスできます。  
  
 [!code-csharp[csProgGuideGenerics#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#25)]  
  
 包含クラスと同じ型パラメーターを受け取るジェネリック メソッドを定義すると、コンパイラでは警告 [CS0693](../../misc/cs0693.md) が生成されます。これは、メソッド範囲内で、内側の `T` に与えられた引数により外側の `T` に与えられた引数が隠されるためです。 クラスがインスタンス化されたときに与えらたれ型パラメーター以外の型パラメーターでジェネリック クラス メソッドを呼び出すという柔軟性が必要な場合、メソッドの型パラメーターに別の識別子を指定することを検討してください。次の例の `GenericList2<T>` をご覧ください。  
  
 [!code-csharp[csProgGuideGenerics#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#26)]  
  
 メソッドの型パラメーターでさらに多くの特殊操作を可能にするために、制約を使用します。 `Swap<T>` という名前が付けられた `SwapIfGreater<T>` のこのバージョンは、<xref:System.IComparable%601> を実装する型引数との併用でのみ利用できます。  
  
 [!code-csharp[csProgGuideGenerics#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#27)]  
  
 ジェネリック メソッドは、いくつかの型パラメーターでオーバーロードできます。 たとえば、次のメソッドはすべて同じクラスに配置できます。  
  
 [!code-csharp[csProgGuideGenerics#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#28)]  
  
## <a name="c-language-specification"></a>C# 言語仕様  
 詳細については、「[C# 言語の仕様](~/_csharplang/spec/classes.md#methods)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic>
- [C# プログラミングガイド](../index.md)
- [ジェネリックの概要](./index.md)
- [メソッド](../classes-and-structs/methods.md)
