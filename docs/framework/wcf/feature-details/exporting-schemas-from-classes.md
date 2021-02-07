---
description: 詳細については、「クラスからのスキーマのエクスポート」を参照してください。
title: クラスからのスキーマのエクスポート
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, schema import and export
- schemas [WCF], exporting from classes
- schemas [WCF]
- XsdDataContractExporter class
- XsdDataContractImporter class
ms.assetid: bb57b962-70c1-45a9-93d5-e721e340a13f
ms.openlocfilehash: f9ca33bb26237b3af9ff26b1ed693088c62e3b13
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99756280"
---
# <a name="exporting-schemas-from-classes"></a>クラスからのスキーマのエクスポート

データ コントラクト モデルで使用されているクラスから XML スキーマ定義言語 (XSD) スキーマを生成するには、 <xref:System.Runtime.Serialization.XsdDataContractExporter> クラスを使います。 このトピックでは、スキーマの作成手順を説明します。  
  
## <a name="the-export-process"></a>エクスポート処理  

 スキーマのエクスポート処理では、まず型の定義をエクスポートするので、型定義を XML 形式で記述した <xref:System.Xml.Schema.XmlSchemaSet> を生成します。  
  
 は、 `XmlSchemaSet` XSD スキーマドキュメントのセットを表す .NET Framework のスキーマオブジェクトモデル (SOM) の一部です。 XSD ドキュメントを `XmlSchemaSet`から生成するために、 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> クラスの `XmlSchemaSet` プロパティから取得した、スキーマのコレクションを使います。 次に各 <xref:System.Xml.Schema.XmlSchema> オブジェクトを、 <xref:System.Xml.Serialization.XmlSerializer>を使ってシリアル化します。  
  
#### <a name="to-export-schemas"></a>スキーマをエクスポートするには  
  
1. <xref:System.Runtime.Serialization.XsdDataContractExporter> のインスタンスを作成します。  
  
2. 任意。 <xref:System.Xml.Schema.XmlSchemaSet> をコンストラクターに渡します。 この場合、空の <xref:System.Xml.Schema.XmlSchemaSet> から開始するのではなく、スキーマのエクスポート時に生成されたスキーマが、この <xref:System.Xml.Schema.XmlSchemaSet>インスタンスに追加されます。  
  
3. 任意。 <xref:System.Runtime.Serialization.XsdDataContractExporter.CanExport%2A> メソッドのいずれかを呼び出します。 指定された型がエクスポート可能かどうかを判定するメソッドです。 このメソッドには、次に説明する `Export` メソッドと同じオーバーロードがあります。  
  
4. <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> メソッドのいずれかを呼び出します。 <xref:System.Type>、 <xref:System.Collections.Generic.List%601> オブジェクトの `Type` 、または <xref:System.Collections.Generic.List%601> オブジェクトの <xref:System.Reflection.Assembly> を受け取る 3 種類のオーバーロードがあります。 3 つ目のメソッドの場合、指定されたアセンブリのすべての型がエクスポートされます。  
  
     `Export` メソッドを何度も呼び出すと、同じ `XmlSchemaSet`に複数の項目が追加されます。 `XmlSchemaSet` に既に存在する型は生成されません。 したがって、 `Export` クラスのインスタンスを複数作成するのではなく、同じ `XsdDataContractExporter` に対して `XsdDataContractExporter` を複数回呼び出すようにします。 これにより、同じスキーマ型が重複して生成される状況を回避できます。  
  
    > [!NOTE]
    > エクスポート処理中に障害が発生した場合、 `XmlSchemaSet` は予測できない状態になります。  
  
5. <xref:System.Xml.Schema.XmlSchemaSet> プロパティを介して、 <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> にアクセスします。  
  
## <a name="export-options"></a>エクスポート オプション  

 <xref:System.Runtime.Serialization.XsdDataContractExporter.Options%2A> の <xref:System.Runtime.Serialization.XsdDataContractExporter> プロパティとして <xref:System.Runtime.Serialization.ExportOptions> クラスのインスタンスを設定することにより、エクスポート処理の方法を制御できます。 具体的には、次のオプションを設定できます。  
  
- <xref:System.Runtime.Serialization.ExportOptions.KnownTypes%2A>. `Type` のコレクションであり、エクスポートされる型に対応する既知の型を表します (詳細については、「 [データコントラクトの既知の型](data-contract-known-types.md)」を参照してください)。これらの既知の型は `Export` 、メソッドに渡される型に加えて、すべての呼び出しでエクスポートされ `Export` ます。  
  
- <xref:System.Runtime.Serialization.ExportOptions.DataContractSurrogate%2A>. このプロパティを介して <xref:System.Runtime.Serialization.IDataContractSurrogate> を渡すことにより、エクスポート処理をカスタマイズできます。 詳細については、「 [データコントラクトサロゲート](../extending/data-contract-surrogates.md)」を参照してください。 既定では、サロゲートは使用されません。  
  
## <a name="helper-methods"></a>ヘルパー メソッド  

 `XsdDataContractExporter` には、主要な機能であるスキーマのエクスポート処理に加え、型に関する情報を調べるためのヘルパー メソッドが定義されています。 次に例を示します。  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetRootElementName%2A> メソッド `Type` を受け取り、この型をルート オブジェクトとしてシリアル化した場合に使用されるルート要素名と名前空間を表す <xref:System.Xml.XmlQualifiedName> を返します。  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaTypeName%2A> メソッド `Type` を引数とし、この型をスキーマにエクスポートした場合に使用される XSD スキーマ型の名前を表す <xref:System.Xml.XmlQualifiedName> を返します。 スキーマ内で匿名型として表される <xref:System.Xml.Serialization.IXmlSerializable> 型を引数として渡すと、 `null`を返します。  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaType%2A> メソッド スキーマ内で匿名型として表される <xref:System.Xml.Serialization.IXmlSerializable> 型を渡した場合にのみ有効なメソッドで、それ以外の型の場合は `null` を返します。 匿名型に対しては、特定の <xref:System.Xml.Schema.XmlSchemaType> を表す `Type`を返します。  
  
 以上のメソッドの動作は、エクスポート オプションの影響を受けます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- [スキーマのインポートとエクスポート](schema-import-and-export.md)
- [クラスを作成するためのスキーマのインポート](importing-schema-to-generate-classes.md)
