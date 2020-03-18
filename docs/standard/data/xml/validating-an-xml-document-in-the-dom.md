---
title: DOM における XML ドキュメントの検証
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2c61c920-d0f8-4c72-bfcc-6524570f3060
ms.openlocfilehash: 93686ddf1afff76926e77acdbf5aa58e93d6cb77
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710038"
---
# <a name="validating-an-xml-document-in-the-dom"></a>DOM における XML ドキュメントの検証

<xref:System.Xml.XmlDocument> クラスでは、既定で、ドキュメント オブジェクト モデル (DOM) 内の XML ドキュメントは、XML スキーマ定義言語 (XSD) スキーマまたはドキュメント型定義 (DTD) に対して検証されません。XML が整形式であることだけが検証されます。

DOM 内の XML を検証するには、スキーマ検証型 <xref:System.Xml.XmlReader> を <xref:System.Xml.XmlDocument.Load%2A> クラスの <xref:System.Xml.XmlDocument> メソッドに渡して、DOM への読み込み時に XML を検証するか、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して、まだ未検証の DOM 内の XML ドキュメントを検証することができます。

## <a name="validating-an-xml-document-as-it-is-loaded-into-the-dom"></a>DOM への読み込み時の XML ドキュメントの検証

検証型 <xref:System.Xml.XmlDocument> が <xref:System.Xml.XmlReader> クラスの <xref:System.Xml.XmlDocument.Load%2A> メソッドに渡されると、<xref:System.Xml.XmlDocument> クラスは DOM への読み込み時に XML データを検証します。

検証が正常に終了すると、スキーマの既定値が適用され、必要に応じてテキスト値がアトミック値に変換され、型情報が検証済みの情報項目に関連付けられます。 その結果、以前の型指定されていない XML データは、型指定された XML データに置き換わります。

### <a name="creating-an-xml-schema-validating-xmlreader"></a>XML スキーマ検証型 XmlReader の作成

XML スキーマ検証型 <xref:System.Xml.XmlReader> を作成するには、次の手順を実行します。

1. 新しい <xref:System.Xml.XmlReaderSettings> インスタンスを作成します。

2. XML スキーマを <xref:System.Xml.XmlReaderSettings.Schemas%2A> インスタンスの <xref:System.Xml.XmlReaderSettings> プロパティに追加します。

3. `Schema` を <xref:System.Xml.XmlReaderSettings.ValidationType%2A> として指定します。

4. オプションとして、検証中に検出したスキーマ検証エラーおよび警告を処理する <xref:System.Xml.XmlReaderSettings.ValidationFlags%2A> と <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> を指定します。

5. 最後に、<xref:System.Xml.XmlReaderSettings> オブジェクトを、スキーマ検証型 <xref:System.Xml.XmlReader.Create%2A> を作成する <xref:System.Xml.XmlReader> クラスの <xref:System.Xml.XmlReader> メソッドに、XML ドキュメントと共に渡します。

### <a name="example"></a>使用例

次のコード サンプルでは、スキーマ検証型 <xref:System.Xml.XmlReader> が DOM に読み込まれた XML データを検証します。 無効な変更を加えられた後、XML ドキュメントが再検証され、スキーマ検証エラーが発生します。 最後に、エラーの 1 つが修正された後、XML ドキュメントの一部が部分的に検証されます。

