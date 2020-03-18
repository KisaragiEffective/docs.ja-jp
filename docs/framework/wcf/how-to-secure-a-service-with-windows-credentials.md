---
title: '方法 : Windows 資格情報でサービスをセキュリティで保護する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, security
ms.assetid: d171b5ca-96ef-47ff-800c-c138023cf76e
ms.openlocfilehash: d02e697b23b6c745a59f3c9c37dd9c565f2f710e
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72320922"
---
# <a name="how-to-secure-a-service-with-windows-credentials"></a>方法 : Windows 資格情報でサービスをセキュリティで保護する

このトピックでは、Windows ドメインに存在し、同じドメイン内のクライアントによって呼び出される Windows Communication Foundation (WCF) サービスでトランスポートセキュリティを有効にする方法について説明します。 このシナリオの詳細については、「 [Windows 認証を使用したトランスポートセキュリティ](./feature-details/transport-security-with-windows-authentication.md)」を参照してください。 サンプルアプリケーションについては、 [WSHttpBinding](./samples/wshttpbinding.md)サンプルを参照してください。

このトピックでは、定義済みのコントラクト インターフェイスと実装が既に存在するものとして、それに機能を追加していきます。 既存のサービスとクライアントを変更することもできます。

Windows 資格情報によるサービスのセキュリティ保護は、完全にコードで実現できます。 または、構成ファイルを使用して、一部のコードを省略することもできます。 このトピックでは両方の方法について説明します。 ただし、どちらか 1 つの方法だけを使うようにして、両方は使用しないでください。

最初の 3 つの手順は、コードを使用してサービスをセキュリティで保護する方法について示しています。 4 番目と 5 番目の手順は、構成ファイルを使用して同様の操作を行う方法について示しています。

## <a name="using-code"></a>コードの使用

サービスとクライアントの完全なコードは、このトピックの最後にある「使用例」のセクションに記載されています。

最初の手順では、コードで <xref:System.ServiceModel.WSHttpBinding> クラスを作成および構成する方法について説明します。 バインディングでは HTTP トランスポートを使用します。 同じバインディングがクライアント上で使用されます。

#### <a name="to-create-a-wshttpbinding-that-uses-windows-credentials-and-message-security"></a>Windows 資格情報とメッセージ セキュリティを使用する WSHttpBinding を作成するには

1. この手順のコードは、「使用例」のセクションに記載されたサービス コードの `Run` クラスの `Test` メソッドの先頭に挿入されています。

2. <xref:System.ServiceModel.WSHttpBinding> クラスのインスタンスを作成します。

3. <xref:System.ServiceModel.WSHttpSecurity.Mode%2A> クラスの <xref:System.ServiceModel.WSHttpSecurity> プロパティを <xref:System.ServiceModel.SecurityMode.Message> に設定します。

4. <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> クラスの <xref:System.ServiceModel.MessageSecurityOverHttp> プロパティを <xref:System.ServiceModel.MessageCredentialType.Windows> に設定します。

