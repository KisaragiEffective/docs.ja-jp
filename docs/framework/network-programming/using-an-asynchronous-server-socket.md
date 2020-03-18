---
title: 非同期サーバー ソケットの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- Socket class, asynchronous server sockets
- data requests, sockets
- sockets, asynchronous server sockets
- requesting data from Internet, sockets
- server sockets
- receiving data, sockets
- asynchronous server sockets
- protocols, sockets
- Internet, sockets
ms.assetid: 813489a9-3efd-41b6-a33f-371d55397676
ms.openlocfilehash: 467804e685d800643c421ed1aad040a842b42886
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180633"
---
# <a name="using-an-asynchronous-server-socket"></a>非同期サーバー ソケットの使用
非同期サーバー ソケットは、.NET Framework の非同期プログラミング モデルを使用してネットワーク サービス要求を処理します。 <xref:System.Net.Sockets.Socket> クラスは、標準の .NET Framework の非同期名前付けパターンに従います。たとえば、同期の <xref:System.Net.Sockets.Socket.Accept%2A> メソッドは非同期の <xref:System.Net.Sockets.Socket.BeginAccept%2A> メソッドと <xref:System.Net.Sockets.Socket.EndAccept%2A> メソッドに対応します。  
  
 非同期サーバー ソケットには、ネットワークからの接続要求の受け入れを開始するメソッド、接続要求を処理してネットワークからデータの受信を開始するコールバック メソッド、データの受信を終了するコールバック メソッドが必要です。 このセクションでは、このそれぞれについて詳しく説明します。  
  
 次の例では、ネットワークから接続要求の受け入れを開始するために、メソッド `StartListening` で **Socket** を初期化してから、**BeginAccept** メソッドを使用して新しい接続の受け入れを開始します。 ソケットで新しい接続要求を受信すると、accept のコールバック メソッドが呼び出されます。 このコールバック メソッドは、接続を処理する **Socket** インスタンスを取得し、要求を処理するスレッドに **Socket** を渡す役割を持ちます。 accept のコールバック メソッドは <xref:System.AsyncCallback> デリゲートを実装しており、void を返して <xref:System.IAsyncResult> 型の 1 つのパラメーターを受け取ります。 accept のコールバック メソッドのシェルの例を次に示します。  
  
```vb  
Sub AcceptCallback(ar As IAsyncResult)  
    ' Add the callback code here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
void AcceptCallback(IAsyncResult ar)
{  
    // Add the callback code here.  
}  
```  
  
 **BeginAccept** メソッドは 2 つのパラメーターを受け取ります。accept のコールバック メソッドを示す **AsyncCallback** デリゲートと、状態情報をコールバック メソッドに渡すために使用されるオブジェクトです。 次の例では、リッスンしている **Socket** が *state* パラメーターを使用してコールバック メソッドに渡されます。 この例では、**AsyncCallback** デリゲートを作成し、ネットワークから接続の受け入れを開始します。  
  
```vb  
listener.BeginAccept( _  
    New AsyncCallback(SocketListener.AcceptCallback),_  
    listener)  
```  
  
```csharp  
listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
```  
  
 非同期ソケットは、システム スレッド プールの複数のスレッドを使用して、受信接続を処理します。 接続の受け入れに使用されるスレッド、各受信接続の処理に使用されるスレッド、接続からデータを受け取るために使用されるスレッドがあります。 スレッド プールが割り当てるスレッドによっては、これらのスレッドが同じスレッドになる可能性があります。 次の例では、<xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> クラスがメイン スレッドの実行を停止し、実行を続行できるようになったらシグナルを送ります。  
  
 ローカル コンピューター上に非同期 TCP/IP Socket を作成し、接続の受け入れを開始する非同期メソッドの例を次に示します。 **というグローバル**ManualResetEvent`allDone` があり、メソッドが `SocketListener` というクラスのメンバーであり、`AcceptCallback` というコールバック メソッドが定義されているとします。  
  
```vb  
Public Sub StartListening()  
    Dim ipHostInfo As IPHostEntry = Dns.Resolve(Dns.GetHostName())  
    Dim localEP = New IPEndPoint(ipHostInfo.AddressList(0), 11000)  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}")  
  
    Dim listener As New Socket(localEP.Address.AddressFamily, _  
       SocketType.Stream, ProtocolType.Tcp)  
  
    Try  
        listener.Bind(localEP)  
        listener.Listen(10)  
  
        While True  
            allDone.Reset()  
  
            Console.WriteLine("Waiting for a connection...")  
            listener.BeginAccept(New _  
                AsyncCallback(SocketListener.AcceptCallback), _  
                listener)  
  
            allDone.WaitOne()  
        End While  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
    Console.WriteLine("Closing the listener...")  
End Sub 'StartListening  
```  
  
```csharp  
public void StartListening()
{  
    IPHostEntry ipHostInfo = Dns.Resolve(Dns.GetHostName());  
    IPEndPoint localEP = new IPEndPoint(ipHostInfo.AddressList[0], 11000);  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}");  
  
    Socket listener = new Socket(localEP.Address.AddressFamily, SocketType.Stream, ProtocolType.Tcp);  
  
    try
    {  
        listener.Bind(localEP);  
        listener.Listen(10);  
  
        while (true)
        {  
            allDone.Reset();  
  
            Console.WriteLine("Waiting for a connection...");  
            listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
  
            allDone.WaitOne();  
        }  
    }
    catch (Exception e)
    {  
        Console.WriteLine(e.ToString());  
    }  
  
    Console.WriteLine("Closing the listener...");  
}  
```  
  
 accept のコールバック メソッド (前の例では `AcceptCallback`) は、処理を継続するためにメイン アプリケーション スレッドにシグナルを送り、クライアントとの接続を確立し、クライアントからデータの非同期読み取りを開始します。 次の例は、`AcceptCallback` メソッドの実装の最初の部分です。 このメソッドのセクションでは、処理を継続するためにメイン アプリケーション スレッドに信号を送り、クライアントへの接続を確立しています。 **というグローバル**ManualResetEvent`allDone` があるとします。  
  
