---
title: .NET リモート処理から WCF への移行
ms.date: 03/30/2017
ms.assetid: 16902a42-ef80-40e9-8c4c-90e61ddfdfe5
ms.openlocfilehash: 4b6996b01da6312486649482ffbb812fcb021306
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452553"
---
# <a name="migrating-from-net-remoting-to-wcf"></a>.NET リモート処理から WCF への移行
この記事では、.NET リモート処理を使用するアプリケーションを、Windows Communication Foundation (WCF) を使用するように移行する方法について説明します。 これらの製品間で類似する概念を比較した後、WCF で一般的なリモート処理のシナリオを実現する方法を説明します。  
  
 .NET リモート処理は、旧バージョンとの互換性のためだけにサポートされているレガシー製品です。 これは、クライアントとサーバーの間で個々の信頼レベルを維持できないため、信頼レベルが混在する環境において安全ではありません。 たとえば、インターネットまたは信頼できないクライアントには、.NET リモート処理のエンドポイントを非公開にする必要があります。 既存のリモート処理アプリケーションを、より新しく安全なテクノロジに移行することをお勧めします。 そのアプリケーションの設計に HTTP のみが使用されていて、かつ RESTful である場合、ASP.NET Web API をお勧めします。 詳細については、ASP.NET Web API を参照してください。 アプリケーションが SOAP ベースの場合や、TCP などの非 HTTP プロトコルを必要とする場合は、WCF をお勧めします。  

## <a name="comparing-net-remoting-to-wcf"></a>.NET リモート処理と WCF の比較  
 このセクションでは、.NET リモート処理の基本的な構成要素と、それに相当する WCF の構成要素を比較します。 これらの構成要素は、WCF で一般的なクライアントとサーバーのシナリオを作成するために後で使用します。 次の表は、.NET リモート処理と WCF の主な類似性と相違点をまとめたものです。  
  
||.NET リモート処理|WCF|  
|-|-------------------|---------|  
|サーバーの種類|サブクラス MarshalByRefObject|[ServiceContract] 属性でマーク|  
|サービス操作|サーバーの型のパブリック メソッド|[OperationContract] 属性でマーク|  
|シリアル化|ISerializable または [Serializable]|DataContractSerializer または XmlSerializer|  
|渡されるオブジェクト|値渡しまたは参照渡し|値渡しのみ|  
|エラーと例外|すべてのシリアル化可能な例外|FaultContract\<TDetail >|  
|クライアント プロキシ オブジェクト|厳密に型指定された透過的プロキシが MarshalByRefObjects から自動的に作成されます|厳密に型指定されたプロキシは、ChannelFactory\<TChannel を使用してオンデマンドで生成され >|  
|必要なプラットフォーム|クライアントとサーバーの両方で Microsoft OS と .NET を使用する必要があります|クロスプラットフォーム|  
|メッセージの形式|プライベート|業界標準 (SOAP、WS-* など)|  
  
### <a name="server-implementation-comparison"></a>サーバー実装の比較  
  
#### <a name="creating-a-server-in-net-remoting"></a>.NET リモート処理でサーバーを作成する  
 .NET リモート処理のサーバーの型は MarshalByRefObject から派生させて、次のようにクライアントから呼び出せるメソッドを定義する必要があります。  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 このサーバーの型のパブリック メソッドは、クライアントから使用できるパブリック コントラクトになります。  サーバーのパブリック インターフェイスとその実装の間には境界がありません。1 つの型によって両方を処理します。  
  
 サーバーの型を定義した後は、次の例のように、クライアントから使用可能な状態にできます。  
  
```csharp
TcpChannel channel = new TcpChannel(8080);  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingConfiguration.RegisterWellKnownServiceType(  
    typeof(RemotingServer),   
    "RemotingServer",   
    WellKnownObjectMode.Singleton);  
Console.WriteLine("RemotingServer is running.  Press ENTER to terminate...");  
Console.ReadLine();  
```  
  
 リモート処理の型をサーバーとして使用できるようにするには、構成ファイルを使用するなど、さまざまな方法があります。 この方法はほんの一例です。  
  
#### <a name="creating-a-server-in-wcf"></a>WCF でサーバーを作成する  
 WCF で同等の手順を実行する場合は、パブリック "サービス コントラクト" と具象実装という 2 つの型を作成します。 1 つ目の型は、[ServiceContract] でマークされたインターフェイスとして宣言します。 クライアントから使用できるメソッドは、[OperationContract] でマークします。  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 サーバーの実装は、次の例のように、別の具象クラスで定義します。  
  
