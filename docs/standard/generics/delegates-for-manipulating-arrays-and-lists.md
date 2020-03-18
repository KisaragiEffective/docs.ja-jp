---
title: 配列とリストの操作に使用する汎用デリゲート
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- delegates [.NET Framework], generic delegates
- chaining delegates
- arrays [.NET Framework], generic delegates
- generic delegates [.NET Framework]
- lists [.NET Framework], generic delegates
- generics [.NET Framework], delegates
ms.assetid: 416be383-cc61-4102-9b1b-88b51adb963e
ms.openlocfilehash: baf8497289ee71c2dbdc544607212de90928289c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708385"
---
# <a name="generic-delegates-for-manipulating-arrays-and-lists"></a>配列とリストの操作に使用する汎用デリゲート
ここでは、配列またはコレクションの要素に対して実行される変換、検索述語、およびアクションの汎用デリゲートの概要について説明します。  
  
## <a name="generic-delegates-for-manipulating-arrays-and-lists"></a>配列とリストの操作に使用する汎用デリゲート  
 <xref:System.Action%601> 汎用デリゲートは、指定した型の要素に対して何らかのアクションを実行するメソッドを表します。 要素に対して必要なアクションを実行するメソッドを作成し、そのメソッドを表す <xref:System.Action%601> デリゲートのインスタンスを作成した後、配列とデリゲートを静的ジェネリック メソッド <xref:System.Array.ForEach%2A?displayProperty=nameWithType> に渡すことができます。 このメソッドは、配列の各要素に対して呼び出されます。  
  
 <xref:System.Collections.Generic.List%601> ジェネリック クラスにも、<xref:System.Collections.Generic.List%601.ForEach%2A> デリゲートを使用する <xref:System.Action%601> メソッドが用意されています。 このメソッドはジェネリックではありません。  
  
> [!NOTE]
> これは、ジェネリック型とジェネリック メソッドに関する興味深い点です。 <xref:System.Array.ForEach%2A?displayProperty=nameWithType> はジェネリック型ではないため、`Shared` メソッドは static (Visual Basic では <xref:System.Array>) かつジェネリックであることが必要です。<xref:System.Array.ForEach%2A?displayProperty=nameWithType> に型を指定して動作させることができるのは、このメソッドが独自の型パラメーター リストを保持しているからです。 これに対して、非ジェネリック <xref:System.Collections.Generic.List%601.ForEach%2A?displayProperty=nameWithType> メソッドは <xref:System.Collections.Generic.List%601> ジェネリック クラスに属しています。したがって、このメソッドは、このクラスの型パラメーターを使用しているにすぎません。 このクラスは厳密に型指定されているため、メソッドをインスタンス メソッドにすることができます。  
  
 <xref:System.Predicate%601> 汎用デリゲートは、特定の要素が定義されている基準を満たしているかどうかを判断するメソッドを表します。 <xref:System.Array> の静的ジェネリック メソッドである <xref:System.Array.Exists%2A>、<xref:System.Array.Find%2A>、<xref:System.Array.FindAll%2A>、<xref:System.Array.FindIndex%2A>、<xref:System.Array.FindLast%2A>、<xref:System.Array.FindLastIndex%2A>、および <xref:System.Array.TrueForAll%2A> でこのデリゲートを使用することにより、要素または要素のセットを検索できます。  
  
 <xref:System.Predicate%601> は、<xref:System.Collections.Generic.List%601> ジェネリック クラスの対応する非ジェネリック インスタンス メソッドも使用します。  
  
 <xref:System.Comparison%601> 汎用デリゲートを使用すると、ネイティブな並べ替え順序のない配列またはリストの要素の並べ替え順序を指定したり、ネイティブな並べ替え順序をオーバーライドしたりできます。 比較を実行するメソッドを作成し、そのメソッドを表す <xref:System.Comparison%601> デリゲートのインスタンスを作成した後、配列とデリゲートを <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Comparison%7B%60%600%7D%29?displayProperty=nameWithType> 静的ジェネリック メソッドに渡します。 <xref:System.Collections.Generic.List%601> ジェネリック クラスには、対応するインスタンス メソッド オーバーロードである <xref:System.Collections.Generic.List%601.Sort%28System.Comparison%7B%600%7D%29?displayProperty=nameWithType> が用意されています。  
  
 <xref:System.Converter%602> 汎用デリゲートを使用すると、2 つの型の間での変換を定義し、一方の型の配列をもう一方の型の配列に変換したり、一方の型のリストをもう一方の型のリストに変換したりできます。 既存のリストの要素を新しい型に変換するメソッドを作成し、そのメソッドを表すデリゲート インスタンスを作成したら、<xref:System.Array.ConvertAll%2A?displayProperty=nameWithType> 静的ジェネリック メソッドを使用して、元の配列から新しい型の配列を生成します。また、元のリストから新しい型のリストを生成する場合は、<xref:System.Collections.Generic.List%601.ConvertAll%60%601%28System.Converter%7B%600%2C%60%600%7D%29?displayProperty=nameWithType> ジェネリック インスタンス メソッドを使用します。  
  
### <a name="chaining-delegates"></a>デリゲートのチェーン  
 これらのデリゲートを使用するメソッドの多くは、別のメソッドに渡すことのできる配列またはリストを返します。 たとえば、配列の特定の要素を選択して新しい型に変換し、新しい配列に保存すると、<xref:System.Array.FindAll%2A> ジェネリック メソッドによって返される配列を <xref:System.Array.ConvertAll%2A> ジェネリック メソッドに渡すことができます。 新しい要素の型にネイティブな並べ替え順序がない場合は、<xref:System.Array.ConvertAll%2A> ジェネリック メソッドによって返された配列を <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Comparison%7B%60%600%7D%29> ジェネリック メソッドに渡すことができます。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [ジェネリック](../../../docs/standard/generics/index.md)
- [.NET Framework のジェネリック コレクション](../../../docs/standard/generics/collections.md)
- [ジェネリック インターフェイス](../../../docs/standard/generics/interfaces.md)
- [共変性と反変性](../../../docs/standard/generics/covariance-and-contravariance.md)
