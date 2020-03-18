---
title: Sorted コレクション型
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- SortedDictionary collection type
- SortedList class, grouping data in collections
- grouping data in collections, SortedList collection type
- SortedList collection type
- collections [.NET Framework], SortedList collection type
ms.assetid: 3db965b2-36a6-4b12-b76e-7f074ff7275a
ms.openlocfilehash: adabda4801abc7a11a9b22181701eb233b35a251
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711338"
---
# <a name="sorted-collection-types"></a>Sorted コレクション型
<xref:System.Collections.SortedList?displayProperty=nameWithType> クラス、<xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> ジェネリック クラス、および <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> ジェネリック クラスは、<xref:System.Collections.Hashtable> インターフェイスを実装する点において <xref:System.Collections.Generic.Dictionary%602> クラスと <xref:System.Collections.IDictionary> ジェネリック クラスに似ていますが、キーによる並べ替え順序で自身の要素を維持し、ハッシュ テーブルの O(1) 挿入と取得の特性はありません。 これら 3 つのクラスには、次のような共通の特徴があります。  
  
- この 3 つのクラスはすべて、<xref:System.Collections.IDictionary?displayProperty=nameWithType> インターフェイスを実装します。 2 つのジェネリック クラスも <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType> ジェネリック インターフェイスを実装します。  
  
- 各要素は、列挙で使用できるようにキーと値のペアになっています。  
  
    > [!NOTE]
    > <xref:System.Collections.SortedList> 非ジェネリック クラスは列挙されると <xref:System.Collections.DictionaryEntry> オブジェクトを返しますが、2 つのジェネリック型のクラスは <xref:System.Collections.Generic.KeyValuePair%602> オブジェクトを返します。  
  
- 要素の並べ替えは <xref:System.Collections.IComparer?displayProperty=nameWithType> の実装 (非ジェネリック クラス <xref:System.Collections.SortedList> の場合) または <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> 実装 (2 つのジェネリック クラスの場合) に基づいて行われます。  
  
- 各クラスが提供するプロパティは、キーのみまたは値のみを含むコレクションを返します。  
  
 次の表に、2 つの並べ替えられたリスト クラスと <xref:System.Collections.Generic.SortedDictionary%602> クラスの違いをいくつか示します。  
  
|<xref:System.Collections.SortedList>非ジェネリック クラスと <xref:System.Collections.Generic.SortedList%602> ジェネリック クラス|<xref:System.Collections.Generic.SortedDictionary%602> ジェネリック クラス|  
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|キーと値を返すプロパティにはインデックスが付けられるため、インデックスを使用して効率的に取得できます。|インデックスを使用して取得することはできません。|  
|取得は O(log `n`) です。|取得は O(log `n`) です。|  
|挿入と削除は一般に O(`n`) ですが、既に並べ替えられているデータの挿入は O(log `n`) であるため、各要素はリストの最後に追加されます。 (これは、サイズの変更が不要であることを前提としています。)|挿入と削除は O(log `n`) です。|  
|使用するメモリは <xref:System.Collections.Generic.SortedDictionary%602> より少なくなります。|<xref:System.Collections.SortedList> 非ジェネリック クラスと <xref:System.Collections.Generic.SortedList%602> ジェネリック クラスより多くのメモリを使用します。|  
  
 並べ替えられたリストや辞書に複数のスレッドから同時にアクセスできる必要がある場合、<xref:System.Collections.Concurrent.ConcurrentDictionary%602> から派生するクラスに並べ替えロジックを追加できます。  
  
> [!NOTE]
> 独自のキーを含む値 (従業員 ID 番号を含む従業員レコードなど) では、<xref:System.Collections.ObjectModel.KeyedCollection%602> ジェネリック クラスから派生することによってリストの特性と辞書の特性を合わせ持つキー付きのコレクションを作成できます。  
  
 .NET Framework 4 以降、<xref:System.Collections.Generic.SortedSet%601> クラスでは、挿入、削除、検索の後に並べ替えられた順序でデータを保持する自己平衡ツリーを提供します。 このクラスと <xref:System.Collections.Generic.HashSet%601> クラスは、<xref:System.Collections.Generic.ISet%601> インターフェイスを実装します。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.IDictionary?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602>
- [ 一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)