```csharp
public class WCFServer : IWCFServer  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 これらの型を定義した後は、次の例のように、WCF サーバーをクライアントから使用可能な状態にできます。  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
Uri baseAddress = new Uri("net.tcp://localhost:8000/wcfserver");  
  
using (ServiceHost serviceHost = new ServiceHost(typeof(WCFServer), baseAddress))  
{  
    serviceHost.AddServiceEndpoint(typeof(IWCFServer), binding, baseAddress);  
    serviceHost.Open();  
  
    Console.WriteLine($"The WCF server is ready at {baseAddress}.");
    Console.WriteLine("Press <ENTER> to terminate service...");  
    Console.WriteLine();  
    Console.ReadLine();  
}  
```  
  
> [!NOTE]
> 両方の例をできる限り似たものにするため、どちらの例でも TCP を使用します。 HTTP を使用する例については、このトピックで後述するシナリオのチュートリアルを参照してください。  
  
 WCF サービスの構成方法やホスト方法には、さまざまなものがあります。 これは、"自己ホスト型" と呼ばれる一例にすぎません。 詳細については、次のトピックを参照してください。  
  
- [方法: サービス コントラクトを定義する](how-to-define-a-wcf-service-contract.md)  
  
- [構成ファイルを使用してサービスを構成する方法](configuring-services-using-configuration-files.md)  
  
- [ホスティング サービス](hosting-services.md)  
  
### <a name="client-implementation-comparison"></a>クライアント実装の比較  
  
#### <a name="creating-a-client-in-net-remoting"></a>.NET リモート処理でクライアントを作成する  
 .NET リモート処理サーバー オブジェクトが使用可能になると、次の例のように、クライアントによって使用することができます。  
  
```csharp
TcpChannel channel = new TcpChannel();  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingServer server = (RemotingServer)Activator.GetObject(  
                            typeof(RemotingServer),   
                            "tcp://localhost:8080/RemotingServer");  
  
RemotingCustomer customer = server.GetCustomer(42);  
Console.WriteLine($"Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 Activator.GetObject() から返される RemotingServer インスタンスは、"透過的プロキシ" と呼ばれています。 これによって RemotingServer 型のパブリック API がクライアントに実装されますが、すべてのメソッドは別のプロセスまたはコンピューターで実行中のサーバー オブジェクトを呼び出します。  
  
#### <a name="creating-a-client-in-wcf"></a>WCF でクライアントを作成する  
 WCF で同等の手順を実行する場合は、チャネル ファクトリを使用してプロキシを明示的に作成します。 リモート処理と同様、プロキシ オブジェクトは、次の例のように、サーバーでの操作の呼び出しに使用できます。  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
String url = "net.tcp://localhost:8000/wcfserver";  
EndpointAddress address = new EndpointAddress(url);  
ChannelFactory<IWCFServer> channelFactory =   
    new ChannelFactory<IWCFServer>(binding, address);  
IWCFServer server = channelFactory.CreateChannel();  
  
Customer customer = server.GetCustomer(42);  
Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 この例では、リモート処理の例に最も近いことから、チャネル レベルでのプログラミングを示しています。 また、Visual Studio の**サービス参照の追加**アプローチを使用して、クライアントプログラミングを簡略化するコードを生成することもできます。 詳細については、次のトピックを参照してください。  
  
- [クライアント チャネル レベルのプログラミング](./extending/client-channel-level-programming.md)  
  
- [方法: サービス参照を追加、更新、または削除する](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)  
  
### <a name="serialization-usage"></a>シリアル化の使用方法  
 .NET リモート処理と WCF は、どちらもクライアントとサーバーの間のオブジェクト送信にシリアル化を使用しますが、次の重要な点が異なります。  
  
1. シリアル化の対象を示すために異なるシリアライザーと規則を使用します。  
  
2. .NET リモート処理では、"参照渡し" のシリアル化をサポートしています。これによって、1 つの階層でメソッドまたはプロパティへのアクセスを行い、セキュリティ境界をまたいだ別の階層でコードを実行できます。 この機能によってセキュリティの脆弱性が露呈することが、信頼できないクライアントにはリモート処理のエンドポイントを決して公開できない、主な理由の 1 つです。  
  
3. リモート処理で使用されるシリアル化はオプトアウト (シリアル化しないものを明示的に除外する) で、WCF でのシリアル化はオプトイン (どのメンバーをシリアル化するのかを明示的にマークする) です。  
  
#### <a name="serialization-in-net-remoting"></a>.NET リモート処理でのシリアル化  
 .NET リモート処理では、クライアントとサーバーの間でオブジェクトをシリアル化および逆シリアル化するために次の 2 つの方法をサポートしています。  
  
- *値渡し*–オブジェクトの値は階層の境界をまたいでシリアル化され、そのオブジェクトの新しいインスタンスがもう一方の層に作成されます。 この新しいインスタンスのメソッドまたはプロパティへの呼び出しはいずれもローカルに実行され、元のオブジェクトまたは階層には影響しません。  
  
- *参照渡し*-特別な "オブジェクト参照" は、階層の境界を越えてシリアル化されます。 1 つの階層がそのオブジェクトのメソッドまたはプロパティとやり取りをする場合、元の階層の元のオブジェクトに対して通信を行います。 参照渡しのオブジェクトは、いずれかの方向、つまりサーバーからクライアント、またはクライアントからサーバーにフローできます。  
  
 リモート処理での値渡し型は、次の例のように、[Serializable] 属性でマークされるか、ISerializable を実装します。  
  
```csharp
[Serializable]  
public class RemotingCustomer  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 参照渡し型は、次の例のように、MarshalByRefObject クラスから派生します。  
  
