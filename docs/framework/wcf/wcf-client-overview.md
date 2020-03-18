---
title: WCF クライアントの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], architecture
ms.assetid: f60d9bc5-8ade-4471-8ecf-5a07a936c82d
ms.openlocfilehash: 180de3f571426441155a19b98ab750fcdbb3888e
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73420658"
---
# <a name="wcf-client-overview"></a>WCF クライアントの概要
このセクションでは、クライアントアプリケーションの動作、Windows Communication Foundation (WCF) クライアントを構成、作成、および使用する方法、およびクライアントアプリケーションをセキュリティで保護する方法について説明します。  
  
## <a name="using-wcf-client-objects"></a>WCF クライアント オブジェクトの使用  
 クライアントアプリケーションは、WCF クライアントを使用して別のアプリケーションと通信するマネージアプリケーションです。 WCF サービスのクライアントアプリケーションを作成するには、次の手順を実行する必要があります。  
  
1. サービス エンドポイントのサービス コントラクト、バインディング、およびアドレス情報を取得します。  
  
2. その情報を使用して WCF クライアントを作成します。  
  
3. 操作を呼び出します。  
  
4. WCF クライアントオブジェクトを閉じます。  
  
 この後の各セクションでは、これらの手順について詳しく説明します。また、次の内容についても簡単に説明します。  
  
- エラー処理  
  
- クライアントの構成とセキュリティ保護  
  
- 双方向サービスのコールバック オブジェクトの作成  
  
- サービスの非同期呼び出し  
  
- クライアント チャネルを使用したサービスの呼び出し  
  
