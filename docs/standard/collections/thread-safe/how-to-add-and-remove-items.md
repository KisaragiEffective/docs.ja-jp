---
title: '方法: ConcurrentDictionary の項目を追加および削除する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, concurrent dictionary
ms.assetid: 81b64b95-13f7-4532-9249-ab532f629598
ms.openlocfilehash: dc4d13e09a91633fac1fcf5bd8ab5b043473bd7d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75711312"
---
# <a name="how-to-add-and-remove-items-from-a-concurrentdictionary"></a>方法: ConcurrentDictionary の項目を追加および削除する
この例では、<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType> の項目を追加、取得、更新、削除する方法を示します。 このコレクション クラスは、スレッド セーフな実装です。 同時に複数のスレッドが要素へのアクセスを試みる可能性がある場合は常に、このクラスを使用することをお勧めします。  
  
 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> では、コードが事前にキーの存在を調べなくてもデータを追加または削除できるようにする便利なメソッドが提供されています。 次の表では、これらのメソッドとそれを使用する状況を示します。  
  
|方法|使用する状況|  
|------------|---------------|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A>|指定したキーで新しい値を追加し、キーが既に存在する場合は値を置換します。|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A>|指定したキーの既存の値を取得し、キーが存在しない場合はキー/値ペアを指定します。|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryAdd%2A>, <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryGetValue%2A> , <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryUpdate%2A> , <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryRemove%2A>|キー/値ペアを追加、取得、更新、または削除し、キーが既に存在するか、他の何らかの理由で操作が失敗する場合は、代わりの操作を実行します。|  
  
## <a name="example"></a>例  
 次の例では、同時に 2 つの <xref:System.Threading.Tasks.Task> インスタンスを使用して複数の要素を <xref:System.Collections.Concurrent.ConcurrentDictionary%602> に追加し、要素が正常に追加されたことを示すためにすべてのコンテンツを出力します。 また、<xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A>、<xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A>、<xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> の各メソッドを使用して、コレクションの項目を追加、更新、および取得する例も示しています。  
  
 [!code-csharp[CDS#16](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds/cs/cds_dictionaryhowto.cs#16)]
 [!code-vb[CDS#16](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds/vb/cds_concdict.vb#16)]  
  
 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> はマルチスレッド シナリオ向けに設計されています。 コレクションの項目を追加または削除するために、コードでロックを使用する必要はありません。 ただし、あるスレッドが値を取得した直後に、別のスレッドが同じキーと新しい値を指定してコレクションを更新する可能性が常にあります。  
  
 また、<xref:System.Collections.Concurrent.ConcurrentDictionary%602> のメソッドはすべてスレッド セーフですが、<xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> や <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> などの一部のメソッドはアトミックではありません。 これらのメソッドに渡されるユーザー デリゲートは、ディクショナリの内部ロックの外側で呼び出されます (これは、不明なコードがすべてのスレッドをブロックするのを阻止するために実行されます)。 そのため、次のような一連のイベントが発生する可能性があります。  
  
 1\) threadA が <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> を呼び出しましたが、項目が見つからないため、valueFactory デリゲートを呼び出すことにより新しい項目を作成して追加します。  
  
 2\) threadB が同時に <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> を呼び出します。その valueFactory デリゲートが呼び出され、threadA より前に内部ロックに到達し、新しいキー/値ペアをディクショナリに追加します。  
  
 3\) threadA のユーザー デリゲートが完了し、スレッドはロックに到達しますが、項目が既に存在することを検出します。  
  
 4\) threadA は "Get" を実行し、threadB によって前に追加されたデータを返します。  
  
 したがって、<xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> によって返されるデータが、スレッドの valueFactory によって作成された同じデータであることは保証されません。 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> を呼び出したときも、同様の一連のイベントが発生する可能性があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [スレッドセーフなコレクション](../../../../docs/standard/collections/thread-safe/index.md)
