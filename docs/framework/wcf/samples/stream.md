---
title: Stream
ms.date: 03/30/2017
ms.assetid: 58a3db81-20ab-4627-bf31-39d30b70b4fe
ms.openlocfilehash: d4166ac0258001b3e4eb0b8ece0f5163863359a4
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716659"
---
# <a name="stream"></a>Stream
このストリーム サンプルでは、ストリーミング転送モードの通信を使用する方法を示します。 サービスは、ストリームを送受信する複数の操作を公開します。 このサンプルは自己ホスト型です。 クライアントとサービスはどちらもコンソール プログラムです。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 Windows Communication Foundation (WCF) は、バッファーまたはストリーミングの2つの転送モードで通信できます。 既定のバッファー転送モードでは、受信側がメッセージを読むためにはメッセージを完全に送信する必要があります。 ストリーミング転送モードでは、メッセージを完全に送信しなくても、受信側がメッセージの処理を開始できます。 ストリーミング モードは、渡される情報が長い場合、または連続的に処理する場合に役立ちます。 ストリーミング モードは、メッセージが大きすぎてすべてをバッファーできない場合にも役立ちます。  
  
## <a name="streaming-and-service-contracts"></a>ストリーミングとサービス コントラクト  
 サービス コントラクトを設計する際は、ストリーミングを考慮します。 操作により大量のデータが受信されたり返されたりする場合、このデータをストリーミングして、入力または出力メッセージのバッファによりメモリの使用率が高くなるのを回避することを検討してください。 データをストリーミングするには、メッセージ内のパラメータが、データを保持するパラメータだけになるようにします。 たとえば、入力メッセージをストリーミングする場合、厳密に 1 つの入力パラメーターが操作に含まれている必要があります。 同様に、出力メッセージをストリーミングする場合、厳密に 1 つの出力パラメーターまたは戻り値が操作に含まれている必要があります。 いずれの場合も、パラメータまたは戻り値の型は `Stream`、`Message`、または `IXmlSerializable` のいずれかである必要があります。 このストリーミングのサンプルに使用されているサービス コントラクトを次に示します。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IStreamingSample  
{  
    [OperationContract]  
    Stream GetStream(string data);  
    [OperationContract]  
    bool UploadStream(Stream stream);  
    [OperationContract]  
    Stream EchoStream(Stream stream);  
    [OperationContract]  
    Stream GetReversedStream();  
  
}  
```  
  
 `GetStream` 操作は、一部の入力データを文字列として受信 (バッファ) し、`Stream` (ストリーミング) を返します。 逆に言えば、`UploadStream` は `Stream` (ストリーミング) を取り込んで、`bool` (バッファー) を返します。 `EchoStream` は `Stream` を出し入れします。これは、入力および出力メッセージがどちらもストリーミングされる操作の例です。 最後に、`GetReversedStream` は入力を行わずに `Stream` (ストリーミング) を返します。  
  
## <a name="enabling-streamed-transfers"></a>ストリーミング転送の有効化  
 前述のように操作コントラクトを定義することにより、プログラミング モデル レベルでのストリーミングが実現します。 ここで作業を終えても、トランスポートはメッセージ全体の内容をバッファします。 トランスポートのストリーミングを有効にするには、トランスポートのバインディング要素で転送モードを選択します。 バインディング要素には、`TransferMode` プロパティがあり、`Buffered`、`Streamed`、`StreamedRequest`、または `StreamedResponse` に設定できます。 転送モードを `Streamed` に設定すると、両方向のストリーミング通信が可能になります。 転送モードを `StreamedRequest` または `StreamedResponse` に設定すると、それぞれ要求または応答のみでのストリーミング通信が可能になります。  
  
 `basicHttpBinding` は、`TransferMode` および `NetTcpBinding` と同様に、バインディングで `NetNamedPipeBinding` プロパティを開示します。 他のトランスポートの場合、転送モードを設定するにはカスタム バインドを作成する必要があります。  
  
 次のサンプルの構成コードでは、`TransferMode` とカスタム HTTP バインディングで、`basicHttpBinding` プロパティをストリーミングに設定しています。  
  
```xml  
<!-- An example basicHttpBinding using streaming. -->  
<basicHttpBinding>  
  <binding name="HttpStreaming" maxReceivedMessageSize="67108864"  
           transferMode="Streamed"/>  
</basicHttpBinding>  
<!-- An example customBinding using HTTP and streaming.-->  
<customBinding>  
  <binding name="Soap12">  
    <textMessageEncoding messageVersion="Soap12WSAddressing10" />  
    <httpTransport transferMode="Streamed"  
                   maxReceivedMessageSize="67108864"/>  
  </binding>  