```csharp
public class RemotingCustomerReference : MarshalByRefObject  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 リモート処理の参照渡しのオブジェクトによる影響を理解することは、非常に重要です。 どちらかの階層 (クライアントまたはサーバー) が参照渡しのオブジェクトをもう一方の階層に送信する場合、すべてのメソッド呼び出しは、オブジェクトを持つ元の階層で実行されます。 たとえば、サーバーから返される参照渡しのオブジェクトでクライアントが呼び出すメソッドは、サーバーでコードを実行します。 同様に、クライアントから提供される参照渡しのオブジェクトでサーバーが呼び出すメソッドは、クライアントでコードを実行します。 このため、.NET リモート処理は、完全に信頼できる環境内でのみ使用することが推奨されます。 パブリック .NET リモート処理のエンドポイントを信頼できないクライアントに公開すると、リモート処理サーバーが攻撃に対して無防備な状態になります。  
  
#### <a name="serialization-in-wcf"></a>WCF でのシリアル化  
 WCF では、値渡しのシリアル化のみをサポートしています。 クライアントとサーバーの間で交換する型を定義するための最も一般的な方法は、次の例のようになります。  
  
```csharp
[DataContract]  
public class WCFCustomer  
{  
    [DataMember]  
    public string FirstName { get; set; }  
  
    [DataMember]  
    public string LastName { get; set; }  
  
    [DataMember]  
    public int CustomerId { get; set; }  
}  
```  
  
 [DataContract] 属性は、この型をクライアントとサーバーの間でシリアル化および逆シリアル化ができる型として識別します。 [DataMember] 属性は、シリアル化する個々のプロパティまたはフィールドを識別します。  
  
 WCF が階層をまたいでオブジェクトを送信する場合は、値のみをシリアル化し、もう一方の階層でオブジェクトの新しいインスタンスを作成します。 オブジェクトの値とのやり取りはローカルでのみ発生し、.NET リモート処理の参照渡しのオブジェクトのような方法でもう一方の階層と通信することはありません。 詳細については、「[シリアル化と逆シリアル化](./feature-details/serialization-and-deserialization.md)」を参照してください。  
  
### <a name="exception-handling-capabilities"></a>例外処理の機能  
  
#### <a name="exceptions-in-net-remoting"></a>.NET リモート処理での例外  
 リモート処理サーバーによってスローされた例外は、シリアル化され、クライアントに送信されて、その他すべての例外と同様に、クライアントでローカルにスローされます。 カスタムの例外は、Exception 型をサブクラス化して [Serializable] でマークすることによって作成できます。   フレームワークの例外の大部分は既にこの方法でマークされているため、サーバーによるスロー、シリアル化、およびクライアントでの再スローが可能です。 この設計は開発中には便利ですが、サーバー側の情報が誤ってクライアントに公開されてしまう可能性があります。 リモート処理の使用を完全に信頼できる環境に限定する必要がある理由はさまざまですが、これもその 1 つです。  
  
#### <a name="exceptions-and-faults-in-wcf"></a>WCF での例外とエラー  
 WCF では、不注意による情報の漏洩につながる可能性があるため、サーバーからクライアントに任意の例外型を返すことはできません。 サービス操作によって予期しない例外がスローされた場合、汎用の FaultException がクライアントでスローされます。 この例外には、問題の発生原因や発生場所の情報は含まれませんが、これらは一部のアプリケーションにとっては必要ありません。 より豊富なエラー情報をクライアントに伝える必要があるアプリケーションは、エラー コントラクトを定義することによってこれを実行します。  
  
 方法としては、まずエラー情報を伝送するための [DataContract] 型を作成します。  
  
```csharp
[DataContract]  
public class CustomerServiceFault  
{  
    [DataMember]  
    public string ErrorMessage { get; set; }  
  
