---
title: DataContractJsonSerializer サンプル
ms.date: 03/30/2017
ms.assetid: 3c2c4747-7510-4bdf-b4fe-64f98428ef4a
ms.openlocfilehash: 52e10ee28137b16bd90e6f3f3ac41f839528f334
ms.sourcegitcommit: dfad244ba549702b649bfef3bb057e33f24a8fb2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2020
ms.locfileid: "75904541"
---
# <a name="datacontractjsonserializer-sample"></a>DataContractJsonSerializer サンプル

> [!NOTE]
> このサンプルは <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>を対象としています。 JSON のシリアル化と逆シリアル化を含むほとんどのシナリオでは、Api を使用することをお勧めし[ます。](../../../standard/serialization/system-text-json-overview.md) 

<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> では、<xref:System.Runtime.Serialization.DataContractSerializer> と同じ型をサポートしています。 JSON データ形式は、特に Asynchronous JavaScript and XML (AJAX) スタイルの Web アプリケーションを作成するときに便利です。 Windows Communication Foundation (WCF) での AJAX サポートは、ScriptManager コントロールを介して ASP.NET AJAX で使用できるように最適化されています。 ASP.NET AJAX で Windows Communication Foundation (WCF) を使用する方法の例については、 [ajax のサンプル](ajax.md)を参照してください。  
  
このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
このサンプルでは、シリアル化および逆シリアル化を示すために、`Person` データ コントラクトを使用しています。  

```csharp
[DataContract]
class Person
{
    [DataMember]
    internal string name;

    [DataMember]
    internal int age;
}
```

 `Person` 型のインスタンスを JSON にシリアル化するには、最初に <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> を作成し、次に `WriteObject` メソッドを使用して JSON データをストリームに書き込みます。  

```csharp
Person p = new Person();
//Set up Person object...
MemoryStream stream1 = new MemoryStream();
DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(Person));
ser.WriteObject(stream1, p);
```

 メモリ ストリームには有効な JSON データが含まれています。
  
```json  
{"age":42,"name":"John"}  
```  
  
 このサンプルでは、JSON データからオブジェクトへの逆シリアル化を示します。 次に、ストリームを巻き戻し、`ReadObject` を呼び出します。  

```csharp
Person p2 = (Person)ser.ReadObject(stream1);
```

 `p2` オブジェクトを調べることで、JSON データが正しく逆シリアル化されていることがわかります。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\JsonSerialization`  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルを設定、ビルド、および実行するには  
  
1. 「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」で説明されているように、ソリューション jsonserialization .sln をビルドします。  
  
2. 作成されたコンソール アプリケーションを実行します。  
