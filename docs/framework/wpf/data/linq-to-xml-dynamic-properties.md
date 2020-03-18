---
title: LINQ to XML 動的プロパティのリファレンス
ms.date: 10/22/2019
ms.topic: reference
ms.openlocfilehash: 48b51e92eb78786b2cc189e3e7daa00875b41585
ms.sourcegitcommit: 5a28f8eb071fcc09b045b0c4ae4b96898673192e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2019
ms.locfileid: "73197055"
---
# <a name="linq-to-xml-dynamic-properties"></a>LINQ to XML の動的プロパティ

ここでは、LINQ to XML の動的プロパティに関する参照情報について説明します。 これらのプロパティは、具体的には <xref:System.Xml.Linq.XAttribute> 名前空間の <xref:System.Xml.Linq.XElement> クラスと <xref:System.Xml.Linq> クラスによって公開されます。

「[LINQ to XML による WPF のデータ バインディングの概要](wpf-data-binding-with-linq-to-xml-overview.md)」のトピックで説明されているように、各動的プロパティには、対応する標準のパブリック プロパティやパブリック メソッドが同じクラスにあります。 ほとんどの用途では、これらの標準のメンバーを使用する必要があります。動的プロパティは、LINQ to XML のデータ バインドのシナリオ専用に用意されています。 これらのクラスの標準のメンバーに関する詳細については、リファレンス トピックの「<xref:System.Xml.Linq.XAttribute>」および「<xref:System.Xml.Linq.XElement>」を参照してください。

このセクションの動的プロパティは、解決される値に関連して次の 2 つのカテゴリに分類されます。

- 1 つの値に解決される単純なプロパティ (`Value` クラスや <xref:System.Xml.Linq.XAttribute> クラスの <xref:System.Xml.Linq.XElement> プロパティなど)。

- インデクサー型に解決されるインデックス値 (<xref:System.Xml.Linq.XElement> の [Elements](elements-xelement-dynamic-property.md) プロパティや [Descendants](descendants-xelement-dynamic-property.md) プロパティなど)。 インデクサー型が目的の値やコレクションに解決されるようにするには、拡張名のパラメーターを渡す必要があります。

<xref:System.Collections.Generic.IEnumerable%601> 型のインデックス値を返す動的プロパティはすべて遅延実行を使用します。 遅延実行について詳しくは、「[LINQ クエリの概要 (C#)](../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)」をご覧ください。

## <a name="reference"></a>辞書／辞典／その他

- <xref:System.Xml.Linq>
- <xref:System.Xml.Linq.XElement?displayProperty=fullName>
- <xref:System.Xml.Linq.XAttribute?displayProperty=fullName>

## <a name="see-also"></a>関連項目

- [LINQ to XML による WPF のデータ バインディング](wpf-data-binding-with-linq-to-xml-overview.md)
- [LINQ to XML による WPF のデータ バインディングの概要](wpf-data-binding-with-linq-to-xml-overview.md)
- [LINQ クエリの概要 (C#)](../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)
