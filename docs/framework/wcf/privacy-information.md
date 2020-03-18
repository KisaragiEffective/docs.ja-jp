---
title: Windows Communication Foundation のプライバシー情報
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, privacy information
- WCF, privacy information
- privacy information [WCF]
ms.assetid: c9553724-f3e7-45cb-9ea5-450a22d309d9
ms.openlocfilehash: b724ff1ce85442f64980fdc972188705992d5a4f
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094983"
---
# <a name="windows-communication-foundation-privacy-information"></a>Windows Communication Foundation のプライバシー情報
Microsoft は、エンドユーザーのプライバシーの保護に努めています。 Windows Communication Foundation (WCF) バージョン3.0 を使用してアプリケーションをビルドすると、アプリケーションがエンドユーザーのプライバシーに影響を与える可能性があります。 たとえば、アプリケーションが明示的にユーザーの連絡先情報を収集することがあります。つまり、アプリケーションがインターネットを経由して Web サイトに情報を要求したり、情報を送信したりすることがあります。 マイクロソフトの技術をアプリケーションに組み込んでいる場合、その技術にプライバシーに影響を与える可能性がある独自の動作が存在することがあります。 WCF は、お客様またはエンドユーザーが Microsoft に送信することを選択しない限り、アプリケーションからマイクロソフトに情報を送信しません。  
  
## <a name="wcf-in-brief"></a>WCF の概要  
 WCF は、開発者が分散アプリケーションを構築できるようにする Microsoft .NET Framework を使用する分散メッセージングフレームワークです。 2 つのアプリケーション間で交換されるメッセージには、ヘッダーと本文情報が入ります。  
  
 ヘッダーには、アプリケーションが使用するサービスに応じて、メッセージ ルーティング、セキュリティ情報、トランザクションなどの情報が追加されます。 メッセージは、通常、既定で暗号化されます。 ただし、`BasicHttpBinding` を使用する場合は例外です。このバインディングは、セキュリティで保護されていない従来の Web サービスで使用するために設計されています。 アプリケーションの設計者であるユーザーには最終設計に対する責任があります。 SOAP 本文内のメッセージには、アプリケーション固有のデータが含まれます。ただし、アプリケーションによって定義された個人情報など、このデータは、WCF 暗号化機能または機密性機能を使用してセキュリティで保護することができます。 次のセクションでは、プライバシーに影響を与える可能性がある機能について説明します。  
  
## <a name="messaging"></a>メッセージング  
 各 WCF メッセージには、メッセージの送信先と応答の送信先を指定するアドレスヘッダーがあります。  
  
 エンドポイント アドレスのアドレス構成要素は、エンドポイントを識別する URI (Uniform Resource Identifier) です。 このアドレスは、ネットワーク アドレスまたは論理アドレスのいずれかです。 アドレスには、コンピューター名 (ホスト名、完全修飾ドメイン名) と IP アドレスを指定できます。 また、エンドポイント アドレスには、グローバル一意識別子 (GUID)、または各アドレスを識別するために使用される一時アドレス指定用に GUID のコレクションを入れることもできます。 各メッセージには、GUID 形式のメッセージ ID があります。 この機能は、WS-Addressing 参照仕様に準拠します。  
  
 WCF メッセージングレイヤーは、個人情報をローカルコンピューターに書き込みません。 ただし、サービスの開発者が個人情報を公開するサービスを作成した場合は、ネットワーク レベルで個人情報を公開することがあります。このような例として、エンドポイント名に個人名を使用している場合や、エンドポイントの Web サービス記述言語に個人情報を追加しても、https を使用して WSDL にアクセスすることをクライアントに要求しない場合などがあります。 また、開発者が個人情報を公開するエンドポイントに対して[ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)ツールを実行した場合、ツールの出力にその情報が含まれている可能性があり、出力ファイルはローカルハードディスクに書き込まれます。  
  
