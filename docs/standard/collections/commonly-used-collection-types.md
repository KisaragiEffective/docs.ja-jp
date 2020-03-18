---
title: 一般的に使用されるコレクション型
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- collections [.NET Framework], generic
- objects [.NET Framework], grouping in collections
- generics [.NET Framework], collections
- IList interface, grouping data in collections
- IDictionary interface, grouping data in collections
- grouping data in collections, generic collection types
- Collections classes
- generic collections
ms.assetid: f5d4c6a4-0d7b-4944-a9fb-3b12d9ebfd55
ms.openlocfilehash: 1004a2f9a0594d9150d147dec1e16b56205e0d13
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711403"
---
# <a name="commonly-used-collection-types"></a>一般的に使用されるコレクション型
コレクション型は、ハッシュ テーブル、キュー、スタック、バッグ、ディクショナリ、リストなど、一般的な種類のデータ コレクションです。  
  
 コレクションは、<xref:System.Collections.ICollection> インターフェイス、<xref:System.Collections.IList> インターフェイス、<xref:System.Collections.IDictionary> インターフェイス、または対応するジェネリックに基づきます。 <xref:System.Collections.IList> インターフェイスと <xref:System.Collections.IDictionary> インターフェイスはどちらも <xref:System.Collections.ICollection> インターフェイスから派生したインターフェイスです。したがって、すべてのコレクションが直接または間接的に <xref:System.Collections.ICollection> インターフェイスに基づきます。 <xref:System.Collections.IList> インターフェイスに基づくコレクション (<xref:System.Array>、<xref:System.Collections.ArrayList>、または <xref:System.Collections.Generic.List%601> など) または <xref:System.Collections.ICollection> インターフェイスに直接基づくコレクション (<xref:System.Collections.Queue>、<xref:System.Collections.Concurrent.ConcurrentQueue%601>、<xref:System.Collections.Stack>、<xref:System.Collections.Concurrent.ConcurrentStack%601>、または <xref:System.Collections.Generic.LinkedList%601> など) では、すべての要素に値のみが含まれます。 <xref:System.Collections.IDictionary> インターフェイスに基づくコレクション (<xref:System.Collections.Hashtable> クラスと <xref:System.Collections.SortedList> クラス、<xref:System.Collections.Generic.Dictionary%602> ジェネリック クラスと <xref:System.Collections.Generic.SortedList%602> ジェネリック クラスなど)、または <xref:System.Collections.Concurrent.ConcurrentDictionary%602> クラスでは、すべての要素にキーと値の両方が含まれます。  <xref:System.Collections.ObjectModel.KeyedCollection%602> クラスは、キーが値に埋め込まれている値リストであるために独特で、そのために、リストのようにもディクショナリのようにも動作します。  
  
 ジェネリック コレクションは、厳密な型指定に対する最適なソリューションです。 ただし、使用する言語でジェネリックがサポートされていない場合、<xref:System.Collections> 名前空間には <xref:System.Collections.CollectionBase>、<xref:System.Collections.ReadOnlyCollectionBase>、<xref:System.Collections.DictionaryBase> などの基本コレクションが含まれており、これらは抽象基本クラスで、厳密に型指定されたコレクション クラスを作成するために拡張できます。 効率的なマルチスレッド コレクション アクセスが必要な場合は、<xref:System.Collections.Concurrent> 名前空間のジェネリック コレクションを使用します。  
  
 コレクションは、要素の格納方法、要素の並べ替え方法、検索の実行方法、および比較の実行方法によって異なります。 <xref:System.Collections.Queue> クラスと <xref:System.Collections.Generic.Queue%601> ジェネリック クラスは先入れ先出しのリストを提供しますが、<xref:System.Collections.Stack> クラスと <xref:System.Collections.Generic.Stack%601> ジェネリック クラスは後入れ先出しのリストを提供します。 <xref:System.Collections.SortedList> クラスと <xref:System.Collections.Generic.SortedList%602> ジェネリック クラスは、<xref:System.Collections.Hashtable> クラスと <xref:System.Collections.Generic.Dictionary%602> ジェネリック クラスの並べ替えバージョンを提供します。 <xref:System.Collections.Hashtable> または <xref:System.Collections.Generic.Dictionary%602> の要素は、要素のキーによってのみアクセスできますが、<xref:System.Collections.SortedList> または <xref:System.Collections.ObjectModel.KeyedCollection%602> の要素は、キーまたは要素のインデックスによってアクセスできます。 すべてのコレクションのインデックスがゼロから始まりますが、<xref:System.Array> は例外で、ゼロから始まらない配列を使用できます。  
  
 LINQ to Objects 機能では、オブジェクト型で <xref:System.Collections.IEnumerable> または <xref:System.Collections.Generic.IEnumerable%601> を実装している限り、LINQ クエリを使用してメモリ内オブジェクトにアクセスできます。 LINQ クエリはデータ アクセス用の一般的なパターンです。通常、これは標準の `foreach` ループよりも簡潔で読みやすく、フィルター処理、並べ替え、およびグループ化機能を備えています。 さらに、LINQ クエリによってパフォーマンスを向上させることができます。 詳細については、「[LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md)」、「[LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)」、および「[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)」を参照してください。  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[コレクションとデータ構造体](../../../docs/standard/collections/index.md)|.NET Framework で利用できるスタック、キュー、リスト、配列、ディクショナリなどのさまざまなコレクション型について説明します。|  
|[Hashtable コレクション型と Dictionary コレクション型](../../../docs/standard/collections/hashtable-and-dictionary-collection-types.md)|ジェネリックと非ジェネリックのハッシュ ベースのディクショナリ型の機能について説明します。|  
|[Sorted コレクション型](../../../docs/standard/collections/sorted-collection-types.md)|リストとセットの並べ替え機能を提供するクラスについて説明します。|  
|[ジェネリック](../../../docs/standard/generics/index.md)|ジェネリック コレクション、ジェネリック デリゲート、ジェネリック インターフェイスなど、.NET Framework に用意されているジェネリック機能について説明します。 C#、Visual Basic、および Visual C++ の機能についてのドキュメント、およびリフレクションなどのサポート テクノロジへのリンクを示します。|  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Collections?displayProperty=nameWithType>  
  
 <xref:System.Collections.Generic?displayProperty=nameWithType>  
  
 <xref:System.Collections.ICollection?displayProperty=nameWithType>  
  
 <xref:System.Collections.Generic.ICollection%601?displayProperty=nameWithType>  
  
 <xref:System.Collections.IList?displayProperty=nameWithType>  
  
 <xref:System.Collections.Generic.IList%601?displayProperty=nameWithType>  
  
 <xref:System.Collections.IDictionary?displayProperty=nameWithType>  
  
 <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
