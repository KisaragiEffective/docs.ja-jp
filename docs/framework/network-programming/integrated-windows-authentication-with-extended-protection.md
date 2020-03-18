---
title: 拡張保護付き統合 Windows 認証
ms.date: 03/30/2017
ms.assetid: 81731998-d5e7-49e4-ad38-c8e6d01689d0
ms.openlocfilehash: c4afc008f600c9be0040f8d7623f5e20623dfd7d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74444231"
---
# <a name="integrated-windows-authentication-with-extended-protection"></a>拡張保護付き統合 Windows 認証
<xref:System.Net.HttpWebRequest> 名前空間および関連名前空間の <xref:System.Net.HttpListener>、<xref:System.Net.Mail.SmtpClient>、<xref:System.Net.Security.SslStream>、<xref:System.Net.Security.NegotiateStream>、<xref:System.Net>、および関連クラスによる統合 Windows 認証の処理方法に影響を与える、機能強化が行われました。 セキュリティ強化のため、拡張保護のサポートが追加されました。  
  
 これらの変更は、これらのクラスを使用して Web 要求を作成し、統合 Windows 認証が使用されている応答を受信するアプリケーションに影響を及ぼす場合があります。 この変更は、統合 Windows 認証を使用するように構成されている Web サーバーおよびクライアント アプリケーションにも影響を与える可能性があります。  
  
 これらの変更は、これらのクラスを使用して他の種類の要求を作成し、統合 Windows 認証が使用されている応答を受信するアプリケーションにも影響を及ぼす場合があります。  
  
 拡張保護をサポートするための変更は、Windows 7 と Windows Server 2008 R2 でのアプリケーションでのみ使用できます。 拡張保護機能は、Windows の以前のバージョンではご利用いただけません。  
  
## <a name="overview"></a>概要  
 統合 Windows 認証の設計では、一部の資格情報のチャレンジ応答をユニバーサルにすることを許可しています。つまり、これらを再利用または転送することができます。 チャレンジ応答は、少なくともターゲット固有の情報と、可能であればいくつかのチャネル固有の情報で構築する必要があります。 これによりサービスは拡張保護を提供して、資格情報のチャレンジ応答にサービス プリンシパル名 (SPN) などのサービス固有の情報が確実に含まれるようにすることができます。 資格情報の交換でこの情報を使用すると、不適切に使用されている可能性がある資格情報のチャレンジ応答の悪用に対してサービスの保護を強化することができます。  
  
 拡張保護の設計は、認証リレー攻撃を軽減することを目的とした認証プロトコルの拡張機能です。 これはチャネルとサービス バインディング情報の概念を中心に展開しています。  
  
 全体的な目標は次のとおりです。  
  
1. 拡張保護をサポートするためにクライアントが更新された場合、アプリケーションがチャネル バインディングとサービス バインディングの情報をサポートされるすべての認証プロトコルに提供する必要があります。 チャネル バインディング情報は、バインドするチャネル (TLS) がある場合にのみ提供できます。 サービス バインディング情報は、常に提供される必要があります。  
  
2. 正しく構成されている更新されたサーバーは、チャネル バインディング情報およびサービス バインディング情報がクライアントの認証トークンにある場合にそれを検証し、チャネル バインディングが一致しない場合は、認証の試行を拒否する場合があります。 展開シナリオによっては、サーバーはチャネル バインディング、サービス バインディングの一方または両方の検証ができます。  
  
3. 更新されたサーバーには、ポリシーに基づいて、チャネル バインディング情報を含まない下位クライアントの要求を承認または拒否することができます。  
  
 拡張保護で使用される情報は、次の 2 つの部分の一方または両方で構成されます。  
  
1. チャネル バインディング トークンまたは CBT。  
  
2. サービス プリンシパル名 (SPN) 形式でのサービス バインディング情報。  
  
 サービス バインディング情報は、特定のサービス エンドポイントを認証するクライアントの意思表示です。 これは、次のプロパティを使用してクライアントからサーバーに伝達されます。  
  
- SPN 値は、クライアント認証を実行するサーバーにクリア テキスト形式で提供する必要があります。  
  
- SPN の値はパブリックです。  
  
- SPN は、man-in-the-middle 攻撃で値が挿入、削除、または変更されないように、転送中は暗号で保護する必要があります。  
  
 CBT は、クライアントが認証した内部チャネルを介して会話に結び付ける (バインド) ために使用されるセキュリティで保護された外部チャネル (TLS など) のプロパティです。 CBT には、次のプロパティ (これらも IETF RFC 5056 で定義されています) が必要です。  
  
- 外部チャネルが存在する場合は、CBT の値は、外部チャネル、またはサーバーのエンドポイントのどちらが、会話のクライアント側とサーバー側の両方によって個別に到着したかを識別するプロパティである必要があります。  
  
