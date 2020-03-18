---
title: 暗号での破壊的変更
description: .NET Core で暗号化に関連する破壊的変更の一覧を示します。
ms.date: 02/10/2020
ms.openlocfilehash: c25eefa8e3ee01ed7a1df4ec4aa9225f2c347a4d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77449222"
---
# <a name="cryptography-breaking-changes"></a>暗号での破壊的変更

このページでは、次の破壊的変更について説明します。

| 破壊的変更 | 導入されたバージョン |
| - | :-: |
| [EnvelopedCms で AES-256 暗号化を既定で使用](#envelopedcms-defaults-to-aes-256-encryption) | 3.0 |
| [RSAOpenSsl キー生成の最小サイズの増加](#minimum-size-for-rsaopenssl-key-generation-has-increased) | 3.0 |
| [.NET Core 3.0 では、OpenSSL 1.0.x よりも OpenSSL 1.1.x を優先する](#net-core-30-prefers-openssl-11x-to-openssl-10x) | 3.0 |
| [Pkcs8PrivateKeyInfo コンストラクターでの引数の検証の改善](#better-argument-validation-in-the-pkcs8privatekeyinfo-constructor) | 3.0 |
| [SignedCms.ComputeSignature のブール型パラメーターの尊重](#boolean-parameter-of-signedcmscomputesignature-is-respected) | 2.1 |

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[EnvelopedCms defaults to AES-256 encryption](~/includes/core-changes/cryptography/3.0/envelopedcms-defaults-to-aes256.md)]

***

[!INCLUDE[Minimum size for RSAOpenSsl key generation has increased](~/includes/core-changes/cryptography/3.0/minimum-rsaopenssl-key-size-change.md)]

***

[!INCLUDE[.NET Core 3.0 prefers OpenSSL 1.1.x to OpenSSL 1.0.x](~/includes/core-changes/cryptography/3.0/net-core-3-0-prefers-openssl-1-1-x.md)]

***

[!INCLUDE[Better argument validation in the Pkcs8PrivateKeyInfo constructor](~/includes/core-changes/cryptography/3.0/better-argument-validation-in-pkcs8privatekeyinfo-ctor.md)]

***

## <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE [Boolean parameter of SignedCms.ComputeSignature is respected](~/includes/core-changes/cryptography/2.1/compute-signature-silent-parameter.md)]

***