## <a name="obtain-the-service-contract-bindings-and-addresses"></a>サービス コントラクト、バインディング、およびアドレスを取得する  
 WCF では、サービスとクライアントは、マネージ属性、インターフェイス、およびメソッドを使用してコントラクトをモデル化します。 クライアント アプリケーションからサービスに接続するには、そのサービス コントラクトの型情報を取得する必要があります。 通常は、 [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して、サービスからメタデータをダウンロードし、それを任意の言語でマネージソースコードファイルに変換し、クライアントアプリケーション構成ファイルを作成します。WCF クライアントオブジェクトを構成するために使用できます。 たとえば、`MyCalculatorService`を呼び出す WCF クライアントオブジェクトを作成し、そのサービスのメタデータが `http://computerName/MyCalculatorService/Service.svc?wsdl`で公開されていることがわかっている場合は、次のコード例では、Svcutil.exe を使用してサービスを含む `ClientCode.vb` ファイルを取得する方法を示します。 マネージコード内のコントラクト。  
  
```console  
svcutil /language:vb /out:ClientCode.vb /config:app.config http://computerName/MyCalculatorService/Service.svc?wsdl  
```  
  
 このコントラクトコードをクライアントアプリケーションにコンパイルするか、クライアントアプリケーションが WCF クライアントオブジェクトを作成するために使用できる別のアセンブリにコンパイルすることができます。 構成ファイルを使用してサービスに正しく接続するクライアント オブジェクトを構成できます。  
  
 このプロセスの例については、「[方法: クライアントを作成](how-to-create-a-wcf-client.md)する」を参照してください。 コントラクトの詳細については、「[コントラクト](./feature-details/contracts.md)」を参照してください。  
  
## <a name="create-a-wcf-client-object"></a>WCF クライアント オブジェクトを作成する  
 WCF クライアントは、クライアントがリモートサービスとの通信に使用できる形式で WCF サービスを表すローカルオブジェクトです。 WCF クライアント型はターゲットサービスコントラクトを実装するので、作成して構成するときに、クライアントオブジェクトを直接使用してサービス操作を呼び出すことができます。 WCF ランタイムは、メソッド呼び出しをメッセージに変換し、サービスに送信し、応答をリッスンして、これらの値を、戻り値として、または `out` または `ref` のパラメーターとして WCF クライアントオブジェクトに返します。  
  
 WCF クライアントチャネルオブジェクトを使用して、サービスに接続し、サービスを使用することもできます。 詳細については、「 [WCF クライアントアーキテクチャ](./feature-details/client-architecture.md)」を参照してください。  
  
#### <a name="creating-a-new-wcf-object"></a>新しい WCF オブジェクトの作成  
 サービスアプリケーションから次の簡単なサービス コントラクトが生成されていることを前提に、<xref:System.ServiceModel.ClientBase%601> クラスの使用方法を説明します。  
  
> [!NOTE]
> Visual Studio を使用して WCF クライアントを作成する場合、プロジェクトにサービス参照を追加すると、オブジェクトが自動的にオブジェクトブラウザーに読み込まれます。  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 Visual Studio を使用していない場合は、生成されたコントラクトコードを調べて、<xref:System.ServiceModel.ClientBase%601> およびサービスコントラクトインターフェイス `ISampleService`を拡張する型を見つけます。 この場合、この型は次のようなコードになります。  
  
 [!code-csharp[C_GeneratedCodeFiles#14](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#14)]  
  
 このクラスを、コンストラクターの 1 つを使用してローカル オブジェクトとして作成し、構成して、型 `ISampleService` のサービスへの接続に使用できます。  
  
 まず、WCF クライアントオブジェクトを作成し、それを使用して単一の try/catch ブロック内で閉じることをお勧めします。 特定のエラーモードで例外がマスクされる可能性があるため、`using` ステートメント (Visual Basic の `Using`) は使用しないでください。 詳細については、次のセクションを参照してください。[また、Close と Abort を使用して WCF クライアントリソースを解放](./samples/use-close-abort-release-wcf-client-resources.md)してください。  
  
### <a name="contracts-bindings-and-addresses"></a>コントラクト、バインディング、およびアドレス  
 WCF クライアントオブジェクトを作成する前に、クライアントオブジェクトを構成する必要があります。 具体的には、使用するサービス*エンドポイント*が必要です。 エンドポイントは、サービス コントラクト、バインディング、およびアドレスの組み合わせです (エンドポイントの詳細については、「[エンドポイント: アドレス、バインディング、およびコントラクト](./feature-details/endpoints-addresses-bindings-and-contracts.md)」を参照してください)。通常、この情報は、クライアントアプリケーション構成ファイルの[\<endpoint >](../configure-apps/file-schema/wcf/endpoint-of-client.md)要素 (svcutil.exe ツールによって生成されるものなど) にあり、クライアントオブジェクトの作成時に自動的に読み込まれます。 どちらの WCF クライアントタイプにも、この情報をプログラムで指定できるようにするオーバーロードがあります。  
  
 たとえば、上記の例で使用した `ISampleService` 用に生成された構成ファイルには、次のエンドポイント情報が含まれます。  
  
 [!code-xml[C_GeneratedCodeFiles#19](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/common/client.exe.config#19)]  
  
 この構成ファイルの `<client>` 要素には、ターゲット エンドポイントが指定されます。 複数のターゲットエンドポイントの使用の詳細については、<xref:System.ServiceModel.ClientBase%601.%23ctor%2A?displayProperty=nameWithType> または <xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A?displayProperty=nameWithType> コンストラクターを参照してください。  
  
## <a name="calling-operations"></a>操作の呼び出し  
 クライアントオブジェクトの作成と構成が完了したら、try/catch ブロックを作成し、オブジェクトがローカルである場合と同じ方法で操作を呼び出して、WCF クライアントオブジェクトを閉じます。 クライアントアプリケーションが最初の操作を呼び出すと、WCF によって基になるチャネルが自動的に開き、オブジェクトがリサイクルされるときに基になるチャネルが閉じられます。 (また、他の操作を呼び出す前後にチャネルを明示的に開いたり閉じたりすることもできます)。  
  
 たとえば、次のようなサービス コントラクトを使用する場合があります。  
  
```csharp  
namespace Microsoft.ServiceModel.Samples  
{  
    using System;  
    using System.ServiceModel;  
  
    [ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
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
}  
```  
  
```vb  
Namespace Microsoft.ServiceModel.Samples  
  
    Imports System  
    Imports System.ServiceModel  
  
    <ServiceContract(Namespace:= _  
    "http://Microsoft.ServiceModel.Samples")> _   
   Public Interface ICalculator  
        <OperationContract> _   
        Function Add(n1 As Double, n2 As Double) As Double  
        <OperationContract> _   
        Function Subtract(n1 As Double, n2 As Double) As Double  
        <OperationContract> _   
        Function Multiply(n1 As Double, n2 As Double) As Double  
        <OperationContract> _   
     Function Divide(n1 As Double, n2 As Double) As Double  
End Interface  
```  
  
 次のコード例に示すように、WCF クライアントオブジェクトを作成し、そのメソッドを呼び出すことによって、操作を呼び出すことができます。 WCF クライアントオブジェクトのオープン、呼び出し、および終了は、単一の try/catch ブロック内で行われることに注意してください。 詳細については、「 [Wcf クライアントを使用したサービスへのアクセス](./feature-details/accessing-services-using-a-client.md)」および「 [Close と Abort を使用して wcf クライアントリソースを解放する](./samples/use-close-abort-release-wcf-client-resources.md)」を参照してください。  
  
 [!code-csharp[C_GeneratedCodeFiles#20](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#20)]  
  
## <a name="handling-errors"></a>エラー処理  
 基になるクライアント チャネルを開いたとき (明示的に開いた場合、または操作を呼び出すことによって自動的に開いた場合)、クライアントまたはチャネル オブジェクトを使用して操作を呼び出したとき、基になるクライアント チャネルを閉じたときときに、クライアント アプリケーションで例外が発生する可能性があります。 少なくともアプリケーションでは、操作から返される SOAP エラーの結果としてスローされる <xref:System.TimeoutException?displayProperty=nameWithType> オブジェクトに加え、可能性のある <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType> 例外と <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> 例外を処理することをお勧めします。 操作コントラクトで指定されている SOAP エラーは、<xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> としてクライアント アプリケーションに送信されます。ここで、型パラメーターは SOAP エラーの詳細な型です。 クライアントアプリケーションでのエラー条件の処理の詳細については、「エラーの[送受信](sending-and-receiving-faults.md)」を参照してください。 クライアントでのエラーの処理方法を示す完全なサンプルについては、「[予期される例外](./samples/expected-exceptions.md)」を参照してください。  
  
## <a name="configuring-and-securing-clients"></a>クライアントの構成とセキュリティ保護  
 クライアントを構成するには、まず、そのクライアントまたはチャネル オブジェクトに必要なターゲット エンドポイント情報を読み込みます。通常は構成ファイルから読み込みますが、クライアント コンストラクターとプロパティを使用してプログラムで読み込むこともできます。 ただし、特定のクライアントの動作を有効にし、多くのセキュリティ シナリオに対応するには、追加の構成手順が必要です。  
  
 たとえば、サービス コントラクトのセキュリティ要件はサービス コントラクト インターフェイスに宣言します。Svcutil.exe で構成ファイルを作成した場合、通常そのファイルにはサービスのセキュリティ要件に対応できるバインディングが含まれています。 ただし、クライアント資格情報の構成など、さらに多くのセキュリティ構成が必要な場合もあります。 WCF クライアントのセキュリティ構成の詳細については、「[クライアント](securing-clients.md)のセキュリティ保護」を参照してください。  
  
 また、カスタム ランタイム動作など、クライアント アプリケーションでいくつかのカスタム変更を有効にすることもできます。 カスタムクライアント動作を構成する方法の詳細については、「[クライアント動作の構成](configuring-client-behaviors.md)」を参照してください。  
  
## <a name="creating-callback-objects-for-duplex-services"></a>双方向サービスのコールバック オブジェクトの作成  
 双方向サービスには、コントラクトの要件に従って呼び出すサービスのコールバック オブジェクトを提供するために、クライアント アプリケーションが実装する必要のあるコールバック コントラクトを指定します。 コールバック オブジェクトは完全なサービスではありません (たとえば、コールバック オブジェクトを使用してチャネルを初期化できません) が、実装と構成という目的においては、一種のサービスとして考えることができます。  
  
 双方向サービスのクライアントでは、以下の処理を行う必要があります。  
  
- コールバック コントラクト クラスを実装します。  
  
- コールバックコントラクト実装クラスのインスタンスを作成し、それを使用して、WCF クライアントコンストラクターに渡す <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType> オブジェクトを作成します。  
  
- 操作を呼び出し、操作のコールバックを処理します。  
  
 双方向 WCF クライアントオブジェクトは、対応していない双方向のクライアントオブジェクトと同様に機能します。ただし、コールバックサービスの構成など、コールバックをサポートするために必要な機能が公開されます。  
  
 たとえば、コールバック クラスの <xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=nameWithType> 属性のプロパティを使用して、コールバック オブジェクトの実行時の動作のさまざまな局面を制御できます。 また、別の例として、<xref:System.ServiceModel.Description.CallbackDebugBehavior?displayProperty=nameWithType> クラスを使用して、例外情報をコールバック オブジェクトを呼び出したサービスに返すこともできます。 詳細については、「[双方向サービス](./feature-details/duplex-services.md)」を参照してください。 完全なサンプルについては、「[二重](./samples/duplex.md)」を参照してください。  
  
 インターネット インフォメーション サービス (IIS) 5.1 を実行する Windows XP コンピューターの場合、双方向クライアントでは <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> クラスを使用してクライアントのベース アドレスを指定する必要があります。そうしない場合は例外がスローされます。 次のコード例は、コードでこれを指定する方法を示します。  
  
 [!code-csharp[S_DualHttp#8](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#8)]
 [!code-vb[S_DualHttp#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_dualhttp/vb/module1.vb#8)]  
  
 次のコードは、構成ファイルでこれを指定する方法を示します。  
  
 [!code-csharp[S_DualHttp#134](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#134)]  
  
## <a name="calling-services-asynchronously"></a>サービスの非同期呼び出し  
 操作の呼び出し方法は、クライアント開発者に完全に依存します。 これは、操作を構成するメッセージは、マネージド コードで表現するときに同期メソッドまたは非同期メソッドのどちらかにマップできるためです。 したがって、操作を非同期に呼び出すクライアントを作成する場合、Svcutil.exe の `/async` オプションを使用して非同期クライアント コードを生成できます。 詳細については、「[方法: サービス操作を非同期に呼び出す](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)」を参照してください。  
  
## <a name="calling-services-using-wcf-client-channels"></a>WCF クライアント チャネルを使用したサービスの呼び出し  
 WCF クライアント型は <xref:System.ServiceModel.ClientBase%601>を拡張し、それ自体が <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> インターフェイスから派生して、基になるチャネルシステムを公開します。 ターゲットのサービス コントラクトと <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> クラスを使用して、サービスを呼び出すことができます。 詳細については、「 [WCF クライアントアーキテクチャ](./feature-details/client-architecture.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>