    [DataMember]  
    public int CustomerId {get;set;}  
}  
```  
  
 各サービス操作に使用するエラー コントラクトを指定します。  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    [FaultContract(typeof(CustomerServiceFault))]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 サーバーは、FaultException のスローによってエラー状態を報告します。  
  
```csharp
throw new FaultException<CustomerServiceFault>(  
    new CustomerServiceFault() {   
        CustomerId = customerId,   
        ErrorMessage = "Illegal customer Id"   
    });  
```  
  
 また、クライアントがサーバーに要求を行う場合は、常にエラーを通常の例外としてキャッチできます。  
  
```csharp
try  
{  
    Customer customer = server.GetCustomer(-1);  
}  
catch (FaultException<CustomerServiceFault> fault)  
{  
    Console.WriteLine($"Fault received: {fault.Detail.ErrorMessage}");
}  
```  
  
 エラー コントラクトの詳細については、「<xref:System.ServiceModel.FaultException>」を参照してください。  
  
### <a name="security-considerations"></a>セキュリティの考慮事項  
  
#### <a name="security-in-net-remoting"></a>.NET リモート処理でのセキュリティ  
 一部の .NET リモート処理チャネルでは、チャネル層 (IPC と TCP) での認証および暗号化などのセキュリティ機能をサポートしています。 HTTP チャネルでは、認証と暗号化の両方にインターネット インフォメーション サービス (IIS) を利用しています。 こういったサポートがあっても、.NET リモート処理を安全性が低い通信プロトコルと見なし、完全に信頼できる環境内に使用を制限する必要があります。 パブリック リモート処理のエンドポイントをインターネットや信頼できないクライアントに公開することは絶対に避けてください。  
  
#### <a name="security-in-wcf"></a>WCF でのセキュリティ  
 WCF は、.NET リモート処理で見つかった種類の脆弱性に対処する必要性などがあったため、セキュリティを考慮して設計されています。 WCF では、トランスポートとメッセージの両方のレベルでセキュリティが提供され、認証、承認、暗号化などのために、数多くのオプションが利用できます。 詳細については、次のトピックを参照してください。  
  
- [セキュリティ](./feature-details/security.md)  
  
- [WCF セキュリティガイダンス](./feature-details/security-guidance-and-best-practices.md)  
  
## <a name="migrating-to-wcf"></a>WCF への移行  
  
### <a name="why-migrate-from-remoting-to-wcf"></a>リモート処理から WCF に移行する理由  
  
- **.NET リモート処理は従来の製品です。** 「 [.Net リモート処理](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507%28v=vs.100%29)」で説明されているように、これは従来の製品と見なされ、新規の開発にはお勧めできません。 新規および既存のアプリケーションには、WCF または ASP.NET Web API をお勧めします。  
  
- **WCF では、クロスプラットフォームの標準を使用します。** WCF は、クロスプラットフォームの相互運用性を考慮して設計されており、さまざまな業界標準 (SOAP、WS-Security、WS-Trust など) をサポートしています。 WCF サービスでは、Windows 以外のオペレーション システムで動作中のクライアントとの相互運用ができます。 リモート処理は、主に、サーバーアプリケーションとクライアントアプリケーションの両方が Windows オペレーティングシステムで .NET Framework を使用して実行する環境向けに設計されています。
  
- **WCF にはセキュリティが組み込まれています。** WCF はセキュリティを考慮して設計されており、認証、トランスポートレベルのセキュリティ、メッセージレベルのセキュリティなどのさまざまなオプションを提供しています。リモート処理は、アプリケーションの相互運用を容易にするように設計されていますが、信頼されていない環境ではセキュリティで保護するように設計されていません。 WCF は信頼できる環境と信頼できない環境の両方で動作するよう設計されました。  
  
### <a name="migration-recommendations"></a>移行の推奨事項  
 .NET リモート処理から WCF に移行するための推奨手順を次に示します。  
  
- **サービスコントラクトを作成します。** サービス インターフェイスの型を定義し、[ServiceContract] 属性でマークします。クライアントからの呼び出しを可能にするすべてのメソッドを [OperationContract] でマークします。  
  
- **データコントラクトを作成します。** サーバーとクライアントの間で交換されるデータ型を定義し、[DataContract] 属性でマークします。 クライアントから使用できるようにするすべてのフィールドとプロパティを [DataMember] でマークします。  
  
- **エラーコントラクトを作成します (省略可能)。** エラーが検出されたときにサーバーとクライアントの間で交換される型を作成します。 これらの型を [DataContract] と [DataMember] でマークしてシリアル化できるようにします。 [OperationContract] でマークしたすべてのサービス操作について、どのエラーが返されるのかを示すために [FaultContract] でも同様にマークします。  
  
- **サービスを構成してホストします。** サービス コントラクトの作成後の手順としては、エンドポイントでサービスを公開するためにバインドを構成します。 詳細については、「[エンドポイント: アドレス、バインディング、およびコントラクト](./feature-details/endpoints-addresses-bindings-and-contracts.md)」を参照してください。  
  
 リモート処理アプリケーションの WCF への移行が完了した後は、.NET リモート処理で依存関係を削除する必要があります。 これにより、リモート処理の脆弱性がアプリケーションから確実に取り除かれます。 実行する必要がある手順は、次のとおりです。  
  
- **MarshalByRefObject の使用を中止しています。** MarshalByRefObject 型は、リモート処理のためだけに存在する型で、WCF では使用されません。 MarshalByRefObject をサブクラスに持つアプリケーション型は、削除するか変更する必要があります。  
  
- **[Serializable] と ISerializable の使用を中止しています。** [Serializable] 属性と ISerializable インターフェイスは、信頼できる環境内の型をシリアル化するように設計されたものであり、これらはリモート処理によって使用されます。 WCF のシリアル化では、[DataContract] と [DataMember] でマークされている型を使用します。 アプリケーションで使用されるデータ型は、ISerializable や [Serializable] を使用せずに [DataContract] を使用するように変更する必要があります。  
  
### <a name="migration-scenarios"></a>移行のシナリオ  
 WCF で次の一般的なリモート処理のシナリオを実現する方法を見てみましょう。  
  
1. サーバーからクライアントに値渡しでオブジェクトを返す  
  
2. サーバーからクライアントに参照渡しでオブジェクトを返す  
  
3. クライアントからサーバーに値渡しでオブジェクトを送信する  
  
> [!NOTE]
> WCF では、クライアントからサーバーに参照渡しでオブジェクを送信することはできません。  
  
 これらのシナリオを読み進める際は、.NET リモート処理のベースライン インターフェイスが次の例のようになると想定してください。 ここでは、.NET リモート処理の実装は重要ではありません。その理由は、ここで説明するのは WCF を使用して同等の機能を実装する方法のみであるためです。  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    // Demonstrates server returning object by-value  
    public Customer GetCustomer(int customerId) {…}  
  
    // Demonstrates server returning object by-reference  
    public CustomerReference GetCustomerReference(int customerId) {…}  
  
    // Demonstrates client passing object to server by-value  
    public bool UpdateCustomer(Customer customer) {…}  
}  
```  
  