## <a name="hosting"></a>Hosting  
 WCF のホスティング機能を使用すると、アプリケーションをオンデマンドで開始したり、複数のアプリケーション間でポートの共有を有効にしたりできます。 WCF アプリケーションは、ASP.NET と同様にインターネットインフォメーションサービス (IIS) でホストできます。  
  
 ホストでは、特定の情報をネットワークに公開せず、コンピューター上にデータを格納しません。  
  
## <a name="message-security"></a>メッセージのセキュリティ  
 WCF セキュリティは、メッセージングアプリケーションのセキュリティ機能を提供します。 提供するセキュリティ機能には、認証と承認があります。  
  
 認証は、クライアントとサービスとの間で資格情報を交換することによって実行されます。 認証は、次のように、トランスポート レベルのセキュリティまたは SOAP メッセージ レベルのセキュリティで実行できます。  
  
- SOAP メッセージ セキュリティでは、発行者に応じて、ユーザー名とパスワード、X.509 証明書、Kerberos チケット、SAML トークンのような資格情報を使用して認証が実行されます。このすべての資格情報は、個人情報を含んでいる可能性があります。  
  
- トランスポート セキュリティでは、HTTP 認証方式 (Basic、Digest、Negotiate、Integrated Windows Authorization、NTLM、None、および Anonymous) のような従来のトランスポート認証機構によって認証が実行されます。  
  
 認証によって、通信するエンドポイント間にセキュリティで保護されたセッションを確立できます。 このセッションは、セキュリティ セッションの有効期間が切れるまで有効な GUID によって識別されます。 格納されるデータと格納場所を次の表に示します。  
  
|Data|ストレージ|  
|----------|-------------|  
|ユーザー名、X.509 証明書、Kerberos トークンなどのプレゼンテーション資格情報、および資格情報への参照|Windows 証明書ストアなど、標準の Windows 資格情報管理機構|  
|ユーザー名とパスワードなど、ユーザーのメンバーシップ情報|ASP.NET メンバーシッププロバイダー。|  
|クライアントに対するサービスの認証に使用されるサービスの ID 情報|サービスのエンドポイント アドレス|  
|呼び出し元情報|監査ログ|  
  
## <a name="auditing"></a>監査  
 監査では、認証イベントと承認イベントの成功と失敗を記録します。 監査レコードには、サービス URI、アクション URI、および呼び出し元の ID が入ります。  
  
 また、監査では、管理者がメッセージ ログの設定 (オンまたはオフ) を変更した時刻を記録します。その理由は、メッセージ ログが、ヘッダーと本文のアプリケーション固有のデータをログに記録する可能性があるためです。 Windows XP では、レコードはアプリケーションイベントログに記録されます。 Windows Vista および Windows Server 2003 では、レコードがセキュリティイベントログに記録されます。  
  
## <a name="transactions"></a>トランザクション  
 トランザクション機能は、WCF アプリケーションにトランザクションサービスを提供します。  
  
 トランザクションの伝達で使用されるトランザクション ヘッダーには、GUID 形式のトランザクション ID または登録リスト ID を追加できます。  
  
 トランザクション機能では、Microsoft 分散トランザクション コーディネーター (MSDTC) トランザクション マネージャー (Windows コンポーネントの 1 つ) を使用して、トランザクション状態を管理します。 既定では、トランザクション マネージャー間の通信は暗号化されます。 トランザクション マネージャーは、エンドポイント参照、トランザクション ID、および登録リスト ID を永続状態の一部としてログに記録できます。 この状態の有効期間は、トランザクション マネージャーのログ ファイルの有効期間によって決まります。 MSDTC サービスがこのログを所有し、管理します。  
  
 トランザクション機能は、WS-Coordination 仕様と WS-AtomicTransaction 仕様を実装します。  
  
