---
title: サービスのランタイム動作の指定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5c5450ea-6af1-4b75-a267-613d0ac54707
ms.openlocfilehash: ffa2f906ac2ff4630de83938ce365c1b9d5d4d64
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976634"
---
# <a name="specifying-service-run-time-behavior"></a>サービスのランタイム動作の指定
サービス コントラクトを設計して ([Designing Service Contracts](designing-service-contracts.md))、実装 ([Implementing Service Contracts](implementing-service-contracts.md)) が終わると、サービス ランタイムの実行動作を構成できます。 ここでは、システム指定のサービスと操作の動作について説明し、新しい動作を作成するための詳細情報の参照先を示します。 一部の動作は属性として適用されますが、多くの動作はアプリケーション構成ファイルまたはプログラムを使用して適用されます。 サービスアプリケーションの構成の詳細については、「[サービスの構成](configuring-services.md)」を参照してください。  
  
## <a name="overview"></a>概要  
 コントラクトは、その種類のサービスの入力、出力、データ型、および機能を定義します。 サービス コントラクトを実装すると、クラスが 1 つ作成されます。このクラスを任意のアドレスのバインディングで構成すると、実装されたコントラクトが満たされます。 コントラクト、バインディング、およびアドレスはすべて、クライアントにとって既知の情報です。これらの情報がないと、クライアントはサービスを利用できません。  
  
 ただし、実行の詳細 (スレッド処理の問題点やインスタンスの管理など) は、クライアントには見えません。 サービス コントラクトを実装したら、 *動作*を使用してさまざまな操作特性を構成できます。 動作は、ランタイムプロパティを設定するか、またはカスタマイズの種類をランタイムに挿入することによって、Windows Communication Foundation (WCF) ランタイムを変更するオブジェクトです。 ユーザー定義の動作を作成してランタイムを変更する方法の詳細については、「 [ServiceHost とサービスモデルレイヤーの拡張](./extending/extending-servicehost-and-the-service-model-layer.md)」を参照してください。  
  
 <xref:System.ServiceModel.ServiceBehaviorAttribute?displayProperty=nameWithType> と <xref:System.ServiceModel.OperationBehaviorAttribute?displayProperty=nameWithType> 属性は、最も広範に使用できる動作であり、最も一般的に要求される操作機能を公開します。 これらは属性であるため、サービス実装または操作実装に適用します。 その他の動作 ( <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> や <xref:System.ServiceModel.Description.ServiceDebugBehavior?displayProperty=nameWithType>など) は、通常、アプリケーション構成ファイルを使用して適用されますが、プログラムにより使用することもできます。  
  
 このトピックでは、<xref:System.ServiceModel.ServiceBehaviorAttribute> と <xref:System.ServiceModel.OperationBehaviorAttribute> 属性の概要を示し、動作が動作するさまざまなスコープについて説明します。また、WCF 開発者にとって重要な、さまざまなスコープでのシステム指定の動作の多くについて簡単に説明します。  
  
## <a name="servicebehaviorattribute-and-operationbehaviorattribute"></a>ServiceBehaviorAttribute と OperationBehaviorAttribute  
 最も重要な動作は <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性と <xref:System.ServiceModel.OperationBehaviorAttribute> 属性です。これらの属性を使用すると、以下を制御できます。  
  
- インスタンスの有効期間  
  
- コンカレンシーと同期のサポート  
  
- 構成の動作  
  
- トランザクションの動作  
  
- シリアル化の動作  
  
- メタデータの変換  
  
- セッションの有効期間  
  
- アドレス フィルターとヘッダー処理  
  
- 偽装  
  
