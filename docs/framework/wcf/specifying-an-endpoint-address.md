---
title: エンドポイント アドレスの指定
description: WCF での ServiceEndpoint の一部であるエンドポイント アドレスについて説明します。 WCF サービスを使用して行われるすべての通信では、エンドポイントが使用されます。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- endpoints [WCF], addressing
ms.assetid: ac24f5ad-9558-4298-b168-c473c68e819b
ms.openlocfilehash: e1bd9e5a27d1bc86d2d3e04ee82221a27a4e1fa8
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245986"
---
# <a name="specifying-an-endpoint-address"></a>エンドポイント アドレスの指定

Windows Communication Foundation (WCF) サービスとの通信はすべて、そのエンドポイントを通して行われます。 各 <xref:System.ServiceModel.Description.ServiceEndpoint> は、<xref:System.ServiceModel.Description.ServiceEndpoint.Address%2A>、<xref:System.ServiceModel.Description.ServiceEndpoint.Binding%2A>、および <xref:System.ServiceModel.Description.ServiceEndpoint.Contract%2A> で構成されます。 コントラクトでは、使用できる操作を指定します。 バインディングでは、サービスとの通信方法を指定し、アドレスでは、サービスの場所を指定します。 各エンドポイントには、一意のアドレスを設定する必要があります。 エンドポイント アドレスは、<xref:System.ServiceModel.EndpointAddress> クラスによって表します。このクラスは、サービスのアドレスを表す URI (Uniform Resource Identifier)、サービスのセキュリティ ID を表す <xref:System.ServiceModel.EndpointAddress.Identity%2A>、およびオプションの <xref:System.ServiceModel.EndpointAddress.Headers%2A> のコレクションで構成されます。 オプション ヘッダーは、エンドポイントの識別または対話のために、より詳細なアドレス指定情報を提供します。 たとえば、ヘッダーを使用して、受信メッセージの処理方法や、エンドポイントからの応答メッセージの送信先を指定できるほか、複数のサービス インスタンスが使用できる場合に、特定ユーザーからの受信メッセージの処理に使用するインスタンスを指定できます。

## <a name="definition-of-an-endpoint-address"></a>エンドポイント アドレスの定義

WCF の <xref:System.ServiceModel.EndpointAddress> により、WS-Addressing 仕様で定義されているエンドポイント参照 (EPR) がモデル化されます。

ほとんどのトランスポートの URI アドレスは、4 つの部分から構成されます。 たとえば、`http://www.fabrikam.com:322/mathservice.svc/secureEndpoint` という URI には、次の 4 つの部分があります。

- スキーム : http:

- コンピューター: `www.fabrikam.com`

- (省略可能) ポート: 322

- パス : /mathservice.svc/secureEndpoint

EPR モデルの一部では、各エンドポイント参照は、追加の識別情報を追加する複数の参照パラメーターを伝達できます。 WCF では、これらの参照パラメーターは <xref:System.ServiceModel.Channels.AddressHeader> クラスのインスタンスとしてモデル化されます。

サービスのエンドポイント アドレスはコードを使用して強制的に指定するか、構成を介して宣言として指定できます。 設置済みサービスのバインドおよびアドレスは一般的に、サービスの開発中に使用されるものとは異なるので、コード内でエンドポイントを定義することは通常、実用的ではありません。 一般に、サービス エンドポイントの定義にはコードではなく、構成を使用する方がより実用的です。 バインディング情報とアドレス情報をコードに含めないことで、変更時にアプリケーションの再コンパイルや再展開を行う必要がなくなります。 エンドポイントがコードまたは構成で指定されていない場合、ランタイムは、サービスで実装されたコントラクトごとに、1 つの既定のエンドポイントを各ベース アドレスに追加します。

