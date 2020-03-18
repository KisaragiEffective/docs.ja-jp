---
title: XML スキーマの使用
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: bbbcc70c-bf9a-4f6a-af72-1bab5384a187
ms.openlocfilehash: 0fd7313e800024ebb7e3563cb4323c5780cbf1c3
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710012"
---
# <a name="working-with-xml-schemas"></a>XML スキーマの使用
XML ドキュメントの構造、その要素間のリレーションシップ、データ型、および内容の制約を定義するには、ドキュメント型定義 (DTD: Document Type Definition) または XML スキーマ定義言語 (XSD) スキーマを使用します。 XML ドキュメントは、W3C (World Wide Web Consortium) 勧告『Extensible Markup Language (XML) 1.0』で定義されている構文要件をすべて満たしている場合には整形式と見なされますが、整形式であると同時に DTD またはスキーマに定義されている制約に準拠していなければ有効とは見なされません。 したがって、有効な XML ドキュメントはすべて整形式ですが、整形式の XML ドキュメントがすべて有効であるとは限りません。  
  
 XML の詳細については、[W3C 勧告『XML 1.0』](https://www.w3.org/TR/REC-xml/)を参照してください。 XML スキーマの詳細については、[W3C 勧告『XML Schema Part 1: Structures』](https://www.w3.org/TR/xmlschema-1/)および[『XML Schema Part 2: Datatypes』](https://www.w3.org/TR/xmlschema-2/)を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [XML スキーマ オブジェクト モデル (SOM)](../../../../docs/standard/data/xml/xml-schema-object-model-som.md)  
 <xref:System.Xml.Schema?displayProperty=nameWithType> 名前空間のスキーマ オブジェクト モデル (SOM) について説明します。SOM は、ファイルからスキーマ定義言語 (XSD) スキーマを読み取ったり、プログラムを使用してメモリ内にスキーマを作成したりできる一連のクラスを提供します。  
  
 [スキーマをコンパイルするための XmlSchemaSet](../../../../docs/standard/data/xml/xmlschemaset-for-schema-compilation.md)  
 <xref:System.Xml.Schema.XmlSchemaSet> クラスについて説明します。このクラスは、XSD スキーマを格納または検証できるキャッシュです。  
  
 [XmlSchemaValidator のプッシュ ベースの検証](../../../../docs/standard/data/xml/xmlschemavalidator-push-based-validation.md)  
 <xref:System.Xml.Schema.XmlSchemaValidator> クラスについて説明します。このクラスは、プッシュ ベース方式で、XSD スキーマを基準として XML データを検証する、効率的かつ高性能なしくみを提供します。  
  
 [XML スキーマの推論](../../../../docs/standard/data/xml/inferring-an-xml-schema.md)  
 <xref:System.Xml.Schema.XmlSchemaInference> クラスを使用して XML ドキュメントの構造から XSD スキーマを推論する方法を説明します。  
  
## <a name="reference"></a>参照先  
 <xref:System.Xml.Schema.XmlSchemaSet> &#124; <xref:System.Xml.Schema.XmlSchemaInference> &#124; <xref:System.Xml.XmlReader>  
  
## <a name="related-sections"></a>関連セクション  
 [DOM における XML ドキュメントの検証](../../../../docs/standard/data/xml/validating-an-xml-document-in-the-dom.md)  
 ドキュメント オブジェクト モデル (DOM) の XML を検証する方法を説明します。 検証できる XML は、DOM に読み込まれている XML か、または事前に検証されていない DOM の XML ドキュメントです。  
  
 [XPathNavigator を使用したスキーマ検証](../../../../docs/standard/data/xml/schema-validation-using-xpathnavigator.md)  
 <xref:System.Xml.XPath.XPathNavigator> クラスを使用して操作中および編集中の XML を検証する方法を説明します。
