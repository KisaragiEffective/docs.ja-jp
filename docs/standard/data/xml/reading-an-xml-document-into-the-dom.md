---
title: XML ドキュメントの DOM への読み取り
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: a4fb291f-5630-49ba-a49a-5b66c3b71e49
ms.openlocfilehash: 2e61a9ed1a1ccaa2f9f1543efa1d33c3fcf00061
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130831"
---
# <a name="reading-an-xml-document-into-the-dom"></a>XML ドキュメントの DOM への読み取り
XML 情報は、さまざまな形式からメモリに読み取られます。 XML 情報は、文字列、ストリーム、URL、テキスト リーダー、および <xref:System.Xml.XmlReader> から派生したクラスから読み取ることができます。  
  
 ドキュメントをメモリに読み取る <xref:System.Xml.XmlDocument.Load%2A> メソッドには、オーバーロードされたメソッドが用意されており、異なる形式からデータを取得するために使用できます。 また、文字列から XML を読み取る <xref:System.Xml.XmlDocument.LoadXml%2A> メソッドもあります。  
  
 各 <xref:System.Xml.XmlDocument.Load%2A> メソッドによって、XML ドキュメント オブジェクト モデル (DOM) が読み込まれるときに作成されるノードは異なります。 各種の <xref:System.Xml.XmlDocument.Load%2A> メソッド間の違いと、それについて説明しているトピックを次の表に示します。  
  
|Subject|トピック|  
|-------------|-----------|  
|空白ノードの作成|DOM を読み込むために使用したオブジェクトに応じて、DOM で生成される空白ノードと有意の空白ノードの処理が異なります。 詳細については、「[DOM を読み込むときの空白および有意の空白の処理](../../../../docs/standard/data/xml/white-space-and-significant-white-space-handling-when-loading-the-dom.md)」を参照してください。|  
|特定ノード以降の XML の読み込み、または XML ドキュメント全体の読み込み|<xref:System.Xml.XmlDocument.Load%2A?displayProperty=nameWithType> メソッドを使用して、データを特定のノードから DOM に読み込むことができます。 詳細については、「[リーダーからのデータの読み込み](../../../../docs/standard/data/xml/load-data-from-a-reader.md)」を参照してください。|  
|XML の読み込み時の検証|DOM に読み込む XML データは、読み込みながら検証することができます。 これは検証型の <xref:System.Xml.XmlReader> を使用して行えます。 読み込み時の XML の検証の詳細については、「[DOM における XML ドキュメントの検証](../../../../docs/standard/data/xml/validating-an-xml-document-in-the-dom.md)」を参照してください。|  
  
 <xref:System.Xml.XmlDocument.LoadXml%2A> メソッドによって XML を読み込む例を次に示します。読み込まれたデータは、`data.xml` というテキスト ファイルに保存されます。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
  
Public Class Sample  
  
    Public Shared Sub Main()  
        ' Create the XmlDocument.  
        Dim doc As New XmlDocument()  
        doc.LoadXml(("<book genre='novel' ISBN='1-861001-57-5'>" & _  
                    "<title>Pride And Prejudice</title>" & _  
                    "</book>"))  
        ' Save the document to a file.  
        doc.Save("data.xml")  
    End Sub 'Main  
End Class 'Sample  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
  
public class Sample  
{  
    public static void Main()  
    {  
        // Create the XmlDocument.  
        XmlDocument doc = new XmlDocument();  
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5'>" +  
                    "<title>Pride And Prejudice</title>" +  
                    "</book>");  
  
        // Save the document to a file.  
        doc.Save("data.xml");  
    }  
}  
```  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