WCF でサービスのエンドポイント アドレスを指定するには 2 つの方法があります。 サービスに関連付けられた各エンドポイントに対して絶対アドレスを指定する方法と、<xref:System.ServiceModel.ServiceHost> のベース アドレスを設定して、このベース アドレスから相対的に定義されるアドレスをサービスに関連付けられた各エンドポイントに対して指定する方法です。 サービスのエンドポイント アドレスを指定するには、構成とコードのいずれかで、これらの各方法を使用します。 相対アドレスを指定しない場合、サービスはベース アドレスを使用します。 サービスに対して複数のベース アドレスを設定することもできますが、サービスが各トランスポートに対して設定できるベース アドレスは 1 つに限られます。 複数のエンドポイントがある場合、各エンドポイントには異なるバインディングで構成されるため、それぞれのアドレスは一意になります。 異なるコントラクトで同じバインディングを使用するエンドポイントは同じアドレスを使用できます。

IIS でホストする場合、ユーザーは <xref:System.ServiceModel.ServiceHost> インスタンスを管理できません。 IIS でホストしているサービスでは、サービスの .svc ファイルで指定されているアドレスが常にベース アドレスになります。 そのため、IIS でホストされるサービスのエンドポイントでは、相対エンドポイント アドレスを使用する必要があります。 完全修飾されたエンドポイント アドレスを指定すると、サービスの展開時にエラーとなる可能性があります。 詳細については、「[インターネット インフォメーション サービスでホストされる WCF サービスの配置](./feature-details/deploying-an-internet-information-services-hosted-wcf-service.md)」を参照してください。

## <a name="defining-endpoint-addresses-in-configuration"></a>構成によるエンドポイント アドレスの定義

構成ファイルでエンドポイントを定義するには、[\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md) 要素を使用します。

