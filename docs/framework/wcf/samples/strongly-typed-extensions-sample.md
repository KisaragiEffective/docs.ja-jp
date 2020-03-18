---
title: 厳密に型指定された拡張のサンプル
ms.date: 03/30/2017
ms.assetid: 02220f11-1a83-441c-9e5a-85f9a9367572
ms.openlocfilehash: 4ad0a8e10ecbcb5e3ddf9106dbbaa55356314020
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716619"
---
# <a name="strongly-typed-extensions-sample"></a>厳密に型指定された拡張のサンプル
このサンプルでは、例を示す目的で <xref:System.ServiceModel.Syndication.SyndicationFeed> クラスを使用します。 ただし、このサンプルで示すパターンは、拡張データをサポートするすべての配信クラスで使用できます。  
  
 配信オブジェクト モデル (<xref:System.ServiceModel.Syndication.SyndicationFeed>、<xref:System.ServiceModel.Syndication.SyndicationItem>、および関連クラス) は、<xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> プロパティおよび <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> プロパティを使用することにより、拡張データへの弱い型定義によるアクセスをサポートします。 このサンプルでは、特定のアプリケーション固有の拡張を厳密に型指定されたプロパティとして使用できるようにする <xref:System.ServiceModel.Syndication.SyndicationFeed> および <xref:System.ServiceModel.Syndication.SyndicationItem> のカスタム派生クラスを実装することにより、拡張データへの厳密に型指定されたアクセスを提供する方法を示します。  
  
 例として、このサンプルでは Atom Threading Extensions の RFC 案で定義された拡張要素を実装する方法を示しています。 このサンプルは、使い方を示すために用意されたものであり、仕様案を完全に実装するためのものではありません。  
  
## <a name="sample-xml"></a>サンプル XML  
 次の XML の例は、`<in-reply-to>` 拡張要素が追加された Atom 1.0 エントリを示しています。  
  
```xml  
<entry>  
    <id>tag:example.org,2005:1,2</id>  
    <title type="text">Another response to the original</title>  
    <summary type="text">  
         This is a response to the original entry</summary>  
    <updated>2006-03-01T12:12:13Z</updated>  
    <link href="http://www.example.org/entries/1/2" />  
    <in-reply-to p3:ref="tag:example.org,2005:1"   
                 p3:href="http://www.example.org/entries/1"   
                 p3:type="application/xhtml+xml"   
                 xmlns:p3="http://contoso.org/syndication/thread/1.0"   
                 xmlns="http://contoso.org/syndication/thread/1.0">  
      <anotherElement xmlns="http://www.w3.org/2005/Atom">  
                     Some more data</anotherElement>  
      <aDifferentElement xmlns="http://www.w3.org/2005/Atom">  
                     Even more data</aDifferentElement>  
    </in-reply-to>  
</entry>  
```  
  
 `<in-reply-to>` 要素は、3つの必須属性 (`ref`、`type`、および `href`) を指定しますが、追加の拡張属性と拡張要素も存在できます。  
  
## <a name="modeling-the-in-reply-to-element"></a>In-Reply-To 要素のモデル化  
 このサンプルでは、`<in-reply-to>` 要素は <xref:System.Xml.Serialization.IXmlSerializable> を実装する CLR としてモデル化されており、<xref:System.Runtime.Serialization.DataContractSerializer> と共に使用できます。 さらに、次のサンプル コードに示すように、要素のデータにアクセスするメソッドおよびプロパティも実装しています。  
  
```csharp  
[XmlRoot(ElementName = "in-reply-to", Namespace = "http://contoso.org/syndication/thread/1.0")]  
public class InReplyToElement : IXmlSerializable  
{  
    internal const string ElementName = "in-reply-to";  
    internal const string NsUri =   
                  "http://contoso.org/syndication/thread/1.0";  
    private Dictionary<XmlQualifiedName, string> extensionAttributes;  
    private Collection<XElement> extensionElements;  
  
    public InReplyToElement()  
    {  
        this.extensionElements = new Collection<XElement>();  
        this.extensionAttributes = new Dictionary<XmlQualifiedName,   
                                                          string>();  
    }  
  
    public Dictionary<XmlQualifiedName, string> AttributeExtensions  
    {  
        get { return this.extensionAttributes; }  
    }  
  
    public Collection<XElement> ElementExtensions  
    {  
        get { return this.extensionElements; }  
    }  
  
    public Uri Href  
    { get; set; }  
  
    public string MediaType  
    { get; set; }  
  
    public string Ref  
    { get; set; }  
  
    public Uri Source  
    { get; set; }  
}  
```  
  
 `InReplyToElement` クラスは、必須の属性 (`HRef`、`MediaType`、および `Source`) のプロパティと共に、<xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> および <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> を保持するコレクションを実装します。  
  
 `InReplyToElement` クラスは、<xref:System.Xml.Serialization.IXmlSerializable> インターフェイスを実装します。このインターフェイスは、オブジェクト インスタンスを XML から読み取る、または書き込む方法の直接制御を可能にします。 `ReadXml` メソッドは、渡された `Ref` から `HRef`、`Source`、`MediaType`、および <xref:System.Xml.XmlReader> の各プロパティを最初に読み取ります。 不明な属性は<xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> コレクションに格納されます。 すべての属性が読み取られると、<xref:System.Xml.XmlReader.ReadStartElement> が呼び出されて、リーダーが次の要素に進みます。 次のコードに示すように、このクラスによりモデル化された要素には必須の子がないので、子要素は `XElement` インスタンスにバッファされ、<xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> コレクションに格納されます。  
  
