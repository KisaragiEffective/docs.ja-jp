---
title: WCF サービス付き ASMX クライアント
ms.date: 03/30/2017
ms.assetid: 3ea381ee-ac7d-4d62-8c6c-12dc3650879f
ms.openlocfilehash: a560650dba250d1ee4f0b959ead70a2915c9997f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716136"
---
# <a name="asmx-client-with-a-wcf-service"></a>WCF サービス付き ASMX クライアント

このサンプルでは、Windows Communication Foundation (WCF) を使用してサービスを作成し、ASMX クライアントなどの WCF 以外のクライアントからサービスにアクセスする方法を示します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

このサンプルは、クライアント コンソール プログラム (.exe) と、インターネット インフォメーション サービス (IIS) によってホストされるサービス ライブラリ (.dll) で構成されています。 サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 このコントラクトは `ICalculator` インターフェイスによって定義されており、算術演算 (`Add`、`Subtract`、`Multiply`、`Divide`) を公開しています。 ASMX クライアントは算術演算を同期要求し、サービスは結果を添えて応答します。

サービスは、次に示すコードで定義される `ICalculator` コントラクトを実装します。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"), XmlSerializerFormat]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

<xref:System.Runtime.Serialization.DataContractSerializer> と <xref:System.Xml.Serialization.XmlSerializer> は、CLR 型を XML 表現にマップします。 <xref:System.Runtime.Serialization.DataContractSerializer> は一部の XML 表現を、XmlSerializer とは異なる方法で解釈します。 Wsdl.exe などの WCF 以外のプロキシジェネレーターは、XmlSerializer が使用されている場合に、より使用可能なインターフェイスを生成します。 <xref:System.ServiceModel.XmlSerializerFormatAttribute> は、CLR 型を XML にマッピングするために XmlSerializer が使用されるように、`ICalculator` インターフェイスに適用されます。 このサービス実装は、計算を行い、結果を返します。

サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは構成ファイル (Web.config) で定義します。 エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。 サービスは、インターネット インフォメーション サービス (IIS) ホストから提供されるベース アドレスで、エンドポイントを公開します。 `binding` 属性は basicHttpBinding に設定されます。これにより、WS-I Basic Profile 1.1 に準拠した SOAP 1.1 を使用する HTTP 通信を実現します。次のサンプル構成を参照してください。

```xml
<services>
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <!-- This endpoint is exposed at the base address provided by the host: http://localhost/servicemodelsamples/service.svc.  -->
    <endpoint address=""
              binding="basicHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </service>
</services>
```

ASMX クライアントは、Web サービス記述言語 (wsdl) ユーティリティ (Wsdl.exe) によって生成される型指定されたプロキシを使用して、WCF サービスと通信します。 型指定のあるプロキシは、ファイル generatedClient.cs に含まれています。 WSDL ユーティリティは、指定されたサービスが使用するメタデータを取得し、クライアントが通信に使用する型指定のあるプロキシを生成します。 既定では、フレームワークはメタデータを公開しません。 プロキシを生成するために必要なメタデータを公開するには、次の構成に示すように、 [\<serviceMetadata >](../../../../docs/framework/configure-apps/file-schema/wcf/servicemetadata.md)を追加し、その `httpGetEnabled` 属性を `True` に設定する必要があります。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <!-- Setting httpGetEnabled to True on the serviceMetadata
           behavior exposes the service's wsdl at <base address>?wsdl :
           http://localhost/servicemodelsamples/service.svc?wsdl -->
      <serviceMetadata httpGetEnabled="True"/>
      <serviceDebug includeExceptionDetailInFaults="False" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```

次のコマンドをクライアント ディレクトリでコマンド プロンプトから実行して、型指定のあるプロキシを生成します。

```console
wsdl /n:Microsoft.ServiceModel.Samples /o:generatedClient.cs /urlkey:CalculatorServiceAddress http://localhost/servicemodelsamples/service.svc?wsdl
```

生成された型指定のあるプロキシを使用することにより、クライアントは適切なアドレスを構成して、指定のサービス エンドポイントにアクセスできます。 クライアントは構成ファイル (App.config) を使用して、通信するエンドポイントを指定します。

```xml
<appSettings>
  <add key="CalculatorServiceAddress"
       value="http://localhost/ServiceModelSamples/service.svc"/>
</appSettings>
```

クライアント実装は型指定のあるプロキシのインスタンスを構築し、サービスとの通信を開始します。

```csharp
// Create a client to the CalculatorService.
using (CalculatorService client = new CalculatorService())
{
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = client.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

    // Call the Subtract service operation.
    value1 = 145.00D;
    value2 = 76.54D;
    result = client.Subtract(value1, value2);
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

    // Call the Multiply service operation.
    value1 = 9.00D;
    value2 = 81.25D;
    result = client.Multiply(value1, value2);
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

    // Call the Divide service operation.
    value1 = 22.00D;
    value2 = 7.00D;
    result = client.Divide(value1, value2);
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

}

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

> [!NOTE]
> 複合データ型を渡すと返す方法の詳細については、「 [Windows フォームクライアントでのデータバインディング](../../../../docs/framework/wcf/samples/data-binding-in-a-windows-forms-client.md)」、「 [Windows Presentation Foundation クライアントでのデータバインディング](../../../../docs/framework/wcf/samples/data-binding-in-a-wpf-client.md)」、および「 [ASP.NET クライアントでのデータ](../../../../docs/framework/wcf/samples/data-binding-in-an-aspnet-client.md)バインディング」を参照してください。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\ASMX`
