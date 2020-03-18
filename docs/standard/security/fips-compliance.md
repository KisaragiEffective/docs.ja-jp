---
title: FIPS 準拠-.NET Core
description: .NET Core Federal Information Processing Standards (FIPS) 準拠について説明します。
ms.date: 11/20/2019
author: Rick-Anderson
ms.author: riande
ms.openlocfilehash: 84d6edc6975af0484bb67999ad5891eaf4db057d
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77626959"
---
# <a name="net-core-federal-information-processing-standard-fips-compliance"></a>.NET Core Federal Information Processing Standards (FIPS) 準拠

Federal Information Processing Standards (FIPS) 公開140-2 は、情報技術製品の暗号化モジュールに最低限必要なセキュリティ要件を規定する米国政府の標準です。詳細については、「」のセクション5131で定義されています。1996のテクノロジ管理の改善。

.NET Core:

* は、基になるオペレーティングシステムが提供する標準モジュールに暗号化プリミティブの呼び出しを渡します。
* では、.NET Core アプリで FIPS 承認アルゴリズムまたはキーサイズを使用することは強制**されません**。

システム管理者は、オペレーティングシステムの FIPS 準拠を構成する役割を担います。

コードが FIPS 準拠環境用に記述されている場合、開発者は準拠していない FIPS アルゴリズムが使用されていないことを確認する責任があります。

FIPS 準拠の詳細については、次の記事を参照してください。

* [Windows FIPS 準拠](/windows/security/threat-protection/fips-140-validation)
* [FIPS 準拠のための Windows の構成](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing)
* [Chapter 8連邦標準および規制 Red Hat Enterprise Linux 7](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/chap-federal_standards_and_regulations)
