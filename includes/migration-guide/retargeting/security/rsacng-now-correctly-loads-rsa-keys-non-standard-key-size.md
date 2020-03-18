---
ms.openlocfilehash: 4892f75e4ae673d9d9cc7e9eeb6fb9b1a73f572e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859349"
---
### <a name="rsacng-now-correctly-loads-rsa-keys-of-non-standard-key-size"></a>RSACng で非標準のキー サイズの RSA キーが正しく読み込まれるようになった

|   |   |
|---|---|
|説明|4\.6.2 より前の .NET Framework バージョンでは、RSA 証明書のキー サイズが標準ではない顧客は、拡張メソッドの <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=name> や <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=name> でそのキーにアクセスできません。  <xref:System.Security.Cryptography.CryptographicException?displayProperty=name>The requested key size is not supported (要求されたサイズのキーには対応していません)&quot; というメッセージと共に &quot; がスローされます。 .NET Framework 4.6.2 では、この問題が修正されました。 同様に、<xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)> と <xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)> で <xref:System.Security.Cryptography.CryptographicException?displayProperty=name> をスローすることなく非標準サイズのキーが処理されるようになりました。|
|提案される解決策|非標準サイズのキーが使用されるときに <xref:System.Security.Cryptography.CryptographicException?displayProperty=name> がスローされるという以前の動作に依存する例外処理ロジックがある場合、そのロジックを除くことを検討してください。|
|スコープ|エッジ|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType></li></ul>|
