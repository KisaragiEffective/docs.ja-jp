---
title: Azure Active Directory
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |Azure Active Directory
ms.date: 06/30/2019
ms.openlocfilehash: 207043507a9052c47683383a98cef6417a1a2740
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840731"
---
# <a name="azure-active-directory"></a>Azure Active Directory

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Microsoft Azure Active Directory (Azure AD) は、id およびアクセス管理をサービスとして提供します。 お客様は、ユーザーの所有者、情報の保存先、その情報にアクセスできるユーザー、その情報を管理できるユーザー、およびアプリにアクセスできる対象を構成および管理するために使用します。 AAD は、シングルサインオン (SSO) エクスペリエンスを提供して、ユーザーを使用するように構成されたアプリケーションに対してユーザーを認証できます。 独自に使用することも、オンプレミスで実行されている Windows AD と統合することもできます。

Azure AD はクラウド用に構築されています。 これは、LDAP を使用する Windows AD とは異なり、REST ベースの Graph API とクエリに OData 構文を使用するクラウドネイティブの id ソリューションです。 オンプレミス Active Directory では、Id 同期サービスを使用してユーザー属性をクラウドに同期させることができます。これにより、Azure AD を使用して、すべての認証をクラウドで行うことができます。 または、オンプレミスの Windows AD によって実行される ADFS 経由でローカル Active Directory に渡すために、Connect を使用して認証を構成することもできます。

Azure AD は、オンプレミスでホストされているアプリケーションに SSO を提供するために使用される会社ブランドのサインイン画面、マルチファクトリ認証、およびクラウドベースのアプリケーションプロキシをサポートしています。 さまざまな種類のセキュリティレポートとアラート機能を提供します。

## <a name="references"></a>参照

- [Microsoft id プラットフォーム](https://docs.microsoft.com/azure/active-directory/develop/)

>[!div class="step-by-step"]
>[前へ](authentication-authorization.md)
>[次へ](identity-server.md)
