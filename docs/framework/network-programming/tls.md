---
title: .NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス
description: .NET Framework でトランスポート層セキュリティ (TLS) を使うときのベスト プラクティスについて説明します
ms.date: 10/22/2018
helpviewer_keywords:
- sending data, Internet security
- protocols, Internet security
- Network security
- network resources, Internet security
- receiving data, Internet security
- authentication [.NET Framework], Internet security
- Internet, security
- security [.NET Framework], Internet
- permissions [.NET Framework], Internet
ms.openlocfilehash: 49beb8aa7d488bb6f0649cfb66ce7e78695840b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78848211"
---
# <a name="transport-layer-security-tls-best-practices-with-the-net-framework"></a>.NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス

トランスポート層セキュリティ (TLS) プロトコルは、インターネット経由でやり取りされる情報のプライバシーを保護することを目的として策定された業界標準です。 [TLS 1.2](https://tools.ietf.org/html/rfc5246) は、以前のバージョンよりセキュリティが強化された標準です。 TLS 1.2 はいずれ、最新リリースの標準 [TLS 1.3](https://tools.ietf.org/html/rfc8446) に取って代わられます。最新版は前のより速く、セキュリティが改善されています。 この記事では、TLS プロトコルを使う .NET Framework アプリケーションのセキュリティ保護に関する推奨事項を示します。

.NET Framework アプリケーションのセキュリティ保護を維持するため、TLS のバージョンをハードコーディング**しない**でください。 .NET Framework アプリケーションでは、オペレーティング システム (OS) がサポートしている TLS のバージョンを使う必要があります。

このドキュメントは次のような開発者を対象に書かれています。

- <xref:System.Net> API (<xref:System.Net.Http.HttpClient?displayProperty=nameWithType> や <xref:System.Net.Security.SslStream?displayProperty=nameWithType> など) を直接使用する。
- <xref:System.ServiceModel?displayProperty=nameWithType> 名前空を使って WCF クライアントおよびサービスを直接使用する。

次のようにすることをお勧めします。

- アプリでは .NET Framework 4.7 以降のバージョンを対象とする。 WCF アプリでは .NET Framework 4.7.1 以降のバージョンを対象とする。
- TLS のバージョンを指定しない。 OS に TLS のバージョンを決定させるようにコードを構成します。
- コードの監査を徹底的に実行し、TLS または SSL のバージョンを指定していないことを確認する。

OS が TLS のバージョンを選ぶようにアプリを構成すると、次のようになります。

- 将来的に追加される新しいプロトコル (TLS 1.3 など) を自動的に利用します。
- セキュリティで保護されていないことが検出されたプロトコルを OS がブロックします。

「[コードの監査および変更を行う](#audit-your-code-and-make-code-changes)」では、コードの監査と更新について説明します。

この記事では、お使いのアプリの実行対象になっている .NET Framework のバージョンで使用できる最も強力なセキュリティを有効にする方法について説明します。 アプリでセキュリティ プロトコルとバージョンを明示的に設定すると、他の選択肢および .NET Framework と OS の既定の動作は使われなくなります。 お使いのアプリで TLS 1.2 接続をネゴシエートできるようにしたい場合、それより低いバージョンの TLS に明示的に設定すると、TLS 1.2 の接続は拒否されます。

プロトコルのバージョンをどうしてもハードコーディングしなければならない場合は、TLS 1.2 を指定することを強くお勧めします。 TLS 1.0 への依存関係の識別と除去に関するガイダンスについては、ホワイトペーパー「[Solving the TLS 1.0 Problem](https://www.microsoft.com/download/details.aspx?id=55266)」(TLS 1.0 の問題を解決する) をダウンロードしてください。

WCF は、.NET Framework 4.7 での既定値として TLS 1.0、1.1、1.2 をサポートしています。 .NET Framework 4.7.1 以降の WCF の既定値は、オペレーティング システムで構成されている TLS のバージョンです。 アプリケーションが `SslProtocols.None` で明示的に構成されている場合、WCF は、NetTcp トランスポートを使うとき、オペレーティング システムの既定の設定を使います。

このドキュメントに関してわからないことは、GitHub の問題「[Transport Layer Security (TLS) best practices with the .NET Framework](https://github.com/dotnet/docs/issues/4675)」(.NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス) で質問できます。

## <a name="audit-your-code-and-make-code-changes"></a>コードの監査および変更を行う

ASP.NET アプリケーションの場合、意図したバージョンの .NET Framework を使っていることを確認するには、`<system.web><httpRuntime targetFramework>`web.config_の_ 要素を調べます。

Windows フォームおよび他のアプリケーションについては、「[方法: .NET Framework のターゲット バージョンを指定する](/visualstudio/ide/visual-studio-multi-targeting-overview)」をご覧ください。

以下のセクションで説明するようにして、特定のバージョンの TLS や SSL を使っていないことを確認します。

## <a name="if-your-app-targets-net-framework-47-or-later-versions"></a>アプリの対象が .NET Framework 4.7 以降のバージョンの場合

以下のセクションでは、特定のバージョンの TLS や SSL を使っていないことを確認する方法を説明します。

### <a name="for-http-networking"></a>HTTP ネットワークの場合

<xref:System.Net.ServicePointManager> で .NET Framework 4.7 以降のバージョンを使用すると、OS で構成されている既定のセキュリティ プロトコルが使用されます。 OS による既定の選択を使用することが可能な場合は、<xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType> プロパティの値を設定しないでください。これで既定の <xref:System.Net.SecurityProtocolType.SystemDefault?displayProperty=nameWithType> になります。

<xref:System.Net.SecurityProtocolType.SystemDefault?displayProperty=nameWithType> 設定により <xref:System.Net.ServicePointManager> でオペレーティング システムによって構成された既定のセキュリティ プロトコルが使用されるため、アプリケーションが実行されている OS に基づいて異なる方法で実行される可能性があります。 たとえば、Windows 7 SP1 では TLS 1.0 が使用される一方で、Windows 8 と Windows 10 では TLS 1.2 が使用されます。

HTTP ネットワークで、.NET Framework 4.7 以降のバージョンが対象の場合は、この記事の残りの部分は関係ありません。

### <a name="for-tcp-sockets-networking"></a>TCP ソケット ネットワークの場合

.NET Framework 4.7 以降のバージョンを使う <xref:System.Net.Security.SslStream> の既定の設定は、OS による最適なセキュリティ プロトコルとバージョンの選択です。 OS による既定の最適な選択を使用することが可能な場合は、明示的な <xref:System.Net.Security.SslStream> パラメーターを受け取る <xref:System.Security.Authentication.SslProtocols> メソッドのオーバーロードを使わないでください。 それ以外の場合は、<xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType> を渡します。 <xref:System.Security.Authentication.SslProtocols.Default> を使わないことをお勧めします。`SslProtocols.Default` を設定すると、強制的に SSL 3.0/TLS 1.0 が使われて、TLS 1.2 は使われません。

<xref:System.Net.ServicePointManager.SecurityProtocol> プロパティの値を設定しないでください (HTTP ネットワークの場合)。

明示的な <xref:System.Net.Security.SslStream> パラメーターを受け取る <xref:System.Security.Authentication.SslProtocols> メソッドのオーバーロードを使わないでください (TCP ソケット ネットワークの場合)。 アプリの対象を .NET Framework 4.7 以降のバージョンに変更すると、推奨されるベスト プラクティスに従うようになります。

TCP ソケット ネットワークで、.NET Framework 4.7 以降のバージョンが対象の場合は、このトピックの残りの部分は関係ありません。

<a name="wcf-tcp-cert"></a>

### <a name="for-wcf-tcp-transport-using-transport-security-with-certificate-credentials"></a>証明書資格情報によるトランスポート セキュリティを使う WCF TCP トランスポートの場合

WCF は、.NET Framework の他の部分と同じネットワーク スタックを使います。

4\.7.1 が対象で、次の方法により明示的に構成されていない場合、WCF は既定で OS が最適なセキュリティ プロトコルを選択できるように構成されます。

- アプリケーション構成ファイル。
- **または**、アプリケーションのソース コード。

既定では、.NET Framework 4.7 以降のバージョンは、TLS 1.2 を使い、TLS 1.1 または TLS 1.0 での接続を許可するように構成されます。 OS による最適なセキュリティ プロトコルの選択を許可するように WCF を構成するには、<xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType> を使うようにバインドを構成します。 これは <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols> で設定できます。 `SslProtocols.None` には <xref:System.ServiceModel.NetTcpSecurity.Transport> からアクセスできます。 `NetTcpSecurity.Transport` には <xref:System.ServiceModel.NetTcpBinding.Security> からアクセスできます。

カスタム バインドを使っている場合:

- OS による最適なセキュリティ プロトコルの選択を許可するように WCF を構成するには、<xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols> を使うように <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType> を設定します。
- **または**、構成パス `system.serviceModel/bindings/customBinding/binding/sslStreamSecurity:sslProtocols` で使われるプロトコルを構成します。

カスタム バインドを使用して**おらず**、**かつ**、構成を使って WCF バインドを設定している場合は、構成パス `system.serviceModel/bindings/netTcpBinding/binding/security/transport:sslProtocols` で使われるプロトコルを設定します。

### <a name="for-wcf-message-security-with-certificate-credentials"></a>証明書資格情報を使用する WCF メッセージ セキュリティの場合

.NET Framework 4.7 以降のバージョンは、既定で、<xref:System.Net.ServicePointManager.SecurityProtocol> プロパティで指定されているプロトコルを使います。 [AppContextSwitch](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) `Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols` が `true` に設定されていると、WCF は最適なプロトコル (最高で TLS 1.0 まで) を選択します。

## <a name="if-your-app-targets-a-net-framework-version-earlier-than-47"></a>アプリの対象がバージョン 4.7 より前の .NET Framework の場合

以下のセクションで説明するようにしてコードを監査し、特定のバージョンの TLS や SSL を設定していないことを確認します。

### <a name="for-net-framework-46---462-and-not-wcf"></a>.NET Framework 4.6 ～ 4.6.2 で、WCF を使用していない場合

`DontEnableSystemDefaultTlsVersions` `AppContext` スイッチを `false` に設定します。 「[AppContext スイッチによるセキュリティの構成](#configuring-security-via-appcontext-switches)」をご覧ください。

### <a name="for-wcf-using-net-framework-46---462-using-tcp-transport-security-with-certificate-credentials"></a>.NET Framework 4.6 ～ 4.6.2 と証明書資格情報による TCP トランスポート セキュリティを使用する WCF の場合

OS の最新の修正プログラムをインストールする必要があります。 「[セキュリティ更新プログラム](#security-updates)」をご覧ください。

ユーザーがプロトコルのバージョンを明示的に構成していない場合、WCF フレームワークは、TLS 1.2 以下で使用可能な最高のプロトコルを自動的に選びます。 詳しくは、前の「[証明書資格情報によるトランスポート セキュリティを使う WCF TCP トランスポートの場合](#wcf-tcp-cert)」をご覧ください。

### <a name="for-net-framework-35---452-and-not-wcf"></a>.NET Framework 3.5 から 4.5.2 で、WCF を使用していない場合

お使いのアプリを .NET Framework 4.7 以降のバージョンにアップグレードすることをお勧めします。 アップグレードできない場合は、以下の手順を実行します。 将来的には、.NET Framework 4.7 以降のバージョンにアップグレードするまで、お使いのアプリケーションが動作しなくなる可能性があります。

[SchUseStrongCrypto](#schusestrongcrypto) および [SystemDefaultTlsVersions](#systemdefaulttlsversions) レジストリ キーを 1 に設定します。 「[Windows レジストリによるセキュリティの構成](#configuring-security-via-the-windows-registry)」をご覧ください。 .NET Framework バージョン 3.5 は、明示的な TLS の値が渡された場合にのみ、`SchUseStrongCrypto` フラグをサポートします。

.NET Framework 3.5 を実行している場合は、プログラムで TLS 1.2 を指定できるように、ホット パッチをインストールする必要があります。

| [KB3154518](https://support.microsoft.com/kb/3154518) | 信頼性ロールアップ HR-1605 - Windows 7 SP1 および Server 2008 R2 SP1 の .NET Framework 3.5.1 に含まれる TLS システムの既定バージョンのサポート |
| --- | --- |
| [KB3154519](https://support.microsoft.com/kb/3154519) | 信頼性ロールアップ HR-1605 - Windows Server 2012 の .NET Framework 3.5 に含まれる TLS システムの既定バージョンのサポート |
| [KB3154520](https://support.microsoft.com/kb/3154520) | 信頼性ロールアップ HR-1605 - Windows 8.1 および Windows Server 2012 R2 の .NET Framework 3.5 に含まれる TLS システムの既定バージョンのサポート |
| [KB3156421](https://support.microsoft.com/kb/3156421) | Windows の .NET Framework 4.5.2 および 4.5.1 に対する 1605 修正プログラムロールアップ 3154521 |

### <a name="for-wcf-using-net-framework-35---452-using-tcp-transport-security-with-certificate-credentials"></a>.NET Framework 3.5 ～ 4.5.2 と証明書資格情報による TCP トランスポート セキュリティを使用する WCF の場合

これらのバージョンの WCF フレームワークは、SSL 3.0 と TLS 1.0 を使うようにハードコーディングされています。 これらの値は変更できません。 TLS 1.1 および 1.2 を使うには、更新して対象を NET Framework 4.6 以降のバージョンに変更する必要があります。

## <a name="if-your-app-targets-net-framework-35"></a>アプリの対象が .NET Framework 3.5 の場合

.NET または OS にセキュリティ プロトコルを選択させるのではなく、明示的にセキュリティ プロトコルを設定する必要がある場合は、`SecurityProtocolTypeExtensions` および `SslProtocolsExtension` 列挙型をコードに追加します。 `SecurityProtocolTypeExtensions` および `SslProtocolsExtension` には、`Tls12`、`Tls11`、`SystemDefault` に対する値が含まれます。 詳細については、「[Windows 8.1 と Windows Server 2012 R2 上の .NET Framework 3.5 に含まれる TLS システムの既定のバージョンのサポート](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the--net-framework)」を参照してください。

<a name="configuring-security-via-appcontext-switches"></a>

## <a name="configuring-security-via-appcontext-switches-for-net-framework-46-or-later-versions"></a>AppContext スイッチによるセキュリティの構成 (.NET Framework 4.6 以降のバージョンの場合)

このセクションで説明する [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) スイッチは、お使いのアプリの対象または実行環境が .NET Framework 4.6 以降のバージョンである場合に関係します。 既定によるものか、または明示的な設定によるものかに関係なく、可能な場合にはこれらのスイッチを `false` にする必要があります。 一方または両方のスイッチでセキュリティを構成する必要がある場合は、コードでセキュリティ プロトコルの値を指定しないでください。指定すると、スイッチの設定はオーバーライドされます。

HTTP ネットワーク (<xref:System.Net.ServicePointManager>) または TCP ソケット ネットワーク (<xref:System.Net.Security.SslStream>) のどちらを使っている場合でも、スイッチの効果は同じです。

### <a name="switchsystemnetdontenableschusestrongcrypto"></a>Switch.System.Net.DontEnableSchUseStrongCrypto

`false` の値を `Switch.System.Net.DontEnableSchUseStrongCrypto` に設定すると、アプリは強力な暗号を使うようになります。 `false` を `DontEnableSchUseStrongCrypto` にすると、安全性の高いネットワーク プロトコル (TLS 1.2、TLS 1.1、TLS 1.0) が使われ、セキュリティ保護されていないプロトコルはブロックされます。 詳しくは、「[SCH_USE_STRONG_CRYPTO フラグ](#the-sch_use_strong_crypto-flag)」をご覧ください。 値 `true` は、アプリの強力な暗号を無効にします。

アプリの対象が .NET Framework 4.6 以降のバージョンの場合、このスイッチの既定値は `false` です。 これは、推奨されるセキュリティ保護が有効な既定値です。 アプリの実行環境が .NET Framework 4.6 であっても、以前のバージョンが対象になっている場合は、このスイッチの既定値は `true` です。 その場合は、`false` に明示的に設定する必要があります。

強力な暗号化をサポートしておらず、アップグレードできない、レガシ サービスに接続する必要がある場合は、`DontEnableSchUseStrongCrypto` の値を常に `true` にする必要があります。

### <a name="switchsystemnetdontenablesystemdefaulttlsversions"></a>Switch.System.Net.DontEnableSystemDefaultTlsVersions

`false` の値を `Switch.System.Net.DontEnableSystemDefaultTlsVersions` に設定すると、お使いのアプリはオペレーティング システムによるプロトコルの選択を許可するようになります。 値を `true` にすると、アプリは .NET Framework によって選択されたプロトコルを使います。

アプリの対象が .NET Framework 4.7 以降のバージョンの場合、このスイッチの既定値は `false` です。 これは、推奨されるセキュリティ保護が有効な既定値です。 アプリの実行環境が .NET Framework 4.7 以降のバージョンであっても、以前のバージョンが対象になっている場合は、このスイッチの既定値は `true` です。 その場合は、`false` に明示的に設定する必要があります。

### <a name="switchsystemservicemodeldisableusingservicepointmanagersecurityprotocols"></a>Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols

`false` の値を `Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols` に設定すると、お使いのアプリケーションは、証明書資格情報を使うメッセージ セキュリティに対して `ServicePointManager.SecurityProtocols` で定義されている値を使うようになります。 値が `true` のときは、TLS1.0 以下で利用可能な最も高いプロトコルを使います。

.NET Framework 4.7 以降のバージョンを対象とするアプリケーションでは、この値の既定値は `false` です。 .NET Framework 4.6.2 以前のバージョンを対象とするアプリケーションでは、この値の既定値は `true` です。

### <a name="switchsystemservicemodeldontenablesystemdefaulttlsversions"></a>Switch.System.ServiceModel.DontEnableSystemDefaultTlsVersions

`false` の値を `Switch.System.ServiceModel.DontEnableSystemDefaultTlsVersions` に設定すると、オペレーティング システムによるプロトコルの選択を許可するように既定の構成が設定されます。 値が `true` のときは、TLS1.2 以下で利用可能な最も高いプロトコルに既定値が設定されます。

.NET Framework 4.7.1 以降のバージョンを対象とするアプリケーションでは、この値の既定値は `false` です。 .NET Framework 4.7 以前のバージョンを対象とするアプリケーションでは、この値の既定値は `true` です。

TLS プロトコルについて詳しくは、「[軽減策: TLS プロトコル](../migration-guide/mitigation-tls-protocols.md)」をご覧ください。 `AppContext` スイッチについて詳しくは、「[`<AppContextSwitchOverrides> Element`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)」をご覧ください。

## <a name="configuring-security-via-the-windows-registry"></a>Windows レジストリによるセキュリティの構成

> [!WARNING]
> レジストリ キーを設定すると、システム上のすべてのアプリケーションに影響します。 この方法は、マシンを完全に制御していて、レジストリの変更を制御できる場合にのみ使用します。

`AppContext` スイッチの一方または両方を設定できない場合は、このセクションで説明する Windows レジストリ キーにより、アプリが使うセキュリティ プロトコルを制御することができます。 .NET Framework 4.5.2 以前のバージョンでアプリを実行している場合、または構成ファイルを編集できない場合は、一方または両方の `AppContext` スイッチを使用できないことがあります。 レジストリでセキュリティを構成する場合は、コードでセキュリティ プロトコルの値を指定しないでください。指定すると、レジストリ設定はオーバーライドされます。

レジストリ キーの名前は対応する `AppContext` スイッチの名前に似ていますが、名前の前に `DontEnable` が付いていません。 たとえば、`AppContext` スイッチ `DontEnableSchUseStrongCrypto` に対応するレジストリ キーの名前は [SchUseStrongCrypto](#schusestrongcrypto) です。

これらのキーは、最新のセキュリティ修正プログラムが適用されているすべての .NET Framework のバージョンで使用できます。 「[セキュリティ更新プログラム](#security-updates)」をご覧ください。

HTTP ネットワーク (<xref:System.Net.ServicePointManager>) または TCP ソケット ネットワーク (<xref:System.Net.Security.SslStream>) のどちらを使っている場合でも、以下で説明するすべてのレジストリ キーの効果は同じです。

### <a name="schusestrongcrypto"></a>SchUseStrongCrypto

`HKEY_LOCAL_MACHINE\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\<VERSION>: SchUseStrongCrypto` レジストリ キーの値は DWORD 型です。 値を 1 に設定すると、アプリは強力な暗号を使うようになります。 強力な暗号では、安全性の高いネットワーク プロトコル (TLS 1.2、TLS 1.1、TLS 1.0) が使われ、セキュリティ保護されていないプロトコルはブロックされます。 値 0 は、強力な暗号を無効にします。 詳しくは、「[SCH_USE_STRONG_CRYPTO フラグ](#the-sch_use_strong_crypto-flag)」をご覧ください。

アプリの対象が .NET Framework 4.6 以降のバージョンの場合、このキーの既定値は 1 です。 これは、推奨されるセキュリティ保護が有効な既定値です。 アプリの実行環境が .NET Framework 4.6 であっても、対象が以前のバージョンの場合は、このキーの既定値は 0 です。 その場合は、値を 1 に明示的に設定する必要があります。

強力な暗号化をサポートしておらず、アップグレードできない、レガシ サービスに接続する必要がある場合は、このキーの値を常に 0 にする必要があります。

### <a name="systemdefaulttlsversions"></a>SystemDefaultTlsVersions

`HKEY_LOCAL_MACHINE\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\<VERSION>: SystemDefaultTlsVersions` レジストリ キーの値は DWORD 型です。 値を 1 に設定すると、お使いのアプリはオペレーティング システムによるプロトコルの選択を許可するようになります。 値を 0 にすると、アプリは .NET Framework によって選択されたプロトコルを使います。

`<VERSION>` は、v4.0.30319 (.NET Framework 4 以降の場合) または v2.0.50727 (.NET Framework 3.5 の場合) にする必要があります。

アプリの対象が .NET Framework 4.7 以降のバージョンの場合、このキーの既定値は 1 です。 これは、推奨されるセキュリティ保護が有効な既定値です。 アプリの実行環境が .NET Framework 4.7 以降のバージョンであっても、以前のバージョンが対象になっている場合は、このキーの既定値は 0 になります。 その場合は、値を 1 に明示的に設定する必要があります。

詳しくは、「[Windows 10 Version 1511 および Windows Server 2016 Technical Preview 4 用の累積的な更新プログラム(2016 年 5 月 10 日)](https://support.microsoft.com/help/3156421/cumulative-update-for-windows-10-version-1511-and-windows-server-2016)」をご覧ください。

.NET Framework 3.5.1 の場合について詳しくは、「[Windows 7 SP1 と Server 2008 R2 SP1 上の .NET Framework 3.5.1 に含まれている TLS システムの既定のバージョンのサポート](https://support.microsoft.com/help/3154518/support-for-tls-system-default-versions-included-in-the--net-framework)」を参照してください。

次の _.REG_ ファイルは、レジストリ キーとそのバリエーションを最も安全な値に設定します。

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001
```

## <a name="configuring-schannel-protocols-in-the-windows-registry"></a>Windows レジストリでの Schannel プロトコルの構成

レジストリを使って、クライアントやサーバー アプリがネゴシエートするプロトコルをきめ細かく制御することができます。 アプリのネットワークは Schannel ([Secure Channel (セキュリティで保護されたチャネル)](/windows/desktop/SecAuthN/secure-channel) の別名) を通してやり取りされます。 `Schannel` を構成することにより、アプリの動作を構成することができます。

最上位は `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols` レジストリ キーです。 そのキーの下に、`SSL 2.0`、`SSL 3.0`、`TLS 1.0`、`TLS 1.1`、`TLS 1.2` のセットで任意のサブキーを作成できます。 これらの各サブキーの下に、サブキー `Client` と `Server` の一方または両方を作成できます。 `Client` および `Server` の下には、DWORD 値 `DisabledByDefault` (0 または 1) および `Enabled` (0 または 0xFFFFFFFF) を作成できます。

## <a name="the-sch_use_strong_crypto-flag"></a>SCH_USE_STRONG_CRYPTO フラグ

有効になっている場合 (既定では、`AppContext` スイッチまたは Windows レジストリにより)、アプリが TLS セキュリティ プロトコルを要求すると、.NET Framework は `SCH_USE_STRONG_CRYPTO` フラグを使います。 `SCH_USE_STRONG_CRYPTO` フラグは、`AppContext` スイッチまたはレジストリを使って、既定で有効にすることができます。 OS は `Schannel` にフラグを渡して、既知の脆弱な暗号アルゴリズム、暗号スイート、および相互運用性向上のために他で有効になっている可能性のある TLS/SSL プロトコルのバージョンを無効にするよう指示します。 詳細については次を参照してください:

- [セキュリティで保護されたチャネル](/windows/desktop/SecAuthN/secure-channel)
- [SCHANNEL_CRED 構造体](/windows/win32/api/schannel/ns-schannel-schannel_cred)

`SCH_USE_STRONG_CRYPTO` フラグは、ユーザーが `Schannel` または `Tls` の `Tls11` (TLS 1.0)、`Tls12`、または <xref:System.Net.SecurityProtocolType> 列挙値を明示的に使ったときも、<xref:System.Security.Authentication.SslProtocols> に渡されます。

## <a name="security-updates"></a>セキュリティ更新プログラム

この記事のベスト プラクティスに従うには、最新のセキュリティ更新プログラムがインストールされている必要があります。 これらの更新プログラムをインストールすると、.NET Framework 4.7 以降の高度な機能を利用できるようになります。 最新のセキュリティ更新プログラムは、お使いのアプリが .NET Framework 4.7 以降のバージョンで実行する場合に重要です (対象が以前のバージョンであっても)。

オペレーティング システムが最適なバージョンの TLS を選んで使えるように .NET Framework を更新するには、少なくとも次のものをインストールする必要があります。

- [.NET Framework August 2017 Preview の品質ロールアップ](https://devblogs.microsoft.com/dotnet/net-framework-august-2017-preview-of-quality-rollup/)。
- **または**、[.NET Framework September 2017 のセキュリティおよび品質ロールアップ](https://devblogs.microsoft.com/dotnet/net-framework-september-2017-security-and-quality-rollup/)。

参照:

- [.NET Framework のバージョンおよび依存関係](../migration-guide/versions-and-dependencies.md)
- [方法: インストールされている .NET Framework バージョンを確認する](../migration-guide/how-to-determine-which-versions-are-installed.md)

## <a name="support-for-tls-12"></a>TLS 1.2 のサポート

アプリが TLS 1.2 をネゴシエートするには、OS と .NET Framework の両方のバージョンが TLS 1.2 をサポートしている必要があります。

**TLS 1.2 をサポートするためのオペレーティング システムの要件**

TLS 1.2 と TLS 1.1 の一方または両方をサポートするシステムでこれらを有効化または再有効化する方法については、「[Transport Layer Security (TLS) registry settings](/windows-server/security/tls/tls-registry-settings)」(トランスポート層セキュリティ (TLS) のレジストリ設定) をご覧ください。

| **OS** | **TLS 1.2 のサポート** |
| --- | --- |
| Windows 10<br>Windows Server 2016 | サポートされていて、既定で有効。 |
| Windows 8.1<br>Windows Server 2012 R2 | サポートされていて、既定で有効。 |
| Windows 8.0<br>Windows Server 2012 | サポートされていて、既定で有効。 |
| Windows 7 SP1<br>Windows Server 2008 R2 SP1 | サポートされていますが、既定では有効になっていません。 TLS 1.2 を有効にする方法について詳しくは、「[Transport Layer Security (TLS) registry settings](/windows-server/security/tls/tls-registry-settings)」(トランスポート層セキュリティ (TLS) のレジストリ設定) Web ページをご覧ください。 |
| Windows Server 2008 | TLS 1.2 および TLS 1.1 のサポートには、更新プログラムが必要。 「[Windows Server 2008 SP2 に TLS 1.1 および TLS 1.2 のサポートを追加する更新プログラム](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s)」をご覧ください。 |
| Windows Vista | サポートされていません。 |

Windows の各バージョンにおいて既定で有効にされる TLS/SSL プロトコルについては、「[Protocols in TLS/SSL (Schannel SSP)](/windows/desktop/SecAuthN/protocols-in-tls-ssl--schannel-ssp-)」(TLS/SSL でのプロトコル (Schannel SSP)) をご覧ください。

**.NET Framework 3.5 で TLS 1.2 をサポートするための要件**

次の表では、.NET Framework 3.5 で TLS 1.2 をサポートするために必要な OS の更新プログラムを示します。 すべての OS 更新プログラムを適用することをお勧めします。

| **OS** | **.NET Framework 3.5 で TLS 1.2 をサポートするために必要な最低限の更新プログラム** |
| --- | --- |
| Windows 10<br>Windows Server 2016 | [Windows 10 Version 1511 および Windows Server 2016 Technical Preview 4 用の累積的な更新プログラム(2016 年 5 月 10 日)](https://support.microsoft.com/help/3156421/cumulative-update-for-windows-10-version-1511-and-windows-server-2016) |
| Windows 8.1<br>Windows Server 2012 R2 | [Windows 8.1 および Windows Server 2012 R2 の .NET Framework 3.5 に含まれる TLS システムの既定バージョンのサポート](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows 8.0<br>Windows Server 2012 | [Windows Server 2012 の .NET Framework 3.5 に含まれる TLS システムの既定バージョンのサポート](https://support.microsoft.com/help/3154519/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows 7 SP1<br>Windows Server 2008 R2 SP1 | [Windows 7 SP1 および Server 2008 R2 SP1 の .NET Framework 3.5.1 に含まれる TLS システムの既定バージョンのサポート](https://support.microsoft.com/help/3154518/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows Server 2008 | [Windows Vista SP2 および Server 2008 SP2 の .NET Framework 2.0 SP2 に含まれる TLS システムの既定バージョンのサポート](https://support.microsoft.com/help/3154517/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows Vista | サポートされていません |