## <a name="reliable-sessions"></a>信頼できるセッション  
 WCF の信頼できるセッションでは、トランスポートまたは中継エラーが発生したときにメッセージが転送されます。 信頼できるセッションは、基になるトランスポート (たとえば、ワイヤレス ネットワーク上の TCP 接続) が切断している場合やメッセージを紛失した場合 (HTTP プロキシでの送受信メッセージの破棄) でも、正確に 1 回のメッセージ転送を実行します。 また、メッセージが送信された順序を維持するので、メッセージの並べ替え (マルチパス ルーティングの場合に発生する可能性がある) を回復できます。  
  
 信頼できるセッションは、WS-ReliableMessaging (WS-RM) プロトコルを使用して実装されています。 これによって追加される WS-RM ヘッダーには、特定の信頼できるセッションに関連付けられたすべてのメッセージの識別に使用されるセッション情報が含まれています。 各 WS-RM セッションには、GUID 形式の識別子があります。  
  
 エンドユーザーのコンピューターには、個人情報は保持されません。  
  
## <a name="queued-channels"></a>キューに置かれたチャネル  
 キューは、送信元アプリケーションが送ったメッセージを受信側アプリケーションに代わって保存しておき、後で受信側アプリケーションに転送します。 受信側アプリケーションが一時的な場合なども、キューによって、送信元アプリケーションから受信側アプリケーションへのメッセージの転送を確実に実行できます。 WCF では、トランスポートとして Microsoft Message Queuing (MSMQ) を使用したキューのサポートを提供しています。  
  
 キューに置かれたチャネル機能では、ヘッダーをメッセージに追加しません。 代わりに、適切なメッセージ キューのメッセージ プロパティ セットを備えたメッセージ キューのメッセージを作成し、メッセージ キューのメソッドを呼び出して、メッセージ キューのキューにメッセージを追加します。 メッセージ キューは、Windows に付属しているオプション コンポーネントです。  
  
 キューに置かれたチャネル機能によってエンドユーザーのコンピューターに保持される情報はありません。これは、メッセージキューをキューインフラストラクチャとして使用するためです。  
  
## <a name="com-integration"></a>COM+ 統合  
 この機能は、既存の COM および COM + 機能をラップして、WCF サービスと互換性のあるサービスを作成します。 この機能は、特定のヘッダーを使用せず、エンドユーザーのコンピューター上のデータを保持しません。  
  
## <a name="com-service-moniker"></a>COM サービス モニカー  
 これにより、標準の WCF クライアントにアンマネージラッパーが提供されます。 この機能は、ネットワーク上で特定のヘッダーを使用せず、コンピューターにデータを保持しません。  
  
## <a name="peer-channel"></a>ピア チャネル  
 ピアチャネルを使用すると、WCF を使用してマルチパーティアプリケーションを開発できます。 マルチパーティ メッセージングはメッシュのコンテキストで実行されます。 メッシュは、ノードが参加できる名前によって識別されます。 ピア チャネル内の各ノードは、ユーザー指定のポートで TCP リスナーを作成し、メッシュ内の他のノードとの接続を確立することによって、回復力を確保します。 メッシュ内の他のノードに接続するため、ノードは、リスナー アドレスやコンピューターの IP アドレスなど一部のデータをメッシュ内の他のノードと交換します。 メッシュ内で送信されるメッセージに送信者に関するセキュリティ情報を入れ、メッセージのなりすましと改ざんを防止することができます。  
  
 エンドユーザーのコンピューターには、個人情報は保存されません。  
  
## <a name="it-professional-experience"></a>IT 専門家向けの機能  
  