- クライアントによって送信された CBT の値は、攻撃者が影響を与えられるものであってはなりません。  
  
- CBT 値の機密性については、保証は一切されません。 ただしこれは、CBT を実行するプロトコルが暗号化している可能性があるため、サービス バインディング情報とチャネル バインディング情報の値が、認証を実行するサーバー以外で常に検査されることを意味しているわけではありません。  
  
- CBT は、攻撃者が値を挿入、削除、または変更できないようにするため、転送中は暗号で完全に保護する必要があります。  
  
 チャネル バインディングは、クライアントが改ざんを防止する方法で SPN および CBT をサーバーに転送することで実現されます。 サーバーは、そのポリシーに従ってチャネル バインディング情報を検証し、目的のターゲットであると信じられない認証試行を拒否します。 これにより、2 つのチャンネルが暗号でバインドされます。  
  
 既存のクライアントとアプリケーションとの互換性を維持するため、拡張保護をまだサポートしていないクライアントによるサーバー認証の試行を許可するようにサーバーを構成できます。 これは、"完全にセキュリティを強化した" 構成と対比して、"部分的にセキュリティを強化した" 構成と呼ばれます。  
  
 <xref:System.Net> 名前空間と <xref:System.Net.Security> 名前空間内の複数のコンポーネントは、呼び出し元のアプリケーションに代わって統合 Windows 認証を実行します。 このセクションでは、統合 Windows 認証の使用で拡張保護を追加するための System.Net コンポーネントへの変更について説明します。  
  
 拡張保護は、Windows 7 で現在サポートされています。 オペレーティング システムで拡張保護がサポートされているかをどうかをアプリケーションが判断できるように、メカニズムが提供されています。  
  
## <a name="changes-to-support-extended-protection"></a>拡張保護をサポートするための変更  
 使用される認証プロトコルに応じて、統合 Windows 認証で使用される認証プロセスには、多くの場合、ターゲット コンピューターによって発行され、クライアント コンピューターに送り返されるチャレンジが含まれています。 拡張保護によりこの認証プロセスに新しい機能が追加されます。  
  
 <xref:System.Security.Authentication.ExtendedProtection> 名前空間は、アプリケーションの拡張保護を使用した認証のサポートを提供します。 この名前空間内の <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> クラスは、チャネル バインディングを表します。 この名前空間内の <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> クラスは、受信クライアント接続を検証するためにサーバーで使用される拡張保護ポリシーを表します。 他のクラス メンバーは、拡張保護で使用されます。  
  
 サーバー アプリケーションの場合、これらのクラスには次が含まれます。  
  
 次の要素を持つ <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>:  
  
- オペレーティング システムが統合 Windows 認証と拡張保護をサポートしているかどうかを示す <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy.OSSupportsExtendedProtection%2A> プロパティ。  
  
- 拡張保護ポリシーを適用するタイミングを示す <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> 値。  
  
- 展開シナリオを示す <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> 値。 これは、拡張保護がチェックされる方法に影響します。  
  
- 認証の目的のターゲットとしてクライアントから提供された SPN と照合するために使用されるカスタム SPN のリストを含む <xref:System.Security.Authentication.ExtendedProtection.ServiceNameCollection> (省略可能)。  
  
- 検証に使用するカスタム チャネル バインディングを含む <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> (省略可能)。 このシナリオは、一般的なケースではありません。  
  
 <xref:System.Security.Authentication.ExtendedProtection.Configuration> 名前空間は、アプリケーションの拡張保護を使用した認証の構成のサポートを提供します。  
  
 既存の <xref:System.Net> 名前空間で拡張保護をサポートするため、多くの機能変更が行われました。 これらの変更には、以下が含まれます。  
  
- トランスポート コンテキストを表す <xref:System.Net.TransportContext> 名前空間に追加された新しい <xref:System.Net> クラス。  
  
- クライアント アプリケーションの拡張保護をサポートするために <xref:System.Net.HttpWebRequest.EndGetRequestStream%2A> の取得を許可する <xref:System.Net.HttpWebRequest.GetRequestStream%2A> クラス内の新しい <xref:System.Net.HttpWebRequest> および <xref:System.Net.TransportContext> オーバーロード メソッド。  
  
- サーバー アプリケーションをサポートするための <xref:System.Net.HttpListener> クラスと <xref:System.Net.HttpListenerRequest> クラスへの追加。  
  
 既存の <xref:System.Net.Mail> 名前空間で SMTP クライアント アプリケーションの拡張保護をサポートするため、次の機能変更が行われました。  
  
- SMTP クライアント アプリケーションに拡張保護を使用する際に、認証に使用する SPN を表す <xref:System.Net.Mail.SmtpClient.TargetName%2A> クラス内の <xref:System.Net.Mail.SmtpClient> プロパティ。  
  
 既存の <xref:System.Net.Security> 名前空間で拡張保護をサポートするため、多くの機能変更が行われました。 これらの変更には、以下が含まれます。  
  
