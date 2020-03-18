---
title: '軽減策: WCF サービスと証明書認証'
ms.date: 03/30/2017
ms.assetid: ef19c91a-b9df-4bf0-a28e-eb1e99c4bc95
ms.openlocfilehash: 8c8493efa2c3223809ad87e01e3faddaea859ca8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73457796"
---
# <a name="mitigation-wcf-services-and-certificate-authentication"></a>軽減策: WCF サービスと証明書認証

.NET Framework 4.6 では、WCF SSL プロトコルの既定の一覧に TLS 1.1 および TLS 1.2 が追加されます。 クライアントとサーバーの両方のマシンに .NET Framework 4.6 以降がインストールされている場合は、TLS 1.2 がネゴシエーションに使用されます。

## <a name="impact"></a>影響

TLS 1.2 では MD5 証明書認証がサポートされません。 そのため、顧客がハッシュ アルゴリズムで MD5 を使用する SSL 証明書を使用すると、WCF クライアントは WCF サービスに接続できません。 詳細については、「[Mitigation: WCF Services and Certificate Authentication](mitigation-wcf-services-and-certificate-authentication.md)」(軽減策: WCF サービスと証明書認証) を参照してください。

## <a name="mitigation"></a>対応策

次のいずれかの操作を実行することで、この問題を回避して、WCF クライアントを WCF サーバーに接続できるようになります。

- MD5 アルゴリズムを使用しないように証明書を更新します。 この解決策をお勧めします。

- バインドがソース コードで動的に構成されていない場合は、TLS 1.1 または以前のバージョンのプロトコルを使用するようにアプリケーションの構成ファイルを更新します。 これにより、引き続き MD5 ハッシュ アルゴリズムによる証明書を使用することができます。

  > [!CAUTION]
  > MD5 ハッシュ アルゴリズムによる証明書は安全でないと見なされるため、この回避策はお勧めできません。

  次の構成ファイルでこれを行います。

  ```xml
  <configuration>
      <system.serviceModel>
          <bindings>
              <netTcpBinding>
                  <binding>
                      <security mode= "None|Transport|Message|TransportWithMessageCredential" >
                          <transport clientCredentialType="None|Windows|Certificate"
                                              protectionLevel="None|Sign|EncryptAndSign"
                                              sslProtocols="Ssl3|Tls1|Tls11">
                          </transport>
                      </security>
                  </binding>
              </netTcpBinding>
          </bindings>
      </system.serviceModel>
  </configuration>
  ```

- バインドがソース コードで動的に構成されている場合は、ソース コード内で TLS 1.1 (<xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType>) または以前のバージョンのプロトコルを使用するように <xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType> プロパティを更新します。

  > [!CAUTION]
  > MD5 ハッシュ アルゴリズムによる証明書は安全でないと見なされるため、この回避策はお勧めできません。

## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