### <a name="tracing"></a>トレース  
 WCF インフラストラクチャの診断機能では、トランスポート層とサービスモデルレイヤーを通過するメッセージと、これらのメッセージに関連付けられているアクティビティとイベントがログに記録されます。 この機能は既定で無効になっています。 アプリケーションの構成ファイルを使用して有効にし、実行時に WCF WMI プロバイダーを使用してトレース動作を変更することができます。 有効にすると、トレース インフラストラクチャは、メッセージ、アクティビティ、および処理イベントを含んだ診断トレースを構成済みリスナーに出力します。 出力の形式と場所は、管理者が選択するリスナー構成によって決まりますが、通常は XML 形式のファイルです。 管理者は、トレース ファイルでアクセス制御リスト (ACL) を設定する必要があります。 特に、Windows アクティベーション システム (WAS) でホストするとき、管理者は、ファイルがパブリック仮想ルート ディレクトリから提供されていないことを確認する必要があります (提供されることを望まない場合)。  
  
 トレースには、メッセージ ログとサービス モデル診断トレースの 2 種類があります。この 2 種類のトレースについて次のセクションで説明します。 この 2 つのトレースは、それぞれ <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A> と <xref:System.ServiceModel> というトレース ソースから構成されます。 このログ トレース ソースは両方とも、アプリケーションにローカルなデータを取り込みます。  
  
### <a name="message-logging"></a>メッセージ ログ  
 メッセージ ログのトレース ソース (<xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>) によって管理者は、システムを通過するメッセージをログに記録できます。 ユーザーは構成によって、メッセージ全体またはメッセージ ヘッダーだけをログに記録するかどうか、トランスポート レイヤーまたはサービス モデル レイヤーでログに記録するかどうか、および形式が正しくないメッセージをログに記録するかどうかを設定できます。 また、ユーザーは、フィルター処理を設定して、ログに記録するメッセージを制限できます。  
  
 既定では、メッセージ ログは無効になっています。 ローカル コンピューターの管理者は、アプリケーション レベルの管理者がメッセージ ログを有効にするのを防止できます。  
  
#### <a name="encrypted-and-decrypted-message-logging"></a>暗号化および復号化されるメッセージ ログ  
 メッセージは、以下に説明するようにログに記録され、暗号化および復号化されます。  
  
 トランスポート ログ  
 トランスポート レベルで送受信されるメッセージをログに記録します。 このメッセージは、すべてのヘッダーを含んでいて、ネットワーク上に送信される前と受信されるときに暗号化できます。  
  
 メッセージがネットワーク上に送信される前と受信されるときに暗号化される場合、そのメッセージは暗号化されたままログに記録されます。 例外は、セキュリティ プロトコル (https) を使用する場合です。メッセージは、ネットワーク上で暗号化されている場合でも、送信前と受信後は復号化された状態でログに記録されます。  
  
 サービス ログ  
 チャネル ヘッダー処理が実行された後、ユーザー コードを入力する直前と直後に、サービス モデル レベルで送受信されるメッセージをログに記録します。  
  
 このレベルでログに記録されるメッセージは、ネットワーク上でセキュリティ保護され、暗号化されている場合でも復号化されます。  
  
 形式が正しくないメッセージのログ  
 WCF インフラストラクチャが理解または処理できないメッセージをログに記録します。  
  
 メッセージは、暗号化の有無を問わず、そのままの状態でログに記録されます。  
  
 暗号化されていない形式または暗号化されていない形式でメッセージをログに記録する場合、既定では、WCF は、メッセージをログに記録する前に、セキュリティキーや個人情報を削除します 次のセクションでは、削除される情報と削除が実行される状況について説明します。 キーと個人情報の可能性がある情報をログに記録するには、コンピューターの管理者とアプリケーションを配置するユーザーの両方が、所定の構成操作を実行して既定の動作を変更する必要があります。  
  
