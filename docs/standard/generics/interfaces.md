---
title: ジェネリック インターフェイス
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- generic interfaces [.NET Framework]
- equality comparisons [.NET Framework]
- generics [.NET Framework], interfaces
- ordering comparisons [.NET Framework]
ms.assetid: 88bf5b04-d371-4edb-ba38-01ec7cabaacf
ms.openlocfilehash: 704ada32d428c468d5b71a3f1390568ca586079e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708325"
---
# <a name="generic-interfaces"></a>ジェネリック インターフェイス
このトピックでは、ジェネリック型のファミリ間に共通する機能を提供するジェネリック インターフェイスについて概説します。  
  
## <a name="generic-interfaces"></a>ジェネリック インターフェイス  
 ジェネリック インターフェイスは、順序付け比較や等価比較、およびジェネリック コレクション型で共有される機能のために、非ジェネリック インターフェイスに対応するタイプ セーフな機能を提供します。  
  
> [!NOTE]
> .NET Framework 4 以降では、いくつかのジェネリック インターフェイスの型パラメーターが共変または反変としてマークされ、それらのインターフェイスを実装する型をより柔軟に割り当てたり使用したりできます。 「 [共変性と反変性](../../../docs/standard/generics/covariance-and-contravariance.md)を参照してください。  
  
### <a name="equality-and-ordering-comparisons"></a>等価比較と順序付け比較  
 <xref:System> 名前空間で、<xref:System.IComparable%601?displayProperty=nameWithType> と <xref:System.IEquatable%601?displayProperty=nameWithType> ジェネリック インターフェイスは、対応する非ジェネリック インターフェイスと同じように、順序付け比較と等価比較の方法をそれぞれ定義します。 型は、そのような比較の実施能力を提供するために、これらのインターフェイスを実装します。  
  
 <xref:System.Collections.Generic> 名前空間で、<xref:System.Collections.Generic.IComparer%601> と <xref:System.Collections.Generic.IEqualityComparer%601> ジェネリック インターフェイスは、<xref:System.IComparable%601?displayProperty=nameWithType> や <xref:System.IEquatable%601?displayProperty=nameWithType> ジェネリック インターフェイスを実装しない型のために、順序付け比較または等価比較を定義する方法を提供します。また、実装する型のために、それらの関係を再定義する方法も提供します。 これらのインターフェイスは、多くのジェネリック コレクション クラスのメソッドやコンストラクターで使用されます。 たとえば、ジェネリック <xref:System.Collections.Generic.IComparer%601> オブジェクトを <xref:System.Collections.Generic.SortedDictionary%602> クラスのコンストラクターに渡して、ジェネリック <xref:System.IComparable%601?displayProperty=nameWithType> を実装しない型の並べ替え順序を指定することができます。 ジェネリック <xref:System.Array.Sort%2A?displayProperty=nameWithType> 実装を使用して配列やリストを並べ替えるための、<xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> ジェネリック静的メソッドと <xref:System.Collections.Generic.IComparer%601> インスタンス メソッドのオーバーロードがあります。  
  
 <xref:System.Collections.Generic.Comparer%601> と <xref:System.Collections.Generic.EqualityComparer%601> ジェネリック クラスは、<xref:System.Collections.Generic.IComparer%601> と <xref:System.Collections.Generic.IEqualityComparer%601> ジェネリック インターフェイスの実装の基本クラスを提供し、それぞれの <xref:System.Collections.Generic.Comparer%601.Default%2A?displayProperty=nameWithType> および <xref:System.Collections.Generic.EqualityComparer%601.Default%2A?displayProperty=nameWithType> プロパティによって、既定の順序付け比較と等価比較を提供します。  
  
### <a name="collection-functionality"></a>コレクションの機能  
 <xref:System.Collections.Generic.ICollection%601> ジェネリック インターフェイスは、ジェネリック コレクション型の基本インターフェイスです。 これは、要素を追加、削除、コピー、および列挙するための基本的な機能を提供します。 <xref:System.Collections.Generic.ICollection%601> は、ジェネリック <xref:System.Collections.Generic.IEnumerable%601> と非ジェネリック <xref:System.Collections.IEnumerable> の両方から継承します。  
  
 <xref:System.Collections.Generic.IList%601> ジェネリック インターフェイスは、インデックス付きの取得のための手段により、<xref:System.Collections.Generic.ICollection%601> ジェネリック インターフェイスが機能拡張されたものです。  
  
 <xref:System.Collections.Generic.IDictionary%602> ジェネリック インターフェイスは、キー付きの取得のための手段により、<xref:System.Collections.Generic.ICollection%601> ジェネリック インターフェイスが機能拡張されたものです。 .NET Framework の基本クラス ライブラリのジェネリック ディクショナリ型は、非ジェネリック <xref:System.Collections.IDictionary> インターフェイスも実装します。  
  
 <xref:System.Collections.Generic.IEnumerable%601> ジェネリック インターフェイスは、ジェネリック列挙子の構造体を提供します。 ジェネリック列挙子によって実装される <xref:System.Collections.Generic.IEnumerator%601> ジェネリック インターフェイスは、非ジェネリック <xref:System.Collections.IEnumerator> インターフェイスを継承します。型パラメーター <xref:System.Collections.IEnumerator.MoveNext%2A> に依存しない <xref:System.Collections.IEnumerator.Reset%2A> および `T` メンバーは、非ジェネリック インターフェイスにのみ表示されます。 つまり、非ジェネリック インターフェイスのコンシューマーはすべて、ジェネリック インターフェイスも使用できます。  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [ジェネリック](../../../docs/standard/generics/index.md)
- [.NET Framework のジェネリック コレクション](../../../docs/standard/generics/collections.md)
- [配列とリストの操作に使用する汎用デリゲート](../../../docs/standard/generics/delegates-for-manipulating-arrays-and-lists.md)
- [共変性と反変性](../../../docs/standard/generics/covariance-and-contravariance.md)
