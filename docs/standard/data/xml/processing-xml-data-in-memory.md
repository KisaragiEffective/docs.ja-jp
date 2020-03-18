---
title: メモリ内の XML データの処理
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 1bbb4d05-ead7-4bda-8ece-f86d35c57ad4
ms.openlocfilehash: 038bcfcb9d40ee6087efa3487b6f27f252393f2c
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710428"
---
# <a name="processing-xml-data-in-memory"></a>メモリ内の XML データの処理
Microsoft .NET Framework には、XML データを処理するための 3 つのモデルである <xref:System.Xml.XmlDocument> クラス、<xref:System.Xml.XPath.XPathDocument> クラス、および [LINQ to XML (C#)](../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) と [LINQ to XML (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md) が含まれています。  
  
 <xref:System.Xml.XmlDocument> クラスは、W3C ドキュメント オブジェクト モデル (DOM) 勧告の DOM Level 1 Core および DOM Level 2 Core を実装しています。 DOM は XML ドキュメントのメモリ内 (キャッシュ) ツリー表現です。 <xref:System.Xml.XmlDocument> およびその関連クラスを使用すると、XML ドキュメントの作成、データの読み込みとアクセス、データの変更、および変更の保存が可能です。  
  
 <xref:System.Xml.XPath.XPathDocument> クラスは、XPath データ モデルに基づく、読み取り専用のメモリ内データ ストアです。 <xref:System.Xml.XPath.XPathNavigator> クラスは、読み取り専用の <xref:System.Xml.XPath.XPathDocument> クラスと <xref:System.Xml.XmlDocument> クラス内の XML ドキュメント全体にカーソル モデルを使用して、いくつかの編集オプションとナビゲーション機能を提供します。  
  
 [LINQ to XML](../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) は、XML データの処理を目的として .NET Framework バージョン 3.5 で導入されたモデルです。 [統合言語クエリ (LINQ)](../../../csharp/programming-guide/concepts/linq/index.md) を活用するメモリ内モデルです。 LINQ では C# および Visual Basic の言語構文を拡張することで、新しいクエリ機能を実現しています。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [DOM モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)  
 <xref:System.Xml.XmlDocument> とその関連クラスを使用した XML データの処理について説明します。  
  
 [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)  
 <xref:System.Xml.XPath.XPathDocument>、<xref:System.Xml.XmlDocument>、および <xref:System.Xml.XPath.XPathNavigator> クラスを使用した XML データの処理について説明します。  
  
 [LINQ to XML を使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-linq-to-xml.md)  
 LINQ to XML の概要を簡単に説明し、LINQ to XML に関する参照先のリンクを示します。  
  
## <a name="related-sections"></a>関連セクション  
 [XML ドキュメントと XML データ](../../../../docs/standard/data/xml/index.md)
