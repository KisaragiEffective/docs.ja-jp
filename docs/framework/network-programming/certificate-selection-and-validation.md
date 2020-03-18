---
title: 証明書の選択と検証
ms.date: 03/30/2017
ms.assetid: c933aca2-4cd0-4ff1-9df9-267143f25a6f
ms.openlocfilehash: aea47360ab1bb9dad446a5a7b19a91ea688953c4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048747"
---
# <a name="certificate-selection-and-validation"></a>証明書の選択と検証
<xref:System.Net> クラスは、Secure Socket Layer (SSL) 接続の <xref:System.Security.Cryptography.X509Certificates> を選択および検証する方法を複数サポートしています。 クライアントは、サーバーに対する認証に 1 つまたは複数の証明書を選択できます。 サーバーは、クライアント証明書の認証に固有の属性が 1 つまたは複数あることを必須にすることができます。  
  
## <a name="definition"></a>定義  
 証明書は、公開キー、属性 (バージョン番号、シリアル番号、有効期限など)、および証明機関のデジタル署名を含む ASCII バイト ストリームです。 証明書は、暗号化された接続を確立するため、またはサーバーに対してクライアントを認証するために使用されます。  
  
## <a name="client-certificate-selection-and-validation"></a>クライアント証明書の選択と検証  
 クライアントは、特定の SSL 接続に 1 つまたは複数の証明書を選択できます。 クライアント証明書は、Web サーバーまたは SMTP メール サーバーとの SSL 接続と関連付けることができます。 クライアントは、<xref:System.Security.Cryptography.X509Certificates.X509Certificate> または <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> クラス オブジェクトのコレクションに証明書を追加します。 たとえば、電子メールを使用する場合、証明書コレクションは、<xref:System.Security.Cryptography.X509Certificates.X509CertificateCollection> クラスの <xref:System.Net.Mail.SmtpClient.ClientCertificates%2A> プロパティと関連付けられた <xref:System.Net.Mail.SmtpClient> のインスタンスです。 <xref:System.Net.HttpWebRequest> クラスには類似した <xref:System.Net.HttpWebRequest.ClientCertificates%2A> プロパティがあります。  
  
 <xref:System.Security.Cryptography.X509Certificates.X509Certificate> クラスと <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> クラスの主な違いは、秘密キーが <xref:System.Security.Cryptography.X509Certificates.X509Certificate> クラスの証明書ストアに存在する必要がある点です。  
  
 証明書がコレクションに追加され、特定の SSL コレクションと関連付けられている場合でも、サーバーが必須としていない場合、サーバーに証明書は送信されません。 1 つの接続で複数のクライアント証明書が送信された場合、サーバーから提供された証明書発行者の一覧と、クライアント証明書の発行者名との一致を考慮したアルゴリズムに基づいて、最適な証明書が使用されます。  
  
 <xref:System.Net.Security.SslStream> クラスは、SSL ハンドシェイクをさらに細かく制御できます。 クライアントは、使用するクライアント証明書を選択するデリゲートを指定できます。  
  
 リモート サーバーは、クライアント証明書が有効で最新であり、適切な証明機関が署名していることを検証できます。 デリゲートを <xref:System.Net.ServicePointManager.ServerCertificateValidationCallback%2A> に追加して、証明書の検証を行うことができます。  
  
## <a name="client-certificate-selection"></a>クライアント証明書の選択  
 .NET Framework は、次の方法でサーバーに提示するクライアント証明書を選択します。  
  
1. 以前にクライアント証明書がサーバーに提示されていた場合、最初に提示されたときに証明書はキャッシュされ、以降のクライアント証明書要求があった場合に再利用されます。  
  
2. デリゲートが存在する場合は、選択するクライアント証明書として常にデリゲートの結果が使用されます。 可能な限りキャッシュされた証明書の使用を試行してください。ただし、デリゲートが null を返し、証明書コレクションが空ではない場合、キャッシュされた匿名の資格情報は使用しないでください。  
  
3. これがクライアント証明書の最初のチャレンジの場合、.NET Framework は、接続に関連付けられている <xref:System.Security.Cryptography.X509Certificates.X509Certificate> または <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> クラス オブジェクトの証明書を列挙し、サーバーから提供された証明書発行元の一覧と、クライアント証明書の発行元の名前との一致を検索します。 一致した最初の証明書がサーバーに送信されます。 一致する証明書がない場合、または証明書コレクションが空の場合、匿名の資格情報がサーバーに送信されます。  
  
## <a name="tools-for-certificate-configuration"></a>証明書構成のツール  
 クライアントとサーバーの証明書構成には複数のツールを使用できます。  
  
 *Winhttpcertcfg.exe* ツールは、クライアント証明書の構成に使用できます。 *Winhttpcertcfg.exe* ツールは、Windows Server 2003 リソース キットに付属するツールの 1 つとして提供されています。 このツールは、Windows Server 2003 リソース キット ツールの一部として [www.microsoft.com](https://www.microsoft.com) からダウンロードすることもできます。  
  
*HttpCfg.exe* ツールは、<xref:System.Net.HttpListener> クラスのサーバー証明書の構成に使用できます。 *HttpCfg.exe* ツールは、Windows Server 2003 および Windows XP Service Pack 2 のサポート ツールの 1 つとして提供されています。 *HttpCfg.exe* とその他のサポート ツールは、Windows Server 2003 または Windows XP の既定ではインストールされません。 Windows Server 2003 の場合、 サポート ツールは、Windows Server 2003 CD-ROM 上の次のフォルダーとファイルとは別にインストールされます。  
  
 \Support\Tools\Suptools.msi  
  
 Windows XP Service Pack 2 で使用する場合、Windows XP サポート ツールは、[www.microsoft.com](https://www.microsoft.com) からのダウンロードとして使用できます。  
  
 *HttpCfg.exe* ツールのバージョンのソース コードは、Windows Server SDK にサンプルとしても付属しています。 *HttpCfg.exe* サンプルのソース コードは、Windows SDK の一部としてネットワーク サンプルの既定で次のフォルダーにインストールされます。  
  
 *C:\Program Files\Microsoft SDKs\Windows\v1.0\Samples\NetDS\http\serviceconfig*  
  
 これらのツールに加え、<xref:System.Security.Cryptography.X509Certificates.X509Certificate> クラスと <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> クラスには、ファイル システムから証明書を読み込むメソッドがあります。  
  
## <a name="see-also"></a>参照

- [ネットワーク プログラミングにおけるセキュリティ](security-in-network-programming.md)
- [.NET Framework のネットワーク プログラミング](index.md)
