---
title: XSLT 処理中の外部リソースの解決
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 3a59d31c-0ec5-4de6-a2a9-558531c8116e
ms.openlocfilehash: 58407d5f0c6e602af15f5b19b9a19cc6379b9af7
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710285"
---
# <a name="resolving-external-resources-during-xslt-processing"></a>XSLT 処理中の外部リソースの解決
XSLT 変換中には、外部リソースの解決が必要になる場合があります。  
  
## <a name="using-the-xmlresolver-class"></a>XmlResolver クラスの使い方  
 外部リソースを解決するには、<xref:System.Xml.XmlResolver> クラスを使用します。 XSLT 処理中に <xref:System.Xml.XmlResolver> が使用される場合を次の表に示します。  
  
|XSLT タスク|XmlResolver が使用される場合|  
|---------------|--------------------------------------|  
|スタイル シートのコンパイル。|スタイル シートの URI の解決。<br /><br /> および<br /><br /> `xsl:import` 要素または `xsl:include` 要素の URI リファレンスの解決。|  
|スタイル シートの実行。|コンテキスト ドキュメントの URI の解決。<br /><br /> および<br /><br /> XSLT の `document()` 関数での URI リファレンスの解決。|  
  
 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> メソッドおよび <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドには、<xref:System.Xml.XmlResolver> オブジェクトを引数の 1 つとして受け取るオーバーロードが含まれます。 <xref:System.Xml.XmlResolver> を指定しない場合は、資格情報を持たない既定の <xref:System.Xml.XmlUrlResolver> が使用されます。  
  
 <xref:System.Xml.XmlResolver> オブジェクトを使用する場合の説明を次の一覧に示します。  
  
- XSLT 処理で認証が必要なネットワーク リソースにアクセスする必要がある場合、必要な資格情報に対して <xref:System.Xml.XmlResolver> を使用します。  
  
- XSLT 処理がアクセスできるリソースを制限する場合、適切なアクセス許可セットに対して <xref:System.Xml.XmlSecureResolver> を使用します。 制御対象外の (信頼できない) リソースを開く場合には、<xref:System.Xml.XmlSecureResolver> クラスを使用します。  
  
- 動作をカスタマイズする場合は、独自の <xref:System.Xml.XmlResolver> クラスを実装し、これを使用してリソースを解決することができます。  
  
- 外部リソースにアクセスできないようにする場合は、<xref:System.Xml.XmlResolver> の引数に `null` を指定します。  
  
## <a name="example"></a>使用例  
 ネットワーク リソースに格納されているスタイル シートをコンパイルする例を次に示します。 <xref:System.Xml.XmlUrlResolver> オブジェクトには、スタイル シートにアクセスするのに必要な資格情報を指定します。  
  
 [!code-csharp[XslCompiledTransform.Load#11](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Load/CS/Xslt_Load_v2.cs#11)]
 [!code-vb[XslCompiledTransform.Load#11](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Load/VB/Xslt_Load_v2.vb#11)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.Xsl.XslCompiledTransform>
- <xref:System.Xml.Xsl.XsltSettings>
- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
