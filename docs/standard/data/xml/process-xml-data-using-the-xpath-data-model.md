---
title: XPath データ モデルを使用した XML データの処理
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 536c6fce-1453-4654-9c72-bca54d47e081
ms.openlocfilehash: f964864577cf08eb074bdfb9af7f7daf3ffb37b9
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710441"
---
# <a name="process-xml-data-using-the-xpath-data-model"></a>XPath データ モデルを使用した XML データの処理
<xref:System.Xml?displayProperty=nameWithType> 名前空間は、<xref:System.Xml.XmlDocument> または <xref:System.Xml.XPath.XPathDocument> クラスを使用して、メモリ内の XML ドキュメント、フラグメント、ノード、またはノードセットのプログラム表現を提供します。  
  
 <xref:System.Xml.XPath.XPathDocument> クラスは、XPath データ モデルによる XML ドキュメントの高速、読み取り専用のメモリ内表現を提供します。 <xref:System.Xml.XmlDocument> クラスは、W3C ドキュメント オブジェクト モデル (DOM) の DOM Level 1 Core および DOM Level 2 Core を実装する XML ドキュメントの編集可能なメモリ内表現です。 どちらのクラスも <xref:System.Xml.XPath.IXPathNavigable> インターフェイスを実装し、基になる XML データの選択、評価、移動、および (場合によっては) 編集に使用される <xref:System.Xml.XPath.XPathNavigator> オブジェクトを返します。  
  
 以下では、<xref:System.Xml.XPath.XPathNavigator> クラスの機能を、それを返すクラスに基づいて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [XPathDocument および XmlDocument を使用した XML データの読み取り](../../../../docs/standard/data/xml/reading-xml-data-using-xpathdocument-and-xmldocument.md)  
 XML ドキュメントを読むために読み取り専用の <xref:System.Xml.XPath.XPathDocument> クラス オブジェクトを作成する方法、および XML ドキュメントを読み込んで編集するために編集可能な <xref:System.Xml.XmlDocument> クラス オブジェクトを作成する方法について説明します。 このトピックでは、各クラスから <xref:System.Xml.XPath.XPathNavigator> オブジェクトを返して、XML ドキュメント内を移動して編集する方法について説明します。  
  
 [XPathNavigator を使用した XML データの選択、評価、および照合](../../../../docs/standard/data/xml/selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)  
 XPath クエリによる <xref:System.Xml.XPath.XPathNavigator> または <xref:System.Xml.XPath.XPathDocument> オブジェクト内のノードの選択、XPath 式の結果の評価と検査、および XML ドキュメントのノードが指定された XPath 式に一致するかどうかの判定に使用される <xref:System.Xml.XmlDocument> クラスのメソッドについて説明します。  
  
 [XPathNavigator による XML データへのアクセス](../../../../docs/standard/data/xml/accessing-xml-data-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトまたは <xref:System.Xml.XPath.XPathDocument> オブジェクト内で、ノード間の移動、XML データの抽出、および厳密に型指定された XML データへのアクセスに使用される <xref:System.Xml.XmlDocument> クラスのメソッドについて説明します。  
  
 [XPathNavigator による XML データの編集](../../../../docs/standard/data/xml/editing-xml-data-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトに含まれる XML ドキュメントでノードと値の挿入、変更、および削除に使用される <xref:System.Xml.XmlDocument> クラスのメソッドについて説明します。  
  
 [XPathNavigator を使用したスキーマ検証](../../../../docs/standard/data/xml/schema-validation-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathDocument> または <xref:System.Xml.XmlDocument> オブジェクトに含まれる XML コンテンツの検証方法について説明します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [DOM モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)
