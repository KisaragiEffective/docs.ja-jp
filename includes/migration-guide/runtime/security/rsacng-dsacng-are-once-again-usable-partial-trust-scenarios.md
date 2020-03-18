---
ms.openlocfilehash: 8b41e3234c00059ecb5088bbf2597611d7f139b8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857231"
---
### <a name="rsacng-and-dsacng-are-once-again-usable-in-partial-trust-scenarios"></a>部分信頼のシナリオで RSACng と DSACng が再び使用可能に

|   |   |
|---|---|
|説明|CngLightup (<xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType> などの複数の上位レベル暗号化 API で使われます) および <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> は、完全信頼に依存する場合があります。 たとえば、<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> アクセス許可をアサートしない P/Invoke や、<xref:System.Security.Cryptography.CngKey?displayProperty=nameWithType> に <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> に対するアクセス許可要求があるコード パスなどです。 .NET Framework 4.6.2 以降では、CngLightup は可能な場合に常に <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> に切り替えるために使われました。 その結果、<xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType> を正常に使っていた部分信頼アプリが、失敗して <xref:System.Security.SecurityException> 例外をスローするようになりました。この変更では、CngLightup を使っているすべての関数が必要なアクセス許可を持つように、必要なアサートが追加されます。|
|提案される解決策|.NET Framework 4.6.2 でのこの変更により部分信頼アプリに悪影響があった場合は、.NET Framework 4.7.1 にアップグレードしてください。|
|スコープ|エッジ|
|バージョン|4.6.2|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.DSACng.%23ctor(System.Security.Cryptography.CngKey)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.LegalKeySizes?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.CreateSignature(System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.VerifySignature(System.Byte[],System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.%23ctor(System.Security.Cryptography.CngKey)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.Decrypt(System.Byte[],System.Security.Cryptography.RSAEncryptionPadding)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.SignHash(System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType></li></ul>|
