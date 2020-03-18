---
title: コレクションとデータ構造体
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- grouping data in collections
- objects [.NET Framework], grouping in collections
- Array class, grouping data in collections
- threading [.NET Framework], safety
- Collections classes
- collections [.NET Framework]
ms.assetid: 60cc581f-1db5-445b-ba04-a173396bf872
ms.openlocfilehash: 3ca340e19d7340d7bea133fa62c6d8bbc3c0512a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160392"
---
# <a name="collections-and-data-structures"></a>コレクションとデータ構造体
多くの場合、類似するデータはコレクションとして格納および操作すると、より効率的に処理できます。 <xref:System.Array?displayProperty=nameWithType> クラスまたは <xref:System.Collections>、<xref:System.Collections.Generic>、<xref:System.Collections.Concurrent>、System.Collections.Immutable の各名前空間のクラスを使用して、コレクションの個々の要素または一定の範囲の要素を追加、削除、および変更することができます。  
  
 主要なコレクションの型として、ジェネリック コレクションと非ジェネリック コレクションの 2 つがあります。 ジェネリック コレクションは .NET Framework 2.0 で追加されたもので、コンパイル時にタイプ セーフなコレクションを提供します。 このため、通常、ジェネリック コレクションの方がパフォーマンスが高くなります。 ジェネリック コレクションは構築時に型パラメーターを受け取りますが、項目をコレクションに追加またはコレクションから削除するときに <xref:System.Object> 型との間でキャストする必要はありません。  また、ほとんどのジェネリック コレクションが Windows ストア アプリでサポートされています。 非ジェネリック コレクションは、項目を <xref:System.Object> として格納し、キャストが必要であり、ほとんどが Windows ストア アプリの開発でサポートされていません。 ただし、以前のコードに非ジェネリック コレクションが含まれている場合があります。  
  
 .NET Framework 4 より、<xref:System.Collections.Concurrent> 名前空間のコレクションによって、複数のスレッドからコレクション項目にアクセスするための効率的なスレッド セーフ操作が可能になります。 System.Collections.Immutable 名前空間の変更できないコレクション クラス ([NuGet パッケージ](https://www.nuget.org/packages/System.Collections.Immutable)) は、操作が元のコレクションのコピーで実行され、元のコレクションは変更不可能なため、本質的にスレッドセーフです。  

<a name="BKMK_Commoncollectionfeatures"></a>
## <a name="common-collection-features"></a>一般的なコレクションの機能  
 すべてのコレクションに、コレクション内の項目を追加、削除、または検索するためのメソッドが用意されています。 また、<xref:System.Collections.ICollection> インターフェイスまたは <xref:System.Collections.Generic.ICollection%601> インターフェイスを直接または間接的に実装するすべてのコレクションで、次の機能を共有しています。  
  
- **コレクションを列挙する機能**  
  
     .NET Framework のコレクションでは <xref:System.Collections.IEnumerable?displayProperty=nameWithType> または <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> を実装すると、コレクションを反復処理できます。 列挙子は、コレクション内の任意の要素への移動可能なポインターと考えることができます。 [foreach, in](../../csharp/language-reference/keywords/foreach-in.md) ステートメントと [For Each...Next ステートメント](../../visual-basic/language-reference/statements/for-each-next-statement.md) では、<xref:System.Collections.IEnumerable.GetEnumerator%2A> メソッドによって公開される列挙子を使用して、列挙子の操作の複雑さを隠しています。 また、<xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> を実装するコレクションはいずれも*クエリ可能型*と見なされ、LINQ で照会できます。 LINQ クエリでは、データにアクセスするための共通パターンが提供されます。 通常、これらは、標準の `foreach` ループよりも簡潔で読みやすく、フィルター処理、並べ替え、およびグループ化の機能を利用できます。 さらに、LINQ クエリによってパフォーマンスを向上させることができます。 詳細については、[LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md)、[LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)、[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)、[LINQ クエリの概要 (C#)](../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)、および[基本的なクエリ操作 (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md) を参照してください。  
  
- **コレクションの内容を配列にコピーする機能**  
  
     **CopyTo** メソッドを使ってすべてのコレクションを配列にコピーできます。ただし、新しい配列の要素の順序は、列挙子が返す順序に基づきます。 結果の配列は、常に下限がゼロの 1 次元です。  
  
 また、多くのコレクション クラスに次の機能が含まれています。  
  
- **容量とカウントのプロパティ**  
  
     コレクションの容量とは、そこに含むことのできる要素の数です。 コレクションのカウントとは、実際に含まれている要素の数です。 コレクションによっては、容量またはカウント、あるいはその両方を内部で処理する場合があります。  
  
     ほとんどのコレクションで、現在の容量に達すると自動的に容量が拡張されます。 メモリが再割り当てされ、要素は古いコレクションから新しいコレクションにコピーされます。 これにより、コレクションを使用するために必要なコードが削減されます。ただし、コレクションのパフォーマンスに悪影響を及ぼす可能性があります。 たとえば、<xref:System.Collections.Generic.List%601> では、<xref:System.Collections.Generic.List%601.Count%2A> が <xref:System.Collections.Generic.List%601.Capacity%2A> 未満の場合、項目の追加は O(1) 操作になります。 容量を増やして新しい要素を格納する必要がある場合、項目の追加は O(n) 操作になります。ここで、n は <xref:System.Collections.Generic.List%601.Count%2A> です。 複数の再割り当てによるパフォーマンスの低下を回避するには、初期容量をコレクションの見積もりサイズに設定しておくことが最善の方法です。  
  
     <xref:System.Collections.BitArray> は特殊なケースで、その容量はその長さと同じで、長さはそのカウントと同じです。  
  
- **一貫した下限**  
  
     コレクションの下限とは、最初の要素のインデックスです。 <xref:System.Collections> 名前空間のすべてのインデックス付きコレクションの下限がゼロです。つまり、インデックスがゼロから始まります。 <xref:System.Array> の下限は既定でゼロですが、**を使用して**Array<xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> クラスのインスタンスを作成する場合には、別の下限を定義できます。  
  
- **複数のスレッドからのアクセスの同期** (<xref:System.Collections> クラスのみ)。  
  
     <xref:System.Collections> 名前空間の非ジェネリック コレクション型では、同期によるスレッド セーフが提供され、通常、<xref:System.Collections.ICollection.SyncRoot%2A> メンバーと <xref:System.Collections.ICollection.IsSynchronized%2A> メンバーを介して公開されます。 既定では、これらのコレクションはスレッド セーフではありません。 拡張性が高く効率的な、コレクションへのマルチスレッド アクセスが必要な場合は、<xref:System.Collections.Concurrent> 名前空間のいずれかのクラスを使用するか、変更できないコレクションを使用することを検討します。 詳しくは、「[スレッド セーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)」を参照してください。  
  
<a name="BKMK_Choosingacollection"></a>
## <a name="choosing-a-collection"></a>コレクションの選択  
 通常は、ジェネリック コレクションを使用します。 次の表では、一般的なコレクションのシナリオとこれらのシナリオに使用できるコレクション クラスについて説明します。 ジェネリック コレクションに対する知識がない場合、このテーブルは、タスクに最適なジェネリック コレクションを選択するのに役立ちます。  

|やりたいこと|ジェネリック コレクションのオプション|非ジェネリック コレクションのオプション|スレッド セーフまたは変更できないコレクションのオプション|  
|-|-|-|-|  
|キーによるクイック検索用のキー/値のペアとして項目を保存する|<xref:System.Collections.Generic.Dictionary%602>|<xref:System.Collections.Hashtable><br /><br /> (キーのハッシュ コードに基づいて編成された、キーと値のペアのコレクション。)|<xref:System.Collections.Concurrent.ConcurrentDictionary%602><br /><br /> <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableDictionary%602>|  
|インデックスで項目にアクセスする|<xref:System.Collections.Generic.List%601>|<xref:System.Array><br /><br /> <xref:System.Collections.ArrayList>|<xref:System.Collections.Immutable.ImmutableList%601><br /><br /> <xref:System.Collections.Immutable.ImmutableArray>|  
|先入れ先出し (FIFO) で項目を使用する|<xref:System.Collections.Generic.Queue%601>|<xref:System.Collections.Queue>|<xref:System.Collections.Concurrent.ConcurrentQueue%601><br /><br /> <xref:System.Collections.Immutable.ImmutableQueue%601>|  
|後入れ先出し (LIFO) でデータを使用する|<xref:System.Collections.Generic.Stack%601>|<xref:System.Collections.Stack>|<xref:System.Collections.Concurrent.ConcurrentStack%601><br /><br /> <xref:System.Collections.Immutable.ImmutableStack%601>|  
|項目に順次アクセスする|<xref:System.Collections.Generic.LinkedList%601>|推奨しません|推奨しません|  
|項目がコレクションから削除またはコレクションに追加されるときに通知を受け取る。 (<xref:System.ComponentModel.INotifyPropertyChanged> と <xref:System.Collections.Specialized.INotifyCollectionChanged> を実装する)|<xref:System.Collections.ObjectModel.ObservableCollection%601>|推奨しません|推奨しません|  
|並べ替えられたコレクション|<xref:System.Collections.Generic.SortedList%602>|<xref:System.Collections.SortedList>|<xref:System.Collections.Immutable.ImmutableSortedDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|  
|数学関数のセット|<xref:System.Collections.Generic.HashSet%601><br /><br /> <xref:System.Collections.Generic.SortedSet%601>|推奨しません|<xref:System.Collections.Immutable.ImmutableHashSet%601><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|  
  
<a name="BKMK_RelatedTopics"></a>
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[コレクション クラスの選択](../../../docs/standard/collections/selecting-a-collection-class.md)|さまざまなコレクションについて説明し、いずれかのシナリオを選択できるよう支援します。|  
|[ 一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)|<xref:System.Array?displayProperty=nameWithType>、<xref:System.Collections.Generic.List%601?displayProperty=nameWithType>、<xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> などの一般的に使用されるジェネリックと非ジェネリック コレクション型について説明します。|  
|[ジェネリック コレクションの使用に適した状況](../../../docs/standard/collections/when-to-use-generic-collections.md)|ジェネリック コレクション型の使用について説明します。|  
|[コレクション内での比較と並べ替え](../../../docs/standard/collections/comparisons-and-sorts-within-collections.md)|コレクションでの等価比較と並べ替え比較の使用について説明します。|  
|[Sorted コレクション型](../../../docs/standard/collections/sorted-collection-types.md)|並べ替えられたコレクションのパフォーマンスと特性について説明します|  
|[Hashtable コレクション型と Dictionary コレクション型](../../../docs/standard/collections/hashtable-and-dictionary-collection-types.md)|ジェネリックと非ジェネリックのハッシュをベースにしたディクショナリ型の機能について説明します。|  
|[スレッドセーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)|複数のスレッドからの安全で効率的な同時アクセスをサポートする <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> や <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> などのコレクション型について説明します。|  
|System.Collections.Immutable|変更できないコレクションを導入し、コレクション型へのリンクを提供します。|  
  
<a name="BKMK_Reference"></a>
## <a name="reference"></a>リファレンス  
 <xref:System.Array?displayProperty=nameWithType>  
 <xref:System.Collections?displayProperty=nameWithType>  
 <xref:System.Collections.Concurrent?displayProperty=nameWithType>  
 <xref:System.Collections.Generic?displayProperty=nameWithType>  
 <xref:System.Collections.Specialized?displayProperty=nameWithType>  
 <xref:System.Linq?displayProperty=nameWithType>  
 <xref:System.Collections.Immutable>
