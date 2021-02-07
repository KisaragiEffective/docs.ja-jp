---
description: '詳細については、「方法: ASP.NET Web サービスクライアントと相互運用するように WCF サービスを構成する」を参照してください。'
title: '方法: WCF サービスおよび ASP.NET Web サービス クライアントを相互運用するために構成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 48e1cd90-de80-4d6c-846e-631878955762
ms.openlocfilehash: 180307763054f8dfe5696fb0bbf294ec2b5ee3db
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99743396"
---
# <a name="how-to-configure-wcf-service-to-interoperate-with-aspnet-web-service-clients"></a>方法: WCF サービスおよび ASP.NET Web サービス クライアントを相互運用するために構成する

ASP.NET Web サービスクライアントと相互運用できるように Windows Communication Foundation (WCF) サービスエンドポイントを構成するには、 <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> サービスエンドポイントのバインドの種類として型を使用します。  
  
 必要に応じて、HTTPS およびトランスポート レベルのクライアント認証のサポートをバインディングで有効にできます。 ASP.NET Web サービスクライアントは MTOM メッセージエンコーディングをサポートしていないため、 <xref:System.ServiceModel.BasicHttpBinding.MessageEncoding%2A?displayProperty=nameWithType> プロパティは既定値 () のままにしておく必要があり <xref:System.ServiceModel.WSMessageEncoding.Text?displayProperty=nameWithType> ます。 ASP.NET Web サービスクライアントでは WS-SECURITY がサポートされていないため、 <xref:System.ServiceModel.BasicHttpBinding.Security%2A?displayProperty=nameWithType> をに設定する必要があり <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> ます。  
  
 WCF サービスのメタデータを ASP.NET Web サービスプロキシ生成ツール (つまり、 [web サービス記述言語ツール (Wsdl.exe)](/previous-versions/dotnet/netframework-4.0/7h3ystb6(v=vs.100))、 [Web サービス検出ツール (Disco.exe)](/previous-versions/dotnet/netframework-4.0/cy2a3ybs(v=vs.100))、および Visual Studio の [ **web 参照の追加** ] 機能) で使用できるようにするには、HTTP/GET メタデータエンドポイントを公開する必要があります。  
  
## <a name="add-an-endpoint-in-code"></a>コードにエンドポイントを追加する  
  
1. 新しい <xref:System.ServiceModel.BasicHttpBinding> インスタンスを作成します。  
  
2. 必要に応じて、バインディングのセキュリティ モードを <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> に設定して、このサービス エンドポイント バインディングのトランスポート セキュリティを有効にします。 詳細については、「 [トランスポートセキュリティ](transport-security.md)」を参照してください。  
  
3. 上で作成したバインディング インスタンスを使用して、サービス ホストに新しいアプリケーション エンドポイントを追加します。 コードでサービスエンドポイントを追加する方法の詳細については、「 [方法: コードでサービスエンドポイントを作成](how-to-create-a-service-endpoint-in-code.md)する」を参照してください。  
  
4. サービス用に HTTP/GET メタデータのエンドポイントを有効にします。 詳細については [、「方法: コードを使用してサービスのメタデータを公開](how-to-publish-metadata-for-a-service-using-code.md)する」を参照してください。  
  
## <a name="add-an-endpoint-in-a-configuration-file"></a>構成ファイルにエンドポイントを追加する  
  
1. 新しい <xref:System.ServiceModel.BasicHttpBinding> バインディング構成を作成します。 詳細については、「 [方法: 構成でサービスバインディングを指定](../how-to-specify-a-service-binding-in-configuration.md)する」を参照してください。  
  
2. 必要に応じて、バインディングのセキュリティ モードを <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> に設定して、このサービス エンドポイント バインド構成のトランスポート セキュリティを有効にします。 詳細については、「 [トランスポートセキュリティ](transport-security.md)」を参照してください。  
  
3. 上で作成したバインド構成を使用して、サービスに新しいアプリケーション エンドポイントを構成します。 構成ファイルにサービスエンドポイントを追加する方法の詳細については、「 [方法: 構成でサービスエンドポイントを作成](how-to-create-a-service-endpoint-in-configuration.md)する」を参照してください。  
  
4. サービス用に HTTP/GET メタデータのエンドポイントを有効にします。 詳細については、「 [方法: 構成ファイルを使用してサービスのメタデータを公開](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)する」を参照してください。  
  
## <a name="example"></a>例  

 次のコード例は、ASP.NET Web サービスクライアントと互換性のある WCF エンドポイントをコードで、または構成ファイルで追加する方法を示しています。  
  
 [!code-csharp[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/cs/program.cs#0)]
 [!code-vb[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/vb/program.vb#0)]
 [!code-xml[C_HowTo-WCFServiceAndASMXClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/common/app.config#1)]
  
## <a name="see-also"></a>関連項目

- [方法: コード内にサービス エンドポイントを作成する](how-to-create-a-service-endpoint-in-code.md)
- [方法: コードを使用してサービスのメタデータを公開する](how-to-publish-metadata-for-a-service-using-code.md)
- [方法: 構成でサービス バインディングを指定する](../how-to-specify-a-service-binding-in-configuration.md)
- [方法: 構成にサービス エンドポイントを作成する](how-to-create-a-service-endpoint-in-configuration.md)
- [方法: 構成ファイルを使用してサービスのメタデータを公開する](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
- [トランスポートセキュリティ](transport-security.md)
- [メタデータを使用する](using-metadata.md)
