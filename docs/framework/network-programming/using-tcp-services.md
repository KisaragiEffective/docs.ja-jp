---
title: TCP サービスの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- requesting data from Internet, TCP
- receiving data, TCP
- TcpClient class, about TcpClient class
- data requests, TCP
- application protocols, TCP
- network resources, TCP
- sending data, TCP
- TCP
- protocols, TCP
- Internet, TCP
ms.assetid: d2811830-3bcb-495c-b82d-cda9cf919aad
ms.openlocfilehash: 3678586647dcf9c47b4494197fbf56cab865b3d3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73039487"
---
# <a name="using-tcp-services"></a>TCP サービスの使用

<xref:System.Net.Sockets.TcpClient> クラスは、TCP を使用してインターネット リソースのデータを要求します。 **TcpClient** のメソッドとプロパティは、TCP を使用したデータの要求と受信用に <xref:System.Net.Sockets.Socket> を作成する詳細を抽象化します。 リモート デバイスへの接続はストリームとして表されるため、.NET Framework のストリーム処理技術を使用してデータの読み取りと書き込みを行うことができます。

TCP プロトコルは、リモート エンドポイントとの接続を確立してから、その接続を使用してデータ パケットを送受信します。 TCP は、データ パケットをエンドポイントに送信し、受信時に正しい順序で構成されるようにする処理を担当します。

TCP 接続を確立するには、必要なサービスをホストするネットワーク デバイスのアドレスを知っている必要があります。また、サービスが通信に使用する TCP ポートも知っている必要があります。 Internet Assigned Numbers Authority (IANA) では、一般的なサービスのポート番号が定義されています (「[Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)」 (サービス名および転送プロトコル ポート番号レジストリ) を参照してください)。 IANA の一覧に掲載されていないサービスが、1,024 から 65,535 の範囲のポート番号を使用している可能性があります。

TCP ポート 13 でタイム サーバーに接続するように **TcpClient** を設定する例を次に示します。

```vb
Imports System.Net.Sockets
Imports System.Text

Public Class TcpTimeClient
    Private const portNum As Integer = 13
    Private const hostName As String = "host.contoso.com"

    ' Entry point  that delegates to C-style main Private Function.
    Public Overloads Shared Sub Main()
        System.Environment.ExitCode = _
            Main(System.Environment.GetCommandLineArgs())
    End Sub

    Overloads Public Shared Function Main(args As String()) As Integer
        Try
            Dim client As New TcpClient(hostName, portNum)

            Dim ns As NetworkStream = client.GetStream()

            Dim bytes(1024) As Byte
            Dim bytesRead As Integer = ns.Read(bytes, 0, bytes.Length)

            Console.WriteLine(Encoding.ASCII.GetString(bytes, 0, bytesRead))

        Catch e As Exception
            Console.WriteLine(e.ToString())
        End Try

        client.Close()

        Return 0
    End Function
End Class
```

```csharp
using System;
using System.Net.Sockets;
using System.Text;

public class TcpTimeClient
{
    private const int portNum = 13;
    private const string hostName = "host.contoso.com";

    public static int Main(String[] args)
    {
        try
        {
            var client = new TcpClient(hostName, portNum);

            NetworkStream ns = client.GetStream();

            byte[] bytes = new byte[1024];
            int bytesRead = ns.Read(bytes, 0, bytes.Length);

            Console.WriteLine(Encoding.ASCII.GetString(bytes,0,bytesRead));

            client.Close();

        }
        catch (Exception e)
        {
            Console.WriteLine(e.ToString());
        }

        return 0;
    }
}
```

<xref:System.Net.Sockets.TcpListener> を使用して、受信要求の TCP ポートを監視します。次に、クライアントへの接続を管理する **Socket** または **TcpClient** を作成します。 <xref:System.Net.Sockets.TcpListener.Start%2A> メソッドでリッスンが有効になり、<xref:System.Net.Sockets.TcpListener.Stop%2A> メソッドでポートのリッスンが無効になります。 <xref:System.Net.Sockets.TcpListener.AcceptTcpClient%2A> メソッドは受信接続要求を受け取り、**TcpClient** を作成して要求を処理します。<xref:System.Net.Sockets.TcpListener.AcceptSocket%2A> メソッドは受信接続要求を受け取り、**Socket** を作成して要求を処理します。

**TcpListener** を使用して TCP ポート 13 を監視するネットワーク タイム サーバーを作成する例を次に示します。 受信接続要求が受け取られると、タイム サーバーはホスト サーバーの現在の日時で応答します。

```vb
Imports System.Net
Imports System.Net.Sockets
Imports System.Text

Public Class TcpTimeServer

    Private const portNum As Integer = 13

    ' Entry point that delegates to C-style main Private Function.
    Public Overloads Shared Sub Main()
        System.Environment.ExitCode = _
            Main(System.Environment.GetCommandLineArgs())
    End Sub

    Overloads Public Shared Function Main(args As String()) As Integer
        Dim done As Boolean = False

        Dim listener As New TcpListener(IPAddress.Any, portNum)

        listener.Start()

        While Not done
            Console.Write("Waiting for connection...")
            Dim client As TcpClient = listener.AcceptTcpClient()

            Console.WriteLine("Connection accepted.")
            Dim ns As NetworkStream = client.GetStream()

            Dim byteTime As Byte() = _
                Encoding.ASCII.GetBytes(DateTime.Now.ToString())

            Try
                ns.Write(byteTime, 0, byteTime.Length)
                ns.Close()
                client.Close()
            Catch e As Exception
                Console.WriteLine(e.ToString())
            End Try
        End While

        listener.Stop()

        Return 0
    End Function
End Class
```

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

public class TcpTimeServer
{

    private const int portNum = 13;

    public static int Main(String[] args)
    {
        bool done = false;

        var listener = new TcpListener(IPAddress.Any, portNum);

        listener.Start();

        while (!done)
        {
            Console.Write("Waiting for connection...");
            TcpClient client = listener.AcceptTcpClient();

            Console.WriteLine("Connection accepted.");
            NetworkStream ns = client.GetStream();

            byte[] byteTime = Encoding.ASCII.GetBytes(DateTime.Now.ToString());

            try
            {
                ns.Write(byteTime, 0, byteTime.Length);
                ns.Close();
                client.Close();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.ToString());
            }
        }

        listener.Stop();

        return 0;
    }

}
```
