---
title: WCF で使用されるセキュリティの概要
ms.date: 03/30/2017
ms.assetid: 3b9dfcf5-4bf1-4f35-9070-723171c823a1
ms.openlocfilehash: faf7b44c0ff1b207a7b017163ad2b032f26199b8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743882"
---
# <a name="security-concepts-used-in-wcf"></a>WCF で使用されるセキュリティの概要
Windows Communication Foundation (WCF) セキュリティは、さまざまなセキュリティインフラストラクチャで既に使用および展開されている概念に基づいて構築されています。  
  
 WCF は、Secure Sockets Layer (SSL) over HTTP (HTTPS) などの一部のインフラストラクチャをサポートします。 ただし、WCF は、SOAP でエンコードされたメッセージに対して、より新しい相互運用可能なセキュリティ標準 (WS-SECURITY など) を実装することで、既存のセキュリティインフラストラクチャをサポートすることを超えています。 既存の機構と新規の相互運用可能な標準のどちらを使用する場合でも、背後にあるセキュリティ概念に変わりはありません。 既存のインフラストラクチャおよび新しい標準を支える概念を理解することが、アプリケーションにとって最善のセキュリティ モデルを実装するための要になります。  
  
## <a name="introduction-to-security-for-wcf-web-services"></a>WCF Web サービスのセキュリティの概要  

Microsoft のパターンとプラクティスグループでは、 [WCF セキュリティガイド](https://archive.codeplex.com/?p=wcfsecurityguide)と呼ばれる詳細なホワイトペーパーを作成しました。 このホワイトペーパーでは、web サービス、主要な WCF セキュリティの概念、イントラネットアプリケーションのシナリオ、およびインターネットアプリケーションのシナリオに関連する基本的なセキュリティの概念について説明します。  
  
## <a name="industry-wide-security-specifications"></a>業界標準のセキュリティ仕様  
  
### <a name="public-key-infrastructure"></a>公開キー基盤  

公開キー基盤 (PKI) は、デジタル証明書、証明機関、およびその他の登録機関のシステムであり、公開キー暗号化を使用して、電子取引に関連する各パーティを検証および認証します。
  
### <a name="kerberos-protocol"></a>Kerberos プロトコル  
 *Kerberos プロトコル*は、Windows ドメインでユーザーを認証するセキュリティメカニズムを作成するための仕様です。 ユーザーはドメイン内の他のエンティティと、セキュリティで保護されたコンテキストを確立できます。 Windows 2000 以降のプラットフォームでは、Kerberos プロトコルが既定で使用されます。 システムの機構を理解することは、イントラネット クライアントと対話するサービスを作成する場合に役立ちます。 さらに、 *Web Services Security Kerberos バインド*が広く公開されているため、kerberos プロトコルを使用してインターネットクライアントと通信できます (つまり、kerberos プロトコルは相互運用可能です)。 Windows で Kerberos プロトコルを実装する方法の詳細については、「 [Microsoft kerberos](/windows/win32/secauthn/microsoft-kerberos)」を参照してください。  
  
### <a name="x509-certificates"></a>X.509 証明書  
 X.509 証明書は、セキュリティ アプリケーションで使用される主要な資格情報の形式です。 X.509 証明書の詳細については、「 [X.509 公開キー証明書](/windows/win32/seccertenroll/about-x-509-public-key-certificates)」を参照してください。 X.509 証明書は証明書ストア内に格納されます。 Windows を実行しているコンピューターには、それぞれ目的が異なる数種類の証明書ストアがあります。 さまざまなストアの詳細については、「[証明書ストア](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc757138(v=ws.10))」を参照してください。  
  
## <a name="web-services-security-specifications"></a>Web サービスのセキュリティ仕様  
 システム定義のバインディングでは、一般に使用されるさまざまな Web サービスのセキュリティ仕様をサポートしています。 システム指定のバインディングとそれらがサポートする web サービスの仕様の完全な一覧については、「[システム提供の相互運用性バインディングでサポートされる Web サービスプロトコル](../../../../docs/framework/wcf/feature-details/web-services-protocols-supported-by-system-provided-interoperability-bindings.md)」を参照してください。  
  
## <a name="access-control-mechanisms"></a>アクセス制御機構  
 WCF には、サービスや操作へのアクセスを制御するさまざまな方法が用意されています。 次に例を示します。  
  
1. <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
2. ASP.NET メンバーシップ プロバイダー  
  
3. ASP.NET ロール プロバイダー  
  
4. 承認マネージャー  
  
5. ID モデル  
  
 これらのトピックの詳細については、「」、「 [Access Control メカニズム](../../../../docs/framework/wcf/feature-details/access-control-mechanisms.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