[!code-xml[S_UEHelloWorld#5](./snippets/specifying-an-endpoint-address/serviceapp2.config#5)]

<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> メソッドが呼び出されると (このメソッドは、ホスト アプリケーションがサービスを開始しようとしたときに呼び出されます)、システムにより、"UE.Samples.HelloService" を指定する name 属性を持つ [\<service>](../configure-apps/file-schema/wcf/service.md) 要素が検索されます。 その [\<service>](../configure-apps/file-schema/wcf/service.md) 要素が見つかった場合、システムにより、構成ファイルにあるエンドポイント定義を使用して、指定されたクラスが読み込まれ、エンドポイントが作成されます。 このしくみによって、2 行のコードでサービスを読み込んで開始でき、バインディングとアドレス指定情報をコード外に維持することができます。 この方法の利点は、アプリケーションを再度コンパイルしたり、展開したりすることなく、この 2 つの情報を変更できる点です。

オプション ヘッダーは [\<headers>](../configure-apps/file-schema/wcf/headers-element.md) で宣言されています。 次に示す例では、構成ファイルでサービスのエンドポイントを指定するための要素で、2 つのヘッダー (`http://tempuri1.org/` からの "Gold" クライアントと `http://tempuri2.org/` からの "Standard" クライアント) が識別されています。 このサービスを呼び出すクライアントの構成ファイルには、適切な [\<headers>](../configure-apps/file-schema/wcf/headers-element.md) が記述されている必要があります。

[!code-xml[S_UEHelloWorld#1](./snippets/specifying-an-endpoint-address/serviceapp.config#1)]

ヘッダーは、(上記のように) エンドポイントのすべてのメッセージに対してではなく、個別のメッセージに設定できます。 この設定は、次の例で示すように、<xref:System.ServiceModel.OperationContextScope> を使用してクライアント アプリケーションに新しいコンテキストを作成し、送信メッセージにカスタム ヘッダーを追加することで行います。

[!code-csharp[OperationContextScope#4](../../../samples/snippets/csharp/VS_Snippets_CFX/operationcontextscope/cs/client.cs#4)]
[!code-vb[OperationContextScope#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/operationcontextscope/vb/client.vb#4)]

## <a name="endpoint-address-in-metadata"></a>メタデータ内のエンドポイント アドレス

Web サービス記述言語 (WSDL) では、エンドポイント アドレスは、対応するエンドポイントの `EndpointReference` 要素内で WS-Addressing の `wsdl:port` (EPR) 要素として表されます。 EPR には、エンドポイントのアドレスのほかに、アドレスのすべてのプロパティが含まれます。 `wsdl:port` 内にある EPR では、次の例に示すように `soap:Address` を置き換えるので注意してください。

## <a name="defining-endpoint-addresses-in-code"></a>コードによるエンドポイント アドレスの定義

エンドポイント アドレスは、コードで <xref:System.ServiceModel.EndpointAddress> クラスを使用して作成できます。 エンドポイント アドレスに指定する URI は、完全修飾パスまたはサービスのベース アドレスを基準にしたパスです。 <xref:System.ServiceModel.EndpointAddress> クラスのインスタンスを作成し、そのインスタンスを、サービスをホストする <xref:System.ServiceModel.ServiceHost> インスタンスに追加する方法を次のコードに示します。

次の例は、コードで完全なエンドポイント アドレスを指定する方法を示しています。

[!code-csharp[S_UEHelloWorld#2](../../../samples/snippets/csharp/VS_Snippets_CFX/s_uehelloworld/cs/snippet.cs#2)]

次の例は、サービス ホストのベース アドレスに相対アドレス ("MyService") を追加する方法を示しています。

[!code-csharp[S_UEHelloWorld#3](../../../samples/snippets/csharp/VS_Snippets_CFX/s_uehelloworld/cs/snippet.cs#3)]

> [!NOTE]
> <xref:System.ServiceModel.Description.ServiceDescription> の <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A> メソッドの後で、サービス アプリケーションの <xref:System.ServiceModel.ServiceHostBase> の各プロパティを変更しないでください。 このメソッドの後で変更すると、<xref:System.ServiceModel.ServiceHostBase.Credentials%2A> および `AddServiceEndpoint` の <xref:System.ServiceModel.ServiceHostBase> プロパティや <xref:System.ServiceModel.ServiceHost> メソッドなどの一部のメンバーは例外をスローします。 変更を許可するメンバーもありますが、結果は未定義の状態になります。
>
> 同様に、クライアントで、<xref:System.ServiceModel.Description.ServiceEndpoint> の <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A> 呼び出しの後で、<xref:System.ServiceModel.ChannelFactory> 値を変更しないでください。 この呼び出しの後で変更すると、<xref:System.ServiceModel.ChannelFactory.Credentials%2A> プロパティは例外をスローします。 その他のクライアント記述値は、エラーを発生させずに変更できますが、結果は未定義の状態になります。
>
> サービスとクライアントのどちらの場合も、<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> の呼び出しの前に記述を変更することをお勧めします。

## <a name="using-default-endpoints"></a>既定のエンドポイントの使用

エンドポイントがコードまたは構成で指定されていない場合、ランタイムは、サービスで実装されたサービス コントラクトごとに、1 つの既定のエンドポイントを各ベース アドレスに追加することで、既定のエンドポイントを提供します。 ベース アドレスはコードまたは構成で指定することができ、既定のエンドポイントは、<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> が <xref:System.ServiceModel.ServiceHost> で呼び出されるときに追加されます。

エンドポイントを明示的に指定しない場合、<xref:System.ServiceModel.ServiceHostBase.AddDefaultEndpoints%2A> を呼び出す前に、<xref:System.ServiceModel.ServiceHost> で <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> を呼び出すことによって、既定のエンドポイントを引き続き追加できます。 既定のエンドポイントについては、「[Simplified Configuration](simplified-configuration.md)」 (簡易構成) と「[Simplified Configuration for WCF Services](./samples/simplified-configuration-for-wcf-services.md)」 (WCF サービスの簡易構成) を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.EndpointAddress>
- [サービス ID と認証](./feature-details/service-identity-and-authentication.md)
- [エンドポイントの作成の概要](endpoint-creation-overview.md)
- [ホスティング](./feature-details/hosting.md)
