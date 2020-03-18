---
ms.openlocfilehash: fc315faef750d93d914104dd568078aa3fc430d4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "72887796"
---
### <a name="rsacngverifyhash-now-returns-false-for-any-verification-failure"></a>RSACng.VerifyHash で検証が失敗した場合に False が返されるようになった

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 以降では、署名自体の形式が正しくない場合、このメソッドでは **False** が返されます。 今すぐ false を返しますの検証に失敗しました。 .NET Framework 4.6 および 4.6.1 では、メソッドによってスローされる、<xref:System.Security.Cryptography.CryptographicException?displayProperty=name>署名自体が正しくフォーマットされている場合。|
|提案される解決策|検証が失敗し、メソッドで <xref:System.Security.Cryptography.CryptographicException?displayProperty=name>False**が返される場合は、代わりにその実行が** の処理に依存するコードが実行される必要があります。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.RSACng.VerifyHash(System.Byte[],System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType></li></ul>|