- クライアント アプリケーションの拡張保護をサポートするために CBT を渡すことを許可する <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsClient%2A> クラス内の新しい <xref:System.Net.Security.NegotiateStream.AuthenticateAsClient%2A> および <xref:System.Net.Security.NegotiateStream> オーバーロード メソッド。  
  
- サーバー アプリケーションの拡張保護をサポートするために <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsServer%2A> を渡すことを許可する <xref:System.Net.Security.NegotiateStream.AuthenticateAsServer%2A> クラス内の新しい <xref:System.Net.Security.NegotiateStream> および <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> オーバーロード メソッド。  
  
- クライアントおよびサーバー アプリケーションの拡張保護をサポートするための <xref:System.Net.Security.SslStream.TransportContext%2A> クラス内の新しい <xref:System.Net.Security.SslStream> プロパティ。  
  
 <xref:System.Net.Configuration.SmtpNetworkElement> 名前空間で SMTP クライアントに拡張保護の構成をサポートするため、<xref:System.Net.Security> プロパティが追加されました。  
  
## <a name="extended-protection-for-client-applications"></a>クライアント アプリケーションの拡張保護  
 ほとんどのクライアント アプリケーションの拡張保護のサポートは自動的に行われます。 <xref:System.Net.HttpWebRequest> クラスと <xref:System.Net.Mail.SmtpClient> クラスは、基本の Windows のバージョンが拡張保護をサポートしている場合は常に、拡張保護をサポートします。 <xref:System.Net.HttpWebRequest> インスタンスは <xref:System.Uri> から構築された SPN を送信します。 既定では、<xref:System.Net.Mail.SmtpClient> インスタンスは SMTP メール サーバーのホスト名から構築された SPN を送信します。  
  
 カスタム認証の場合、クライアント アプリケーションは、<xref:System.Net.HttpWebRequest.EndGetRequestStream%28System.IAsyncResult%2CSystem.Net.TransportContext%40%29?displayProperty=nameWithType> メソッドを使用して <xref:System.Net.HttpWebRequest.GetRequestStream%28System.Net.TransportContext%40%29?displayProperty=nameWithType> および CBT の取得を許可する <xref:System.Net.HttpWebRequest> クラス内で <xref:System.Net.TransportContext> メソッドまたは <xref:System.Net.TransportContext.GetChannelBinding%2A> メソッドを使用できます。  
  
 <xref:System.Net.HttpWebRequest> インスタンスによって指定されたサービスに送信された統合 Windows 認証に使用する SPN は、<xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A> プロパティを設定することでオーバーライドすることができます。  
  
 <xref:System.Net.Mail.SmtpClient.TargetName%2A> プロパティは、SMTP 接続の統合 Windows 認証にカスタム SPN を使用するように設定するために使用できます。  
  
## <a name="extended-protection-for-server-applications"></a>サーバー アプリケーションの拡張保護  
 <xref:System.Net.HttpListener> は、HTTP 認証を実行するときに、サービス バインディングを検証するためのメカニズムを提供します。  
  
 最も安全なシナリオは、HTTPS:// プレフィックスの拡張保護を有効にすることです。 この場合、<xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> または <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> に設定された <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> を使用して <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> を <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> に設定し、<xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> を <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected> に設定します。<xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> の値は、<xref:System.Net.HttpListener> を部分的にセキュリティを強化したモードにし、同時に <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> は完全にセキュリティを強化したモードに対応します。  
  
 この構成では、セキュリティで保護された外部チャネル経由でサーバーに要求が作成されると、その外部チャネルがチャネル バインディングに対して照会されます。 このチャネル バインディングは、認証 BLOB でチャネル バインディングが一致することを検証する認証の SSPI 呼び出しに渡されます。 考えられる結果は 3 つあります。  
  
