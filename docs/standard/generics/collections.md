---
title: .NET の汎用コレクション
ms.date: 02/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET], collections
- generic collections [.NET]
- generic types [.NET]
ms.assetid: 5b646751-6ab7-465c-916c-b1a76aefa9f5
ms.openlocfilehash: dce0e38b0198396ec0dbc3ced7f2f59c2b112b56
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708411"
---
# <a name="generic-collections-in-net"></a>.NET の汎用コレクション

 .NET クラス ライブラリでは、<xref:System.Collections.Generic> および <xref:System.Collections.ObjectModel> の名前空間に多数のジェネリック コレクション クラスが用意されています。 各クラスについて詳しくは、「[一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)」を参照してください。  
  
## <a name="systemcollectionsgeneric"></a>System.Collections.Generic

 ジェネリック コレクション型の多くは、非ジェネリック型に直接類似しています。 <xref:System.Collections.Generic.Dictionary%602> は、<xref:System.Collections.Hashtable> のジェネリック バージョンです。これは列挙体のために <xref:System.Collections.Generic.KeyValuePair%602> ではなくジェネリック構造体 <xref:System.Collections.DictionaryEntry> を使用します。  
  
 <xref:System.Collections.Generic.List%601> は <xref:System.Collections.ArrayList> のジェネリック バージョンです。 ジェネリックの <xref:System.Collections.Generic.Queue%601> および <xref:System.Collections.Generic.Stack%601> クラスには、非ジェネリックのバージョンに対応するものがあります。  
  
 <xref:System.Collections.Generic.SortedList%602> には、ジェネリックおよび非ジェネリックのバージョンがあります。 どちらのバージョンも、ディクショナリとリストのハイブリッドです。 <xref:System.Collections.Generic.SortedDictionary%602> ジェネリック クラスは純粋なディクショナリであり、対応する非ジェネリックのバージョンはありません。  
  
 <xref:System.Collections.Generic.LinkedList%601> ジェネリック クラスは、実際のリンク リストです。 これには、対応する非ジェネリックのバージョンはありません。  
  
## <a name="systemcollectionsobjectmodel"></a>System.Collections.ObjectModel

 <xref:System.Collections.ObjectModel.Collection%601> ジェネリック クラスは、独自のジェネリック コレクション型を派生させるための基本クラスを提供します。 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> クラスは、<xref:System.Collections.Generic.IList%601> ジェネリック インターフェイスを実装する任意の型から、読み取り専用のコレクションを生成するための簡単な方法を提供します。 <xref:System.Collections.ObjectModel.KeyedCollection%602> ジェネリック クラスは、独自のキーを含むオブジェクトを格納するための方法を提供します。  
  
## <a name="other-generic-types"></a>その他のジェネリック型

 <xref:System.Nullable%601> ジェネリック構造体により、`null` が割り当て可能であるかのように値型を使用できます。 これは、値型が含まれるフィールドが欠落している可能性のあるデータベース クエリで作業するときに便利です。 ジェネリック型パラメーターには、任意の値型を指定できます。  
  
> [!NOTE]
> C# および Visual Basic には null 許容型の構文があるので、その言語では <xref:System.Nullable%601> を明示的に使用する必要はありません。 「[null 許容値型 (C# リファレンス)](../../csharp/language-reference/builtin-types/nullable-value-types.md)」および「[null 許容値型 (Visual Basic)](../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)」を参照してください。
  
 <xref:System.ArraySegment%601> ジェネリック構造体は、任意の型の 0 から始まる 1 次元の配列内で、要素の範囲を区切る方法を提供します。 ジェネリック型パラメーターは、配列の要素の型です。  
  
 <xref:System.EventHandler%601> ジェネリック デリゲートにより、イベントが .NET Framework によって使用されるイベント処理パターンに従う場合に、イベントを処理するためにデリゲート型を宣言する必要がなくなります。 たとえば、イベントのデータを保持するために、`MyEventArgs` から派生した <xref:System.EventArgs> クラスを作成したとします。 その場合、イベントを次のように宣言できます。  
  
 [!code-cpp[Conceptual.Generics.Overview#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source2.cpp#7)]
 [!code-csharp[Conceptual.Generics.Overview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source2.cs#7)]
 [!code-vb[Conceptual.Generics.Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source2.vb#7)]  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [ジェネリック](../../../docs/standard/generics/index.md)
- [配列とリストの操作に使用する汎用デリゲート](../../../docs/standard/generics/delegates-for-manipulating-arrays-and-lists.md)
- [ジェネリック インターフェイス](../../../docs/standard/generics/interfaces.md)
