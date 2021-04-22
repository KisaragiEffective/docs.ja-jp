---
title: 入門サンプル
description: WCF を使用して一般的なサービスと一般的なクライアントを実装する方法を説明します。 このサンプルは、他のすべての基本的な技術サンプルの基礎になります。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- basic samples [WCF], getting started
ms.assetid: 967a3d94-0261-49ff-b85a-20bb07f1af20
ms.openlocfilehash: b23be1b33f227154b916429c063ec4106229bb3c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246234"
---
# <a name="getting-started-sample"></a>入門サンプル

入門サンプルでは、Windows Communication Foundation (WCF) を使用して一般的なサービスと一般的なクライアントを実装する方法を示します。 このサンプルは、他のすべての基本的な技術サンプルの基礎になります。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation (WCF) および Windows Workflow Foundation (WF) のサンプル](https://www.microsoft.com/download/details.aspx?id=21459)」にアクセスして、Windows Communication Foundation (WCF) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\GettingStarted\GettingStarted`

サービスが実行する操作は、メタデータとしてパブリックに公開するサービス コントラクト内に表されています。 また、サービスにはその操作を実装するためのコードが含まれています。

クライアントには、サービス コントラクトの定義と、サービスにアクセスするためのプロキシ クラスが含まれています。 プロキシ コードは、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) を使用してサービス メタデータから生成します。

Windows Vista では、サービスは Windows アクティベーション サービス (WAS) 内でホストされます。 Windows XP と Windows Server 2003 では、インターネット インフォメーション サービス (IIS) と ASP.NET によってホストされます。 サービスを IIS または WAS でホストすることにより、サービスに初めてアクセスしたときにそのサービスを自動的にアクティブ化できます。

> [!NOTE]
> IIS ではなくコンソール アプリケーションでサービスをホストするサンプルから始める場合は、[自己ホスト](self-host.md) サンプルを参照してください。

サービスとクライアントは、アクセスの詳細を構成ファイルの設定により指定します。この方法によって展開時の柔軟性が得られます。 この設定には、アドレス、バインディング、およびコントラクトを指定するエンドポイント定義が含まれます。 バインディングは、サービスへのアクセス方法を示すためのトランスポートとセキュリティの詳細を指定します。

サービスは、メタデータを公開するようにランタイムの動作を構成します。

サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 このコントラクトは `ICalculator` インターフェイスによって定義されており、算術演算 (加算、減算、乗算、および 除算) を公開しています。 クライアントは指定された算術演算を要求し、サービスは結果と共に応答します。 サービスは、次に示すコードで定義される `ICalculator` コントラクトを実装します。

```vb
' Define a service contract.
    <ServiceContract(Namespace:="http://Microsoft.Samples.GettingStarted")>
     Public Interface ICalculator
        <OperationContract()>
        Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double
    End Interface
```

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
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

サービス実装は計算を行い、結果を返します。次のコード例を参照してください。

```vb
' Service class which implements the service contract.
Public Class CalculatorService
Implements ICalculator
Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Add
Return n1 + n2
End Function

Public Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Subtract
Return n1 - n2
End Function

Public Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Multiply
Return n1 * n2
End Function

Public Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Divide
Return n1 / n2
End Function
End Class
```

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは構成ファイル (Web.config) を使用して定義します。次のサンプル構成を参照してください。

```xaml
<services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
        <!-- ICalculator is exposed at the base address provided by
         host: http://localhost/servicemodelsamples/service.svc.  -->
       <endpoint address=""
              binding="wsHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
       ...
    </service>
</services>
```

サービスは、IIS ホストまたは WAS ホストから提供されるベース アドレスで、エンドポイントを公開します。 バインディングの構成には、標準の <xref:System.ServiceModel.WSHttpBinding> を使用します。これは HTTP 通信と Web サービスの標準プロトコルを提供し、アドレス指定とセキュリティをサポートします。 コントラクトは、サービスによって実装される `ICalculator` です。

