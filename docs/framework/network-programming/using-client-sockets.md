---
title: クライアント ソケットの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- receiving data, sockets
- Socket class, client sockets
- protocols, sockets
- Internet, sockets
- sockets, client sockets
- client sockets
ms.assetid: 81de9f59-8177-4d98-b25d-43fc32a98383
ms.openlocfilehash: fe2ad55c3f60347369c0e92bc834d81d98f3870e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71046951"
---
# <a name="using-client-sockets"></a>クライアント ソケットの使用
<xref:System.Net.Sockets.Socket> を使用して会話を開始するには、まずアプリケーションとリモート デバイス間にデータ パイプを作成する必要があります。 他のネットワーク アドレス ファミリとプロトコルもありますが、この例では、リモート サービスとの TCP/IP 接続を作成する方法を説明します。  
  
 TCP/IP はネットワーク アドレスとサービス ポート番号を使用して、サービスを一意に識別しています。 ネットワーク アドレスは、ネットワーク上の特定のデバイスを識別します。ポート番号は、そのデバイス上の特定のサービスの接続先を識別します。 ネットワーク アドレスとサービス ポートの組み合わせはエンドポイントと呼ばれます。 .NET Framework では、エンドポイントは <xref:System.Net.EndPoint> クラスで表されます。 **EndPoint** の子孫は、サポートされるアドレス ファミリごとに定義されます。IP アドレス ファミリの場合、クラスは <xref:System.Net.IPEndPoint> です。  
  
 <xref:System.Net.Dns> クラスは、TCP/IP インターネット サービスを使用するアプリケーションにドメインネーム サービスを提供します。 <xref:System.Net.Dns.Resolve%2A> メソッドは、ユーザー フレンドリなドメイン名 ("host.contoso.com" など) を数値のインターネット アドレス (192.168.1.1 など) にマップするクエリを DNS サーバーに送信します。 **Resolve** は、要求した名前のアドレスとエイリアスの一覧を含む <xref:System.Net.IPHostEntry> を返します。 ほとんどの場合、<xref:System.Net.IPHostEntry.AddressList%2A> 配列で返された最初のアドレスを使用できます。 次のコードでは、サーバー host.contoso.com の IP アドレスを含む <xref:System.Net.IPAddress> を取得します。  
  
```vb  
Dim ipHostInfo As IPHostEntry = Dns.Resolve("host.contoso.com")  
Dim ipAddress As IPAddress = ipHostInfo.AddressList(0)  
```  
  
```csharp  
IPHostEntry ipHostInfo = Dns.Resolve("host.contoso.com");  
IPAddress ipAddress = ipHostInfo.AddressList[0];  
```  
  
 Internet Assigned Numbers Authority (IANA) では、一般的なサービスのポート番号が定義されています (詳細については、「[Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)」 (サービス名および転送プロトコル ポート番号レジストリ) を参照してください)。 他のサービスが、1,024 から 65,535 の範囲内でポート番号を登録している可能性があります。 次のコードでは、host.contoso.com の IP アドレスとポート番号を組み合わせて、接続のリモート エンドポイントを作成します。  
  
```vb  
Dim ipe As New IPEndPoint(ipAddress, 11000)  
```  
  
```csharp  
IPEndPoint ipe = new IPEndPoint(ipAddress,11000);  
```  
  
 リモート デバイスのアドレスを決定し、接続に使用するポートを選択すると、アプリケーションはそのリモート デバイスと接続の確立を試行できるようになります。 次の例では、既存の **IPEndPoint** を使用してリモート デバイスに接続し、スローされるすべての例外をキャッチします。  
  
```vb  
Try  
    s.Connect(ipe)  
Catch ae As ArgumentNullException  
    Console.WriteLine("ArgumentNullException : {0}", _  
        ae.ToString())  
Catch se As SocketException  
    Console.WriteLine("SocketException : {0}", se.ToString())  
Catch e As Exception  
    Console.WriteLine("Unexpected exception : {0}", e.ToString())  
End Try  
```  
  
```csharp  
try {  
    s.Connect(ipe);  
} catch(ArgumentNullException ae) {  
    Console.WriteLine("ArgumentNullException : {0}", ae.ToString());  
} catch(SocketException se) {  
    Console.WriteLine("SocketException : {0}", se.ToString());  
} catch(Exception e) {  
    Console.WriteLine("Unexpected exception : {0}", e.ToString());  
}  
```  
  
## <a name="see-also"></a>参照

- [同期クライアント ソケットの使用](using-a-synchronous-client-socket.md)
- [非同期クライアント ソケットの使用](using-an-asynchronous-client-socket.md)
- [方法: ソケットを作成する](how-to-create-a-socket.md)
- [ソケット](sockets.md)
