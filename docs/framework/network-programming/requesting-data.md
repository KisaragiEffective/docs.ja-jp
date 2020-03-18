---
title: データの要求
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sending data
- WebRequest class, sending and receiving data
- requesting data from Internet, about requesting data
- WebClient class, sending and receiving data
- network, requesting data
- receiving data
- sending data, about sending data
- response to Internet request, about responding to Internet requests
- data requests
- receiving data, about receiving data
- Internet, requesting data
ms.assetid: df6f1e1d-6f2a-45dd-8141-4a85c3dafe1d
ms.openlocfilehash: 1f367caf7656a83597b6262a5746686df15d33b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047312"
---
# <a name="requesting-data"></a>データの要求
今日のインターネットの分散操作環境で動作するアプリケーションを開発するには、あらゆる種類のリソースからデータを取得するための効率的で使いやすい方法が必要です。 プラグ可能なプロトコルを使うと、単一のインターフェイスを使って複数のインターネット プロトコルからデータを取得するアプリケーションを開発できます。  
  
## <a name="uploading-and-downloading-data-from-an-internet-server"></a>インターネット サーバーへのデータのアップロードとサーバーからのダウンロード  
 単純な要求/応答トランザクションの場合、<xref:System.Net.WebClient> クラスが、インターネット サーバーとの間でデータをアップロードまたはダウンロードする最も簡単な方法を提供します。 **WebClient** には、ファイルのアップロードとダウンロード、ストリームの送信と受信、およびサーバーへのデータ バッファーの送信と応答の受信を行うメソッドが用意されています。 **WebClient** は、<xref:System.Net.WebRequest> および <xref:System.Net.WebResponse> クラスを使ってインターネット リソースへの実際の接続を行うので、登録されているどのプラグ可能プロトコルでも使用可能です。  
  
 より複雑なトランザクションを行う必要があるクライアント アプリケーションは、**WebRequest** クラスとその子孫を使って、サーバーにデータを要求します。 **WebRequest** は、サーバーへの接続、要求の送信、応答の受信の詳細をカプセル化します。 **WebRequest** は、プラグ可能なプロトコルを使うすべてのアプリケーションで使うことができるプロパティとメソッドのセットを定義している抽象クラスです。 **WebRequest** の子孫 (<xref:System.Net.HttpWebRequest> など) は、**WebRequest** によって定義されているプロパティとメソッドを、基になるプロトコルと整合性があるように実装します。  
  
 **WebRequest** クラスは、 **メソッドに渡された URI の値を使って、作成する特定の派生クラス インスタンスを決定することで、** WebRequest<xref:System.Net.WebRequest.Create%2A> の子孫のプロトコル固有のインスタンスを作成します。 アプリケーションは、子孫のコンストラクターを **メソッドに登録することで、要求の処理に使う必要のある**WebRequest<xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> の子孫を示します。  
  
 インターネット リソースに対する要求は、<xref:System.Net.WebRequest.GetResponse%2A>WebRequest**の** メソッドを呼び出すことによって行われます。 **GetResponse** メソッドは、**WebRequest** のプロパティからプロトコル固有の要求を作成し、サーバーに TCP または UDP ソケット接続を行って、要求を送信します。 HTTP の **Post** 要求や FTP の **Put** 要求など、サーバーにデータを送信する要求の場合は、<xref:System.Net.WebRequest.GetRequestStream%2A?displayProperty=nameWithType> メソッドがデータを送信するネットワーク ストリームを提供します。  
  
 **GetResponse** メソッドは、**WebRequest** と一致するプロトコル固有の **WebResponse** を返します。  
  
 **WebResponse** クラスも、プラグ可能なプロトコルを使うすべてのアプリケーションで使うことができるプロパティとメソッドを定義している抽象クラスです。 **WebResponse** の子孫は、基になるプロトコルのこれらのプロパティとメソッドを実装します。 たとえば、<xref:System.Net.HttpWebResponse> クラスは、HTTP 用の **WebResponse** クラスを実装します。  
  
 サーバーから返されたデータは、<xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType> メソッドによって返されるストリームでアプリケーションに提供されます。 次の例で示すように、他のストリームと同じようにこのストリームを使うことができます。  
  
```csharp  
StreamReader sr =  
   new StreamReader(resp.GetResponseStream(), Encoding.ASCII);  
```  
  
```vb  
Dim sr As StreamReader  
sr = New StreamReader(resp.GetResponseStream(), Encoding.ASCII)  
```  
  
## <a name="see-also"></a>参照

- [.NET Framework のネットワーク プログラミング](index.md)
- [方法: Web ページを要求し、ストリームとして結果を取得する](how-to-request-a-web-page-and-retrieve-the-results-as-a-stream.md)
- [方法: WebRequest に一致するプロトコル固有の WebResponse を取得する](how-to-retrieve-a-protocol-specific-webresponse-that-matches-a-webrequest.md)