#### <a name="scenario-1-service-returns-an-object-by-value"></a>シナリオ 1: サービスが値渡しでオブジェクトを返す  
 このシナリオでは、値渡しでクライアントにオブジェクトを返すサーバーについて説明します。 WCF は常にサーバーから値渡しでオブジェクトを返すため、次の各手順では、単に通常の WCF サービスを構築する方法について説明します。  
  
1. まずは、WCF サービスのパブリック インターフェイスを定義し、[ServiceContract] 属性でマークします。 クライアントが呼び出すサーバー側のメソッドを識別するために [OperationContract] を使用します。  
  
   ```csharp
   [ServiceContract]  
   public interface ICustomerService  
   {  
       [OperationContract]  
       Customer GetCustomer(int customerId);  
  
       [OperationContract]  
       bool UpdateCustomer(Customer customer);  
   }  
   ```  
  
2. 次の手順は、このサービスのデータ コントラクトの作成です。 これを行うには、[DataContract] 属性でマークされたクラス (インターフェイスではない) を作成します。 クライアントとサーバーの両方に対して表示する個々のプロパティやフィールドは、[DataMember] でマークします。 派生型を許可する場合は、それを識別するために [KnownType] 属性を使用する必要があります。 WCF がこのサービス用にシリアル化または逆シリアル化を許可する型は、サービス インターフェイス内の型と、これらの "既知の型" のみです。 この一覧にないその他の型を交換しようとすると、拒否されます。  
  
   ```csharp
   [DataContract]  
   [KnownType(typeof(PremiumCustomer))]  
   public class Customer  
   {  
       [DataMember]  
       public string FirstName { get; set; }  
  
       [DataMember]  
       public string LastName { get; set; }  
  
       [DataMember]  
       public int CustomerId { get; set; }  
   }  
  
   [DataContract]  
   public class PremiumCustomer : Customer   
   {  
       [DataMember]  
       public int AccountId { get; set; }  
   }  
   ```  
  
