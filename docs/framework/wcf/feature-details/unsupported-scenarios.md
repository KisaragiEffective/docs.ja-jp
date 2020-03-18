---
title: サポートされていないシナリオ
ms.date: 03/30/2017
ms.assetid: 72027d0f-146d-40c5-9d72-e94392c8bb40
ms.openlocfilehash: b643e6df8a877860ce36fc6ee34c4e4ca08ec748
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921157"
---
# <a name="unsupported-scenarios"></a>サポートされていないシナリオ

Windows Communication Foundation (WCF) では、さまざまな理由により、特定のセキュリティシナリオがサポートされていません。 たとえば、Windows XP Home Edition では SSPI または Kerberos 認証プロトコルが実装されていないため、WCF では、そのプラットフォームで Windows 認証を使用したサービスの実行はサポートされていません。 Windows XP Home Edition で WCF を実行する場合は、ユーザー名/パスワードや HTTP/HTTPS 統合認証などの他の認証メカニズムがサポートされます。

## <a name="impersonation-scenarios"></a>権限借用のシナリオ

### <a name="impersonated-identity-might-not-flow-when-clients-make-asynchronous-calls"></a>クライアントが非同期呼び出しを行うときに、偽装された id が流れないことがある
 WCF クライアントが、偽装で Windows 認証を使用して WCF サービスへの非同期呼び出しを行うと、偽装 ID ではなくクライアント プロセスの ID で認証が行われる場合があります。

### <a name="windows-xp-and-secure-context-token-cookie-enabled"></a>Windows XP およびセキュリティで保護されたコンテキストトークン cookie 有効

WCF では偽装がサポートされていないため、次の条件に該当する場合は <xref:System.InvalidOperationException> がスローされます。

- オペレーティングシステムは Windows XP です。

- 認証モードで Windows ID が生成された。

- <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> の <xref:System.ServiceModel.OperationBehaviorAttribute> プロパティが <xref:System.ServiceModel.ImpersonationOption.Required> に設定されています。

- 状態ベースのセキュリティ コンテキスト トークン (SCT: Security Context Token) が作成された (既定では、作成は無効になっています)。

 状態ベースの SCT はカスタム バインディングの使用によってのみ作成できます。 詳細については、「[方法: セキュリティで保護されたセッションのセキュリティコンテキストトークンを作成](how-to-create-a-security-context-token-for-a-secure-session.md)する」を参照してください。コードでは、トークンは <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement%28System.Boolean%29?displayProperty=nameWithType> または <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%28System.ServiceModel.Channels.SecurityBindingElement%2CSystem.Boolean%29?displayProperty=nameWithType> メソッドを使用してセキュリティバインド要素 (<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> または <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>) を作成し、`requireCancellation` パラメーターを `false`に設定することによって有効にします。 このパラメーターは、SCT のキャッシュを参照します。 値を `false` に設定することによって、状態ベースの SCT 機能が有効になります。

 または、構成において、<`customBinding`> を作成し、<`security`> 要素を追加して `authenticationMode` 属性に SecureConversation および `requireSecurityContextCancellation` 属性に `true` を設定することによってもトークンが有効になります。

> [!NOTE]
> 上記の要件は限定的です。 たとえば、<xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A> は Windows ID を生成するバインド要素を作成しますが、SCT を確立しません。 そのため、Windows XP の `Required` オプションと共に使用できます。

### <a name="possible-aspnet-conflict"></a>考えられる ASP.NET の競合

WCF と ASP.NET は、どちらも偽装を有効または無効にすることができます。 ASP.NET が WCF アプリケーションをホストする場合、WCF と ASP.NET の構成設定の間に競合が存在する可能性があります。 競合が発生した場合は、<xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> プロパティが <xref:System.ServiceModel.ImpersonationOption.NotAllowed>に設定されていない限り、WCF 設定が優先されます。この場合、ASP.NET impersonation 設定が優先されます。

### <a name="assembly-loads-may-fail-under-impersonation"></a>アセンブリの読み込みが偽装の下で失敗することがある