- これらの属性を使用するには、サービス実装または操作実装をそのスコープに適切な属性でマークし、プロパティを設定します。 たとえば、次のコード例は、 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A?displayProperty=nameWithType> プロパティを使用した操作実装を示しています。この実装では、この操作の呼び出し元が偽装をサポートするように要求します。  
  
 [!code-csharp[OperationBehaviorAttribute_Impersonation#1](../../../samples/snippets/csharp/VS_Snippets_CFX/operationbehaviorattribute_impersonation/cs/services.cs#1)]
 [!code-vb[OperationBehaviorAttribute_Impersonation#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/operationbehaviorattribute_impersonation/vb/services.vb#1)]  
  
 これらのプロパティの多くは、バインディングによる追加のサポートを必要とします。 たとえば、クライアントからのトランザクションを要求する操作は、フロー トランザクションをサポートするバインディングを使用するように構成される必要があります。  
  
### <a name="well-known-singleton-services"></a>既知のシングルトン サービス  
 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性と <xref:System.ServiceModel.OperationBehaviorAttribute> 属性を使用すると、特定の有効期間 ( <xref:System.ServiceModel.InstanceContext> の有効期間と操作を実装するサービス オブジェクトの有効期間の両方) を制御できます。  
  
 たとえば、 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> プロパティは <xref:System.ServiceModel.InstanceContext> の解放頻度を制御し、 <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> プロパティと <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A?displayProperty=nameWithType> プロパティはサービス オブジェクトを解放するタイミングを制御します。  
  
 ただし、サービス オブジェクトを独自に作成し、そのオブジェクトを使用するサービス ホストを作成することもできます。 そのためには、 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> プロパティを <xref:System.ServiceModel.InstanceContextMode.Single> に設定するか、サービス ホストが開かれたときに例外をスローする必要があります。  
  
 このようなサービスを作成するには、 <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Object%2CSystem.Uri%5B%5D%29?displayProperty=nameWithType> コンストラクターを使用します。 この方法は、シングルトン サービスが使用する特定のオブジェクト インスタンスを提供する場合に、カスタムの <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer?displayProperty=nameWithType> を実装する代わりに使用できます。 このオーバーロードは、サービス実装型を構築するのが困難な場合 (たとえば、パラメーターなしのパブリックコンストラクターが実装されていない場合) に使用できます。
  
 このコンストラクターにオブジェクトが提供されている場合は、Windows Communication Foundation (WCF) のインスタンス化動作に関連する一部の機能が異なる動作をすることに注意してください。 たとえば、既知のオブジェクト インスタンスを指定した場合、 <xref:System.ServiceModel.InstanceContext.ReleaseServiceInstance%2A?displayProperty=nameWithType> を呼び出しても効果はありません。 他のインスタンス解放機構も、同様に無視されます。 <xref:System.ServiceModel.ServiceHost> クラスは常に、すべての操作について <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> プロパティが <xref:System.ServiceModel.ReleaseInstanceMode.None?displayProperty=nameWithType> に設定されているかのように動作します。  
  
## <a name="other-service-endpoint-contract-and-operation-behaviors"></a>その他のサービス、エンドポイント、コントラクト、および操作の動作  
 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性などのサービスの動作は、サービス全体に影響します。 たとえば、 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> プロパティを <xref:System.ServiceModel.ConcurrencyMode.Multiple?displayProperty=nameWithType> に設定すると、そのサービスの各操作で発生するスレッドの同期の問題を自分で処理する必要があります。 エンドポイントの動作はエンドポイント全体に影響し、システム指定のエンドポイントの動作はクライアント機能に対して影響します。 また、コントラクトの動作はコントラクト レベルで影響し、操作の動作は操作の実行を変更します。  
  
 これらの動作の多くは属性に実装され、適切なサービス クラスまたは操作実装に適用することによって <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性および <xref:System.ServiceModel.OperationBehaviorAttribute> 属性を使用するときにこれらの動作を利用できます。 その他の動作 ( <xref:System.ServiceModel.Description.ServiceMetadataBehavior> オブジェクトや <xref:System.ServiceModel.Description.ServiceDebugBehavior> オブジェクトなど) は、通常、アプリケーション構成ファイルを使用して適用されますが、プログラムにより使用することもできます。  
  
 たとえば、メタデータの公開は、 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> オブジェクトを使用して構成されます。 次のアプリケーション構成ファイルは、最も一般的な使用方法を示しています。  
  
 [!code-xml[ServiceMetadataBehavior#1](../../../samples/snippets/csharp/VS_Snippets_CFX/servicemetadatabehavior/cs/hostapplication.exe.config#1)]  
  
 以下のセクションでは、サービスまたはクライアントのランタイム実行を変更する際に最も役立つ、さまざまなシステム指定の動作について説明します。 リファレンス トピックを参照して、各動作の使用方法を確認してください。  
  
### <a name="service-behaviors"></a>サービスの動作  
 次の動作は、サービスに影響します。  
  
- <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute>. WCF サービスに適用され、そのサービスを ASP.NET 互換モードで実行できるかどうかを示します。  
  
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>. サービスがクライアントのクレームを承認する方法を制御します。  
  
- <xref:System.ServiceModel.Description.ServiceCredentials>. サービス資格情報を構成します。 このクラスを使用して、X.509 証明書などのサービスの資格情報を指定します。  
  
- <xref:System.ServiceModel.Description.ServiceDebugBehavior>. WCF サービスのデバッグ機能とヘルプ情報機能を有効にします。  
  
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior>. サービス メタデータと関連情報の公開を制御します。  
  
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>. セキュリティ イベントの監査動作を指定します。  
  
- <xref:System.ServiceModel.Description.ServiceThrottlingBehavior>. サービス パフォーマンスを調整できるようにするランタイム スループット設定を構成します。  
  
### <a name="endpoint-behaviors"></a>エンドポイントの動作  
 以下の動作は、エンドポイントに影響します。 これらの動作の多くは、クライアント アプリケーションで使用されます。  
  
- <xref:System.ServiceModel.CallbackBehaviorAttribute>. 双方向クライアント アプリケーションのコールバック サービス実装を構成します。  
  
- <xref:System.ServiceModel.Description.CallbackDebugBehavior>. WCF コールバックオブジェクトのサービスデバッグを有効にします。  
  
- <xref:System.ServiceModel.Description.ClientCredentials>. クライアントとサービスの資格情報、およびクライアントで使用するサービス資格情報の認証設定をユーザーが構成できるようにします。  
  
- <xref:System.ServiceModel.Description.ClientViaBehavior>. トランスポート チャネルを作成する対象の URI (Uniform Resource Identifier) を指定するために、クライアントが使用します。  
  
- <xref:System.ServiceModel.Description.MustUnderstandBehavior>. `MustUnderstand` の処理を無効にするよう WCF に指示します。  
  
- <xref:System.ServiceModel.Description.SynchronousReceiveBehavior>. チャネルの同期受信プロセスを使用するようにランタイムに指示します。  
  
- <xref:System.ServiceModel.Description.TransactedBatchingBehavior>. トランザクションの受信をサポートするトランスポートの受信操作を最適化します。  
  
### <a name="contract-behaviors"></a>コントラクトの動作  
 <xref:System.ServiceModel.DeliveryRequirementsAttribute>. バインディングがサービスまたはクライアントの実装に対して提供する必要がある機能要件を指定します。  
  
### <a name="operation-behaviors"></a>操作の動作  
 以下の操作の動作は、操作のシリアル化制御およびトランザクション制御を指定します。  
  
- <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior>. <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>のランタイム動作を表します。  
  
- <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior>. `XmlSerializer` のランタイム動作を制御し、操作に関連付けます。  
  
- <xref:System.ServiceModel.TransactionFlowAttribute>. サービス操作がトランザクション ヘッダーを受け入れるレベルを指定します。  
  
## <a name="see-also"></a>関連項目

- [サービスの構成](configuring-services.md)
- [方法 : サービスのインスタンス化を制御する](./feature-details/how-to-control-service-instancing.md)
