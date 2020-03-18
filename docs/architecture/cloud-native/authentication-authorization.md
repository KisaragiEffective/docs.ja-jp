---
title: クラウドネイティブアプリでの認証と承認
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |クラウドネイティブアプリでの認証と承認
ms.date: 06/30/2019
ms.openlocfilehash: a397175e98c2e8103d2a8c372c81fcca65f19b11
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840749"
---
# <a name="authentication-and-authorization-in-cloud-native-apps"></a>クラウドネイティブのアプリにおける認証と承認

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

*認証*は、セキュリティプリンシパルの id を特定するプロセスです。 *承認*とは、認証されたプリンシパルのアクセス許可を付与して、アクションを実行したり、リソースにアクセスしたりする行為です。 `AuthN` に対して認証が短縮され、`AuthZ`に対して承認が短縮されることがあります。 クラウドネイティブアプリケーションは、クライアントとアプリケーションの両方が任意のプラットフォームまたはデバイスで実行される可能性があるため、セキュリティプリンシパルを認証するためにオープン HTTP ベースのプロトコルに依存する必要があります。 唯一の共通要素は HTTP です。

多くの組織は、Active Directory フェデレーションサービス (AD FS) (ADFS) などのローカル認証サービスを引き続き利用しています。 このアプローチは従来、オンプレミスの認証ニーズに適していますが、クラウドネイティブアプリケーションは、クラウド専用に設計されたシステムの恩恵を受けています。 最近の2019英国国立サイバーセキュリティセンター (NCSC) 勧告では、"Azure AD をプライマリ認証ソースとして使用する組織は、ADFS と比較して実際にリスクを下げることができます。" [この分析](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)では、次のような理由が考えられます。

- Microsoft credential protection テクノロジの完全なセットへのアクセス。
- ほとんどの組織は、既に何らかの範囲の Azure AD に依存しています。
- NTLM ハッシュを二重にハッシュすると、ローカル Active Directory で動作する資格情報が侵害されることがなくなります。

## <a name="references"></a>参照

- [認証の基本](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)
- [アクセストークンと要求](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
- [オンプレミスの認証サービスを溝に時間がかかる場合があります](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)

>[!div class="step-by-step"]
>[前へ](identity.md)
>[次へ](azure-active-directory.md)
