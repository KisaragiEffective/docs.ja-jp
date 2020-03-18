---
title: 弱く型指定された拡張のサンプル
ms.date: 03/30/2017
ms.assetid: 56ce265b-8163-4b85-98e7-7692a12c4357
ms.openlocfilehash: f3beed9b9ca1dd6b1d4bb32078e6cd35a636501c
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714881"
---
# <a name="loosely-typed-extensions-sample"></a>弱く型指定された拡張のサンプル
配信オブジェクト モデルでは、拡張データをさまざまな方法で処理できます。拡張データとは、配信フィードの XML 表現に含まれているが、<xref:System.ServiceModel.Syndication.SyndicationFeed> や <xref:System.ServiceModel.Syndication.SyndicationItem> などのクラスによって明示的に公開されない情報のことです。 このサンプルでは、拡張データを処理する基本的な方法を示します。  
  
 このサンプルでは、例を示す目的で <xref:System.ServiceModel.Syndication.SyndicationFeed> クラスを使用します。 ただし、このサンプルで示すパターンは、拡張データをサポートするすべての配信クラスで使用できます。  
  
 <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
 <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
 <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
 <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
## <a name="sample-xml"></a>サンプル XML  
 このサンプルで使用される XML ドキュメントは次のとおりです。  
  
```xml  
<?xml version="1.0" encoding="IBM437"?>  
<feed myAttribute="someValue" xmlns="http://www.w3.org/2005/Atom">  
  <title type="text"></title>  
  <id>uuid:8f60c7b3-a3c0-4de7-a642-2165d77ce3c1;id=1</id>  
  <updated>2007-09-07T22:15:34Z</updated>  
  <simpleString xmlns="">hello, world!</simpleString>  
  <simpleString xmlns="">another simple string</simpleString>  
  <DataContractExtension xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.d  
atacontract.org/2004/07/Microsoft.Syndication.Samples">  
    <Key>X</Key>  
    <Value>4</Value>  
  </DataContractExtension>  
  <XmlSerializerExtension xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://ww  
w.w3.org/2001/XMLSchema" xmlns="">  
    <Key>Y</Key>  
    <Value>8</Value>  
  </XmlSerializerExtension>  
  <xElementExtension xmlns="">  
    <Key attr1="someValue">Z</Key>  
    <Value attr1="someValue">15</Value>  
  </xElementExtension>  
</feed>  
```  
  
 このドキュメントには、次の拡張データが含まれています。  
  
- `myAttribute` 要素の `<feed>` 属性。  
  
- `<simpleString>` 要素。  
  
- `<DataContractExtension>` 要素。  
  
- `<XmlSerializerExtension>` 要素。  
  
- `<xElementExtension>` 要素。  
  
## <a name="writing-extension-data"></a>拡張データの書き込み  
 属性の拡張は、次のサンプル コードに示すように、エントリを <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> コレクションに追加することによって作成されます。  
  
```csharp  
//Attribute extensions are stored in a dictionary indexed by   
// XmlQualifiedName  
feed.AttributeExtensions.Add(new XmlQualifiedName("myAttribute", ""), "someValue");  
```  
  
 要素拡張は、エントリを <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> コレクションに追加することによって作成されます。 これらの拡張には、文字列などの基本的な値、.NET Framework オブジェクトの XML シリアル化、および手動で入力された XML ノードを指定できます。  
  
 次のサンプル コードでは、`simpleString` という拡張要素が作成されます。  
  
```csharp  
feed.ElementExtensions.Add("simpleString", "", "hello, world!");  
```  
  
 この要素の XML 名前空間は空の名前空間 ("") で、その値は文字列 "hello, world!" を含むテキストノードです。  
  
 入れ子になった多数の要素で構成される複雑な要素拡張を作成する方法の 1 つに、次の例に示すように、.NET Framework API をシリアル化に使用する方法があります (<xref:System.Runtime.Serialization.DataContractSerializer> と <xref:System.Xml.Serialization.XmlSerializer> の両方がサポートされています)。  
  
```csharp  
feed.ElementExtensions.Add( new DataContractExtension() { Key = "X", Value = 4 } );  
feed.ElementExtensions.Add( new XmlSerializerExtension { Key = "Y", Value = 8 }, new XmlSerializer( typeof( XmlSerializerExtension ) ) );  
```  
  
 この例の `DataContractExtension` と `XmlSerializerExtension` は、シリアライザで使用するために記述されたカスタム型です。  
  
 <xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection> クラスを使用すると、<xref:System.Xml.XmlReader> インスタンスから要素拡張を作成することもできます。 これによって、次のサンプル コードに示すように、<xref:System.Xml.Linq.XElement> などの XML 処理 API と簡単に統合できるようになります。  
  
```csharp  
feed.ElementExtensions.Add(new XElement("xElementExtension",  
        new XElement("Key", new XAttribute("attr1", "someValue"), "Z"),  
        new XElement("Value", new XAttribute("attr1", "someValue"),   
        "15")).CreateReader());  
```  
  
## <a name="reading-extension-data"></a>拡張データの読み取り  
 属性の拡張の値は、次のサンプル コードに示すように、<xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> を使用して <xref:System.Xml.XmlQualifiedName> コレクションの属性を検索することによって取得できます。  
  
```csharp  
Console.WriteLine( feed.AttributeExtensions[ new XmlQualifiedName( "myAttribute", "" )]);  
```  
  
 要素拡張には、`ReadElementExtensions<T>` メソッドを使用してアクセスします。  
  
```csharp  
foreach( string s in feed2.ElementExtensions.ReadElementExtensions<string>("simpleString", ""))  
{  
    Console.WriteLine(s);  
}  
  
foreach (DataContractExtension dce in feed2.ElementExtensions.ReadElementExtensions<DataContractExtension>("DataContractExtension",  
"http://schemas.datacontract.org/2004/07/SyndicationExtensions"))  
{  
    Console.WriteLine(dce.ToString());  
}  
  
foreach (XmlSerializerExtension xse in feed2.ElementExtensions.ReadElementExtensions<XmlSerializerExtension>("XmlSerializerExtension", "", new XmlSerializer(typeof(XmlSerializerExtension))))  
{  
    Console.WriteLine(xse.ToString());  
}  
```  
  
 `XmlReader` メソッドを使用して、個々の要素拡張で <xref:System.ServiceModel.Syndication.SyndicationElementExtension.GetReader> を取得することも可能です。  
  
```csharp  
foreach (SyndicationElementExtension extension in feed2.ElementExtensions.Where<SyndicationElementExtension>(x => x.OuterName == "xElementExtension"))  
{  
    XNode xelement = XElement.ReadFrom(extension.GetReader());  
    Console.WriteLine(xelement.ToString());  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\LooselyTypedExtensions`  
  
## <a name="see-also"></a>参照

- [厳密に型指定された拡張](../../../../docs/framework/wcf/samples/strongly-typed-extensions-sample.md)
- [WCF 配信](../../../../docs/framework/wcf/feature-details/wcf-syndication.md)
