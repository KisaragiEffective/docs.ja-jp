---
title: HTTP および HTTPS の構成
ms.date: 04/08/2019
helpviewer_keywords:
- configuring HTTP [WCF]
ms.assetid: b0c29a86-bc0c-41b3-bc1e-4eb5bb5714d4
ms.openlocfilehash: f7fd2bad6ced09b638cc1bb5d539fab1b9ce7d25
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336699"
---
# <a name="configuring-http-and-https"></a>HTTP および HTTPS の構成

WCF サービスと WCF クライアントは、HTTP および HTTPS を介して通信できます。 HTTP または HTTPS の設定は、インターネット インフォメーション サービス (IIS) またはコマンド ライン ツールを使用して構成します。 WCF サービスが IIS でホストされている場合は、IIS 内で HTTP または HTTPS の設定を構成できます (inetmgr.exe ツールを使用)。 WCF サービスが自己ホスト型の場合は、コマンド ライン ツールを使用して HTTP または HTTPS の設定を構成します。

少なくとも、URL 登録を構成し、サービスが使用する URL に対してファイアウォールの例外を追加する必要があります。 これらの設定は、Netsh.exe ツールを使用して構成できます。

## <a name="configuring-namespace-reservations"></a>名前空間予約の構成

名前空間予約では、HTTP URL 名前空間の一部に対する権限を特定のユーザー グループに割り当てます。 予約により、これらのユーザーには名前空間のその部分をリッスンするサービスを作成する権利が与えられます。 予約は、予約パスのすべてのサブパスを対象とする URL プレフィックスです。 名前空間予約では、2 つの方法でワイルドカードを使用できます。 HTTP Server API ドキュメントでは、[ワイルドカードを含む名前空間クレーム間の解決の順序](/windows/desktop/Http/routing-incoming-requests)について説明します。

実行中のアプリケーションは、同様の要求を作成して、名前空間登録を追加できます。 名前空間の同じ部分について、登録と予約の間で競合が発生します。 [ワイルドカードを含む名前空間クレーム間の解決](/windows/desktop/Http/routing-incoming-requests)順序に従って、予約が優先順位に従って、登録より優先される場合があります。 この場合、実行中のアプリケーションがクレームを受信する動作が、予約によってブロックされます。

次の例では、Netsh.exe ツールを使用します。

```console
netsh http add urlacl url=http://+:80/MyUri user=DOMAIN\user
```

このコマンドは、DOMAIN\user アカウントの指定した URL 名前空間の URL 予約を追加します。 Netsh コマンドの使用方法の詳細については、コマンドプロンプトで「`netsh http add urlacl /?`」と入力し、Enter キーを押します。

## <a name="configuring-a-firewall-exception"></a>ファイアウォールの例外の構成

HTTP 経由の通信を行う WCF サービスを自己ホストする際は、例外をファイアウォール構成に追加して、特定の URL を使用した着信接続を可能にする必要があります。

## <a name="configuring-ssl-certificates"></a>SSL 証明書の構成

SSL (Secure Sockets Layer) プロトコルは、クライアントとサーバー上で証明書を使用して暗号化キーを格納します。 サーバーは、クライアントがサーバーの ID を検証できるように接続時に SSL 証明書を提供します。 また、サーバーはクライアントに証明書を要求して、接続の両側で相互認証を実行することもできます。

証明書は、接続の IP アドレスとポート番号に基づいて集中ストアに格納されます。 特別な IP アドレス 0.0.0.0 は、ローカル コンピューターの任意の IP アドレスに一致します。 証明書ストアは、パスに基づいて Url を区別しないことに注意してください。 同じ IP アドレスとポートの組み合わせを持つサービスは、そのサービスの URL でのパスが異なる場合でも証明書を共有する必要があります。

詳細な手順については、「[方法: SSL 証明書を使用してポートを構成する](how-to-configure-a-port-with-an-ssl-certificate.md)」を参照してください。

## <a name="configuring-the-ip-listen-list"></a>IP リッスン一覧の構成

HTTP Server API は、ユーザーが URL を登録すると、IP アドレスとポートだけにバインドします。 既定では、HTTP Server API は、コンピューターのすべての IP アドレスに対して、URL でポートにバインドします。 HTTP サーバー API を使用していないアプリケーションが、以前に IP アドレスとポートの組み合わせにバインドされている場合、競合が発生します。 IP リッスン一覧を使用すると、コンピューターの一部の IP アドレスに対してポートを使用するアプリケーションと WCF サービスを共存させることができます。 IP リッスン一覧にエントリがある場合、HTTP Server API は、その一覧に指定されている IP アドレスだけにバインドします。 IP リッスン一覧を変更するには、管理特権が必要です。

次の例に示すように、netsh ツールを使用して IP リッスン一覧を変更します。

```console
netsh http add iplisten ipaddress=0.0.0.0:8000
```

## <a name="other-configuration-settings"></a>その他の構成設定

<xref:System.ServiceModel.WSDualHttpBinding> を使用すると、クライアント接続では、名前空間予約と Windows ファイアウォールに対応できる既定値が使用されます。 双方向接続のクライアント ベース アドレスをカスタマイズする場合、クライアント上で HTTP 設定を行い、新しいアドレスに一致させる必要があります。

HTTP サーバー API には、Httpcfg.exe では利用できない高度な構成設定がいくつかあります。 この設定は、レジストリで管理され、HTTP Server API を使用するシステムで実行中のすべてのアプリケーションに適用されます。 これらの設定の詳細については、「 [IIS の http.sys レジストリ設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)」を参照してください。 ほとんどのユーザーは、これらの設定を変更する必要はありません。

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.WSDualHttpBinding>
- [方法 : SSL 証明書を使用してポートを構成する](how-to-configure-a-port-with-an-ssl-certificate.md)