3. 次に、サービス インターフェイスの実装を提供します。  
  
   ```csharp  
   public class CustomerService : ICustomerService  
   {  
       public Customer GetCustomer(int customerId)  
       {  
           // read from database  
       }  
  
       public bool UpdateCustomer(Customer customer)  
       {  
           // write to database  
       }  
   }  
   ```  
  
4. WCF サービスを実行するには、特定の WCF バインドを使用して特定の URL で上記のサービス インターフェイスを公開するエンドポイントを宣言する必要があります。 通常、これを行うには、サーバー プロジェクトの web.config ファイルに次のセクションを追加します。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
5. WCF サービスは次のコードで開始できます。  
  
   ```csharp
   ServiceHost customerServiceHost = new ServiceHost(typeof(CustomerService));  
       customerServiceHost.Open();  
   ```  
  
     この ServiceHost は、開始されると web.config ファイルを使用して適切なコントラクト、バインド、エンドポイントを確立します。 構成ファイルの詳細については、「[構成ファイルを使用したサービスの構成](./configuring-services-using-configuration-files.md)」を参照してください。 このようなサーバーの開始方法は、自己ホストと呼ばれています。 WCF サービスをホストする他の選択肢の詳細については、「[ホスティングサービス](./hosting-services.md)」を参照してください。  
  
6. クライアント プロジェクトの app.config では、サービスのエンドポイントの対応するバインド情報を宣言する必要があります。 Visual Studio でこれを行う最も簡単な方法は、**サービス参照の追加**を使用することです。これにより、app.config ファイルが自動的に更新されます。 また、これと同じ変更を手動で加えることもできます。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     **サービス参照の追加**の使用方法の詳細については、「[方法: サービス参照を追加、更新、または削除](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)する」を参照してください。  
  