```vb  
Public Sub AcceptCallback(ar As IAsyncResult)  
    allDone.Set()  
  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Additional code to read data goes here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
public void AcceptCallback(IAsyncResult ar)
{  
    allDone.Set();  
  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Additional code to read data goes here.
}  
```  
  
 クライアント ソケットからデータを読み取るには、非同期呼び出しの間で値を渡す状態オブジェクトが必要です。 次の例では、リモート クライアントから文字列を受信する状態オブジェクトを実装しています。 クライアント ソケットのフィールド、データを受信するデータ バッファー、クライアントから送信されるデータ文字列を作成する <xref:System.Text.StringBuilder> が含まれています。 これらのフィールドを状態オブジェクトに格納することで、複数の呼び出し間で値を保持し、クライアント ソケットからデータ読み取ることができます。  
  
```vb  
Public Class StateObject  
    Public workSocket As Socket = Nothing  
    Public BufferSize As Integer = 1024  
    Public buffer(BufferSize) As Byte  
    Public sb As New StringBuilder()  
End Class 'StateObject  
```  
  
```csharp  
public class StateObject
{  
    public Socket workSocket = null;  
    public const int BufferSize = 1024;  
    public byte[] buffer = new byte[BufferSize];  
    public StringBuilder sb = new StringBuilder();  
}  
```  
  
 クライアント ソケットからデータの受信を開始する `AcceptCallback` メソッドのセクションでは、まず `StateObject` クラスのインスタンスを初期化してから、<xref:System.Net.Sockets.Socket.BeginReceive%2A> メソッドを呼び出してクライアント ソケットからデータの読み取りを非同期に開始します。  
  
 `AcceptCallback` メソッドのサンプル コード全体を次に示します。 **クラスが定義されている** というグローバル `allDone,`ManualResetEvent`StateObject` があり、`ReadCallback` メソッドが `SocketListener` というクラスで定義されているとします。  
  
```vb  
Public Shared Sub AcceptCallback(ar As IAsyncResult)  
    ' Get the socket that handles the client request.  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Signal the main thread to continue.  
    allDone.Set()  
  
    ' Create the state object.  
    Dim state As New StateObject()  
    state.workSocket = handler  
    handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
        AddressOf AsynchronousSocketListener.ReadCallback, state)  
End Sub 'AcceptCallback  
```  
  
```csharp  
public static void AcceptCallback(IAsyncResult ar)
{  
    // Get the socket that handles the client request.  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Signal the main thread to continue.  
    allDone.Set();  
  
    // Create the state object.  
    StateObject state = new StateObject();  
    state.workSocket = handler;  
    handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
        new AsyncCallback(AsynchronousSocketListener.ReadCallback), state);  
}  
```  
  
 非同期ソケット サーバーのために実装が必要な最後のメソッドは、クライアントから送信されたデータを返す read のコールバック メソッドです。 accept のコールバック メソッドと同様に、read のコールバック メソッドは **AsyncCallback** デリゲートです。 このメソッドは、クライアント ソケットからデータ バッファーに 1 から数バイトのデータを読み取ってから、クライアントから送信されたデータが完了するまで、**BeginReceive** メソッドを再び呼び出します。 メッセージ全体がクライアントから読み取られたら、コンソールに文字列が表示され、クライアントへの接続を処理するサーバー ソケットが閉じられます。  
  
 `ReadCallback` メソッドを実装する例を次に示します。 `StateObject` クラスが定義されているとします。  
  
```vb  
Public Shared Sub ReadCallback(ar As IAsyncResult)  
    Dim state As StateObject = CType(ar.AsyncState, StateObject)  
    Dim handler As Socket = state.workSocket  
  
    ' Read data from the client socket.
    Dim read As Integer = handler.EndReceive(ar)  
  
    ' Data was read from the client socket.  
    If read > 0 Then  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, read))  
        handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
            AddressOf ReadCallback, state)  
    Else  
        If state.sb.Length > 1 Then  
            ' All the data has been read from the client;  
            ' display it on the console.  
            Dim content As String = state.sb.ToString()  
            Console.WriteLine($"Read {content.Length} bytes from socket. {ControlChars.Cr} Data : {content}")  
        End If  
    End If  
End Sub 'ReadCallback  
```  
  
```csharp  
public static void ReadCallback(IAsyncResult ar)
{  
    StateObject state = (StateObject) ar.AsyncState;  
    Socket handler = state.WorkSocket;  
  
    // Read data from the client socket.  
    int read = handler.EndReceive(ar);  
  
    // Data was read from the client socket.  
    if (read > 0)
    {  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer,0,read));  
        handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
            new AsyncCallback(ReadCallback), state);  
    }
    else
    {  
        if (state.sb.Length > 1)
        {  
            // All the data has been read from the client;  
            // display it on the console.  
            string content = state.sb.ToString();  
            Console.WriteLine($"Read {content.Length} bytes from socket.\n Data : {content}");
        }  
        handler.Close();  
    }  
}  
```  
  
## <a name="see-also"></a>参照

- [同期サーバー ソケットの使用](using-a-synchronous-server-socket.md)
- [非同期サーバー ソケットの例](asynchronous-server-socket-example.md)
- [スレッド化](../../standard/threading/index.md)
- [リッスン (ソケットで)](listening-with-sockets.md)
