---
title: XSLT のセキュリティに関する考慮事項
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: fea695be-617c-4977-9567-140e820436fc
ms.openlocfilehash: e6e490c0f637aace57dacc88ef49cc9be87532cd
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709687"
---
# <a name="xslt-security-considerations"></a>XSLT のセキュリティに関する考慮事項
XSLT 言語には、優れた性能と柔軟性を兼ね備えた豊富な機能が用意されています。 ただし、多くの機能を利用できることが便利であると同時に、外部から不正に利用される可能性もあります。 XSLT を安全に使用するために、XSLT の使用に伴うさまざまなセキュリティ上の問題とそのリスクを軽減するための基本的な対策を理解しておく必要があります。  
  
## <a name="xslt-extensions"></a>XSLT 拡張機能  
 スタイル シート スクリプトと拡張オブジェクトの 2 つは、XSLT の一般的な拡張機能です。 これらの拡張機能を使用すると、XSLT プロセッサでコードを実行できます。  
  
- 拡張オブジェクトは、XSLT にプログラミング機能を追加します。  
  
- スクリプトは、拡張要素である `msxsl:script` 要素を使用してスタイル シートに埋め込むことができます。  
  
### <a name="extension-objects"></a>拡張オブジェクト  
 拡張オブジェクトは、<xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> メソッドを使用して追加します。 拡張オブジェクトをサポートするには、FullTrust アクセス許可セットが必要です。 このアクセス許可セットを設定すれば、拡張オブジェクト コードの実行時にアクセス許可の昇格が起こりません。 FullTrust アクセス許可なしで <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> メソッドを呼び出そうとすると、セキュリティ例外がスローされます。  
  
### <a name="style-sheet-scripts"></a>スタイル シート スクリプト  
 スクリプトは、`msxsl:script` 拡張要素を使用してスタイル シートに埋め込むことができます。 スクリプトのサポートは、<xref:System.Xml.Xsl.XslCompiledTransform> クラス (既定で無効) のオプションの機能です。 スクリプト機能を有効にするには、<xref:System.Xml.Xsl.XsltSettings.EnableScript%2A?displayProperty=nameWithType> プロパティを `true` に設定して、<xref:System.Xml.Xsl.XsltSettings> オブジェクトを <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> メソッドに渡します。  
  
#### <a name="guidelines"></a>ガイドライン  
 スクリプト機能を有効にするのは、スタイル シートの作成元が信頼できる場合のみにしてください。 スタイル シートの作成元が確認できない場合、またはスタイル シートの作成元が信頼できない場合には、XSLT 設定の引数に `null` を渡してください。  
  
## <a name="external-resources"></a>外部資料  
 XSLT 言語には、`xsl:import`、`xsl:include`、または `document()` 関数などの機能があります。これらの機能では、プロセッサが URI リファレンスを解決する必要があります。 外部リソースを解決するには、<xref:System.Xml.XmlResolver> クラスを使用します。 外部リソースの解決が必要になる可能性があるのは、次の 2 つの場合です。  
  
- スタイル シートをコンパイルする場合には、<xref:System.Xml.XmlResolver> および `xsl:import` を解決するために `xsl:include` を使用します。  
  
- 変換を実行する場合には、<xref:System.Xml.XmlResolver> 関数を解決するために `document()` を使用します。  
  
    > [!NOTE]
    > `document()` クラスでは、<xref:System.Xml.Xsl.XslCompiledTransform> 関数は既定で無効になっています。 この機能を有効にするには、<xref:System.Xml.Xsl.XsltSettings.EnableDocumentFunction%2A?displayProperty=nameWithType> プロパティを `true` に設定して、<xref:System.Xml.Xsl.XsltSettings> オブジェクトを <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> メソッドに渡します。  
  
 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> および <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> メソッドには、<xref:System.Xml.XmlResolver> を引数の 1 つとして許容するオーバーロードがそれぞれ含まれます。 <xref:System.Xml.XmlResolver> を指定しない場合は、資格情報を持たない既定の <xref:System.Xml.XmlUrlResolver> が使用されます。  
  
#### <a name="guidelines"></a>ガイドライン  
 `document()` 関数を有効にするのは、スタイル シートの作成元が信頼できる場合のみにしてください。  
  
 <xref:System.Xml.XmlResolver> オブジェクトを使用する場合の説明を次の一覧に示します。  
  
- XSLT 処理で認証が必要なネットワーク リソースにアクセスする必要がある場合、必要な資格情報に対して <xref:System.Xml.XmlResolver> を使用します。  
  
- XSLT 処理がアクセスできるリソースを制限する場合、適切なアクセス許可セットに対して <xref:System.Xml.XmlSecureResolver> を使用します。 制御対象外の (信頼できない) リソースを開く場合には、<xref:System.Xml.XmlSecureResolver> クラスを使用します。  
  
- 動作をカスタマイズする場合は、独自の <xref:System.Xml.XmlResolver> クラスを実装し、これを使用してリソースを解決することができます。  
  
- 外部リソースにアクセスできないようにする場合は、<xref:System.Xml.XmlResolver> の引数に `null` を指定します。  
  
## <a name="see-also"></a>関連項目

- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
- [XSLT 処理中の外部リソースの解決](../../../../docs/standard/data/xml/resolving-external-resources-during-xslt-processing.md)
- [コード アクセス セキュリティ](../../../../docs/framework/misc/code-access-security.md)