7. これで、クライアントから WCF サービスを呼び出すことができます。 WCF サービスを呼び出すには、そのサービスのチャネル ファクトリを作成し、チャネルを要求して、そのチャネルで必要なメソッドを直接呼び出します。 この方法が可能なのは、チャネルがサービスのインターフェイスを実装し、基になる要求/応答のロジックを処理するためです。 このメソッドの呼び出しからの戻り値は、サーバーの応答の逆シリアル化されたコピーです。  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   Customer customer = service.GetCustomer(42);  
   Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
   ```  
  
 WCF によってサーバーからクライアントに返されるオブジェクトは、常に値渡しです。 オブジェクトは、サーバーから送信されたデータの逆シリアル化されたコピーです。 クライアントは、コールバックによってサーバー コードを呼び出すリスクを冒すことなく、これらのローカル コピーでメソッドを呼び出すことができます。  
  
#### <a name="scenario-2-server-returns-an-object-by-reference"></a>シナリオ 2: サーバーから参照渡しでオブジェクトを返す  
 このシナリオでは、参照渡しでクライアントにオブジェクトを提供するサーバーについて説明します。 .NET リモート処理では、参照渡しでシリアル化された MarshalByRefObject から派生したすべての型に対してこの処理が自動的に行われます。 このシナリオの例としては、たとえば複数のクライアントが、独立したセッションフルなサーバー側オブジェクトを持てるようにします。 前述したように、WCF サービスによって返されるオブジェクトは常に値渡しであるため、参照渡しのオブジェクトに直接相当するものはありませんが、<xref:System.ServiceModel.EndpointAddress10> オブジェクトを使用して参照渡しセマンティクスに類似するものを実現することは可能です。 これは、サーバー上のセッションフルな参照渡しのオブジェクトを取得するためにクライアントが使用できる、シリアル化可能な値渡しのオブジェクトです。 これによって、独立したセッションフルなサーバー側オブジェクトを持つ複数のクライアントを用意するというシナリオが可能になります。  
  
1. まずは、セッションフル オブジェクト自体に対応する WCF サービス コントラクトを定義する必要があります。  
  
   ```csharp
   [ServiceContract(SessionMode = SessionMode.Allowed)]  
       public interface ISessionBoundObject  
       {  
           [OperationContract]  
           string GetCurrentValue();  
  
           [OperationContract]  
           void SetCurrentValue(string value);  
       }  
   ```  
  
    > [!TIP]
    > セッションフル オブジェクトは [ServiceContract] でマークされ、通常の WCF サービス インターフェイスになっていることに注意してください。 SessionMode プロパティを設定することは、それがセッションフル サービスになることを意味します。 WCF では、セッションは 2 つのエンドポイント間で送信される複数のメッセージを関連付ける方法です。 つまり、クライアントがこのサービスへの接続を確保すると、クライアントとサーバーの間でセッションが確立されます。 クライアントは、サーバー側オブジェクトの単一で一意のインスタンスを、この 1 つのセッション内でのすべてのやり取りに使用することになります。  
  
2. 次に、このサービス インターフェイスの実装を提供する必要があります。 [ServiceBehavior] でこれを示し、InstanceContextMode を設定することによって、この型の一意のインスタンスを各セッションに使用することを WCF に伝えます。  
  
   ```csharp
   [ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]  
       public class MySessionBoundObject : ISessionBoundObject  
       {  
           private string _value;  
  
           public string GetCurrentValue()  
           {  
               return _value;  
           }  
  
           public void SetCurrentValue(string val)  
           {  
               _value = val;  
           }  
  
       }  
   ```  
  
3. ここで、このセッションフル オブジェクトのインスタンスを入手する方法が必要です。 これは、EndpointAddress10 オブジェクトを返すもう 1 つの WCF サービス インターフェイスを作成することによって行います。 これはクライアントがセッションフル オブジェクトの作成に使用できる、エンドポイントのシリアル化可能な形式です。  
  
   ```csharp
   [ServiceContract]  
       public interface ISessionBoundFactory  
       {  
           [OperationContract]  
           EndpointAddress10 GetInstanceAddress();  
       }  
   ```  
  
     さらに、この WCF サービスを実装します。  
  
   ```csharp
   public class SessionBoundFactory : ISessionBoundFactory  
       {  
           public static ChannelFactory<ISessionBoundObject> _factory =   
               new ChannelFactory<ISessionBoundObject>("sessionbound");  
 
           public SessionBoundFactory()  
           {  
           }  
  
           public EndpointAddress10 GetInstanceAddress()  
           {  
               IClientChannel channel = (IClientChannel)_factory.CreateChannel();  
               return EndpointAddress10.FromEndpointAddress(channel.RemoteAddress);  
           }  
       }  
   ```  
  
     この実装では、セッションフル オブジェクトを作成するためにシングルトン チャネル ファクトリを保持しています。 チャネル ファクトリは、GetInstanceAddress() が呼び出されるとチャネルを作成し、このチャネルに関連付けられているリモート アドレスを効率的にポイントする EndpointAddress10 オブジェクトを作成します。 EndpointAddress10 は、単に値渡しでクライアントに返すことのできるデータ型です。  
  
4. 以下の例に示すように、次の 2 つの処理を実行して、サーバーの構成ファイルを変更する必要があります。  
  
    1. セッションフルオブジェクトのエンドポイントを記述する \<クライアント > セクションを宣言します。 この処理が必要な理由は、この状況ではサーバーがクライアントとしても機能するためです。  
  
    2. ファクトリおよびセッションフル オブジェクトのエンドポイントを宣言します。 この処理は、クライアントがサービス エンドポイントと通信して EndpointAddress10 の取得とセッションフル チャネルの作成を実行できるようにするために必要です。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
         <client>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
        </client>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
          <service name="Server.MySessionBoundObject">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundObject"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundObject" />  
          </service>  
          <service name="Server.SessionBoundFactory">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundFactory"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundFactory" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     これで、次のサービスを開始できます。  
  
   ```csharp
   ServiceHost factoryHost = new ServiceHost(typeof(SessionBoundFactory));  
   factoryHost.Open();  
  
   ServiceHost sessionHost = new ServiceHost(typeof(MySessionBoundObject));  
   sessionHost.Open();  
   ```  
  
5. クライアントの構成は、そのプロジェクトの app.config ファイル内でこれらの同じエンドポイントを宣言することによって行います。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
          <endpoint name="factory"  
                    address="net.tcp://localhost:8081/SessionBoundFactory"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundFactory"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
6. セッションフル オブジェクトを作成して使用するために、クライアントでは、次の手順を実行する必要があります。  
  
    1. ISessionBoundFactory サービスへのチャネルを作成する。  
  
    2. このチャネルを使用してサービスを呼び出し、EndpointAddress10 を取得する。  
  
    3. EndpointAddress10 を使用してチャネルを作成し、セッションフル オブジェクトを取得する。  
  
    4. セッションフル オブジェクトと対話し、複数の呼び出し間でインスタンスが同じであることを示す。  
  
   ```csharp
   ChannelFactory<ISessionBoundFactory> channelFactory =   
       new ChannelFactory<ISessionBoundFactory>("factory");  
   ISessionBoundFactory sessionFactory = channelFactory.CreateChannel();  
  
   EndpointAddress10 address1 = sessionFactory.GetInstanceAddress();  
   EndpointAddress10 address2 = sessionFactory.GetInstanceAddress();  
  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory1 =   
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),   
                                               address1.ToEndpointAddress());  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory2 =   
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),   
                                               address2.ToEndpointAddress());  
  
   ISessionBoundObject sessionInstance1 = sessionObjectFactory1.CreateChannel();  
   ISessionBoundObject sessionInstance2 = sessionObjectFactory2.CreateChannel();  
  
   sessionInstance1.SetCurrentValue("Hello");  
   sessionInstance2.SetCurrentValue("World");  
  
   if (sessionInstance1.GetCurrentValue() == "Hello" &&  
       sessionInstance2.GetCurrentValue() == "World")  
   {  
       Console.WriteLine("sessionful server object works as expected");  
   }  
   ```  
  
 WCF は常に値渡しでオブジェクトを返しますが、EndpointAddress10 を使用することで、参照渡しのセマンティクスに相当するものをサポートすることは可能です。 これによって、クライアントからセッションフル WCF サービス インスタンスを要求できるようになり、要求後は他のすべての WCF サービスのように対話ができます。  
  
#### <a name="scenario-3-client-sends-server-a-by-value-instance"></a>シナリオ 3: クライアントから値渡しのインスタンスをサーバーに送信する  
 このシナリオでは、サーバーに値渡しで非プリミティブ オブジェクト インスタンスを送信するクライアントについて説明します。 WCF は値渡しでのみオブジェクトを送信するため、このシナリオでは通常の WCF の使用方法を示します。  
  
1. シナリオ 1 と同じ WCF サービスを使用します。  
  
2. クライアントを使用して新しい値渡しのオブジェクト (Customer) を作成し、チャネルを作成して ICustomerService サービスと通信し、それにオブジェクトを送信します。  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   PremiumCustomer customer = new PremiumCustomer {   
   FirstName = "Bob",   
   LastName = "Jones",   
   CustomerId = 43,   
   AccountId = 99};  
   bool success = service.UpdateCustomer(customer);  
   Console.WriteLine($"  Server returned {success}.");
   ```  
  
     Customer オブジェクトはシリアル化され、サーバーに送信されて、そのオブジェクトの新しいコピーへと逆シリアル化されます。  
  
    > [!NOTE]
    > このコードでは、派生型 (PremiumCustomer) の送信についても示しています。 サービス インターフェイスは Customer オブジェクトを要求していますが、Customer クラスの [KnownType] 属性は、PremiumCustomer も同様に許可されていることを示していました。 このサービス インターフェイスによって他の型をシリアル化または逆シリアル化しようとしても、WCF は必ず失敗します。  
  
 WCF での通常のデータ交換は、値渡しで行われます。 これにより、こういったデータ オブジェクトの 1 つでのメソッド呼び出しはローカルでのみ実行され、他の階層ではコードが呼び出されないことが保証されています。 サーバー*から*返される参照渡しのオブジェクトのような処理を実行することは可能ですが、クライアントが参照渡しのオブジェクトをサーバー*に*渡すことはできません。 クライアントとサーバーの間で対話を必要とするシナリオは、WCF で双方向サービスを使用することによって実現できます。 詳細については、「[双方向サービス](./feature-details/duplex-services.md)」を参照してください。  
  
## <a name="summary"></a>要約  
 .NET リモート処理は、完全に信頼できる環境内のみでの使用を意図した通信フレームワークです。 これはレガシー製品であり、旧バージョンとの互換性のためだけにサポートされています。 新しいアプリケーションの構築には使用できません。 反対に、WCF はセキュリティを考慮して設計されており、新規および既存のアプリケーション向けに推奨されています。 既存のリモート処理アプリケーションから、WCF または ASP.NET Web API を使用したアプリケーションに移行することをお勧めします。