5. この手順で使用するコードは、次のようになります。

    [!code-csharp[c_SecureWindowsService#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#1)]
    [!code-vb[c_SecureWindowsService#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#1)]

### <a name="using-the-binding-in-a-service"></a>サービスでのバインディングの使用

この 2 番目の手順では、自己ホスト型サービスでバインディングを使用する方法を示します。 ホスティングサービスの詳細については、「[ホスティングサービス](hosting-services.md)」を参照してください。

##### <a name="to-use-a-binding-in-a-service"></a>サービスでバインディングを使用するには

1. 前の手順のコードの後に、この手順のコードを挿入します。

2. <xref:System.Type> という名前の `contractType` 変数を作成し、その変数にインターフェイスの型 (`ICalculator`) を割り当てます。 Visual Basic を使用する場合は、`GetType` 演算子を使用します。を使用C#する場合は、`typeof` キーワードを使用します。

3. <xref:System.Type> という名前の 2 つ目の `serviceType` 変数を作成し、その変数に実装されたコントラクトの型 (`Calculator`) を割り当てます。

4. <xref:System.Uri> という名前で、サービスのベース アドレスが指定された `baseAddress` クラスのインスタンスを作成します。 ベース アドレスには、トランスポートに一致するスキームを指定する必要があります。 この場合、トランスポートスキームは HTTP であり、アドレスには、特別な Uniform Resource Identifier (URI) "localhost" とポート番号 (8036)、およびベースエンドポイントアドレス ("serviceModelSamples/): `http://localhost:8036/serviceModelSamples/`が含まれます。

5. <xref:System.ServiceModel.ServiceHost> 変数と `serviceType` 変数を指定して、`baseAddress` クラスのインスタンスを作成します。

6. `contractType`、バインディング、およびエンドポイント名 (secureCalculator) を使用して、サービスにエンドポイントを追加します。 クライアントは、サービスへの呼び出しを開始するときに、ベース アドレスとエンドポイント名を連結する必要があります。

7. <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> メソッドを呼び出してサービスを起動します。 この手順で使用するコードは次のとおりです。

    [!code-csharp[c_SecureWindowsService#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#2)]
    [!code-vb[c_SecureWindowsService#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#2)]

### <a name="using-the-binding-in-a-client"></a>クライアントでのバインディングの使用

この手順では、サービスと通信するプロキシの生成方法を示します。 プロキシは、サービスメタデータを使用してプロキシを作成する[ServiceModel Metadata Utility Tool (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して生成されます。

この手順では、サービスと通信するための <xref:System.ServiceModel.WSHttpBinding> クラスのインスタンスも作成され、サービスが呼び出されます。

この例では、コードだけを使用してクライアントを作成します。 この手順の後のセクションに示す構成ファイルを使用することもできます。

#### <a name="to-use-a-binding-in-a-client-with-code"></a>コードによってクライアントでバインディングを使用するには

1. SvcUtil.exe ツールを使用して、サービスのメタデータからプロキシ コードを生成します。 詳細については、「[方法: クライアントを作成](how-to-create-a-wcf-client.md)する」を参照してください。 生成されたプロキシコードは <xref:System.ServiceModel.ClientBase%601> クラスから継承されます。これにより、すべてのクライアントに、WCF サービスと通信するために必要なコンストラクター、メソッド、およびプロパティが確実に与えられます。 この例では、生成されたコードに、`CalculatorClient` インターフェイスを実装した `ICalculator` クラスが追加されるので、サービス コードとの互換が可能になります。

2. この手順のコードは、クライアント プログラムの `Main` メソッドの先頭に挿入します。

3. <xref:System.ServiceModel.WSHttpBinding> クラスのインスタンスを作成し、そのセキュリティ モードを `Message` に、そのクライアント資格情報の種類を `Windows` に設定します。 この例では、変数に `clientBinding` という名前を付けます。

4. <xref:System.ServiceModel.EndpointAddress> という名前の `serviceAddress` クラスのインスタンスを作成します。 エンドポイント名が連結されたベース アドレスでインスタンスを初期化します。

5. `serviceAddress` 変数と `clientBinding` 変数を指定して、生成されたクライアント クラスのインスタンスを作成します。

6. 次のコードに示すように、<xref:System.ServiceModel.ClientBase%601.Open%2A> メソッドを呼び出します。

7. サービスを呼び出し、結果を表示します。

    [!code-csharp[c_secureWindowsClient#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#1)]
    [!code-vb[c_secureWindowsClient#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#1)]

## <a name="using-the-configuration-file"></a>構成ファイルの使用

手順コードを使用してバインディングを作成する代わりに、構成ファイルのバインディング セクションに次のコードを記述することもできます。

サービスがまだ定義されていない場合は、「[サービスの設計と実装](designing-and-implementing-services.md)」および「[サービスの構成](configuring-services.md)」を参照してください。

> [!NOTE]
> この構成コードは、サービスとクライアントの両方の構成ファイルで使用されます。

#### <a name="to-enable-transfer-security-on-a-service-in-a-windows-domain-using-configuration"></a>構成を使用して Windows ドメインのサービスで転送セキュリティを有効にするには

1. 構成ファイルの[\<binding >](../configure-apps/file-schema/wcf/bindings.md) element セクションに[\<wsHttpBinding >](../configure-apps/file-schema/wcf/wshttpbinding.md)要素を追加します。

2. <`WSHttpBinding`> 要素に <`binding`> 要素を追加し、`configurationName` 属性をアプリケーションに適した値に設定します。

3. > 要素`security`< を追加し、`mode` 属性を Message に設定します。

4. > 要素`message`< を追加し、`clientCredentialType` 属性を Windows に設定します。

5. サービスの構成ファイルで、`<bindings>` セクションを次のコードに置き換えます。 サービス構成ファイルがまだない場合は、「Using binding [To Configure service and](using-bindings-to-configure-services-and-clients.md)client」を参照してください。

    ```xml
    <bindings>
      <wsHttpBinding>
       <binding name = "wsHttpBinding_Calculator">
         <security mode="Message">
           <message clientCredentialType="Windows"/>
         </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

### <a name="using-the-binding-in-a-client"></a>クライアントでのバインディングの使用

この手順では、サービスと通信するプロキシと構成ファイルの 2 つのファイルの生成方法を示します。 また、クライアント上で使用される 3 つ目のファイルであるクライアント プログラムへの変更点についても説明します。

#### <a name="to-use-a-binding-in-a-client-with-configuration"></a>構成によってクライアントでバインディングを使用するには

1. SvcUtil.exe ツールを使用して、サービスのメタデータからプロキシ コードと構成ファイルを生成します。 詳細については、「[方法: クライアントを作成](how-to-create-a-wcf-client.md)する」を参照してください。

2. 生成された構成ファイルの[\<バインド >](../configure-apps/file-schema/wcf/bindings.md)セクションを、前のセクションの構成コードに置き換えます。

3. 手順コードは、クライアント プログラムの `Main` メソッドの先頭に挿入します。

4. 生成されたクライアント クラスのインスタンスを作成します。このとき、構成ファイルのバインディングの名前を入力パラメーターとして渡します。

5. 次のコードに示すように、<xref:System.ServiceModel.ClientBase%601.Open%2A> メソッドを呼び出します。

6. サービスを呼び出し、結果を表示します。

    [!code-csharp[c_secureWindowsClient#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#2)]

## <a name="example"></a>例

[!code-csharp[c_SecureWindowsService#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#0)]

[!code-csharp[c_SecureWindowsClient#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#0)]
[!code-vb[c_SecureWindowsClient#0](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#0)]

## <a name="see-also"></a>参照

- <xref:System.ServiceModel.WSHttpBinding>
- [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [方法: クライアントを作成する](how-to-create-a-wcf-client.md)
- [サービスのセキュリティ保護](securing-services.md)
- [セキュリティの概要](./feature-details/security-overview.md)
