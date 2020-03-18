---
title: ジェネリック インターフェイス - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generic interfaces
- generics [C#], interfaces
ms.assetid: a8fa49a1-6e78-4a09-87e5-84a0b9f5ffbe
ms.openlocfilehash: 4cce23da7579e30ecff80b3afb92a5a58795c1bd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712209"
---
# <a name="generic-interfaces-c-programming-guide"></a>ジェネリック インターフェイス (C# プログラミング ガイド)
ジェネリック コレクション クラスのインターフェイスか、コレクション内の項目を表すジェネリック クラスのインターフェイスを定義すると、多くの場合、便利です。 ジェネリック クラスの優先設定の意図は、値型に対するボックス化とボックス化解除を回避する目的で、<xref:System.IComparable%601> ではなく <xref:System.IComparable> など、ジェネリック インターフェイスを利用することにあります。 .NET Framework クラス ライブラリにより、<xref:System.Collections.Generic> 名前空間のコレクション クラスと共に利用するためのジェネリック インターフェイスがいくつか定義されます。  
  
 インターフェイスが型パラメーターの制約として指定される場合、インターフェイスを実装する型のみを利用できます。 `SortedList<T>` クラスから派生する `GenericList<T>` クラスを示したのが次のコード サンプルです。 詳細については、「[ジェネリックの概要](./index.md)」を参照してください。 `SortedList<T>` により制約 `where T : IComparable<T>` が追加されます。 これにより、`BubbleSort` の `SortedList<T>` メソッドは、一覧要素でジェネリック <xref:System.IComparable%601.CompareTo%2A> メソッドを利用できます。 この例では、一覧要素は単純なクラスである `Person` です。これは `IComparable<Person>` を実装します。  
  
 [!code-csharp[csProgGuideGenerics#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics2.cs#29)]  
  
 次のように、複数のインターフェイスを単一の型で制約として指定できます。  
  
 [!code-csharp[csProgGuideGenerics#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#30)]  
  
 1 つのインターフェイスで、次のように、複数の型パラメーターを定義できます。  
  
 [!code-csharp[csProgGuideGenerics#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#31)]  
  
 クラスに適用される継承規則は、インターフェイスにも適用されます。  
  
 [!code-csharp[csProgGuideGenerics#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#32)]  
  
 ジェネリック インターフェイスが反変の場合、つまり、その型パラメーターを戻り値としてのみ利用する場合、ジェネリック インターフェイスは非ジェネリック インターフェイスから継承できます。 .NET Framework クラス ライブラリでは、<xref:System.Collections.Generic.IEnumerable%601> は <xref:System.Collections.IEnumerable> から継承します。これは、<xref:System.Collections.Generic.IEnumerable%601> が `T` の戻り値と <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> プロパティ ゲッターの <xref:System.Collections.Generic.IEnumerator%601.Current%2A> のみを利用するためです。  
  
 具象クラスは、次のように、構築されたクローズ型インターフェイスを実装できます。  
  
 [!code-csharp[csProgGuideGenerics#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#33)]  
  
 ジェネリック クラスは、クラス パラメーターの一覧からインターフェイスが必要とするすべての引数が提供される限り、ジェネリック インターフェイスや構築されたクローズ型インターフェイスを実装できます。  
  
 [!code-csharp[csProgGuideGenerics#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#34)]  
  
 メソッドのオーバーロードを制御する規則は、ジェネリック クラス、ジェネリック構造体、ジェネリック インターフェイス内のメソッドの規則と同じです。 詳細については、「[ジェネリック メソッド](./generic-methods.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [ジェネリックの概要](./index.md)
- [interface](../../language-reference/keywords/interface.md)
- [ジェネリック](../../../standard/generics/index.md)
