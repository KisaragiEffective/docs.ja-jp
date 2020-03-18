---
title: バージョン 3.5 SP1 における HttpWebRequest の NTLM 認証への変更
ms.date: 03/30/2017
ms.assetid: 8bf0b428-5a21-4299-8d6e-bf8251fd978a
ms.openlocfilehash: 388e6dc648e1fd68e24a852cb08de107f09f9c9f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "64754881"
---
# <a name="changes-to-ntlm-authentication-for-httpwebrequest-in-version-35-sp1"></a>バージョン 3.5 SP1 における HttpWebRequest の NTLM 認証への変更

セキュリティの変更は、System.Net 名前空間の <xref:System.Net.HttpWebRequest>、<xref:System.Net.HttpListener>、<xref:System.Net.Security.NegotiateStream>、および関連クラスによる統合 Windows 認証の処理方法に影響を与える、.NET Framework Version 3.5 SP1 以降で行われました。 これらの変更は、これらのクラスを使用して Web 要求を作成し、NTLM に基づく統合 Windows 認証が使用されている応答を受信するアプリケーションに影響を及ぼす場合があります。 この変更は、統合 Windows 認証を使用するように構成されている Web サーバーおよびクライアント アプリケーションに影響を与える可能性があります。

## <a name="overview"></a>概要

統合 Windows 認証の設計では、一部の資格情報の応答をユニバーサルにすることを許可しています。つまり、これらを再利用または転送することができます。 この設計機能が特に必要ではない場合は、認証プロトコルで、ターゲット固有の情報とチャネル固有の情報を伝達する必要があります。 これによりサービスは拡張保護を提供して、資格情報の応答にサービス プリンシパル名 (SPN) などのサービス固有の情報が確実に含まれるようにすることができます。 資格情報の交換でこの情報を使用すると、不適切に使用されている可能性がある資格情報の応答の不適切な取得に対してサービスの保護を強化することができます。

<xref:System.Net> 名前空間と <xref:System.Net.Security> 名前空間内の複数のコンポーネントは、呼び出し元のアプリケーションに代わって統合 Windows 認証を実行します。 このセクションでは、統合 Windows 認証の使用で拡張保護を追加するための System.Net コンポーネントへの変更について説明します。

## <a name="changes"></a>[変更点]

統合 Windows 認証で使用される NTLM 認証プロセスには、クライアント コンピューターに送り返される、ターゲット コンピューターによって発行されるチャレンジが含まれています。 コンピューター自体が生成したチャレンジをコンピューターが受け取った場合、接続がループ バック接続 (たとえば、IPv4 アドレス 127.0.0.1) でない限り、認証は失敗します。

内部 Web サーバー上で実行されているサービスにアクセスする場合、`http://contoso/service` や `https://contoso/service` のような URL を使用してサービスにアクセスするのが一般的です。 "contoso" という名前は、多くの場合、サービスが展開されているコンピューターの名前ではありません。 <xref:System.Net> と関連する名前空間は、名前をアドレスに解決するために、Active Directory、DNS、NetBIOS、ローカル コンピューターのホスト ファイル (通常は WINDOWS\system32\drivers\etc\hosts など)、またはローカル コンピューターの lmhosts ファイル名 (通常は WINDOWS\system32\drivers\etc\lmhosts など) の使用をサポートしています。 "contoso" という名前は、"contoso" に送信された要求が適切なサーバー コンピューターに送信されるように解決されます。

大規模な展開用に構成されている場合、クライアント アプリケーションとエンド ユーザーが使用しない基礎のコンピューター名に単一の仮想サーバー名を展開に指定することも一般的です。 たとえば、サーバー `www.contoso.com` を呼び出しても、内部ネットワークでは単に "contoso" を使用することがあります。 この名前は、クライアント Web 要求で Host ヘッダーと呼ばれます。 HTTP プロトコルの指定に従い、Host 要求ヘッダー フィールドは、要求されているリソースのインターネット ホストとポート番号を指定します。 この情報は、ユーザーまたは参照リソース (HTTP URL で生成されるもの) に指定された元の URI から取得されます。 .NET Framework Version 4 では、この情報は、新しい <xref:System.Net.HttpWebRequest.Host%2A> プロパティを使用して設定することもできます。

<xref:System.Net.AuthenticationManager> クラスは、<xref:System.Net.WebRequest> 派生クラスと <xref:System.Net.WebClient> クラスが使用するマネージド認証コンポーネント ("モジュール") を制御します。 <xref:System.Net.AuthenticationManager> クラスは、<xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A?displayProperty=nameWithType> オブジェクトを公開するプロパティを提供します。このオブジェクトは、URI 文字列でインデックスが作成され、認証中に使用されるカスタム SPN 文字列をアプリケーションが提供するために使用されます。

Version 3.5 SP1 の既定では、<xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A> プロパティが設定されていない場合、NTLM (NT LAN マネージャー) 認証交換の SPN の要求 URL で使用されたホスト名を指定します。 要求 URL で使用されたホスト名は、クライアント要求の <xref:System.Net.HttpRequestHeader?displayProperty=nameWithType> に指定された Host ヘッダーとは異なる可能性があります。 要求 URL に使用されているホスト名は、サーバーの実際のホスト名、サーバーのコンピューター名、コンピューターの IP アドレス、またはループバック アドレスとは異なる可能性があります。 このような場合、Windows は認証要求に失敗します。 この問題を解決するには、クライアント要求の要求 URL に使用されているホスト名 (たとえば "contoso") が、実際はローカル コンピューターの代替名であると Windows に通知する必要があります。

サーバー アプリケーションがこの変更を回避するには、いくつかの方法があります。 推奨されるアプローチは、要求 URL に使用されているホスト名を、サーバーのレジストリ内の `BackConnectionHostNames` キーにマップすることです。 通常、`BackConnectionHostNames` レジストリ キーは、ホスト名をループバック アドレスにマップするために使用されます。 手順は次のとおりです。

ループバック アドレスにマップされているホスト名を指定し、ローカル コンピューター上の Web サイトに接続するには、次の手順を実行します。

1. [スタート] ボタンをクリックし、[ファイル名を指定して実行] をクリックして「regedit」と入力し、[OK] をクリックします。

2. レジストリ エディターで、次のレジストリ キーを探してクリックします。

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0`

3. [MSV1_0] を右クリックし、[新規作成] をポイントし、[複数行文字列値] をクリックします。

4. 「`BackConnectionHostNames`」と入力して Enter キーを押します。

5. `BackConnectionHostNames` を右クリックし、[変更] をクリックします。

6. [値のデータ] ボックスに、サイトの 1 つまたは複数のホスト名 (要求 URL に使用されているホスト名) を入力し、[OK] をクリックします。

7. レジストリ エディターを終了し、IISAdmin サービスを再起動して、IISReset を実行します。

<https://support.microsoft.com/kb/896861> で説明されているように、あまり安全ではない回避策はループ バック チェックを無効にすることです。 これによって、リフレクション攻撃に対する保護が無効になります。 そのため、実際に使用するコンピューターと想定するコンピューターにのみ、代替名のセットを制限することをお勧めします。

## <a name="see-also"></a>参照

- <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A?displayProperty=nameWithType>
- <xref:System.Net.HttpRequestHeader?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest.Host%2A?displayProperty=nameWithType>
