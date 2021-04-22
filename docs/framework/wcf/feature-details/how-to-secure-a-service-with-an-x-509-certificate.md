---
description: '詳細情報: 方法: X.509 証明書を使用してサービスをセキュリティで保護する'
title: '方法: X.509 証明書を使用してサービスをセキュリティで保護する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2d06c2aa-d0d7-4e5e-ad7e-77416aa1c10b
ms.openlocfilehash: 57c8e78777c620fd3078651dbafbbf010fbd3768
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643385"
---
# <a name="how-to-secure-a-service-with-an-x509-certificate"></a>方法: X.509 証明書を使用してサービスをセキュリティで保護する

X.509 証明書を使用してサービスをセキュリティ保護することは、Windows Communication Foundation (WCF) の大半のバインディングで使用される基本的な手法です。 ここでは、X.509 証明書を使用して自己ホスト サービスを構成する手順を示します。  
  
 サーバーの認証に使用できる有効な証明書があることが前提条件になります。 この証明書は、信頼された証明機関によってサーバーに対して発行される必要があります。 証明書が無効な場合、サービスの使用を試みるすべてのクライアントがサービスを信頼しなくなるため、接続が作成されません。 証明書の使用に関する詳細については、「[証明書の使用](working-with-certificates.md)」を参照してください。  
  
### <a name="to-configure-a-service-with-a-certificate-using-code"></a>コードにより証明書を使用してサービスを構成するには  
  
1. サービス コントラクトを作成し、サービスを実装します。 詳細については、「[サービスの設計と実装](../designing-and-implementing-services.md)」を参照してください。  
  
2. 次のコードに示すように、<xref:System.ServiceModel.WSHttpBinding> クラスのインスタンスを作成し、そのセキュリティ モードを <xref:System.ServiceModel.SecurityMode.Message> に設定します。  
  
     [!code-csharp[C_SecureWithCertificate#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#1)]
     [!code-vb[C_SecureWithCertificate#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#1)]  
  
3. 次のコードに示すように、2 つの <xref:System.Type> 変数を作成し、それぞれコントラクト型と実装されたコントラクトに割り当てます。  
  
     [!code-csharp[C_SecureWithCertificate#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#2)]
     [!code-vb[C_SecureWithCertificate#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#2)]  
  
4. サービスのベース アドレス用に <xref:System.Uri> クラスのインスタンスを作成します。 `WSHttpBinding` では HTTP トランスポートを使用するため、URI (Uniform Resource Identifier) がこのスキーマで開始されている必要があります。そうでない場合、Windows Communication Foundation (WCF) は、サービスが開かれたときに例外をスローします。  
  
     [!code-csharp[C_SecureWithCertificate#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#3)]
     [!code-vb[C_SecureWithCertificate#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#3)]  
  
5. 実装したコントラクト型変数と URI を使用して <xref:System.ServiceModel.ServiceHost> クラスの新しいインスタンスを作成します。  
  
     [!code-csharp[C_SecureWithCertificate#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#4)]
     [!code-vb[C_SecureWithCertificate#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#4)]  
  
6. <xref:System.ServiceModel.Description.ServiceEndpoint> メソッドを使用して、サービスに <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> を追加します。 次のコードに示すように、コントラクト、バインディング、およびエンドポイント アドレスをコンストラクターに渡します。  
  
     [!code-csharp[C_SecureWithCertificate#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#5)]
     [!code-vb[C_SecureWithCertificate#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#5)]  
  
7. 任意。 サービスからメタデータを取得するには、新しい <xref:System.ServiceModel.Description.ServiceMetadataBehavior> オブジェクトを作成し、<xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A> プロパティを `true` に設定します。  
  
     [!code-csharp[C_SecureWithCertificate#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#6)]
     [!code-vb[C_SecureWithCertificate#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#6)]  
  
8. <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> クラスの <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> メソッドを使用して、有効な証明書をサービスに追加します。 このメソッドでは、いくつかある方法の 1 つを使用して証明書を見つけます。 この例では、<xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectName> 列挙体を使用します。 この列挙体では、提供された値が証明書の発行先のエンティティの名前であることを指定します。  
  
     [!code-csharp[C_SecureWithCertificate#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#7)]
     [!code-vb[C_SecureWithCertificate#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#7)]  
  
9. <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> メソッドを呼び出して、サービスのリッスンを開始します。 コンソール アプリケーションを作成する場合は、<xref:System.Console.ReadLine%2A> メソッドを呼び出して、サービスをリッスン状態に保持します。  
  
     [!code-csharp[C_SecureWithCertificate#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#8)]
     [!code-vb[C_SecureWithCertificate#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#8)]  
  
## <a name="example"></a>例  

 次の例では、<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> メソッドを使用して、X.509 証明書でサービスを構成します。  
  
 [!code-csharp[C_SecureWithCertificate#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#9)]
 [!code-vb[C_SecureWithCertificate#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#9)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  

 コードのコンパイルには次の名前空間が必要です。  
  
- <xref:System>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.ServiceModel.Channels>  
  
- <xref:System.Web.Services.Description>  
  
- <xref:System.Security.Cryptography.X509Certificates>  
  
- <xref:System.Runtime.Serialization>  
  
## <a name="see-also"></a>関連項目

- [証明書の使用](working-with-certificates.md)
