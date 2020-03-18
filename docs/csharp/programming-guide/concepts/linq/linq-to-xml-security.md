---
title: LINQ to XML のセキュリティ (C#)
ms.date: 07/20/2015
ms.assetid: ef2c0dc9-ecf9-4c17-b24e-144184ab725f
ms.openlocfilehash: 5b7eb815b058cba008f1db2cf683c8934c19b743
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73423371"
---
# <a name="linq-to-xml-security-c"></a>LINQ to XML のセキュリティ (C#)
ここでは、LINQ to XML に関連するセキュリティの問題について説明し、 セキュリティ上の脆弱性を緩和するためのガイドラインを紹介します。  
  
## <a name="linq-to-xml-security-overview"></a>LINQ to XML のセキュリティの概要  
 LINQ to XML は、厳密なセキュリティ要件が必要なサーバー側アプリケーション向けというよりも、プログラミングの利便性に重点を置いて設計されています。 ほとんどの XML のシナリオでは、サーバーにアップロードされる信頼できない XML ドキュメントではなく、信頼できる XML ドキュメントが処理されます。 LINQ to XML は、こうした一般的なシナリオに最適化されています。  
  
 ソースが不明な信頼できないデータを処理する必要がある場合は、既知の XML サービス拒否 (DoS) 攻撃をフィルターで除外するように構成された <xref:System.Xml.XmlReader> クラスのインスタンスを使用することをお勧めします。  
  
 サービス拒否攻撃を緩和するように <xref:System.Xml.XmlReader> を構成したら、それを使用して LINQ to XML ツリーを設定できます。これにより、LINQ to XML によるプログラミングの生産性向上のメリットも受けられます。 このように、セキュリティの問題を緩和するように構成されたリーダーを作成し、その構成済みのリーダーを使用して XML ツリーをインスタンス化する方法は、多くの軽減技法で利用されています。  
  
 XML は、ドキュメントのサイズ、深さ、要素名のサイズなどの制限がないため、サービス拒否攻撃に対して本質的に脆弱です。 XML の処理に使用するコンポーネントに関係なく、リソースが過剰に使用されている場合は、常にアプリケーション ドメインを再利用できるようにしておく必要があります。  
  
## <a name="mitigation-of-xml-xsd-xpath-and-xslt-attacks"></a>XML、XSD、XPath、および XSLT の攻撃の緩和  
 LINQ to XML は、<xref:System.Xml.XmlReader> と <xref:System.Xml.XmlWriter> に基づいて構築されており、 <xref:System.Xml.Schema?displayProperty=nameWithType> 名前空間と <xref:System.Xml.XPath?displayProperty=nameWithType> 名前空間の拡張メソッドによって XSD と XPath をサポートしています。 <xref:System.Xml.XmlReader>、<xref:System.Xml.XPath.XPathNavigator>、および <xref:System.Xml.XmlWriter> の各クラスを LINQ to XML と組み合わせて使用することにより、XSLT を呼び出して XML ツリーを変換することができます。  
  
 運用環境の安全性が高くない場合は、XML や <xref:System.Xml?displayProperty=nameWithType>、<xref:System.Xml.Schema?displayProperty=nameWithType>、<xref:System.Xml.XPath?displayProperty=nameWithType>、および <xref:System.Xml.Xsl?displayProperty=nameWithType> の各クラスの使用に関連するセキュリティの問題が発生する可能性が高いといえます。 そのような問題の一部を次に示します。  
  
- XSD、XPath、および XSLT は文字列に基づく言語であり、時間やメモリを大量に消費する操作を指定することができます。 信頼されていないソースの XSD、XPath、または XSLT の文字列を受け取る場合、アプリケーション プログラマは、その文字列が悪意のあるものでないかどうかを検証する必要があります。また、その文字列の評価によってシステム リソースが過剰に消費されないように監視し、過剰に消費された場合はその状況を緩和する必要があります。  
  
- XSD スキーマ (インライン スキーマを含む) は、サービス拒否攻撃に対して本質的に脆弱です。信頼できないソースのスキーマは受け入れないようにしてください。  
  
- XSD や XSLT には他のファイルへの参照が含まれている場合があり、それらの参照が原因で、ゾーンやドメインを越えた攻撃を受ける可能性があります。  
  
- DTD 内の外部エンティティが原因で、ゾーンやドメインを越えた攻撃を受ける可能性があります。  
  
- DTD はサービス拒否攻撃に対して脆弱です。  
  
- 極端に階層の深い XML ドキュメントが原因で、サービス拒否攻撃を受ける可能性があります。XML ドキュメントの階層を制限することをお勧めします。  
  
- 信頼できないアセンブリからの <xref:System.Xml.NameTable>、<xref:System.Xml.XmlNamespaceManager>、および <xref:System.Xml.XmlResolver> オブジェクトなどのサポート コンポーネントは受け入れないようにします。  
  
- サイズの大きなドキュメントによる攻撃を緩和するためにデータをチャンク単位で読み取ります。  
  
- XSLT スタイル シート内のスクリプト ブロックが原因で、多くの攻撃を受ける場合があります。  
  
- 動的な XPath 式を作成する場合は事前に慎重に検証します。  
  
## <a name="linq-to-xml-security-issues"></a>LINQ to XML のセキュリティの問題  
 ここに示されているセキュリティの問題には、特に優先順位はありません。 すべての問題が重要であり、それぞれに適切に対処する必要があります。  
  
 権限の昇格攻撃が成功すると、悪意のあるアセンブリがその環境をより自由に制御できるようになります。 その結果、データの公開攻撃やサービス拒否攻撃などを受ける可能性があります。  
  
 アプリケーションでは、権限のないユーザーにデータを公開しないようにする必要があります。  
  
 サービス拒否攻撃を受けると、XML パーサーや LINQ to XML がメモリや CPU 時間を大量に消費するようになります。 サービス拒否攻撃は、権限の昇格攻撃やデータの公開攻撃ほど重大に捉えられていませんが、 信頼できないソースからの XML ドキュメントをサーバーで処理する必要があるシナリオでは重大な問題です。  
  
### <a name="exceptions-and-error-messages-might-reveal-data"></a>例外やエラー メッセージによってデータが漏洩する可能性がある  
 変換されるデータ、ファイル名、実装の詳細などのデータが、エラーの説明から漏洩する可能性があります。 信頼されていない呼び出し元に対してはエラー メッセージが表示されないようにする必要があります。 すべてのエラーをキャッチして、カスタム エラー メッセージでエラーを報告するようにしてください。  
  
### <a name="do-not-call-codeaccesspermissionsassert-in-an-event-handler"></a>イベント ハンドラーで CodeAccessPermissions.Assert を呼び出さない  
 アセンブリの権限は、低い場合もあれば高い場合もあります。 高い権限を持つアセンブリは、コンピューターやその環境をより自由に制御できます。  
  
 高い権限を持つアセンブリのコードがイベント ハンドラーで <xref:System.Security.CodeAccessPermission.Assert%2A?displayProperty=nameWithType> を呼び出した後に、権限が制限されている悪意のあるアセンブリに XML ツリーが渡されると、悪意のあるアセンブリによってイベントが発生させられる可能性があります。 このイベントは、高い権限を持つアセンブリ内のコードを実行するため、悪意のあるアセンブリが高度な特権で動作するようになります。  
  
 イベント ハンドラーでは <xref:System.Security.CodeAccessPermission.Assert%2A?displayProperty=nameWithType> を呼び出さないようにすることをお勧めします。  
  
### <a name="dtds-are-not-secure"></a>DTD は安全ではない  
 DTD 内のエンティティは、本質的に安全ではありません。 DTD を含む悪意のある XML ドキュメントが原因で、サービス拒否攻撃を受け、メモリと CPU 時間のすべてがパーサーによって使用されるといった状況が発生する可能性があります。 このため、LINQ to XML では DTD の処理が既定で無効になっています。 信頼されていないソースの DTD は受け入れないようにしてください。  
  
 たとえば、DTD を参照する XML ファイルと DTD ファイルをアップロードすることを Web ユーザーに対して許可する Web アプリケーションは、信頼されていないソースの DTD を受け入れる例の 1 つです。 この場合は、ファイルの検証の際に、悪意のある DTD によってサーバーに対するサービス拒否攻撃が行われる可能性があります。 そのほか、匿名 FTP アクセスが許可されているネットワーク共有の DTD を参照する場合も、信頼されていないソースの DTD を受け入れる例になります。  
  
### <a name="avoid-excessive-buffer-allocation"></a>過剰なバッファーの割り当てを回避する  
 アプリケーション開発者は、極端に大きなデータ ソースが原因でリソース消費攻撃やサービス拒否攻撃を受ける可能性があることを認識しておく必要があります。  
  
 悪意のあるユーザーによって非常に大きな XML ドキュメントが送信されたり、アップロードされたりすると、LINQ to XML がシステム リソースを過剰に消費するようになります。 このような状況が、サービス拒否攻撃の原因となる場合があります。 これを防ぐには、<xref:System.Xml.XmlReaderSettings.MaxCharactersInDocument%2A?displayProperty=nameWithType> プロパティを設定して、読み込むことのできるドキュメントのサイズが制限されたリーダーを作成します。 その後、そのリーダーを使用して XML ツリーを作成します。  
  
 たとえば、信頼されていないソースからの XML ドキュメントの最大サイズが 50K バイト未満と予測される場合は、<xref:System.Xml.XmlReaderSettings.MaxCharactersInDocument%2A?displayProperty=nameWithType> を 100,000 に設定します。 これにより、XML ドキュメントの処理を妨げずに、大量のメモリを消費するドキュメントがアップロードされるサービス拒否攻撃の脅威を緩和できます。  
  
### <a name="avoid-excess-entity-expansion"></a>過剰なエンティティの展開を回避する  
 DTD の使用時に発生するサービス拒否攻撃の 1 つに、エンティティを過剰に展開するドキュメントによる攻撃があります。 これを防ぐには、<xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities%2A?displayProperty=nameWithType> プロパティを設定して、エンティティの展開により生成される文字数が制限されたリーダーを作成します。 その後、そのリーダーを使用して XML ツリーを作成します。  
  
### <a name="limit-the-depth-of-the-xml-hierarchy"></a>XML 階層の深さを制限する  
 サービス拒否攻撃は、極端に深い階層を持つドキュメントが送信された場合にも発生します。 これを防ぐには、要素の深さをカウントする独自のクラスで <xref:System.Xml.XmlReader> をラップします。 これにより、要素の深さが事前に設定したレベルを超えている場合に、その悪意のあるドキュメントの処理を終了できます。  
  
### <a name="protect-against-untrusted-xmlreader-or-xmlwriter-implementations"></a>信頼されていない XmlReader や XmlWriter の実装から保護する  
 管理者は、外部から提供された <xref:System.Xml.XmlReader> や <xref:System.Xml.XmlWriter> の実装について、厳密な名前が使用されているかどうか、コンピューターの構成に登録されているかどうかを確認する必要があります。 これにより、リーダーやライターを装った悪意のあるコードが読み込まれるのを防ぐことができます。  
  
### <a name="periodically-free-objects-that-reference-xname"></a>XName を参照するオブジェクトを定期的に解放する  
 アプリケーション プログラマは、ある種の攻撃に対する保護のために、アプリケーション ドメインで <xref:System.Xml.Linq.XName> オブジェクトを参照しているすべてのオブジェクトを定期的に解放する必要があります。  
  
### <a name="protect-against-random-xml-names"></a>ランダムな XML 名から保護する  
 信頼されていないソースのデータを受け取るアプリケーションを作成する場合、<xref:System.Xml.XmlReader> をカスタム コードでラップして使用し、ランダムな XML 名や XML 名前空間の使用を検査するように考慮する必要があります。 これにより、ランダムな XML 名や XML 名前空間が検出された場合に、アプリケーションでその悪意のあるドキュメントの処理を終了できます。  
  
 特定の名前空間の名前の数 (および名前空間に含まれない名前の数) を制限することをお勧めします。  
  
### <a name="annotations-are-accessible-by-software-components-that-share-a-linq-to-xml-tree"></a>LINQ to XML ツリーを共有するソフトウェア コンポーネントは注釈にアクセスできる  
 LINQ to XML を使用すると、さまざまなアプリケーション コンポーネントによって XML データの読み込み、検証、クエリ、変換、更新、および保存が行われる処理パイプラインを構築できます。XML データは、XML ツリーとしてコンポーネント間で受け渡されます。 この場合、オブジェクトを読み込んで XML テキストにシリアル化する際のオーバーヘッドがパイプラインの最後にしか発生しないため、パフォーマンスを最適化することができます。 ただし、開発者は、あるコンポーネントによって作成される注釈やイベント ハンドラーはすべて、他のコンポーネントからもアクセスできることを認識しておく必要があります。 このことは、異なる信頼レベルのコンポーネントが混在している場合に数々の脆弱性の原因になります。 信頼レベルの低いコンポーネントが含まれる場合に安全なパイプラインを構築するには、信頼されていないコンポーネントにデータを渡す前に LINQ to XML オブジェクトを XML テキストにシリアル化する必要があります。  
  
 ある程度のセキュリティが共通言語ランタイム (CLR) によって提供されます。 たとえば、プライベート クラスを含まないコンポーネントは、そのクラスによってキー指定された注釈にはアクセスできません。 ただし、注釈を読み取れないコンポーネントでも、注釈を削除することはできます。 このことが、改ざん攻撃に利用される可能性があります。  
  
## <a name="see-also"></a>参照

- [プログラミング ガイド (LINQ to XML) (C#)](linq-to-xml-overview.md)
