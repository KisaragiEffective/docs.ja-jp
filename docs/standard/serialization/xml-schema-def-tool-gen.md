---
title: '方法 : XML スキーマ定義ツールを使用してクラスと XML スキーマ ドキュメントを生成する'
ms.date: 03/30/2017
helpviewer_keywords:
- generating XML classes using XML Schema Definition tool
- generating XML Schema Document using XML Schema Definition tool
- XML Schema Definition tool, using to generate classes that conform to specific schema
- XML Schema Definition tool, using to generate XML Schema Document
ms.assetid: 51f0edc3-993d-4051-b7f2-77753694d3d1
ms.openlocfilehash: 9b2cd67a1c4f30e6fe246124be5b8f7081c539a6
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392863"
---
# <a name="how-to-use-the-xml-schema-definition-tool-to-generate-classes-and-xml-schema-documents"></a>方法 : XML スキーマ定義ツールを使用してクラスと XML スキーマ ドキュメントを生成する
XML スキーマ定義ツール (Xsd.exe) を使用して、クラスを説明する XML スキーマを生成したり、XML スキーマで定義されるクラスを生成したりできます。 次の手順では、これらの操作の実行方法を示します。  
  
### <a name="to-generate-classes-that-conform-to-a-specific-schema"></a>特定のスキーマに準拠するクラスを生成するには  
  
1. コマンド プロンプトを開きます。  
  
2. XML スキーマ定義ツールに XML スキーマを引数として渡します。XML スキーマ定義ツールは、次のように XML スキーマに正確に一致するクラスのセットを作成します。  
  
    ```console  
    xsd mySchema.xsd  
    ```  
  
     ツールは、2001 年 3 月 16 日付けの World Wide Web Consortium XML 仕様を参照するスキーマのみを処理できます。 言い換えると、次の例に示すように、XML スキーマ名前空間は "http://www.w3.org/2001/XMLSchema" である必要があります。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema attributeFormDefault="qualified" elementFormDefault="qualified" targetNamespace="" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    ```  
  
3. 必要に応じて、クラスのメソッド、プロパティ、またはフィールドを変更します。 属性によるクラスの変更の詳細については、「[属性を使用した XML シリアル化の制御](../../../docs/standard/serialization/controlling-xml-serialization-using-attributes.md)」と「[エンコード済み SOAP シリアル化を制御する属性](../../../docs/standard/serialization/attributes-that-control-encoded-soap-serialization.md)」を参照してください。  
  
 クラスのインスタンスがシリアル化されるときに生成される XML ストリームのスキーマを調べることは、さまざまな場合に役立ちます。 たとえば、他のユーザーが使用できるようにスキーマを公開したり、準拠しようとしているスキーマと自分のスキーマを比較したりできます。  
  
#### <a name="to-generate-an-xml-schema-document-from-a-set-of-classes"></a>クラスのセットから XML スキーマ ドキュメントを生成するには  
  
1. クラスを DLL にコンパイルします。  
  
2. コマンド プロンプトを開きます。  
  
3. 次の例に示すように、DLL を引数として Xsd.exe に渡します。  
  
    ```console  
    xsd MyFile.dll  
    ```  
  
     スキーマが、"schema0.xsd" という名前から順に書き込まれます。  
  
## <a name="see-also"></a>参照

- <xref:System.Data.DataSet>
- [XML スキーマ定義ツールと XML シリアル化](../../../docs/standard/serialization/the-xml-schema-definition-tool-and-xml-serialization.md)
- [XML シリアル化の概要](../../../docs/standard/serialization/introducing-xml-serialization.md)
- [XML スキーマ定義ツール (Xsd.exe)](../../../docs/standard/serialization/xml-schema-definition-tool-xsd-exe.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [方法 : オブジェクトをシリアル化する](../../../docs/standard/serialization/how-to-serialize-an-object.md)
- [方法 : オブジェクトを逆シリアル化する](../../../docs/standard/serialization/how-to-deserialize-an-object.md)
