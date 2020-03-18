---
title: クラウドネイティブアプリ用のサーバー
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |IdentityServer
ms.date: 06/30/2019
ms.openlocfilehash: 48d0b95a40682f3127127851781b4d0e26e44630
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728576"
---
# <a name="identityserver-for-cloud-native-applications"></a>クラウドネイティブアプリケーション用のサーバー

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

サービスは、OpenID Connect (OIDC) と OAuth 2.0 標準を実装するオープンソースの認証サーバーで、ASP.NET Core に使用します。 これは、web、ネイティブ、モバイル、API のいずれのエンドポイントであるかにかかわらず、すべてのアプリケーションに対する要求を認証するための一般的な方法を提供するように設計されています。 サーバーを使用して、複数のアプリケーションとアプリケーションの種類にシングルサインオン (SSO) を実装できます。 サインインフォームや同様のユーザーインターフェイスを使用して実際のユーザーを認証するために使用できます。また、通常は、ユーザーインターフェイスなしでトークンの発行、検証、更新を含むサービスベースの認証も行います。 サービスはカスタマイズ可能なソリューションとして設計されています。 各インスタンスは、通常、個々の組織やアプリケーションのニーズに合わせてカスタマイズされます。

## <a name="common-web-app-scenarios"></a>一般的な web アプリのシナリオ

通常、アプリケーションでは、次のシナリオの一部またはすべてをサポートする必要があります。

- ブラウザーを使用して web アプリケーションにアクセスする人のユーザー。
- ブラウザーベースのアプリからバックエンド Web Api にアクセスする人のユーザー。
- バックエンド Web Api にアクセスするモバイル/ネイティブクライアントの人のユーザー。
- バックエンド Web Api にアクセスする他のアプリケーション (アクティブなユーザーまたはユーザーインターフェイスなし)。
- アプリケーションは、独自の id を使用するか、ユーザーの id を委任することによって、他の Web Api と対話する必要がある場合があります。

![アプリケーションの種類とシナリオ](./media/application-types.png)
**図 8-1**。 アプリケーションの種類とシナリオ。

これらの各シナリオでは、公開されている機能を承認されていない使用から保護する必要があります。 これは、少なくとも、リソースの要求を行うユーザーまたはプリンシパルを認証する必要があります。 この認証では、SAML2p、WS-ATOMICTRANSACTION、OpenID Connect など、いくつかの一般的なプロトコルのいずれかを使用できます。 Api との通信は、通常、OAuth2 プロトコルとセキュリティトークンのサポートを使用します。 これらの重要なクロスカットセキュリティの問題とその実装の詳細をアプリケーション自体から分離することで、一貫性が確保され、セキュリティと保守性が向上します。 このような懸念を、独自の製品にアウトソーシングすると、すべてのアプリケーションがこれらの問題を解決するのに役立ちます。

サーバーは、ASP.NET Core アプリケーション内で実行されるミドルウェアを提供し、OpenID Connect と OAuth2 のサポートを追加します (「[サポートされる仕様](http://docs.identityserver.io/en/latest/intro/specs.html)」を参照してください)。 組織は、すべてのトークンベースのセキュリティプロトコルの STS として機能するために、ユーザーが独自の ASP.NET Core アプリを作成します。 サーバーミドルウェアは、次のような標準的な機能をサポートするためにエンドポイントを公開します。

- 承認 (エンドユーザーの認証)
- Token (プログラムによるトークンの要求)
- 検出 (サーバーに関するメタデータ)
- ユーザー情報 (有効なアクセストークンを使用してユーザー情報を取得する)
- デバイスの承認 (デバイスフローの承認を開始するために使用)
- イントロスペクション (トークンの検証)
- 失効 (トークンの失効)
- セッションの終了 (すべてのアプリでのシングルサインアウトのトリガー)

## <a name="getting-started"></a>作業の開始

IdentityServer4 はオープンソースであり、自由に使用できます。 NuGet パッケージを使用して、アプリケーションに追加することができます。 メインパッケージは、400万回以上ダウンロードされた[IdentityServer4](https://www.nuget.org/packages/IdentityServer4/)です。 基本パッケージには、ユーザーインターフェイスコードは含まれず、メモリ構成でのみサポートされます。 データベースで使用するには、Entity Framework Core を使用して、ユーザーの構成データとオペレーションデータを格納する[IdentityServer4](https://www.nuget.org/packages/IdentityServer4.EntityFramework)のようなデータプロバイダーも必要になります。 ユーザーインターフェイスでは、[クイックスタート UI リポジトリ](https://github.com/IdentityServer/IdentityServer4.Quickstart.UI)から ASP.NET Core MVC アプリケーションにファイルをコピーして、ユーザーのサインインとサインアウトのサポートを追加することができます。

## <a name="configuration"></a>構成

ユーザーは、各カスタムインストールの一部として構成できるさまざまな種類のプロトコルとソーシャル認証プロバイダーをサポートしています。 これは通常、`ConfigureServices` メソッドの ASP.NET Core アプリケーションの `Startup` クラスで行われます。 構成には、サポートされるプロトコル、および使用されるサーバーとエンドポイントへのパスの指定が含まれます。 図8-2 は、IdentityServer4 クイックスタート UI プロジェクトから取得した構成の例を示しています。

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();

        // some details omitted
        services.AddIdentityServer();

          services.AddAuthentication()
            .AddGoogle("Google", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;

                options.ClientId = "<insert here>";
                options.ClientSecret = "<insert here>";
            })
            .AddOpenIdConnect("demoidsrv", "IdentityServer", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;
                options.SignOutScheme = IdentityServerConstants.SignoutScheme;

                options.Authority = "https://demo.identityserver.io/";
                options.ClientId = "implicit";
                options.ResponseType = "id_token";
                options.SaveTokens = true;
                options.CallbackPath = new PathString("/signin-idsrv");
                options.SignedOutCallbackPath = new PathString("/signout-callback-idsrv");
                options.RemoteSignOutPath = new PathString("/signout-idsrv");

                options.TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name",
                    RoleClaimType = "role"
                };
            });
    }
}
```

**図 8-2**。 サーバーを構成しています。

また、サービスは、さまざまなプロトコルと構成をテストするために使用できるパブリックデモサイトもホストします。 これは[https://demo.identityserver.io/](https://demo.identityserver.io/)にあり、提供された `client_id` に基づいて動作を構成する方法に関する情報が含まれています。

## <a name="javascript-clients"></a>JavaScript クライアント

多くのクラウドネイティブアプリケーションでは、フロントエンドでサーバー側 Api とリッチクライアントシングルページアプリケーション (spa) を利用しています。 ユーザーは、NPM を使用して[JavaScript クライアント](http://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html)(`oidc-client.js`) を提供します。これを spas に追加することにより、web api のサインイン、サインアウト、トークンベースの認証に使用できるようになります。

## <a name="references"></a>References

- [サーバーのドキュメント](http://docs.identityserver.io/en/latest/)
- [アプリケーションの種類](https://docs.microsoft.com/azure/active-directory/develop/app-types)
- [JavaScript OIDC クライアント](http://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html)

>[!div class="step-by-step"]
>[前へ](azure-active-directory.md)
>[次へ](security.md)
