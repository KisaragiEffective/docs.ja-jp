---
description: '詳細については、「方法: WCF クライアントを使用して WSE 3.0 サービスにアクセスする」を参照してください。'
title: '方法 : WCF クライアントで WSE 3.0 サービスにアクセスする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1f9bcd9b-8f8f-47fa-8f1e-0d47236eb800
ms.openlocfilehash: 5a117b4c3d743d783c37ed3e27e3cf11e44abec3
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99742928"
---
# <a name="how-to-access-a-wse-30-service-with-a-wcf-client"></a>方法 : WCF クライアントで WSE 3.0 サービスにアクセスする

Windows Communication Foundation (WCF) クライアントは、WS-Addressing 仕様の8月2004バージョンを使用するように WCF クライアントが構成されている場合、Microsoft .NET サービスの Web サービス拡張 (WSE) 3.0 とのワイヤレベルの互換性があります。 ただし、WSE 3.0 サービスでは、metadata exchange (MEX) プロトコルがサポートされていないため、 [ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) を使用して wcf クライアントクラスを作成すると、生成された wcf クライアントにセキュリティ設定が適用されません。 そのため、WCF クライアントが生成された後に、WSE 3.0 サービスが必要とするセキュリティ設定を指定する必要があります。  
  
 これらのセキュリティ設定を適用するには、カスタムバインディングを使用して WSE 3.0 サービスの要件と、WSE 3.0 サービスと WCF クライアントの間の相互運用可能な要件を考慮する必要があります。 これらの相互運用性要件には、前述の 2004 年 8 月版 WS-Addressing 仕様の使用と、WSE 3.0 の既定のメッセージ保護が <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> であることが含まれます。 WCF の既定のメッセージ保護は <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature> です。 このトピックでは、WSE 3.0 サービスと相互運用する WCF バインドを作成する方法について詳しく説明します。 また、WCF には、このバインディングが組み込まれたサンプルも用意されています。 このサンプルの詳細については、「 [ASMX Web サービスとの相互運用](../samples/interoperating-with-asmx-web-services.md)」を参照してください。  
  
### <a name="to-access-a-wse-30-web-service-with-a-wcf-client"></a>WCF クライアントで WSE 3.0 サービスにアクセスするには  
  
1. [ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)を実行して、WSE 3.0 Web サービス用の WCF クライアントを作成します。  
  
     WSE 3.0 Web サービスの場合、WCF クライアントが作成されます。 WSE 3.0 は MEX プロトコルをサポートしていないため、このツールを使用して Web サービスのセキュリティ要件を取得することはできません。 アプリケーション開発者は、クライアントのセキュリティ設定を追加する必要があります。  
  
     WCF クライアントの作成の詳細については、「 [方法: クライアントを作成](../how-to-create-a-wcf-client.md)する」を参照してください。  
  
2. WSE 3.0 Web サービスと通信できるバインディングを表すクラスを作成します。  
  
     次のクラスは、 [WSE との相互運用](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90)) のサンプルに含まれています。  
  
    1. <xref:System.ServiceModel.Channels.Binding> クラスから派生するクラスを作成します。  
  
         `WseHttpBinding` クラスから派生する、<xref:System.ServiceModel.Channels.Binding> という名前のクラスを作成する方法を次のコード例に示します。  
  
         [!code-csharp[c_WCFClientToWSEService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#1)]
         [!code-vb[c_WCFClientToWSEService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#1)]  
  
    2. WSE サービスで使用する WSE 設定不要アサーション、派生キーが必要かどうか、セキュリティで保護されたセッションを使用するかどうか、署名の確認が必要かどうか、およびメッセージ保護設定を指定するプロパティを、このクラスに追加します。 WSE 3.0 では、ターンキーアサーションでクライアントまたは Web サービスのセキュリティ要件を指定します。 WCF でのバインディングの認証モードに似ています。  
  
         WSE 設定不要アサーション、派生キーが必要かどうか、セキュリティで保護されたセッションを使用するかどうか、署名の確認が必要かどうか、およびメッセージ保護設定をそれぞれ指定する、`SecurityAssertion`、`RequireDerivedKeys`、`EstablishSecurityContext`、および `MessageProtectionOrder` の各プロパティを定義するコード例を次に示します。  
  
         [!code-csharp[c_WCFClientToWSEService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#3)]
         [!code-vb[c_WCFClientToWSEService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#3)]  
  
    3. <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> メソッドをオーバーライドして、バインディング プロパティを設定します。  
  
         `SecurityAssertion` プロパティと `MessageProtectionOrder` プロパティの値を取得することで、トランスポート、メッセージ エンコーディング、メッセージ保護設定を指定するコード例を次に示します。  
  
         [!code-csharp[c_WCFClientToWSEService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#2)]
         [!code-vb[c_WCFClientToWSEService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#2)]  
  
3. クライアントのアプリケーション コードでは、コードを追加してバインディングのプロパティを設定します。  
  
     次のコード例では、WCF クライアントで、WSE 3.0 のターンキーセキュリティアサーションによる定義に従ってメッセージの保護と認証を使用する必要があることを指定し `AnonymousForCertificate` ます。 また、セキュリティで保護されたセッションと派生キーが必要です。  
  
     [!code-csharp[c_WCFClientToWSEService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#4)]
     [!code-vb[c_WCFClientToWSEService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#4)]  
  
## <a name="example"></a>例  

 WSE 3.0 の設定不要のセキュリティ アサーションのプロパティに対応するプロパティを公開するカスタムのバインディングを定義するコード例を次に示します。 次に、という名前のカスタムバインディングを使用して、 `WseHttpBinding` WSSecurityAnonymous WSE 3.0 クイックスタートサンプルと通信する WCF クライアントのバインディングプロパティを指定します。  

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.Binding>
- [WSE との相互運用](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90))
