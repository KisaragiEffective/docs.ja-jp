---
title: ジェネリック コレクションを使用する状況
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- collections [.NET Framework], generic
- generic collections [.NET Framework]
ms.assetid: e7b868b1-11fe-4ac5-bed3-de68aca47739
ms.openlocfilehash: 7d59259c1cab6842ef62888bf5326225394d8d44
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711208"
---
# <a name="when-to-use-generic-collections"></a>ジェネリック コレクションを使用する状況
通常は、ジェネリック コレクションを使用することをお勧めします。これは、基本コレクション型から派生して型固有のメンバーを実装することなく、タイプ セーフの利点を即座に得ることができるためです。 また、ジェネリックでは要素をボックス化する必要がないため、コレクションの要素が値型である場合、一般的に、ジェネリック コレクション型は、対応する非ジェネリック コレクション型 (および、非ジェネリックの基本コレクション型から派生した型) よりもパフォーマンスに優れています。  
  
 .NET Framework 4 以降を対象にしたプログラムで、複数のスレッドで同時に項目をコレクションに追加またはコレクションから削除する可能性がある場合は、<xref:System.Collections.Concurrent> 名前空間のジェネリック コレクション クラスを使用する必要があります。  
  
 次のジェネリック型は、既存のコレクション型に対応しています。  
  
- <xref:System.Collections.Generic.List%601> は、 <xref:System.Collections.ArrayList>に対応するジェネリック クラスです。  
  
- <xref:System.Collections.Generic.Dictionary%602> および <xref:System.Collections.Concurrent.ConcurrentDictionary%602> は、 <xref:System.Collections.Hashtable>に対応するジェネリック クラスです。  
  
- <xref:System.Collections.ObjectModel.Collection%601> は、 <xref:System.Collections.CollectionBase>に対応するジェネリック クラスです。 <xref:System.Collections.ObjectModel.Collection%601> は基本クラスとして使用できますが、<xref:System.Collections.CollectionBase> とは異なり、抽象クラスではありません。 そのため、より簡単に使用できます。  
  
- <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> は、 <xref:System.Collections.ReadOnlyCollectionBase>に対応するジェネリック クラスです。 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> は抽象クラスではなく、既存の <xref:System.Collections.Generic.List%601> を読み取り専用のコレクションとして簡単に公開できるようにするコンストラクターを含んでいます。  
  
- <xref:System.Collections.Generic.Queue%601>、 <xref:System.Collections.Concurrent.ConcurrentQueue%601>、 <xref:System.Collections.Generic.Stack%601>、 <xref:System.Collections.Concurrent.ConcurrentStack%601>、および <xref:System.Collections.Generic.SortedList%602> ジェネリック クラスは、同じ名前を持つそれぞれの非ジェネリック クラスに対応しています。  
  
## <a name="additional-types"></a>その他の型  
 いくつかのジェネリック コレクション型には、対応する非ジェネリック型がありません。 次のオプションが含まれます。  
  
- <xref:System.Collections.Generic.LinkedList%601> は、挿入と削除の O(1) 操作を提供する汎用のリンク リストです。  
  
- <xref:System.Collections.Generic.SortedDictionary%602> は、挿入と取得の O(log `n`) 操作で並べ替えられたディクショナリで、 <xref:System.Collections.Generic.SortedList%602>の代替として役立ちます。  
  
- <xref:System.Collections.ObjectModel.KeyedCollection%602> は、リストとディクショナリを組み合わせたもので、独自のキーを含むオブジェクトの格納方法を提供します。  
  
- <xref:System.Collections.Concurrent.BlockingCollection%601> は、境界ブロッキング機能を備えたコレクション クラスを実装します。  
  
- <xref:System.Collections.Concurrent.ConcurrentBag%601> は、順序のない要素の高速な挿入および削除を提供します。  
  
## <a name="linq-to-objects"></a>LINQ to Objects  
 LINQ to Objects 機能では、オブジェクト型が <xref:System.Collections.IEnumerable?displayProperty=nameWithType> インターフェイスまたは <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> インターフェイスを実装している限り、LINQ クエリを使用してメモリ内オブジェクトにアクセスできます。 LINQ クエリはデータ アクセス用の一般的なパターンです。通常、これは標準の `foreach` ループよりも簡潔で読みやすく、フィルター処理、並べ替え、およびグループ化機能を備えています。 さらに、LINQ クエリによってパフォーマンスを向上させることができます。 詳細については、「[LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md)」、「[LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)」、および「[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)」を参照してください。  
  
## <a name="additional-functionality"></a>その他の機能  
 一部のジェネリック型には、非ジェネリック コレクション型にはない機能があります。 たとえば、非ジェネリックの <xref:System.Collections.Generic.List%601> クラスに対応する <xref:System.Collections.ArrayList> クラスには、リストを検索するメソッドを指定できる <xref:System.Predicate%601> デリゲートや、リストの各要素に作用するメソッドを表す <xref:System.Action%601> デリゲートや、型間の変換を定義できるようにする <xref:System.Converter%602> デリゲートなどの、汎用デリゲートを受け取るメソッドが多数あります。  
  
 <xref:System.Collections.Generic.List%601> クラスを使用すると、リストの並べ替えと検索を行う独自の <xref:System.Collections.Generic.IComparer%601> ジェネリック インターフェイス実装を指定できます。 <xref:System.Collections.Generic.SortedDictionary%602> クラスと <xref:System.Collections.Generic.SortedList%602> クラスにもこの機能があります。 また、これらのクラスを使用すると、コレクションの作成時に比較演算子を指定できます。 同様に、 <xref:System.Collections.Generic.Dictionary%602> クラスと <xref:System.Collections.ObjectModel.KeyedCollection%602> クラスを使用すると、独自の等値比較子を指定できます。  
  
## <a name="see-also"></a>参照

- [コレクションとデータ構造体](../../../docs/standard/collections/index.md)
- [ 一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)
- [ジェネリック](../../../docs/standard/generics/index.md)
