---
title: WCF クライアントを使用したサービスへのアクセス
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], consuming services
ms.assetid: d780af9f-73c5-42db-9e52-077a5e4de7fe
ms.openlocfilehash: 462d9a3923009f0124c2b90b6fa86dfa9869a3c5
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72316535"
---
# <a name="accessing-services-using-a-wcf-client"></a>WCF クライアントを使用したサービスへのアクセス

サービスを作成した後、次の手順では、WCF クライアントプロキシを作成します。 クライアントアプリケーションは、WCF クライアントプロキシを使用してサービスと通信します。 通常、クライアントアプリケーションは、サービスのメタデータをインポートして、サービスを呼び出すために使用できる WCF クライアントコードを生成します。

 WCF クライアントを作成するための基本的な手順は次のとおりです。

1. サービス コードをコンパイルします。

2. WCF クライアントプロキシを生成します。

3. WCF クライアント プロキシをインスタンス化します。

WCF クライアント プロキシは、サービス モデル メタデータ ユーティリティ ツール (SvcUtil.exe) を使用することによって、手動で生成することができます。詳細は「[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)」を参照してください。 WCF クライアントプロキシは、**サービス参照の追加**機能を使用して Visual Studio 内で生成することもできます。 いずれかの方法で WCF クライアント プロキシを生成するには、サービスが実行中であることが必要です。 サービスが自己ホスト型の場合は、ホストを実行する必要があります。 サービスが IIS/WAS でホストされている場合、特に必要な操作はありません。

## <a name="servicemodel-metadata-utility-tool"></a>ServiceModel メタデータ ユーティリティ ツール
 [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)は、メタデータからコードを生成するためのコマンドラインツールです。 基本的な Svcutil.exe コマンドの使用例を次に示します。

```console
Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
```

 また、Svcutil.exe は、ファイル システム上の Web サービス記述言語 (WSDL) ファイルや XML スキーマ定義言語 (XSD) ファイルを指定して使用することもできます。

```console
Svcutil.exe <list of WSDL and XSD files on file system>
```

 結果として、クライアントアプリケーションがサービスを呼び出すために使用できる WCF クライアントコードを含むコードファイルが生成されます。

 このツールを使用して構成ファイルを生成することもできます。

```console
Svcutil.exe <file1 [,file2]>
```

 ファイル名を 1 つだけ指定した場合、それは出力ファイルの名前になります。 ファイル名を 2 つ指定した場合は、1 番目のファイルが入力構成ファイルになり、そのファイルの内容と生成された構成がマージされ、2 番目のファイルに書き出されます。 構成の詳細については、「[サービスのバインドの構成](configuring-bindings-for-wcf-services.md)」を参照してください。

> [!IMPORTANT]
> セキュリティで保護されていないメタデータ要求には、セキュリティで保護されていないネットワーク要求と同様の一定の危険が伴います。通信先のエンドポイントが、本当に相手から通知されたとおりのエンドポイントかどうかわからない場合、取得した情報は悪質なサービスからのメタデータである可能性があります。

## <a name="add-service-reference-in-visual-studio"></a>Visual Studio の "サービス参照の追加"

 サービスが実行されている状態で、WCF クライアントプロキシを含むプロジェクトを右クリックし、[ > **サービス参照**の**追加**] を選択します。 [**サービス参照の追加] ダイアログボックス**で、呼び出すサービスの URL を入力**し、[実行] ボタンを**クリックします。 このダイアログ ボックスには、指定したアドレスで利用可能なサービスの一覧が表示されます。 サービスをダブルクリックして、使用可能なコントラクトと操作を確認し、生成されたコードの名前空間を指定して、 **[OK]** ボタンをクリックします。

## <a name="example"></a>例
 サービス用に作成されたコントラクトのコード例を次に示します。

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    // Other methods are not shown here.
}
```

```vb
' Define a service contract.
<ServiceContract(Namespace:="http://Microsoft.ServiceModel.Samples")> _
Public Interface ICalculator
    <OperationContract()>  _
    Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
    ' Other methods are not shown here.