#### <a name="information-removed-from-message-headers-when-logging-decryptedunencrypted-messages"></a>復号化されたまたは暗号化されていないメッセージをログに記録するときにメッセージ ヘッダーから削除される情報  
 メッセージが復号化された状態または暗号化されていない状態でログに記録される場合、既定ではメッセージがログに記録される前に、セキュリティ キーと個人情報の可能性がある情報がメッセージ ヘッダーとメッセージ本文から削除されます。 次の一覧は、WCF がキーおよび可能性のある個人情報を考慮することを示しています。  
  
 削除されるキー :  
  
 xmlns: wst = "http://schemas.xmlsoap.org/ws/2004/04/trust" と xmlns: wst = "http://schemas.xmlsoap.org/ws/2005/02/trust" の \-  
  
 wst:BinarySecret  
  
 wst:Entropy  
  
 xmlns: wsse = "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.1.xsd" と xmlns: wsse = "http://docs.oasis-open.org/wss/2005/xx/oasis-2005xx-wss-wssecurity-secext-1.1.xsd" の \-  
  
 wsse:Password  
  
 wsse:Nonce  
  
 削除される個人情報の可能性がある情報 :  
  
 xmlns: wsse = "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.1.xsd" と xmlns: wsse = "http://docs.oasis-open.org/wss/2005/xx/oasis-2005xx-wss-wssecurity-secext-1.1.xsd" の \-  
  
 wsse:Username  
  
 wsse:BinarySecurityToken  
  
 xmlns: saml = "urn: oasis: names: tc: SAML: 1.0: assertion" の \- は、太字 (下) の項目が削除されます。  
  
 \<アサーション  
  
 MajorVersion="1"  
  
 MinorVersion="1"  
  
 AssertionId="[ID]"  
  
 Issuer="[string]"  
  
 IssueInstant="[dateTime]"  
  
 >  
  
 \<条件 NotBefore = "[dateTime]" NotOnOrAfter = "[dateTime]" >  
  
 \<AudienceRestrictionCondition >  
  
 \<オーディエンス > [uri]\</対象ユーザー > +  
  
 \</AudienceRestrictionCondition > *  
  
 \<DoNotCacheCondition/> *  
  
 <\!--抽象基本データ型  
  
 \<条件/> *  
  
 -->  
  
 \</条件 >?  
  
 \<通知 >  
  
 \<AssertionIDReference > [ID]\</AssertionIDReference > *  
  
 \<Assertion > [assertion]\</assertion > *  
  
 [any]*  
  
 \</アドバイス > ですか?  
  
 <\!--抽象基本型  
  
 \<ステートメント/> *  
  
 \<SubjectStatement >  
  
 \<サブジェクト >  
  
 `<NameIdentifier`  
  
 `NameQualifier="[string]"?`  
  
 `Format="[uri]"?`  
  
 `>`  
  
 `[string]`  
  
 `</NameIdentifier>?`  
  
 \<SubjectConfirmation >  
  
 \<ConfirmationMethod > [anyUri]\</ConfirmationMethod > +  
  
 \<SubjectConfirmationData > [any]\</SubjectConfirmationData >?  
  
 \<ds: KeyInfo >...\</ds: KeyInfo >?  
  
 \</> 確認しますか?  
  
 \</件名 >  
  
 \</subjectstatement > *  
  
 -->  
  
 \<AuthenticationStatement  
  
 AuthenticationMethod="[uri]"  
  
 AuthenticationInstant="[dateTime]"  
  
 >  
  
 [Subject]  
  
 `<SubjectLocality`  
  
 `IPAddress="[string]"?`  
  
 `DNSAddress="[string]"?`  
  
 `/>?`  
  
 < AuthorityBinding  
  
 AuthorityKind="[QName]"  
  
 Location="[uri]"  
  
 Binding="[uri]"  
  
 />*  
  
 \</authenticationstatement > *  
  
 \<AttributeStatement >  
  
 [Subject]  
  
 \<属性  
  
 AttributeName="[string]"  
  
 AttributeNamespace="[uri]"  
  
 >  
  
 `<AttributeValue>[any]</AttributeValue>+`  
  
 \</属性 > +  
  
 \</attributestatement > *  
  
 \<Authorizationdecisionstatement に  
  
 Resource="[uri]"  
  
 Decision="[Permit&#124;Deny&#124;Indeterminate]"  
  
 >  
  
 [Subject]  
  
 \<Action Namespace = "[uri]" > [string]\</Action > +  
  
 証拠 > の \<  
  
 \<AssertionIDReference > [ID]\</AssertionIDReference > +  
  
 \<Assertion > [assertion]\</assertion > +  
  
 \</証拠 > ですか?  
  
 \</AuthorizationDecisionStatement > *  
  
 \</アサーション >  
  
