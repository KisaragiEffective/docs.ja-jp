---
title: DataContractSerializer サンプル
ms.date: 03/30/2017
helpviewer_keywords:
- XML Formatter
ms.assetid: e0a2fe89-3534-48c8-aa3c-819862224571
ms.openlocfilehash: 59bbeb4091c101efeac4e0562f0e3cbd5a8b5f79
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716363"
---
# <a name="datacontractserializer-sample"></a>DataContractSerializer サンプル
DataContractSerializer サンプルでは、<xref:System.Runtime.Serialization.DataContractSerializer> を示して、データ コントラクト クラスに対応した一般的なシリアル化および逆シリアル化の各サービスを実行します。 このサンプルでは、`Record` オブジェクトを作成し、それをメモリストリームにシリアル化し、メモリストリームを別の `Record` オブジェクトに逆シリアル化して、<xref:System.Runtime.Serialization.DataContractSerializer>の使用方法を示します。 サンプルではその後、バイナリ ライタを使用して `Record` オブジェクトをシリアル化し、バイナリ ライタがシリアル化にどのように影響するかを示します。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 `Record` のデータ コントラクトを次のサンプル コードに示します。  
  
```csharp  
[DataContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
internal class Record  
{  
    private double n1;  
    private double n2;  
    private string operation;  
    private double result;  
  
    internal Record(double n1, double n2, string operation, double result)  
    {  
        this.n1 = n1;  
        this.n2 = n2;  
        this.operation = operation;  
        this.result = result;  
    }  
  
    [DataMember]  
    internal double OperandNumberOne  
    {  
        get { return n1; }  
        set { n1 = value; }  
    }  
  
    [DataMember]  
    internal double OperandNumberTwo  
    {  
        get { return n2; }  
        set { n2 = value; }  
    }  
  
    [DataMember]  
    internal string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [DataMember]  
    internal double Result  
    {  
        get { return result; }  
        set { result = value; }  
    }  
  
    public override string ToString()  
    {  
        return $"Record: {n1} {operation} {n2} = {result}";
    }  
}  
```  
  
 このサンプル コードは `Record` という `record1` オブジェクトを作成し、これを表示します。  
  
```csharp
Record record1 = new Record(1, 2, "+", 3);  
Console.WriteLine("Original record: {0}", record1.ToString());  
```  
  
 次に、<xref:System.Runtime.Serialization.DataContractSerializer> を使用して `record1` をメモリ ストリームにシリアル化します。  
  
```csharp  
MemoryStream stream1 = new MemoryStream();  
  
//Serialize the Record object to a memory stream using DataContractSerializer.  
DataContractSerializer serializer = new DataContractSerializer(typeof(Record));  
serializer.WriteObject(stream1, record1);  
```  
  
 さらに、<xref:System.Runtime.Serialization.DataContractSerializer> を使用してメモリ ストリームを新しい `Record` オブジェクトに逆シリアル化し、これを表示します。  
  
```csharp  
stream1.Position = 0;  
  
//Deserialize the Record object back into a new record object.  
Record record2 = (Record)serializer.ReadObject(stream1);  
  
Console.WriteLine("Deserialized record: {0}", record2.ToString());  
```  
  
 既定では、`DataContractSerializer` は XML のテキスト表現を使用して、オブジェクトをストリームにエンコードします。 ただし、別のライタを渡すことによって XML のエンコーディングを制御できます。 このサンプルでは、<xref:System.Xml.XmlDictionaryWriter.CreateBinaryWriter%2A> を呼び出してバイナリ ライタを作成します。 その後、<xref:System.Runtime.Serialization.DataContractSerializer.WriteObjectContent%2A> を呼び出す際に、このライタと Record オブジェクトをシリアライザに渡します。 最後に、ライタをフラッシュしてストリームの長さを報告します。  
  
```csharp  
MemoryStream stream2 = new MemoryStream();  
  
XmlDictionaryWriter binaryDictionaryWriter = XmlDictionaryWriter.CreateBinaryWriter(stream2);  
serializer.WriteObject(binaryDictionaryWriter, record1);  
binaryDictionaryWriter.Flush();  
  
//report the length of the streams  
Console.WriteLine("Text Stream is {0} bytes long", stream1.Length);  
Console.WriteLine("Binary Stream is {0} bytes long", stream2.Length);  
```  
  
 このサンプルを実行すると、元の Record オブジェクトと逆シリアル化された Record オブジェクトが表示され、続いてテキスト エンコーディングの長さとバイナリ エンコーディングの長さとが比較されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。  
  
```console  
Original record: Record: 1 + 2 = 3  
Deserialized record: Record: 1 + 2 = 3  
Text Stream is 233 bytes long  
Binary Stream is 156 bytes long  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. このサンプルを実行するには、コマンド プロンプトで「client\bin\client.exe」と入力してクライアントを開始します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractSerializer`  
