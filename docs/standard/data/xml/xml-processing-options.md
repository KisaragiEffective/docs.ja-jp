---
title: XML の処理オプション
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 33ced8ee-1745-4e71-8dee-ebe70ec067c7
ms.openlocfilehash: 1707ed10d944a518872132dded40751026a4c8e7
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709921"
---
# <a name="xml-processing-options"></a>XML の処理オプション
XML データの処理に使用できる Microsoft テクノロジの一覧については、次の表を参照してください。  
  
## <a name="net-framework-options"></a>.NET Framework のオプション  
  
|**オプション**|**処理の種類**|**説明**|  
|----------------|-------------------------|---------------------|  
|[LINQ to XML (C#)](../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) <br/> [LINQ to XML (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md) <br />(<xref:System.Xml.Linq> 名前空間)|メモリ内|- .NET Framework の統合言語クエリ (LINQ) テクノロジに基づいています。<br />- オブジェクト、リレーショナル データ、XML データに対して SQL と同じようにクエリを利用できます。<br />- 直感的なドキュメント作成および変換機能を提供します。<br />- このオプションは、新しいコードを記述する場合に使用します。|  
|<xref:System.Xml.XmlReader?displayProperty=nameWithType>|ストリーム ベース|- 高速、非キャッシュ、前方参照専用の XML データ アクセス手段を提供します。<br />- <xref:System.Xml.XmlReader.Create%2A?displayProperty=nameWithType> メソッドを使用してオブジェクトを作成し、<xref:System.Xml.XmlReaderSettings> クラスを使用して、そのオブジェクトで有効にする一連の機能を指定できます。|  
|<xref:System.Xml.XmlWriter?displayProperty=nameWithType>|ストリーム ベース|- 高速、非キャッシュ、前方参照専用の XML データ生成手段を提供します。<br />- <xref:System.Xml.XmlWriter.Create%2A?displayProperty=nameWithType> メソッドを使用してオブジェクトを作成し、<xref:System.Xml.XmlWriterSettings> クラスを使用して、そのオブジェクトで有効にする一連の機能を指定できます。|  
|<xref:System.Xml.XmlDocument?displayProperty=nameWithType>|メモリ内|- [W3C ドキュメント オブジェクト モデル (DOM) 勧告の DOM Level 1 Core](https://www.w3.org/TR/REC-DOM-Level-1/level-one-core.html) および [DOM Level 2 Core](https://www.w3.org/TR/DOM-Level-2-Core/) を実装します。<br />- 使い慣れた DOM モデルに基づくメソッドとプロパティを使用して、ノードを作成、挿入、削除、変更することができます。<br />- このオプションは、W3C DOM を利用する既存のコードを変更する場合に使用します。|  
|<xref:System.Xml.XPath.XPathNavigator?displayProperty=nameWithType>|メモリ内|- カーソル モデルを利用する複数の編集オプションとナビゲーション機能を提供します。<br />- XML ドキュメントを <xref:System.Xml.XPath.XPathDocument> オブジェクトまたは <xref:System.Xml.XmlDocument> オブジェクトに格納できます。<br />- XML の読み取り専用処理で優れたパフォーマンスを発揮します。<br />- このオプションは、XPath クエリや XSLT 変換を使用して既存のコードを変更する場合に使用します。|  
|<xref:System.Xml.Xsl.XslCompiledTransform>|メモリ内|- XSL 変換を使用して XML データを変換するためのオプションを提供します。<br />- [XSLT コンパイラ (xsltc.exe)](../../../../docs/standard/data/xml/xslt-compiler-xsltc-exe.md) を使用すると、プリコンパイルした変換をアプリで参照できます。|  
  
## <a name="win32-and-com-based-options"></a>Win32 オプションと COM ベースのオプション  
  
|**オプション**|**説明**|  
|----------------|---------------------|  
|[XmlLite](https://docs.microsoft.com/previous-versions/windows/desktop/ms752872(v=vs.85))|- 高性能 XML アプリの構築に役立つ高速、安全、非キャッシュ、前方参照専用の XML パーサー。<br />- ダイナミック リンク ライブラリ (DLL) を使用できる任意の言語で動作します。C++ の使用をお勧めします。|  
|[MSXML](https://docs.microsoft.com/previous-versions/windows/desktop/ms763742(v=vs.85))|- Windows オペレーティング システムに付属する、XML 処理のための COM ベース テクノロジ。<br />- DOM をネイティブで実装し、XPath と XSLT をサポートしています。<br />- SAX2 イベントベース パーサーが含まれます。|  
  
## <a name="see-also"></a>関連項目

- [DOM モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XSLT コンパイラ (xsltc.exe)](../../../../docs/standard/data/xml/xslt-compiler-xsltc-exe.md)
