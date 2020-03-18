---
title: XslTransform への XmlDataDocument の入力
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: a0b536b6-cdb3-4a44-86c2-3b2ebc7bd4c9
ms.openlocfilehash: 5f2a7ef22eb07bb221b6a145db4453996ad8bf8c
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709856"
---
# <a name="xmldatadocument-input-to-xsltransform"></a>XslTransform への XmlDataDocument の入力
> [!NOTE]
> .NET Framework 2.0 では <xref:System.Xml.Xsl.XslTransform> クラスが廃止されています。 <xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。 詳しくは、「[XslCompiledTransform クラスの使用](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md)」および「[XslTransform クラスからの移行](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md)」をご覧ください。  
  
 Microsoft .NET Framework は、XML ドキュメント内のデータにアクセスするための XML ドキュメント オブジェクト モデル (DOM) を実装しているほか、XML ドキュメント内で読み込み、書き込み、移動を行うためのさまざまなクラスも実装しています。 <xref:System.Xml> 名前空間にある <xref:System.Xml.XmlDataDocument> は、<xref:System.Data.DataSet> 内のデータへのリレーショナル アクセス機能およびリレーショナル データとの同期機能を備えています。 <xref:System.Data.DataSet> のリレーショナル表現を介して構造化 XML を表示すると同時に操作したり、<xref:System.Xml.XmlDataDocument> の DOM 表現を介して半構造化 XML を操作したりすることができます。 したがって、<xref:System.Xml.XmlDataDocument> は、XML 環境とリレーショナル環境の境界を越えて機能します。  
  
 データがリレーショナル構造に格納されている場合、そのデータを XSLT 変換への入力として使用するには、そのリレーショナル データを <xref:System.Data.DataSet> に読み込み、<xref:System.Xml.XmlDataDocument> に関連付けます。 <xref:System.Xml.XPath.XPathNavigator> への入力である <xref:System.Xml.Xsl.XslTransform> は、<xref:System.Xml.XmlDataDocument> インターフェイスを介して <xref:System.Xml.XPath.IXPathNavigable> に実装されます。 リレーショナル データを受け取り、それを <xref:System.Data.DataSet> に読み込み、<xref:System.Xml.XmlDataDocument> 内の同期機能を利用することで、そのリレーショナル データに対する XSLT 変換を実行できます。  
  
 リレーショナル データへの変換の適用については、「[DataSet への XSLT 変換の適用](../../../../docs/framework/data/adonet/dataset-datatable-dataview/applying-an-xslt-transform-to-a-dataset.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDataDocument>
- [DataSet と XmlDataDocument の同期](../../../../docs/framework/data/adonet/dataset-datatable-dataview/dataset-and-xmldatadocument-synchronization.md)
- [XslTransform クラスを使用した XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform クラスによる XSLT プロセッサの実装](../../../../docs/standard/data/xml/xsltransform-class-implements-the-xslt-processor.md)
- [変換における XPathNavigator](../../../../docs/standard/data/xml/xpathnavigator-in-transformations.md)
- [変換における XPathNodeIterator](../../../../docs/standard/data/xml/xpathnodeiterator-in-transformations.md)
- [XslTransform への XPathDocument の入力](../../../../docs/standard/data/xml/xpathdocument-input-to-xsltransform.md)
- [XslTransform への XmlDocument の入力](../../../../docs/standard/data/xml/xmldocument-input-to-xsltransform.md)