#### <a name="information-removed-from-message-bodies-when-logging-decryptedunencrypted-messages"></a>復号化されたまたは暗号化されていないメッセージをログに記録するときにメッセージ本文から削除される情報  
 前に説明したように、WCF では、暗号化された暗号化されていないメッセージや暗号化されていないメッセージのメッセージヘッダーから、キーと既知の可能性がある さらに、WCF は、キー交換に関連するセキュリティメッセージについて説明する次の一覧の本文要素とアクションの本文から、キーと既知の可能性がある個人情報を削除します。  
  
 次の名前空間の場合 :  
  
 xmlns: wst = "http://schemas.xmlsoap.org/ws/2004/04/trust" と xmlns: wst = "http://schemas.xmlsoap.org/ws/2005/02/trust" (たとえば、使用可能なアクションがない場合)  
  
 キー交換を伴うこの本文要素について情報が削除されます。  
  
 wst:RequestSecurityToken  
  
 wst:RequestSecurityTokenResponse  
  
 wst:RequestSecurityTokenResponseCollection  
  
 次の各アクションについても情報が削除されます。  
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Issue`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Issue`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Validate`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Validate`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Amend`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Amend`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RST/SCT`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RSTR/SCT`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RST/SCT-Amend`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RSTR/SCT-Amend`
  
#### <a name="no-information-is-removed-from-application-specific-headers-and-body-data"></a>情報が削除されないアプリケーション固有のヘッダーと本文データ  
 WCF では、アプリケーション固有のヘッダー (クエリ文字列など) や本文データ (クレジットカード番号など) の個人情報は追跡されません。  
  
 メッセージ ログが有効な場合、アプリケーション固有のヘッダーと本文情報にある個人情報はログに表示される可能性があります。 そのため、この場合もアプリケーションを配置するユーザーが、構成ファイルとログ ファイルに対する ACL を設定する必要があります。 また、この情報を表示したくない場合はログ記録を無効にしたり、ログ記録後にログファイルからこの情報を除外したりすることもできます。  
  
### <a name="service-model-tracing"></a>サービス モデル トレース  
 サービス モデルのトレース ソース (<xref:System.ServiceModel>) によって、メッセージ処理に関するアクティビティとイベントのトレースを実行できます。 この機能では、<xref:System.Diagnostics> から .NET Framework 診断機能を使用します。 <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A> プロパティと同様、場所とその ACL は、.NET Framework アプリケーションの構成ファイルを使用してユーザーが設定できます。 メッセージ ログと同様、管理者がトレースを有効にするときに必ずファイルの場所が設定されるので、管理者は ACL を制御できます。  
  
 スコープ内のメッセージに含まれるメッセージ ヘッダーをトレースします。 前のセクションで説明した、メッセージ ヘッダーに含まれる個人情報の可能性がある情報を非表示にする場合と同じ規則が適用されます。既定では、先に示した個人情報はトレース時にヘッダーから削除されます。 個人情報の可能性がある情報をログに記録するには、コンピューターの管理者とアプリケーションを配置するユーザーの両方が構成を変更する必要があります。 ただし、アプリケーション固有のヘッダーにある個人情報はトレース時にログに記録されます。 アプリケーションを配置するユーザーは、構成ファイルとトレース ファイルに対する ACL を設定する必要があります。 また、トレースを無効にしてこの情報を非表示にしたり、ログ記録後にトレースファイルからこの情報を除外したりすることもできます。  
  
 ServiceModel トレースの一部として、一意な ID (呼び出されたアクティビティ ID、通常は GUID) は、インフラストラクチャのさまざまな部分を通過するメッセージとして、異なる複数のアクティビティを結合します。  
  