偽装されたコンテキストにアセンブリを読み込むためのアクセス権がない場合、共通言語ランタイム (CLR: Common Language Runtime) が AppDomain のアセンブリを初めて読み込もうとしたときに、その <xref:System.AppDomain> はエラーをキャッシュします。 この場合、偽装を元に戻した後、元に戻されたコンテキストにアセンブリを読み込むためのアクセス権があったとしても、それ以降のアセンブリの読み込みは失敗します。 これは、ユーザーコンテキストが変更された後に CLR が読み込みを再試行しないためです。 このエラーから回復するには、アプリケーション ドメインを再起動する必要があります。

> [!NOTE]
> <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> クラスの <xref:System.ServiceModel.Security.WindowsClientCredential> プロパティの既定値は <xref:System.Security.Principal.TokenImpersonationLevel.Identification> です。 ほとんどの場合、ID レベルの偽装コンテキストには、追加のアセンブリを読み込むための権限がありません。 これは既定値であるため、非常に一般的な状態として認識しておく必要があります。 ID レベルの偽装は、偽装プロセスが `SeImpersonate` 権限を持たない場合にも発生します。 詳細については、「[委任と偽装](delegation-and-impersonation-with-wcf.md)」を参照してください。

### <a name="delegation-requires-credential-negotiation"></a>委任には資格情報ネゴシエーションが必要

委任で Kerberos 認証プロトコルを使用するには、資格情報ネゴシエーションを使用する Kerberos プロトコル ("マルチレッグ" Kerberos または "マルチステップ" Kerberos とも呼ばれます) を実装する必要があります。 資格情報ネゴシエーションを使用しない Kerberos 認証 (ワンショット Kerberos またはシングルレッグ Kerberos とも呼ばれる) を実装した場合は、例外がスローされます。 資格情報ネゴシエーションを実装する方法の詳細については、「 [Windows 認証エラーのデバッグ](debugging-windows-authentication-errors.md)」を参照してください。

## <a name="cryptography"></a>暗号

### <a name="sha-256-supported-only-for-symmetric-key-usages"></a>SHA-256 は、対称キーの使用に対してのみサポートされます。

WCF では、システム指定のバインディングでアルゴリズムスイートを使用して指定できる、さまざまな暗号化および署名ダイジェスト作成アルゴリズムがサポートされています。 セキュリティを強化するために、WCF では、署名ダイジェストハッシュを作成するためのセキュアハッシュアルゴリズム (SHA) 2 アルゴリズム (具体的には SHA-256) がサポートされています。 このリリースは、Kerberos キーなどの対称キーを使用する場合、およびメッセージに署名するために X.509 証明書を使用しない場合に限り SHA-256 をサポートします。 現在のところ、WCF では、x.509 証明256書で使用される RSA 署名はサポートされていません。これは、WinFX で RSA SHA256 がサポートされていないためです。

### <a name="fips-compliant-sha-256-hashes-not-supported"></a>FIPS 準拠の SHA-256 ハッシュはサポートされていません

WCF では、256 SHA-1 FIPS 準拠ハッシュはサポートされていないため、FIPS 準拠アルゴリズムを使用するシステムでは、SHA-256 を使用するアルゴリズムスイートは WCF ではサポートされません。

### <a name="fips-compliant-algorithms-may-fail-if-registry-is-edited"></a>レジストリを編集すると、FIPS 準拠のアルゴリズムが失敗することがある

連邦情報処理規格 (FIPS: Federal Information Processing Standards) 準拠のアルゴリズムを有効または無効にするには、Microsoft 管理コンソール (MMC: Microsoft Management Console) のローカル セキュリティ設定スナップインを使用します。 レジストリでこの設定にアクセスすることもできます。 ただし、WCF では、レジストリを使用した設定のリセットはサポートされていません。 値を 1 または 0 以外に設定すると、CLR とオペレーティング システム間で不整合が発生する可能性があります。

### <a name="fips-compliant-aes-encryption-limitation"></a>FIPS 準拠の AES 暗号化の制限

FIPS 準拠の AES 暗号化は、識別レベルの権限借用の下にある双方向コールバックでは機能しません。

### <a name="cngksp-certificates"></a>CNG/KSP 証明書

*CRYPTOGRAPHY API: Next Generation (CNG)* は、CryptoAPI の長期的な後継です。 この API は、windows Vista、Windows Server 2008、およびそれ以降の Windows バージョンのアンマネージコードで使用できます。

 .NET Framework 4.6.1 以前のバージョンでは、従来の CryptoAPI を使用して CNG/KSP 証明書を処理するため、これらの証明書はサポートされません。 これらの証明書を .NET Framework 4.6.1 以前のバージョンで使用すると、例外が発生します。

 証明書に KSP が使用されているかどうかを知る方法は 2 つあります。