構成のとおり、サービスには、同じコンピューター上のクライアントから `http://localhost/servicemodelsamples/service.svc` でアクセスできます。 リモート コンピューター上のクライアントがサービスにアクセスするには、localhost の代わりに完全修飾ドメイン名を指定する必要があります。

既定では、フレームワークはメタデータを公開しません。 そのため、サービスは <xref:System.ServiceModel.Description.ServiceMetadataBehavior> で有効になり、`http://localhost/servicemodelsamples/service.svc/mex` でメタデータ交換 (MEX) エンドポイントが公開されます。 これを設定する構成を次に示します。

```xaml
<system.serviceModel>
  <services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
      ...
      <!-- the mex endpoint is exposed at
       http://localhost/servicemodelsamples/service.svc/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>

  <!--For debugging purposes set the includeExceptionDetailInFaults
   attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True"/>
        <serviceDebug includeExceptionDetailInFaults="False" />
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```

クライアントは、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) によって生成されるクライアント クラスを使用し、指定されたコントラクト型を使用して通信します。 このように生成されたクライアントは、ファイル generatedClient.cs または generatedClient.vb に含まれています。 このユーティリティは、特定のサービス用のメタデータを取得し、特定のコントラクト型を使用して通信するクライアント アプリケーションによって使用されるクライアントを生成します。 クライアント コードを生成するには、ホストされるサービスを利用できる必要があります。このサービスは、更新されたメタデータの取得に使用されるためです。

 次のコマンドをクライアント ディレクトリで SDK コマンド プロンプトから実行して、型指定のあるプロキシを生成します。

```console
svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /out:generatedClient.cs
```

Visual Basic でクライアントを生成するには、次のコマンドを SDK コマンド プロンプトから入力します。

`Svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /l:vb /out:generatedClient.vb`

生成されたクライアントを使用することにより、クライアントは適切なアドレスとバインディングを構成して、指定のサービス エンドポイントにアクセスできます。 サービスと同様、クライアントは構成ファイル (App.config) を使用して、通信するエンドポイントを指定します。 クライアント エンドポイント構成は、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。次の例を参照してください。

```xaml
<client>
     <endpoint
         address="http://localhost/servicemodelsamples/service.svc"
         binding="wsHttpBinding"
         contract=" Microsoft.ServiceModel.Samples.ICalculator" />
</client>
```

クライアント実装はクライアントをインスタンス化し、型指定のあるインターフェイスを使用してサービスとの通信を開始します。次のコード例を参照してください。

```vb
' Create a client
Dim client As New CalculatorClient()

' Call the Add service operation.
            Dim value1 = 100.0R
            Dim value2 = 15.99R
            Dim result = client.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

' Call the Subtract service operation.
value1 = 145.00R
value2 = 76.54R
result = client.Subtract(value1, value2)
Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

' Call the Multiply service operation.
value1 = 9.00R
value2 = 81.25R
result = client.Multiply(value1, value2)
Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

' Call the Divide service operation.
value1 = 22.00R
value2 = 7.00R
result = client.Divide(value1, value2)
Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

'Closing the client gracefully closes the connection and cleans up resources
```

```csharp
// Create a client.
CalculatorClient client = new CalculatorClient();

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

//Closing the client releases all communication resources.
client.Close();
```

このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

```output
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

この入門サンプルでは、サービスとクライアントの標準的な作成方法を示します。 特定の製品機能を示すためのこのサンプルのもう 1 つの[基本的な](basic-sample.md)ビルド。

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの 1 回限りのセットアップの手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](building-the-samples.md)」の手順に従います。

3. 単一または複数コンピューター構成でサンプルを実行するには、「[Windows Communication Foundation サンプルの実行](running-the-samples.md)」の手順に従います。

## <a name="see-also"></a>関連項目

- [方法: マネージド アプリケーションで WCF サービスをホストする](../how-to-host-a-wcf-service-in-a-managed-application.md)
- [方法: IIS で WCF サービスをホストする](../feature-details/how-to-host-a-wcf-service-in-iis.md)