End Interface
```

 ServiceModel メタデータユーティリティツールと Visual Studio の**サービス参照の追加**によって、次の WCF クライアントクラスが生成されます。 このクラスは <xref:System.ServiceModel.ClientBase%601> ジェネリック クラスから継承されたもので、`ICalculator` インターフェイスを実装します。 このツールは、`ICalculator` インターフェイス (この例には表示されていません) も生成します。

```csharp
public partial class CalculatorClient : System.ServiceModel.ClientBase<ICalculator>, ICalculator
{
    public CalculatorClient()
    {}

    public CalculatorClient(string endpointConfigurationName) :
            base(endpointConfigurationName)
    {}

    public CalculatorClient(string endpointConfigurationName, string remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(string endpointConfigurationName,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(System.ServiceModel.Channels.Binding binding,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(binding, remoteAddress)
    {}

    public double Add(double n1, double n2)
    {
        return base.Channel.Add(n1, n2);
    }
}
```

```vb
Partial Public Class CalculatorClient
    Inherits System.ServiceModel.ClientBase(Of ICalculator)
    Implements ICalculator

    Public Sub New()
        MyBase.New
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String)
        MyBase.New(endpointConfigurationName)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String, ByVal remoteAddress As String)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal binding As System.ServiceModel.Channels.Binding,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(binding, remoteAddress)
    End Sub

    Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        Implements ICalculator.Add
        Return MyBase.Channel.Add(n1, n2)
    End Function
End Class
```

## <a name="using-the-wcf-client"></a>WCF クライアントの使用
 WCF クライアントを使用するには、次のコードに示すように、WCF クライアントのインスタンスを作成し、そのメソッドを呼び出します。

```csharp
// Create a client object with the given client endpoint configuration.
CalculatorClient calcClient = new CalculatorClient("CalculatorEndpoint"));
// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = calcClient.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
```

```vb
' Create a client object with the given client endpoint configuration.
Dim calcClient As CalculatorClient = _
New CalculatorClient("CalculatorEndpoint")

' Call the Add service operation.
Dim value1 As Double = 100.00D
Dim value2 As Double = 15.99D
Dim result As Double = calcClient.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)
```

## <a name="debugging-exceptions-thrown-by-a-client"></a>クライアントによってスローされた例外のデバッグ

WCF クライアントによってスローされる多くの例外は、サービスの例外によって発生します。 いくつかの例を次に示します。

- <xref:System.Net.Sockets.SocketException>: 既存の接続はリモートホストによって強制的に切断されました。

- <xref:System.ServiceModel.CommunicationException>: 基になる接続が予期せずに閉じられました。

- <xref:System.ServiceModel.CommunicationObjectAbortedException>: ソケット接続が中止されました。 これは、メッセージ処理時のエラー、リモート ホストでの受信タイムアウトの超過、または基になるネットワーク リソースの問題が原因で発生する可能性があります。

このような種類の例外が発生した場合、問題を解決するには、サービス側でトレースをオンにし、そこで発生した例外を特定することをお勧めします。 トレースの詳細に[ついては、「トレース](./diagnostics/tracing/index.md)と[トレースを使用したアプリケーションのトラブルシューティング](./diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)」を参照してください。

## <a name="see-also"></a>参照

- [方法: クライアントを作成する](how-to-create-a-wcf-client.md)
- [方法 : 双方向コントラクトを使用してサービスにアクセスする](./feature-details/how-to-access-services-with-a-duplex-contract.md)
- [方法 : サービス操作を非同期に呼び出す](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)
- [方法 : 一方向コントラクトと要求/応答コントラクトを使用してサービスにアクセスする](./feature-details/how-to-access-wcf-services-with-one-way-and-request-reply-contracts.md)
- [方法 : WSE 3.0 サービスにアクセスする](./feature-details/how-to-access-a-wse-3-0-service-with-a-wcf-client.md)
- [生成されたクライアント コードの理解](./feature-details/understanding-generated-client-code.md)
- [方法 : XmlSerializer を使用する WCF クライアント アプリケーションの起動時間を短縮する](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)
- [クライアントのランタイム動作の指定](specifying-client-run-time-behavior.md)
- [クライアントの動作の構成](configuring-client-behaviors.md)
