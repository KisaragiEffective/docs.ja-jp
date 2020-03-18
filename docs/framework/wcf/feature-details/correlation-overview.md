---
title: 相関関係の概要
ms.date: 03/30/2017
ms.assetid: edcc0315-5d26-44d6-a36d-ea554c418e9f
ms.openlocfilehash: cb11f65a30c6bdf5a1d910402ef58446c03a5491
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75963939"
---
# <a name="correlation-overview"></a>相関関係の概要
相関関係とは、ワークフロー サービス メッセージを互いに関連付ける、最初の要求への応答などのアプリケーションのインスタンス状態と関連付ける、または特定の注文 ID を注文処理ワークフローの永続化された状態に関連付けるためのしくみです。 ここでは、相関関係の概要について説明します。 このセクションの他のトピックでは、相関関係の各種類についての追加情報を提供します。  
  
## <a name="types-of-correlation"></a>相関関係の種類  
 相関関係には、プロトコル ベースとコンテンツ ベースがあります。 プロトコル ベースの相関関係では、メッセージ間のマッピングを提供するためのメッセージ配信インフラストラクチャで提供されるデータが使用されます。 プロトコル ベースの相関関係を使用して関連付けられたメッセージは、メモリ内の <xref:System.ServiceModel.Channels.RequestContext> などのオブジェクトを使用して、またはトランスポート プロトコルで提供されるトークンによって、互いに関連付けられます。 コンテンツ ベースの相関関係では、メッセージが、アプリケーションで指定されたデータを使用して関連付けられます。 コンテンツ ベースの相関関係を使用して関連付けられたメッセージは、アプリケーションが定義する顧客番号などのメッセージ内のデータによって互いに関連付けられます。  
  
 相関関係に参加するアクティビティは、<xref:System.ServiceModel.Activities.CorrelationHandle> を使用してメッセージング アクティビティを互いに結び付けます。 たとえば、サービスの呼び出しに使用される <xref:System.ServiceModel.Activities.Send> と、それに続く、サービスからのコールバックの受信に使用される <xref:System.ServiceModel.Activities.Receive> は、同じ <xref:System.ServiceModel.Activities.CorrelationHandle> を共有します。 この基本的なパターンは、相関関係がコンテンツ ベースであっても、プロトコル ベースであっても使用されます。 関連付けハンドルは、各アクティビティに明示的に設定する、またはアクティビティを <xref:System.ServiceModel.Activities.CorrelationScope> アクティビティに格納することができます。 <xref:System.ServiceModel.Activities.CorrelationScope> に格納されたアクティビティには、<xref:System.ServiceModel.Activities.CorrelationScope> が管理する関連付けハンドルがあり、<xref:System.ServiceModel.Activities.CorrelationHandle> を明示的に設定する必要がありません。 <xref:System.ServiceModel.Activities.CorrelationScope> スコープでは、要求/応答の相関関係と、もう 1 つの種類の相関関係のための <xref:System.ServiceModel.Activities.CorrelationHandle> 管理が提供されます。 <xref:System.ServiceModel.Activities.WorkflowServiceHost> を使用してホストされるワークフロー サービスには、<xref:System.ServiceModel.Activities.CorrelationScope> アクティビティと同じ既定の関連付け管理があります。 多くのシナリオで、この既定の関連付け管理は、<xref:System.ServiceModel.Activities.CorrelationScope> 内のメッセージング アクティビティ、またはワークフロー サービスが <xref:System.ServiceModel.Activities.CorrelationHandle> セットを要求しないことを意味します。ただし、2 つの <xref:System.ServiceModel.Activities.Receive> アクティビティが並行している、または 2 つの <xref:System.ServiceModel.Activities.Send> アクティビティに 2 つの <xref:System.ServiceModel.Activities.Receive> アクティビティが続くなど、複数のメッセージング アクティビティが並行または重複している場合を除きます。 既定の相関関係の詳細情報は、このセクションの中のそれぞれの種類の相関関係を説明するトピックに記述されています。 メッセージングアクティビティの詳細については、「メッセージング[アクティビティ](../../../../docs/framework/wcf/feature-details/messaging-activities.md)」と「[方法: メッセージングアクティビティを使用してワークフローサービスを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-workflow-service-with-messaging-activities.md)」を参照してください。  
  
## <a name="protocol-based-correlation"></a>プロトコル ベースの相関関係

プロトコル ベースの相関関係では、メッセージを相互に関連付け、該当するインスタンスと関連付けるために、トランスポート機構が使用されます。 システムで提供される一部のプロトコル相関関係には、要求/応答の相関関係とコンテキスト ベースの相関関係が含まれます。 要求/応答の相関関係を使用して、メッセージング アクティビティの 1 つのペアが関連付けられ、<xref:System.ServiceModel.Activities.Send> と <xref:System.ServiceModel.Activities.ReceiveReply> のペアや、<xref:System.ServiceModel.Activities.Receive> と <xref:System.ServiceModel.Activities.SendReply> のペアなどの双方向の操作が形成されます。 また、Visual Studio ワークフローデザイナーには、このパターンを迅速に実装するアクティビティテンプレートのセットも用意されています。 コンテキストベースの相関関係は、「 [.Net コンテキスト交換プロトコルの仕様](https://docs.microsoft.com/openspecs/windows_protocols/mc-netcex/a7f26280-491f-465b-9914-c5eb5322dbb4)」で説明されているコンテキスト交換メカニズムに基づいています。 コンテキスト ベースの相関関係を使用するには、<xref:System.ServiceModel.BasicHttpContextBinding>、<xref:System.ServiceModel.WSHttpContextBinding>、または <xref:System.ServiceModel.NetTcpContextBinding> などのコンテキスト ベースのバインディングがエンドポイントで使用される必要があります。  
  
プロトコルの相関関係の詳細については、「[永続的な二重](../../../../docs/framework/wcf/feature-details/durable-duplex-correlation.md)と[要求-応答](../../../../docs/framework/wcf/feature-details/request-reply-correlation.md)」を参照してください。 Visual Studio のワークフローデザイナーアクティビティテンプレートの使用方法の詳細については、「[メッセージングアクティビティ](../../../../docs/framework/wcf/feature-details/messaging-activities.md)」を参照してください。 サンプルコードについては、 [NetContextExchangeCorrelation](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ee662963%28v%3dvs.100%29)サンプルを参照してください。  
  
## <a name="content-based-correlation"></a>コンテンツ ベースの相関関係

コンテンツ ベースの相関関係では、メッセージ内の一部の情報を特定のインスタンスとの関連付けに使用します。 プロトコル ベースの相関関係と異なり、コンテンツ ベースの相関関係では、各関連メッセージ内のこの情報がある場所に、アプリケーションの作成者が明示的に記述されている必要があります。 コンテンツ ベースの相関関係を使用するアクティビティでは、<xref:System.ServiceModel.MessageQuerySet> を使用して、このメッセージ データを指定します。 コンテンツ ベースの相関関係は、<xref:System.ServiceModel.BasicHttpContextBinding> などのコンテキスト バインディングの 1 つを使用しないサービスと通信する場合に便利です。
  
## <a name="see-also"></a>関連項目

- [NetContextExchangeCorrelation](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ee662963%28v%3dvs.100%29)
