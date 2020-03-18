---
title: ネットワーク上でストリームを使用する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- requesting data from Internet, streams
- Networking
- response to Internet request, streams
- network resources, stream capabilities
- receiving data, stream capabilities
- Network Resources
- sending data, stream capabilities
- downloading Internet resources, streams
- streams, capabilities
- Internet, streams
- streams
ms.assetid: 02b05fba-7235-45ce-94e5-060436ee0875
ms.openlocfilehash: 7d5a2e3eec9b49731a09f6eb41a8d8500a59b45c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180621"
---
# <a name="using-streams-on-the-network"></a>ネットワーク上でストリームを使用する
.NET Framework では、ネットワーク リソースはストリームとして表されます。 .NET Framework は、ストリームを汎用的に扱うことで、次の機能を提供しています。  
  
- Web データを送受信する一般的な方法。 ファイルの実際のコンテンツ (HTML、XML など) にかかわらず、アプリケーションは <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType> と <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType> を使用してデータを送受信します。  
  
- .NET Framework 全体のストリームとの互換性。 ストリームは、.NET Framework 全体で使用されます。 .NET Framework には、ストリームを処理する高機能なインフラストラクチャがあります。 たとえば、ストリームを初期化するコードの数行を変更するだけで、<xref:System.IO.FileStream> から XML データを読み取るアプリケーションを、<xref:System.Net.Sockets.NetworkStream> からデータを読み取るように変更することができます。 **NetworkStream** クラスと他のストリームの主な違いは、**NetworkStream** はシーク不可能であり、<xref:System.Net.Sockets.NetworkStream.CanSeek%2A> プロパティは常に **false** を返し、<xref:System.Net.Sockets.NetworkStream.Seek%2A> メソッドと <xref:System.Net.Sockets.NetworkStream.Position%2A> メソッドは <xref:System.NotSupportedException> をスローする点です。  
  
- データ受信時のデータの処理。 ストリームは、アプリケーションに対して、ダウンロード対象として設定されたデータ全体を強制的に待機させるのではなく、ネットワークからデータを受信したときにデータへのアクセスを提供します。  
  
 <xref:System.Net.Sockets> 名前空間には、特にネットワーク リソースに使用される **クラスを実装する**NetworkStream<xref:System.IO.Stream> クラスが含まれています。 <xref:System.Net.Sockets> 名前空間のクラスは、**NetworkStream** クラスを使用してストリームを表します。  
  
 返されたストリームを使用してネットワークにデータを送信するには、<xref:System.Net.WebRequest.GetRequestStream%2A> で <xref:System.Net.WebRequest> を呼び出します。 **WebRequest** は要求のヘッダーをサーバーに送信します。その後は、返されるストリームで <xref:System.IO.Stream.BeginWrite%2A>、<xref:System.IO.Stream.EndWrite%2A>、または <xref:System.IO.Stream.Write%2A> メソッドを呼び出して、ネットワーク リソースにデータを送信できるようになります。 HTTP など、一部のプロトコルでは、データを送信する前にプロトコル固有のプロパティを設定する必要があります。 データを送信するために、HTTP 固有のプロパティを設定する方法を示すコード例を次に示します。 変数 `sendData` には送信するデータが含まれ、変数 `sendLength` は送信するデータのバイト数を示しているとします。  
  
```csharp  
HttpWebRequest request =
   (HttpWebRequest) WebRequest.Create("http://www.contoso.com/");  
request.Method = "POST";  
request.ContentLength = sendLength;  
try  
{  
   Stream sendStream = request.GetRequestStream();  
   sendStream.Write(sendData,0,sendLength);  
   sendStream.Close();  
}  
catch  
{  
   // Handle errors . . .  
}  
```  
  
```vb  
Dim request As HttpWebRequest = _  
   CType(WebRequest.Create("http://www.contoso.com/"), HttpWebRequest)  
request.Method = "POST"  
request.ContentLength = sendLength  
Try  
   Dim sendStream As Stream = request.GetRequestStream()  
   sendStream.Write(sendData, 0, sendLength)  
   sendStream.Close()  
Catch  
   ' Handle errors . . .  
End Try  
```  
  
 ネットワークからデータを受信するには、<xref:System.Net.WebResponse.GetResponseStream%2A> で <xref:System.Net.WebResponse> を呼び出します。 その後は、返されるストリームで <xref:System.IO.Stream.BeginRead%2A>、<xref:System.IO.Stream.EndRead%2A>、または <xref:System.IO.Stream.Read%2A> メソッドを呼び出すことで、ネットワーク リソースからデータを読み取ることができるようになります。  
  
 ネットワーク リソースからストリームを使用する場合は、次の点に留意してください。  
  
- **CanSeek** プロパティは常に **false** を返します。これは、**NetworkStream** クラスがストリーム内の位置を変更できないためです。 **Seek** メソッドと **Position** メソッドは **NotSupportedException** をスローします。  
  
- **WebRequest** と **WebResponse** を使用すると、**GetResponseStream** を呼び出して作成されるストリーム インスタンスは読み取り専用に、**GetRequestStream** を呼び出して作成されるストリーム インスタンスは書き込み専用になります。  
  
- <xref:System.IO.StreamReader> クラスを使用すると、エンコードが簡単になります。 次のコード例では、**StreamReader** を使用して ASCII エンコードされたストリームを **WebResponse** から読み取ります (この例では要求の作成を示していません)。  
  
- ネットワーク リソースを使用できない場合、**GetResponse** の呼び出しがブロックされる可能性があります。 <xref:System.Net.WebRequest.BeginGetResponse%2A> メソッドと <xref:System.Net.WebRequest.EndGetResponse%2A> メソッドによる非同期要求を使用することを検討してください。  
  
- サーバーへの接続を作成するときに、**GetRequestStream** の呼び出しがブロックされる可能性があります。 <xref:System.Net.WebRequest.BeginGetRequestStream%2A> メソッドと <xref:System.Net.WebRequest.EndGetRequestStream%2A> メソッドによるストリームの非同期要求を使用することを検討してください。  
  
```csharp  
// Create a response object.  
WebResponse response = request.GetResponse();  
// Get a readable stream from the server.  
StreamReader sr =
   new StreamReader(response.GetResponseStream(), Encoding.ASCII);  
// Use the stream. Remember when you are through with the stream to close it.  
sr.Close();  
```  
  
```vb  
' Create a response object.  
Dim response As WebResponse = request.GetResponse()  
' Get a readable stream from the server.  
Dim sr As _
   New StreamReader(response.GetResponseStream(), Encoding.ASCII)  
' Use the stream. Remember when you are through with the stream to close it.  
sr.Close()  
```  
  
## <a name="see-also"></a>参照

- [方法: WebRequest クラスを使用してデータを要求する](how-to-request-data-using-the-webrequest-class.md)
- [データの要求](requesting-data.md)