[!code-cpp[XmlDocumentValidation.Load#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlDocumentValidation.Load/CPP/XmlDocumentValidationExample.cpp#1)]
[!code-csharp[XmlDocumentValidation.Load#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Load/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Load#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Load/VB/XmlDocumentValidationExample.vb#1)]

この例は、`contosoBooks.xml` ファイルを入力として使用します。

[!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]

また、`contosoBooks.xsd` ファイルも入力として使用します。

[!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]

DOM への読み込み時に XML データを検証する場合は、以下を検討します。

- 上の例では、無効な型が見つかるたびに <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> が呼び出されます。 検証を行う <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> で <xref:System.Xml.XmlReader> を設定しなかった場合は、<xref:System.Xml.Schema.XmlSchemaValidationException> を呼び出したときに属性や要素の型が検証スキーマで指定されている型と一致しないと、<xref:System.Xml.XmlDocument.Load%2A> がスローされます。

- XML ドキュメントが既定値を定義した関連付けられたスキーマと共に <xref:System.Xml.XmlDocument> オブジェクトに読み込まれる場合、<xref:System.Xml.XmlDocument> は、これらの既定値があたかも XML ドキュメント内にあるかのように扱います。 これは、スキーマから既定値を得た要素に対して <xref:System.Xml.XmlReader.IsEmptyElement%2A> プロパティは常に `false` を返すことを意味します。 XML ドキュメント内で空要素として書かれている場合も同様です。

## <a name="validating-an-xml-document-in-the-dom"></a>DOM における XML ドキュメントの検証

<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドは、DOM に読み込まれた XML データを <xref:System.Xml.XmlDocument> オブジェクトの <xref:System.Xml.XmlDocument.Schemas%2A> プロパティに対して検証します。 検証が正常に終了すると、スキーマの既定値が適用され、必要に応じてテキスト値がアトミック値に変換され、型情報が検証済みの情報項目に関連付けられます。 その結果、以前の型指定されていない XML データは、型指定された XML データに置き換わります。

以下の例は、上記の「DOM への読み込み時の XML ドキュメントの検証」の例に似ています。 この例では、XML ドキュメントは DOM への読み込み時には検証されませんが、DOM への読み込み後に、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドを使用して検証されます。 <xref:System.Xml.XmlDocument.Validate%2A> メソッドは XML ドキュメントを <xref:System.Xml.XmlDocument.Schemas%2A> の <xref:System.Xml.XmlDocument> プロパティに含まれている XML スキーマに対して検証します。 その後、無効な変更を加えられた後、XML ドキュメントが再検証され、スキーマ検証エラーが発生します。 最後に、エラーの 1 つが修正された後、XML ドキュメントの一部が部分的に検証されます。

[!code-csharp[XmlDocumentValidation.Validate#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Validate/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Validate#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Validate/VB/XmlDocumentValidationExample.vb#1)]

この例は入力として、上記の「DOM への読み込み時の XML ドキュメントの検証」で参照されている `contosoBooks.xml` ファイルと `contosoBooks.xsd` ファイルを使用します。

## <a name="handling-validation-errors-and-warnings"></a>検証エラーおよび警告の処理

XML スキーマ検証エラーは、DOM に読み込まれる XML データの検証時に報告されます。 XML データ読み込み時の検証中またはまだ未検証の XML ドキュメントの検証時に検出されたスキーマ検証エラーのすべてについて通知されます。

検証エラーは <xref:System.Xml.Schema.ValidationEventHandler> によって処理されます。 <xref:System.Xml.Schema.ValidationEventHandler> が <xref:System.Xml.XmlReaderSettings> インスタンスに割り当てられているか、<xref:System.Xml.XmlDocument.Validate%2A> クラスの <xref:System.Xml.XmlDocument> メソッドに渡されている場合、<xref:System.Xml.Schema.ValidationEventHandler> がスキーマ検証エラーを処理します。それ以外の場合は、スキーマ検証エラーの検出時に <xref:System.Xml.Schema.XmlSchemaValidationException> が発生します。

> [!NOTE]
> <xref:System.Xml.Schema.ValidationEventHandler> がプロセスを停止する例外を生成しない限り、スキーマ検証エラーが発生しても XML データは DOM に読み込まれます。
>
> <xref:System.Xml.Schema.XmlSchemaValidationFlags.ReportValidationWarnings> フラグが <xref:System.Xml.XmlReaderSettings> オブジェクトに指定されていない場合、スキーマ検証警告は報告されません。

 <xref:System.Xml.Schema.ValidationEventHandler> を説明する例については、上記の「DOM への読み込み時の XML ドキュメントの検証」と「DOM における XML ドキュメントの検証」を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XmlReader>
- <xref:System.Xml.Schema.ValidationEventHandler>
- <xref:System.Xml.XmlReaderSettings>
- [DOM モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-dom-model.md)
- [XML スキーマの使用](../../../../docs/standard/data/xml/working-with-xml-schemas.md)