- `p/invoke` を `CertGetCertificateContextProperty` に対して実行し、返された `dwProvType` で `CertGetCertificateContextProperty` を調べる。

- 証明書を照会するには、コマンドラインから `certutil` コマンドを使用します。 詳細については、「[証明書のトラブルシューティングに関する Certutil タスク](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc772619(v=ws.10))」を参照してください。

## <a name="message-security-fails-if-using-aspnet-impersonation-and-aspnet-compatibility-is-required"></a>ASP.NET 権限借用を使用し、ASP.NET compatibility が必要な場合は、メッセージセキュリティが失敗する

WCF では、クライアント認証が行われないようにするため、次の設定の組み合わせはサポートされていません。

- ASP.NET の偽装が有効になっています。 これは、web.config ファイルで、<`identity`> 要素の `impersonate` 属性を `true`に設定することによって行われます。

- ASP.NET 互換モードを有効にするには[\<serviceHostingEnvironment >](../../configure-apps/file-schema/wcf/servicehostingenvironment.md)の `aspNetCompatibilityEnabled` 属性を `true`に設定します。

- メッセージ モード セキュリティを使用している。

回避策として、ASP.NET 互換モードをオフにします。 または、ASP.NET 互換モードが必要な場合は、ASP.NET 権限借用機能を無効にし、代わりに WCF によって提供される偽装を使用します。 詳細については、「[委任と偽装](delegation-and-impersonation-with-wcf.md)」を参照してください。

## <a name="ipv6-literal-address-failure"></a>IPv6 リテラルアドレスのエラー

クライアントとサービスが同じコンピューター上に存在し、サービスに対して IPv6 リテラル アドレスが使用されている場合は、セキュリティ要求が失敗します。

 IPv6 リテラル アドレスは、サービスとクライアントがそれぞれ異なるコンピューター上に存在する場合に機能します。

## <a name="wsdl-retrieval-failures-with-federated-trust"></a>フェデレーション信頼での WSDL の取得エラー

WCF では、フェデレーション信頼チェーン内のノードごとに1つの WSDL ドキュメントが必要です。 エンドポイントを指定するときにループにならないように注意する必要があります。 ループになる場合の例の 1 つは、フェデレーション信頼チェーンの WSDL ダウンロードの使用で、同じ WSDL ドキュメントの中に複数のリンクがある場合です。 この問題がよく発生するシナリオは、セキュリティ トークン サーバーとセキュリティ トークン サービスが同じ ServiceHost の中にあるフェデレーション サービスです。

 そのような状況の例は次の 3 つのエンドポイント アドレスを使うサービスです。

- `http://localhost/CalculatorService/service` (サービス)

- `http://localhost/CalculatorService/issue_ticket` (STS)

- `http://localhost/CalculatorService/mex` (メタデータエンドポイント)

 これによって例外がスローされます。

 このシナリオでエラーを避けるには、`issue_ticket` エンドポイントを別のところに置きます。

## <a name="wsdl-import-attributes-can-be-lost"></a>WSDL インポート属性が失われる可能性があります

WSDL インポートの際に、WCF は `<wst:Claims>` テンプレート内の `RST` 要素にあるインポート属性を追跡できなくなることがあります。 これは、クレームの種類のコレクションを直接使用する代わりに `<Claims>` を直接 `WSFederationHttpBinding.Security.Message.TokenRequestParameters` または `IssuedSecurityTokenRequestParameters.AdditionalRequestParameters` 内で指定した場合に、WSDL インポートの際に発生します。  インポートが属性を失うので、WSDL を介して正しいバインディングのラウンド トリップが行われません。したがって、クライアント側で不適切になります。

 解決策は、インポートを行った後、クライアント側で直接バインディングを変更することです。

## <a name="see-also"></a>関連項目

- [セキュリティの考慮事項](security-considerations-in-wcf.md)
- [情報の漏えい](information-disclosure.md)
- [権限の昇格](elevation-of-privilege.md)
- [サービス拒否](denial-of-service.md)
- [改変](tampering.md)
- [リプレイ攻撃](replay-attacks.md)
