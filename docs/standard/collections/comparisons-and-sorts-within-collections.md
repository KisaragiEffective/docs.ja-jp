---
title: コレクション内での比較と並べ替え
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sorting data, collections
- IComparable.CompareTo method
- Collections classes
- Equals method
- collections [.NET Framework], comparisons
ms.assetid: 5e4d3b45-97f0-423c-a65f-c492ed40e73b
ms.openlocfilehash: 3360652f22ed39ccfd99f9863052fe584b78562f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159261"
---
# <a name="comparisons-and-sorts-within-collections"></a>コレクション内での比較と並べ替え
<xref:System.Collections> クラスは、削除する要素を検索するか、キーと値のペアの値を返すかに関係なく、コレクションの管理に関連するほぼすべての処理において比較を実行します。  
  
 通常、コレクションは等値比較子か順序比較子、またはその両方を使用します。 比較には 2 つのコンストラクターが使用されます。  
  
<a name="BKMK_Checkingforequality"></a>
## <a name="checking-for-equality"></a>等価性のチェック  
 `Contains`、<xref:System.Collections.IList.IndexOf%2A>、<xref:System.Collections.Generic.List%601.LastIndexOf%2A>、`Remove` などのメソッドは、コレクション要素に対して等値比較子を使用します。 コレクションがジェネリックの場合、次のガイドラインに従ってアイテムの等価性が比較されます。  
  
- T 型で <xref:System.IEquatable%601> ジェネリック インターフェイスが実装されている場合、等値比較子はそのインターフェイスの <xref:System.IEquatable%601.Equals%2A> メソッドです。  
  
- T 型で <xref:System.IEquatable%601> が実装されていない場合、<xref:System.Object.Equals%2A?displayProperty=nameWithType> が使用されます。  
  
 また、ディクショナリ コレクションのコンストラクターの一部のオーバーロードでは、<xref:System.Collections.Generic.IEqualityComparer%601> の実装が受け付けられて、キーが等しいかどうかを比較するために使用されます。 例については、 <xref:System.Collections.Generic.Dictionary%602.%23ctor%2A?displayProperty=nameWithType> コンストラクターに関する説明を参照してください。  
  
<a name="BKMK_Determiningsortorder"></a>
## <a name="determining-sort-order"></a>並べ替え順序の決定  
 `BinarySearch` 、 `Sort` などのメソッドは、コレクション要素に対して順序比較子を使用します。 コレクションの要素間または要素と指定された値との間で比較を実行できます。 オブジェクトの比較には、 `default comparer` と `explicit comparer`の概念が適用されます。  
  
 既定の比較子は、比較される 1 つ以上のオブジェクトに依存して **IComparable** インターフェイスを実装します。 リスト コレクションの値として使用されるか、またはディクショナリ コレクションのキーとして使用されるすべてのクラスで、 **IComparable** を実装することをお勧めします。 ジェネリック コレクションの場合、等価比較は次の基準に従って決定されます。  
  
- T 型で <xref:System.IComparable%601?displayProperty=nameWithType> ジェネリック インターフェイスが実装されている場合、既定の比較子はそのインターフェイスの <xref:System.IComparable%601.CompareTo%28%600%29?displayProperty=nameWithType> メソッドです。  
  
- T 型で非ジェネリックの <xref:System.IComparable?displayProperty=nameWithType> インターフェイスが実装されている場合、既定の比較子はそのインターフェイスの <xref:System.IComparable.CompareTo%28System.Object%29?displayProperty=nameWithType> メソッドです。  
  
- T 型でいずれのインターフェイスも実装されていない場合、既定の比較子は存在せず、比較子または比較デリゲートを明示的に指定する必要があります。  
  
 明示的な比較を指定するために、一部のメソッドではパラメーターとして **IComparer** 実装を受け取ります。 たとえば、 <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> メソッドは <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> 実装を受け取ります。  
  
 システムの現在のカルチャ設定は、コレクション内の比較と並べ替えに影響を与える可能性があります。 既定では、**Collections** クラスの比較と並べ替えはカルチャに依存します。 カルチャ設定を無視して一貫した比較と並べ替えの結果を得るには、 <xref:System.Globalization.CultureInfo.InvariantCulture%2A> を受け取るメンバー オーバーロードと共に <xref:System.Globalization.CultureInfo>を使用します。 詳細については、次のトピックを参照してください。 [Performing Culture-Insensitive String Operations in Collections](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-collections.md) および [Performing Culture-Insensitive String Operations in Arrays](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-arrays.md).  
  
<a name="BKMK_Equalityandsortexample"></a>
## <a name="equality-and-sort-example"></a>等価性と並べ替えの例  
 次のコードは、単純なビジネス オブジェクトでの <xref:System.IEquatable%601> と <xref:System.IComparable%601> の実装を示しています。 また、オブジェクトがリストに格納され、並べ替えられている場合、 <xref:System.Collections.Generic.List%601.Sort> メソッドを呼び出すと、結果的に `Part` 型の既定の比較子と、匿名メソッドを使用して実装された <xref:System.Collections.Generic.List%601.Sort%28System.Comparison%7B%600%7D%29> メソッドを使用することになります。  
  
 [!code-csharp[System.Collections.Generic.List.Sort#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.collections.generic.list.sort/cs/program.cs#1)]
 [!code-vb[System.Collections.Generic.List.Sort#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.collections.generic.list.sort/vb/module1.vb#1)]  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.IComparer>
- <xref:System.IEquatable%601>
- <xref:System.Collections.Generic.IComparer%601>
- <xref:System.IComparable>
- <xref:System.IComparable%601>
