---
title: セキュリティの概要
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, security
- WCF, security
ms.assetid: f478c80d-792d-4e7a-96bd-a2ff0b6f65f9
ms.openlocfilehash: 1e551572fa6d94e9fd1170eb7e3b258f2e8fb926
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728894"
---
# <a name="windows-communication-foundation-security-overview"></a>Windows Communication Foundation セキュリティの概要
Windows Communication Foundation (WCF) は、SOAP メッセージベースの分散プログラミングプラットフォームです。データを保護するには、クライアントとサービス間のメッセージをセキュリティで保護することが不可欠です。 WCF では、既存のセキュリティインフラストラクチャと SOAP メッセージに対して認識されているセキュリティ標準の両方に基づいて、セキュリティで保護されたメッセージを交換するための相互運用可能なプラットフォームが提供されます。  
  
> [!NOTE]
> WCF セキュリティの包括的なガイドについては、「 [Wcf セキュリティガイダンス](https://archive.codeplex.com/?p=WCFSecurity)」を参照してください。  
  
 WCF では、HTTPS、Windows 統合セキュリティ、ユーザー認証のためのユーザー名とパスワードなど、既存のテクノロジを使用してセキュリティで保護された分散アプリケーションを構築している場合に、使い慣れた概念を使用します。 WCF は、既存のセキュリティインフラストラクチャと統合するだけでなく、セキュリティで保護された SOAP メッセージを使用して、Windows のみのドメインを超えた分散セキュリティも拡張します。 既存のプロトコルに加えて SOAP をプロトコルとして使用するという大きな利点があるため、WCF では既存のセキュリティメカニズムを実装することを検討してください。 たとえば、ユーザー名とパスワードや X.509 証明書など、クライアントまたはサービスを識別する資格情報には、相互運用可能な XML ベースの SOAP プロファイルがあります。 このプロファイルを使用して、XML デジタル署名や XML 暗号化などの公開仕様を利用するセキュリティで保護されたメッセージ交換を行います。 仕様の一覧については、「[システム指定の相互運用性バインディングでサポートされる Web サービスプロトコル](../../../../docs/framework/wcf/feature-details/web-services-protocols-supported-by-system-provided-interoperability-bindings.md)」を参照してください。  
  
 また他にも、Windows プラットフォームのコンポーネント オブジェクト モデル (COM) があり、セキュリティで保護された分散アプリケーションを実現します。 COM は、包括的なセキュリティ機構を有します。このセキュリティ機構により、セキュリティ コンテキストはコンポーネント間をフローできます。また、整合性、機密性、および認証を実現できます。 ただし、COM は、クロスプラットフォームのセキュリティで保護されたメッセージング (WCF など) を有効にしません。 WCF を使用すると、インターネット経由で Windows ドメインから広がるサービスとクライアントを構築できます。 WCF の相互運用可能なメッセージは、お客様の情報のセキュリティを確保するのに役立つ動的なビジネス主導のサービスを構築するために不可欠です。  
  
## <a name="windows-communication-foundation-security-benefits"></a>Windows Communication Foundation セキュリティの利点  
 WCF は、SOAP メッセージに基づく分散プログラミングプラットフォームです。 WCF を使用すると、サービスとサービスクライアントの両方として機能するアプリケーションを作成し、無制限の数のサービスとクライアントからメッセージを作成して処理することができます。 このような分散アプリケーションでは、メッセージをノード間でフローさせ、ファイアウォールを経由してインターネットに送信し、多くの SOAP 中継局を経由させることができます。 しかしこれは、さまざまなメッセージ セキュリティの脅威を招きます。 次の例では、エンティティ間でメッセージを交換するときに WCF セキュリティが軽減するのに役立つ一般的な脅威をいくつか示します。  
  
- 機密情報を取得するネットワーク トラフィックの監視。 たとえば、オンライン バンキング シナリオを考えます。クライアントは口座間の資金の移動を要求します。 悪意のあるユーザーはメッセージを途中受信し、後で口座番号とパスワードを使用して、侵害した口座から資金を移動します。  
  
- クライアントに気付かれることなくサービスとして動作する承認されていないエンティティ。 このシナリオでは、悪意のある (承認されていない) ユーザーがオンライン サービスとして動作し、クライアントからメッセージを途中受信して機密情報を取得します。 次にこの承認されていないユーザーは、盗んだデータを使用して侵害した口座から資金を転送します。 この攻撃は、*フィッシング攻撃*とも呼ばれます。  
  
- 呼び出し元が意図したものと異なる結果を取得するようなメッセージの改ざん。 たとえば、預金先の口座番号を変更して、資金を承認されていない口座に移動できます。  
  
- 迷惑なハッカーは、同じ注文をリプレイします。 たとえば、オンライン書店は数百の注文を受け、注文していない顧客に書籍を送付します。  
  
- クライアントを認証する能力の欠如。 この場合、サービスは、適切なユーザーがトランザクションを実行していることを保証できません。  
  
 転送セキュリティが提供する保証を簡単に説明すると次のようになります。  
  
- サービス エンドポイント (応答者) の認証  
  
- クライアント プリンシパル (イニシエーター) の認証  
  
- メッセージの整合性  
  
- メッセージの機密性  
  
- リプレイの検出  
  
### <a name="integration-with-existing-security-infrastructures"></a>既存のセキュリティ インフラストラクチャとの統合  
 ほとんどの場合、Web サービス展開には、SSL (Secure Sockets Layer) や Kerberos プロトコルなどの既存のセキュリティ ソリューションが実装されています。 Active Directory を使用する Windows ドメインなど、既に展開されているセキュリティ インフラストラクチャを利用する場合もあります。 新しい技術を評価し採用している間に、これらの既存の技術と統合する必要が生じることはよくあります。  
  
 WCF セキュリティは既存のトランスポートセキュリティモデルと統合され、既存のインフラストラクチャを利用して、SOAP メッセージセキュリティに基づく新しい転送セキュリティモデルを利用できます。  
  
### <a name="integration-with-existing-authentication-models"></a>既存の認証モデルとの統合  
 すべての通信セキュリティ モデルにとって、通信のエンティティを識別し認証する機能は重要です。 この通信のエンティティは、"デジタル ID" または資格情報を使用し、通信先のピアで自身を認証します。 分散型の通信プラットフォームの発展により、さまざまな資格情報認証モデルや関連セキュリティ モデルが実装されてきました。 たとえば、インターネットでは、ユーザー名とパスワードを使用してユーザーを識別することが一般的に行われています。 イントラネットでは、ユーザーとサービスの認証をサポートする Kerberos ドメイン コントローラーの使用が一般的になりつつあります。 たとえば、2 つのビジネス パートナー間での特定のシナリオでは、証明書を使用してお互いにパートナーを認証する場合もあります。  
  
 同じサービスを企業内のユーザーや外部のパートナーまたはインターネット ユーザーに公開する Web サービスの世界では、インフラストラクチャがこのような既存のセキュリティ認証モデルとの統合に備えることが重要です。 WCF セキュリティでは、次のようなさまざまな資格情報の種類 (認証モデル) がサポートされています。  
  
- 匿名の呼び出し元  
  
- ユーザー名クライアント資格情報  
  
- 証明書クライアント資格情報  
  
- Windows (Kerberos プロトコルと NT LanMan [NTLM] の両方)  
  
### <a name="standards-and-interoperability"></a>標準と相互運用  
 既存の大規模展開の世界では、均一性はほとんどありません。 分散型のコンピューティングまたは通信のプラットフォームでは、さまざまなベンダーから提供される技術を相互運用する必要があります。 同様に、セキュリティも相互運用可能である必要があります。  
  
 相互運用可能なセキュリティ システムを実現するため、Web サービス業界で活動する企業はさまざまな標準を作成してきました。 特にセキュリティに関して、注目すべき標準がいくつか提案されています。それは、WS-Security の SOAP メッセージ セキュリティ (標準化団体の OASIS で認められ正式には WS-Security として知られる)、WS-Trust、WS-SecureConversation、および WS-SecurityPolicy です。  
  
 WCF では、さまざまな相互運用性のシナリオがサポートされています。 <xref:System.ServiceModel.BasicHttpBinding> クラスは、基本セキュリティ プロファイル (BSP) を対象とし、<xref:System.ServiceModel.WSHttpBinding> クラスは、WS-Security 1.1 や WS-SecureConversation などの最新のセキュリティ標準を対象とします。 これらの標準に準拠することで、WCF セキュリティは、Microsoft Windows 以外のオペレーティングシステムやプラットフォームでホストされている Web サービスと相互運用および統合できます。  
  
## <a name="wcf-security-functional-areas"></a>WCF セキュリティの機能領域  
 WCF セキュリティは、転送セキュリティ、アクセス制御、監査という3つの機能領域に分かれています。 この後の各セクションでは、これらの領域について簡単に説明します。また、詳細情報へのリンクを示します。  
  
### <a name="transfer-security"></a>転送セキュリティ  
 転送セキュリティには、整合性、機密性、および認証の 3 つの主要なセキュリティ機能があります。 *整合性*とは、メッセージが改ざんされたかどうかを検出する機能です。 *機密性*とは、メッセージを目的の受信者以外のユーザーが判読できないようにする機能です。これは、暗号化によって実現されます。 *認証*は、要求された id を検証する機能です。 これらの 3 つの機能を一緒に使用して、メッセージをある地点から別の地点に安全に到着させることができます。  
  
#### <a name="transport-and-message-security-modes"></a>トランスポート セキュリティ モードとメッセージ セキュリティ モード  
 WCF で転送セキュリティを実装するには、*トランスポート*セキュリティモードと*メッセージ*セキュリティモードの2つの主要なメカニズムを使用します。  
  
- *トランスポートセキュリティモード*では、HTTPS などのトランスポートレベルのプロトコルを使用して転送セキュリティを実現します。 トランスポート モードの利点は、広範囲に採用されていること、多くのプラットフォームで利用可能なこと、コンピューター処理上の複雑性が低いことです。 ただし、Point-to-Point からのメッセージだけのセキュリティを保護するという欠点もあります。  
  
- 一方、*メッセージセキュリティモード*では、ws-security (およびその他の仕様) を使用して転送セキュリティを実装します。 メッセージ セキュリティは、SOAP メッセージに直接適用され、また SOAP エンベロープ内にアプリケーション データと共に格納されるため、トランスポート プロトコルに依存しない、拡張性が高い、エンド ツー エンド (Point-to-Point と対照) のセキュリティが保証される、という利点があります。ただし、SOAP メッセージの XML の性質を処理する必要があるため、トランスポート セキュリティ モードよりも処理が数倍遅くなる欠点があります。  
  
 これらの違いの詳細については、「[サービスとクライアントのセキュリティ保護](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)」を参照してください。  
  
 3 番目のセキュリティ モードは、上記の 2 つのモードを使用し、その両方の利点を引き継ぎます。 このモードは、`TransportWithMessageCredential` と呼ばれます。 このモードでは、クライアントの認証にメッセージ セキュリティを使用し、サーバーの認証にトランスポート セキュリティを使用して、メッセージの機密性と整合性を実現します。 これにより、`TransportWithMessageCredential` セキュリティ モードでは、トランスポート セキュリティ モードと同じ速度で処理が実行され、メッセージ セキュリティ モードと同じ方法でクライアント認証の拡張性が提供されます。 ただし、メッセージ セキュリティ モードと異なり、完全なエンド ツー エンド セキュリティは提供されません。  
  
### <a name="access-control"></a>アクセス制御  
 *アクセス制御*は、承認とも呼ばれます。 *承認*により、異なるユーザーがデータを表示するための異なる特権を持つことができます。 たとえば、企業の人事ファイルには従業員の機密データが含まれているため、管理者だけが従業員データの参照を許可されます。 また、管理者は直接報告書のデータのみを参照できます。 このような場合、アクセス制御は、ロール ("manager") と管理者の特定 ID の両方に基づいて行われます (ある管理者が別の管理者の部下のレコードを参照できないようにします)。  
  
 WCF では、アクセス制御機能は、共通言語ランタイム (CLR) <xref:System.Security.Permissions.PrincipalPermissionAttribute> および*id モデル*と呼ばれる一連の api との統合によって提供されます。 アクセス制御と要求ベースの承認の詳細については、「[セキュリティの拡張](../../../../docs/framework/wcf/extending/extending-security.md)」を参照してください。  
  
### <a name="auditing"></a>監査  
 *監査*とは、Windows イベントログへのセキュリティイベントのログ記録です。 認証の失敗 (または成功) などのセキュリティ関連のイベントをログに記録できます。 詳細については、「[監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)」を参照してください。 プログラミングの詳細については、「[方法: セキュリティイベントを監査](../../../../docs/framework/wcf/feature-details/how-to-audit-wcf-security-events.md)する」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- [サービスのセキュリティ保護](../../../../docs/framework/wcf/securing-services.md)
- [一般的なセキュリティ シナリオ](../../../../docs/framework/wcf/feature-details/common-security-scenarios.md)
- [バインディングとセキュリティ](../../../../docs/framework/wcf/feature-details/bindings-and-security.md)
- [サービスおよびクライアントのセキュリティ保護](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)
- [認証](../../../../docs/framework/wcf/feature-details/authentication-in-wcf.md)
- [承認](../../../../docs/framework/wcf/feature-details/authorization-in-wcf.md)
- [フェデレーションと発行済みトークン](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)
- [監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)
- [セキュリティ ガイドラインとベスト プラクティス](../../../../docs/framework/wcf/feature-details/security-guidance-and-best-practices.md)
- [構成ファイルを使用してサービスを構成する方法](../../../../docs/framework/wcf/configuring-services-using-configuration-files.md)
- [システム標準のバインディング](../../../../docs/framework/wcf/system-provided-bindings.md)
- [エンドポイントの作成の概要](../../../../docs/framework/wcf/endpoint-creation-overview.md)
- [セキュリティの拡張](../../../../docs/framework/wcf/extending/extending-security.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