#### <a name="custom-trace-listeners"></a>カスタム トレース リスナー  
 メッセージ ログとトレースの両方で、カスタム トレース リスナーを構成できます。このリスナーは、ネットワーク上で、リモート データベースなどにトレースとメッセージを送信できます。 アプリケーションを配置するユーザーは、カスタム リスナーを構成するか、ユーザーがリスナーを構成できるようにする必要があります。 また、リモートの場所で公開されている個人情報や、この場所に Acl を適切に適用するためにも責任を持ちます。  
  
### <a name="other-features-for-it-professionals"></a>IT 専門家向けのその他の機能  
 WCF には、WMI (Windows に付属) を介して WCF インフラストラクチャの構成情報を公開する WMI プロバイダーがあります。 既定では、WMI インターフェイスは管理者が利用できます。  
  
 WCF 構成では、.NET Framework 構成メカニズムを使用します。 構成ファイルはコンピューターに保存されます。 アプリケーション開発者と管理者が、アプリケーションの要件に応じて構成ファイルと ACL を作成します。 構成ファイルには、エンドポイント アドレスと証明書ストア内の証明書へのリンクを入れることができます。 証明書を使用してアプリケーション データを提供し、アプリケーションによって使用される機能の各プロパティを構成することができます。  
  
 また、WCF では、<xref:System.Environment.FailFast%2A> メソッドを呼び出すことによって .NET Framework プロセスダンプ機能も使用します。  
  
### <a name="it-pro-tools"></a>IT 専門家のツール  
 また、WCF には、Windows SDK に付属する次の IT プロフェッショナル向けツールも用意されています。  
  
#### <a name="svctraceviewerexe"></a>SvcTraceViewer.exe  
 ビューアーには、WCF トレースファイルが表示されます。 このビューアーは、トレースに含まれているすべての情報を表示します。  
  
#### <a name="svcconfigeditorexe"></a>SvcConfigEditor.exe  
 ユーザーは、エディターを使用して、WCF 構成ファイルを作成および編集できます。 このエディターは、構成ファイルに含まれているすべての情報を表示します。 テキスト エディターでも同じタスクを実行できます。  
  
#### <a name="servicemodel_reg"></a>ServiceModel_Reg  
 このツールでは、コンピューター上の ServiceModel のインストールを管理できます。 このツールでは、実行時にステータスメッセージがコンソールウィンドウに表示され、プロセス内で WCF インストールの構成に関する情報が表示される場合があります。  
  
#### <a name="wsatconfigexe-and-wsatuidll"></a>WSATConfig.exe と WSATUI.dll  
 これらのツールを使用すると、IT 担当者は WCF で相互運用可能な ws-atomictransaction ネットワークサポートを構成できます。 このツールは、レジストリに格納された最も一般的に使用される WS-AtomicTransaction 設定の値を表示し、ユーザーはこのツールを使用してその値を変更できます。  
  
## <a name="cross-cutting-features"></a>広範囲に使用できる機能  
 次の機能は広範囲に使用できます。 つまり、前述の任意の機能と共に構成できます。  
  
### <a name="service-framework"></a>サービス フレームワーク  
 ヘッダーには、インスタンス ID を入れることができます。このインスタンス ID は、メッセージを CLR クラスのインスタンスに関連付ける GUID です。  
  
 Web サービス記述言語 (WSDL) には、ポートの定義が入ります。 各ポートには、エンドポイント アドレス、およびアプリケーションが使用するサービスを表すバインディングがあります。 WSDL の公開は、構成を使用して無効にできます。 コンピューターに保持される情報はありません。  
  
## <a name="see-also"></a>参照

- [Windows Communication Foundation](index.md)
- [セキュリティ](./feature-details/security.md)
