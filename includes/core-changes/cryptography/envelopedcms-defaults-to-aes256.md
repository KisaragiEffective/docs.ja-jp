---
ms.openlocfilehash: b965c3a975b0f2cadd906799fef1665261d96d6e
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71182020"
---
### <a name="envelopedcms-defaults-to-aes-256-encryption"></a><span data-ttu-id="4d8af-101">EnvelopedCms が AES-256 暗号化を既定で使用</span><span class="sxs-lookup"><span data-stu-id="4d8af-101">EnvelopedCms defaults to AES-256 encryption</span></span>

<span data-ttu-id="4d8af-102">`EnvelopedCms` で使用される既定の対称暗号化アルゴリズムが TripleDES から AES-256 に変更されました。</span><span class="sxs-lookup"><span data-stu-id="4d8af-102">The default symmetric encryption algorithm used by `EnvelopedCms` has changed from TripleDES to AES-256.</span></span>

#### <a name="details"></a><span data-ttu-id="4d8af-103">説明</span><span class="sxs-lookup"><span data-stu-id="4d8af-103">Details</span></span>

<span data-ttu-id="4d8af-104">.NET Core Preview 7 以前のバージョンでは、コンストラクターのオーバーロードを使って対称暗号化アルゴリズムを指定せずに <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> を使用してデータを暗号化する場合、データは TripleDES/3DES/3DEA/DES3-EDE アルゴリズムで暗号化されていました。</span><span class="sxs-lookup"><span data-stu-id="4d8af-104">In .NET Core Preview 7 and earlier versions, when <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> is used to encrypt data without specifying a symmetric encryption algorithm via a constructor overload, the data was encrypted with the TripleDES/3DES/3DEA/DES3-EDE algorithm.</span></span>

<span data-ttu-id="4d8af-105">.NET Core 3.0 Preview 8 以降では ([System.Security.Cryptography.Pkcs](https://www.nuget.org/packages/System.Security.Cryptography.Pkcs/) NuGet パッケージのバージョン 4.6.0 を介して)、アルゴリズムを最新化し、既定のオプションのセキュリティを向上するために、既定のアルゴリズムが AES-256 に変更されました。</span><span class="sxs-lookup"><span data-stu-id="4d8af-105">Starting with .NET Core 3.0 Preview 8 (via version 4.6.0 of the [System.Security.Cryptography.Pkcs](https://www.nuget.org/packages/System.Security.Cryptography.Pkcs/) NuGet package), the default algorithm has been changed to AES-256 for algorithm modernization and to improve the security of default options.</span></span> <span data-ttu-id="4d8af-106">メッセージ受信者の証明書に Diffie-Hellman (非 EC) 公開キーが含まれている場合、基になるプラットフォームの制限のために、暗号化操作は、<xref:System.Security.Cryptography.CryptographicException> で失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4d8af-106">If a message recipient certificate has a (non-EC) Diffie-Hellman public key, the encryption operation may fail with a <xref:System.Security.Cryptography.CryptographicException> due to limitations in the underlying platform.</span></span>

<span data-ttu-id="4d8af-107">次のサンプル コードでは、.NET Core 3.0 Preview 7 以前で実行されている場合、データは TripleDES で暗号化され、</span><span class="sxs-lookup"><span data-stu-id="4d8af-107">In the following sample code, the data is encrypted with TripleDES if running on .NET Core 3.0 Preview 7 or earlier.</span></span> <span data-ttu-id="4d8af-108">NET Core 3.0 Preview 8 以降で実行されている場合は AES-256 で暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="4d8af-108">If running on .NET Core 3.0 Preview 8 or later, it is encrypted with AES-256.</span></span>

```csharp
EnvelopedCms cms = new EnvelopedCms(content);
cms.Encrypt(recipient);
return cms.Encode();
```

#### <a name="version-introduced"></a><span data-ttu-id="4d8af-109">導入されたバージョン</span><span class="sxs-lookup"><span data-stu-id="4d8af-109">Version introduced</span></span>

<span data-ttu-id="4d8af-110">3.0 Preview 8</span><span class="sxs-lookup"><span data-stu-id="4d8af-110">3.0 Preview 8</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="4d8af-111">推奨される操作</span><span class="sxs-lookup"><span data-stu-id="4d8af-111">Recommended action</span></span>

<span data-ttu-id="4d8af-112">この変更によって悪影響を受ける場合は、型 <xref:System.Security.Cryptography.Pkcs.AlgorithmIdentifier> を含む <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> コンストラクターで暗号化アルゴリズム識別子を明示的に指定することで、TripleDES 暗号化に戻すことができます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4d8af-112">If you are negatively impacted by the change, you can restore TripleDES encryption by explicitly specifying the encryption algorithm identifier in an <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> constructor that includes a parameter of type <xref:System.Security.Cryptography.Pkcs.AlgorithmIdentifier>, such as:</span></span>

```csharp
Oid tripleDesOid = new Oid("1.2.840.113549.3.7", null);
AlgorithmIdentifier tripleDesIdentifier = new AlgorithmIdentifier(tripleDesOid);
EnvelopedCms cms = new EnvelopedCms(content, tripleDesIdentifier);

cms.Encrypt(recipient);
return cms.Encode()l
```

#### <a name="category"></a><span data-ttu-id="4d8af-113">カテゴリ</span><span class="sxs-lookup"><span data-stu-id="4d8af-113">Category</span></span>

<span data-ttu-id="4d8af-114">暗号</span><span class="sxs-lookup"><span data-stu-id="4d8af-114">Cryptography</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="4d8af-115">影響を受ける API</span><span class="sxs-lookup"><span data-stu-id="4d8af-115">Affected APIs</span></span>

- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.ContentInfo)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)?displayProperty=nameWithType>

<!--

### Affected APIs

- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor(System.Security.Cryptography.Pkcs.ContentInfo)`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)`

-->