```csharp
public void ReadXml(System.Xml.XmlReader reader)  
{  
    bool isEmpty = reader.IsEmptyElement;  
  
    if (reader.HasAttributes)  
    {  
        for (int i = 0; i < reader.AttributeCount; i++)  
        {  
            reader.MoveToNextAttribute();  
  
            if (reader.NamespaceURI == "")  
            {  
                if (reader.LocalName == "ref")  
                {  
                    this.Ref = reader.Value;  
                }  
                else if (reader.LocalName == "href")  
                {  
                    this.Href = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "source")  
                {  
                    this.Source = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "type")  
                {  
                    this.MediaType = reader.Value;  
                }  
                else  
                {  
                    this.AttributeExtensions.Add(new   
                                 XmlQualifiedName(reader.LocalName,   
                                 reader.NamespaceURI),   
                                 reader.Value);  
                }  
            }  
        }  
    }  
  
    reader.ReadStartElement();  
  
    if (!isEmpty)  
    {  
        while (reader.IsStartElement())  
        {  
            ElementExtensions.Add(  
                  (XElement) XElement.ReadFrom(reader));  
        }  
        reader.ReadEndElement();  
    }  
}  
```  
  
 `WriteXml` では、`InReplyToElement` メソッドは最初に `Ref`、`HRef`、`Source`、および `MediaType` の各プロパティの値を XML 属性として書き込みます (`WriteXml` は、実際の外側の要素自体は書き込みません。`WriteXml` により行われるためです)。 また、次のコードに示すように、<xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> および <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> の内容をライタに書き込みます。  
  
```csharp
public void WriteXml(System.Xml.XmlWriter writer)  
{  
    if (this.Ref != null)  
    {  
        writer.WriteAttributeString("ref", InReplyToElement.NsUri,   
                                            this.Ref);  
    }  
    if (this.Href != null)  
    {  
        writer.WriteAttributeString("href", InReplyToElement.NsUri,   
                                                this.Href.ToString());  
    }  
    if (this.Source != null)  
    {  
        writer.WriteAttributeString("source", InReplyToElement.NsUri,   
                                              this.Source.ToString());  
    }  
    if (this.MediaType != null)  
    {  
        writer.WriteAttributeString("type", InReplyToElement.NsUri,   
                                                    this.MediaType);  
    }  
  
    foreach (KeyValuePair<XmlQualifiedName, string> kvp in   
                                             this.AttributeExtensions)  
    {  
        writer.WriteAttributeString(kvp.Key.Name, kvp.Key.Namespace,   
                                                   kvp.Value);  
    }  
  
    foreach (XElement element in this.ElementExtensions)  
    {  
        element.WriteTo(writer);  
    }  
}  
```  
  
## <a name="threadedfeed-and-threadeditem"></a>ThreadedFeed および ThreadedItem  
 このサンプルでは、`SyndicationItems` 拡張が存在する `InReplyTo` が `ThreadedItem` クラスによりモデル化されます。 同様に、`ThreadedFeed` クラスは、項目が `SyndicationFeed` のすべてのインスタンスである `ThreadedItem` です。  
  
 `ThreadedFeed` クラスは、`SyndicationFeed` から継承され、`OnCreateItem` をオーバーライドして `ThreadedItem` を返します。 また、次のコードに示すように、`Items` コレクションにアクセスするメソッドを `ThreadedItems` として実装します。  
  
```csharp
public class ThreadedFeed : SyndicationFeed  
{  
    public ThreadedFeed()  
    {  
    }  
  
    public IEnumerable<ThreadedItem> ThreadedItems  
    {  
        get  
        {  
            return this.Items.Cast<ThreadedItem>();  
        }  
    }  
  
    protected override SyndicationItem CreateItem()  
    {  
        return new ThreadedItem();  
    }  
}  
```  
  
 `ThreadedItem` クラスは `SyndicationItem` から継承され、`InReplyToElement` を厳密に型指定されたプロパティとします。 これにより、`InReplyTo` 拡張データにプログラムによって簡単にアクセスできます。 また、次のコードに示すように、その拡張データの読み取り/書き込みを行う `TryParseElement` および `WriteElementExtensions` も実装します。  
  
```csharp
public class ThreadedItem : SyndicationItem  
{  
    private InReplyToElement inReplyTo;  
    // Constructors  
        public ThreadedItem()  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
        public ThreadedItem(string title, string content, Uri itemAlternateLink, string id, DateTimeOffset lastUpdatedTime) : base(title, content, itemAlternateLink, id, lastUpdatedTime)  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
    public InReplyToElement InReplyTo  
    {  
        get { return this.inReplyTo; }  
    }  
  
    protected override bool TryParseElement(  
                        System.Xml.XmlReader reader,   
                        string version)  
    {  
        if (version == SyndicationVersions.Atom10 &&  
            reader.NamespaceURI == InReplyToElement.NsUri &&  
            reader.LocalName == InReplyToElement.ElementName)  
        {  
            this.inReplyTo = new InReplyToElement();  
  
            this.InReplyTo.ReadXml(reader);  
  
            return true;  
        }  
        else  
        {  
            return base.TryParseElement(reader, version);  
        }  
    }  
  
    protected override void WriteElementExtensions(XmlWriter writer,   
                                                 string version)  
    {  
        if (this.InReplyTo != null &&   
                     version == SyndicationVersions.Atom10)  
        {  
            writer.WriteStartElement(InReplyToElement.ElementName,   
                                           InReplyToElement.NsUri);  
            this.InReplyTo.WriteXml(writer);  
            writer.WriteEndElement();  
        }  
  
        base.WriteElementExtensions(writer, version);  
    }  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Syndication\StronglyTypedExtensions`  