</customBinding>  
```  
  
 上の構成コードでは、`transferMode` を `Streamed` に設定することに加え、`maxReceivedMessageSize` を 64 MB に設定しています。 防御機構として、`maxReceivedMessageSize` では受信可能なメッセージの最大サイズが決められます。 既定の `maxReceivedMessageSize` は 64 KB です。これは、多くの場合ストリーミングを行うには小さすぎます。  
  
## <a name="processing-data-as-it-is-streamed"></a>ストリーミングされる際のデータの処理  
 `GetStream`、`UploadStream`、および `EchoStream` の各操作はすべて、ファイルからデータを直接送信したり、データをファイルに直接受信したりします。 ただし場合によっては、大量のデータを送信または受信し、データの送受信時に一部の処理をデータのチャンク上で実行する必要があります。 このようなシナリオに対応する 1 つの方法は、データの読み取りまたは書き込み時にデータを処理するカスタム ストリーム (<xref:System.IO.Stream> から派生するクラス) を記述することです。 `GetReversedStream` 操作と `ReverseStream` クラスがこの例です。  
  
 `GetReversedStream` では、`ReverseStream` の新しいインスタンスを作成して返します。 実際の処理は、システムが `ReverseStream` オブジェクトの読み取りを行うときに発生します。 `ReverseStream.Read` の実装により、基になるファイルからバイトのチャンクが読み取られ、バイトが反転され、その反転されたバイトが返されます。 この処理ではファイル全体の内容は反転されません。一度に 1 つのバイトのチャンクが反転されます。 これは、ストリームの内容を読み取ったりストリームに内容を書き込んだりするときに、ストリーミング処理を実行する方法を示す例です。  
  
```csharp
class ReverseStream : Stream  
{  
  
    FileStream inStream;  
    internal ReverseStream(string filePath)  
    {  
        //Opens the file and places a StreamReader around it.  
        inStream = File.OpenRead(filePath);  
    }  
  
    // Other methods removed for brevity.  
  
    public override int Read(byte[] buffer, int offset, int count)  
    {  
        int countRead=inStream.Read(buffer, offset,count);  
        ReverseBuffer(buffer, offset, countRead);  
        return countRead;  
    }  
  
    public override void Close()  
    {  
        inStream.Close();  
        base.Close();  
    }  
    protected override void Dispose(bool disposing)  
    {  
        inStream.Dispose();  
        base.Dispose(disposing);  
    }  
    void ReverseBuffer(byte[] buffer, int offset, int count)  
    {  
        int i, j;  
        for (i = offset, j = offset + count - 1; i < j; i++, j--)  
        {  
            byte currenti = buffer[i];  
            buffer[i] = buffer[j];  
            buffer[j] = currenti;  
        }  
  
    }  
}  
```  
  
## <a name="running-the-sample"></a>サンプルの実行  
 サンプルを実行するには、最初に、このドキュメントの最後にある指示に従ってサービスとクライアントの両方をビルドします。 次に、サービスとクライアントを 2 つの異なるコンソール ウィンドウで開始します。 クライアントが開始したら待機状態になるので、サービスの準備ができたら Enter キーを押します。 その後、クライアントはまず HTTP を経由し、次に TCP を経由して、メソッド `GetStream()`、`UploadStream()`、および `GetReversedStream()` を呼び出します。 サービスからの出力例、続いてクライアントからの出力例を次に示します。  
  
 サービスからの出力 :  
  
```console  
The streaming service is ready.  
Press <ENTER> to terminate service.  
  
Saving to file D:\...\uploadedfile  
......................  
File D:\...\uploadedfile saved  
Saving to file D:\...\uploadedfile  
...............  
File D:\...\uploadedfile saved  
```  
  
 クライアントからの出力 :  
  
```console  
Press <ENTER> when service is ready  
------ Using HTTP ------   
Calling GetStream()  
Saving to file D:\...\clientfile  
......................  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
Calling UploadStream()  
Calling GetReversedStream()  
Saving to file D:\...\clientfile  
......................  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
------ Using Custom HTTP ------  
Calling GetStream()  
Saving to file D:\...\clientfile  
...............  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
Calling UploadStream()  
Calling GetReversedStream()  
Saving to file D:\...\clientfile  
...............  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
  
Press <ENTER> to terminate client.  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
> [!NOTE]
> Svcutil.exe を使用してこのサンプルの構成を再生成した場合は、クライアント コードに一致するように、クライアント構成内のエンドポイント名を変更してください。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Stream`  