1. サーバーの基本のオペレーティング システムが拡張保護をサポートしません。 要求がアプリケーションに公開されず、権限がないという応答 (401) がクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
2. SSPI 呼び出しの失敗は、クライアントが外部チャネルから取得した予期される値と一致しなかったチャネル バインディングを指定したか、またはサーバーで拡張保護ポリシーが <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> に設定された場合に、クライアントがチャネル バインディングの提供に失敗したことを示します。 どちらの場合も、要求はアプリケーションに公開されず、権限がないという応答 (401) がクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
3. クライアントは正しいチャネル バインディングを指定します。または、サーバーで拡張保護ポリシーが <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> で構成されているため、チャネル バインディングを指定せずにクライアントは接続することができます。要求は処理のためにアプリケーションに返されます。 サービス名チェックは自動的に実行されません。 アプリケーションは、<xref:System.Net.HttpListenerRequest.ServiceName%2A> プロパティを使用して独自のサービス名の検証を実行するように選択できますが、こうした状況では冗長です。  
  
 アプリケーションが独自の SSPI 呼び出しを作成して、HTTP 要求の本体内でやり取りされた BLOB に基づいて認証を実行し、チャネル バインディングをサポートする場合は、チャネル バインディングをネイティブ Win32 <xref:System.Net.HttpListener>AcceptSecurityContext[ 関数に渡すため、](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) を使用して、セキュリティで保護された外部チャネルから、期待されるチャネル バインディングを取得する必要があります。 これを行うには、<xref:System.Net.HttpListenerRequest.TransportContext%2A> プロパティを使用して <xref:System.Net.TransportContext.GetChannelBinding%2A> メソッドを呼び出して CBT を取得します。 エンドポイント バインディングのみがサポートされます。 <xref:System.Security.Authentication.ExtendedProtection.ChannelBindingKind.Endpoint> 以外のものが指定されると、<xref:System.NotSupportedException> がスローされます。 基本のオペレーティング システムがチャネル バインディングをサポートしている場合、<xref:System.Net.TransportContext.GetChannelBinding%2A> メソッドがポインターをラップする <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding><xref:System.Runtime.InteropServices.SafeHandle> を、[AcceptSecurityContext](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) 関数に `pInput` パラメーターで渡される SecBuffer 構造体の pvBuffer メンバーとして渡すのに適したチャネル バインディングに返します。 <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding.Size%2A> プロパティには、チャネル バインディングの長さ (バイト単位) が含まれます。 基本のオペレーティング システムがチャネル バインディングをサポートしていない場合、関数は `null` を返します。  
  
 もう 1 つの考えられるシナリオは、プロキシが使用されていないときに、HTTP:// プレフィックスの拡張保護を有効にすることです。 この場合、<xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> または <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> に設定された <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> を使用して <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> を <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> に設定し、<xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> を <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected> に設定します。<xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> の値は、<xref:System.Net.HttpListener> を部分的にセキュリティを強化したモードにし、同時に <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> は完全にセキュリティを強化したモードに対応します。  
  
 許可されているサービス名の既定のリストが、<xref:System.Net.HttpListener> に登録されているプレフィックスに基づいて作成されます。 この既定のリストは、<xref:System.Net.HttpListener.DefaultServiceNames%2A> プロパティを通じて調べることができます。 このリストが包括的なものでない場合、アプリケーションは、既定のサービス名のリストの代わりに使用される <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> クラスのコンストラクターでカスタム サービス名のコレクションを指定できます。  
  
 この構成では、セキュリティで保護された外部チャネルなしでサーバーに対して要求が作成された場合に、認証は通常、チャネル バインディングのチェックなしで実行されます。 認証に成功した場合、クライアントが提供したサービス名に対してコンテキストが照会され、許容されるサービス名のリストに照らして検証されます。 考えられる結果は 4 つあります。  
  
1. サーバーの基本のオペレーティング システムが拡張保護をサポートしません。 要求がアプリケーションに公開されず、権限がないという応答 (401) がクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
2. クライアントの基本のオペレーティング システムが拡張保護をサポートしません。 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 構成では、認証の試行が成功し、要求がアプリケーションに返されます。 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> 構成では、認証の試行は失敗します。 要求がアプリケーションに公開されず、権限がないという応答 (401) がクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
3. クライアントの基本のオペレーティング システムは拡張保護をサポートしますが、アプリケーションはサービス バインディングを指定しませんでした。 要求がアプリケーションに公開されず、権限がないという応答 (401) がクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
4. クライアントはサービス バインディングを指定しました。 サービス バインディングは、許可されているサービス バインディングのリストと比較されます。 一致すると、要求がアプリケーションに返されます。 そうでない場合、要求がアプリケーションに公開されず、権限がないという応答 (401) が自動的にクライアントに返されます。 失敗の理由を明らかにするメッセージが <xref:System.Net.HttpListener> トレース ソースに記録されます。  
  
 許容されるサービス名の許可リストを使用するこの単純な方法が不十分な場合、アプリケーションは <xref:System.Net.HttpListenerRequest.ServiceName%2A> プロパティのクエリを実行することで、独自のサービス名の検証を提供できます。 上記の 1 および 2 の場合、プロパティは `null` を返します。 3 の場合、空の文字列を返します。 4 の場合、クライアントによって指定されたサービス名が返されます。  
  
 これらの拡張保護機能は、サーバー アプリケーションによって、他の種類の要求での認証に使用したり、信頼されたプロキシが使用されている場合に使用することもできます。  
  
## <a name="see-also"></a>参照

- <xref:System.Security.Authentication.ExtendedProtection>
- <xref:System.Security.Authentication.ExtendedProtection.Configuration>
