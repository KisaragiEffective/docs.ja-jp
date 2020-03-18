---
ms.openlocfilehash: b49b2f88b130bb952b77964d5bf38374dc606385
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74567984"
---
### <a name="net-core-30-prefers-openssl-11x-to-openssl-10x"></a>.NET Core 3.0 では、OpenSSL 1.0.x よりも OpenSSL 1.1.x が優先されます

Linux 用 .NET Core では、複数の Linux ディストリビューションで動作するため、OpenSSL 1.0.x と OpenSSL 1.1.x の両方がサポートされます。  .NET Core 2.1 および .NET Core 2.2 では、最初に 1.0.x が検索されてから、1.1.x にフォールバックされます。 .NET Core 3.0 では、最初に 1.1.x が検索されます。 この変更は、新しい暗号化標準のサポートを追加するために行われました。

この変更は、.NET Core で OpenSSL 固有の相互運用型とのプラットフォーム相互運用を行うライブラリまたはアプリケーションに影響を与える可能性があります。

#### <a name="change-description"></a>変更の説明

.NET Core 2.2 以前のバージョンで、ランタイムでは OpenSSL 1.1.x よりも 1.0.x の読み込みが優先されます。 これは、OpenSSL との相互運用に使用する <xref:System.IntPtr> と <xref:System.Runtime.InteropServices.SafeHandle> の型が優先設定に応じて libcrypto.so.1.0.0/libcrypto.so.1.0/libcrypto.so.10 のいずれかで使用されることを意味します。

.NET Core 3.0 以降で、ランタイムでは OpenSSL 1.0.x よりも OpenSSL 1.1.x の読み込みが優先されるため、OpenSSL との相互運用に使用する <xref:System.IntPtr> と <xref:System.Runtime.InteropServices.SafeHandle> の型は優先設定に応じて libcrypto.so.1.1/libcrypto.so.11/libcrypto.so.1.1.0/libcrypto.so.1.1.1 のいずれかで使用されます。 その結果、OpenSSL と直接相互運用できるライブラリとアプリケーションは、.NET Core 2.1 または .NET Core 2.2 からアップグレードするときに、.NET Core で公開されている値と互換性のないポインターを持つ場合があります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

OpenSSL を直接操作するライブラリやアプリケーションは、.NET Core ランタイムと同じバージョンの OpenSSL を使用するように注意する必要があります。

OpenSSL を直接使用する .NET Core 暗号化の種類から <xref:System.IntPtr> または <xref:System.Runtime.InteropServices.SafeHandle> の値を使用するすべてのライブラリやアプリケーションでは、新しい <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion?displayProperty=nameWithType> プロパティで使用するライブラリのバージョンを比較し、ポインターの互換性を確保する必要があります。

#### <a name="category"></a>カテゴリ

暗号

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.Security.Cryptography.SafeEvpPKeyHandle.%23ctor%2A?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor(System.IntPtr)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSAOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSAOpenSsl.%23ctor(System.IntPtr)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSAOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSAOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.%23ctor(System.IntPtr)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.%23ctor(System.IntPtr)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.X509Certificate.Handle?displayProperty=nameWithType>

<!--

### Affected APIs

- `Overload:System.Security.Cryptography.SafeEvpPKeyHandle.#ctor`
- `M:System.Security.Cryptography.RSAOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.RSAOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.RSAOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.DSAOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.DSAOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.DSAOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.ECDsaOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.ECDsaOpenSsl.#ctor(System.Security.CryptographySafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.ECDsaOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.DuplicateKeyHandle`
- `P:System.Security.Cryptography.X509Certificates.X509Certificate.Handle`

-->
