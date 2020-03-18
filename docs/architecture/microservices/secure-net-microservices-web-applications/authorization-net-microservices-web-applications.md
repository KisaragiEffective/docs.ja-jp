---
title: .NET マイクロサービスと Web アプリケーションでの承認について
description: .NET マイクロサービスと Web アプリケーションのセキュリティ - ASP.NET Core アプリケーションの主な承認オプション (ロールベースとポリシーベース) の概要について説明します。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: f6b69faceac9a9b4819212cc04f89080f3ddad56
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77501764"
---
# <a name="about-authorization-in-net-microservices-and-web-applications"></a>.NET マイクロサービスと Web アプリケーションでの承認について

認証の後に、ASP.NET Core Web API がアクセスを承認する必要があります。 このプロセスによって、サービスが一部の認証ユーザーに API を提供できますが、すべてのユーザーではありません。 [承認](/aspnet/core/security/authorization/introduction)は、ユーザーのロールまたはカスタム ポリシーに基づいて行われ、要求またはその他のヒューリスティックの調査が含まれる場合があります。

次の例に示すように、ASP.NET Core MVC ルートへのアクセス制限は、アクション メソッド (またはすべてのコントローラーのアクションで承認が必要な場合は、コントローラーのクラス) に Authorize 属性を適用するのと同じくらい簡単です。

```csharp
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    [Authorize]
    public ActionResult Logout()
    {
    }
}
```

既定では、パラメーターなしの Authorize 属性を追加すると、そのコントローラーまたはアクションの認証されたユーザーへのアクセスが制限されます。 特定のユーザーだけに利用できる API をさらに制限するには、ユーザーが満たす必要がある必要なロールまたはポリシーを指定するように属性を拡張することができます。

## <a name="implement-role-based-authorization"></a>ロールベースの承認を実装する

ASP.NET Core ID には、ロールの概念が組み込まれています。 ユーザーに加えて、ASP.NET Core ID は、アプリケーションによって使用されるさまざまなロールに関する情報を格納し、どのユーザーがどのロールに割り当てられているかを追跡します。 このような割り当ては、永続化されたストレージ内のロールを更新する `RoleManager` の種類と、ユーザーのロールの付与または取り消しを行う `UserManager` の種類を使用してプログラムで変更できます。

JWT ベアラー トークンによる認証を行う場合、ASP.NET Core JWT ベアラー認証ミドルウェアは、トークンで見つかったロール要求に基づいてユーザーのロールを入力します。 MVC アクションまたはコントローラーへのアクセスを特定のロールのユーザーに制限するために、次のコード フラグメントに示すように、Authorize 注釈 (属性) に Roles パラメーターを含めることができます。

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

この例では、Administrator または PowerUser ロールのユーザーのみが、ControlPanel コントローラーの API (SetTime アクションの実行など) にアクセスできます。 ShutDown API は、Administrator ロールのユーザーのみにアクセスを許可するようにさらに制限します。

ユーザーに複数のロールが割り当てられていることを必須にするには、次の例で示すように、複数の Authorize 属性を使用します。

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
[Authorize(Roles = "RemoteEmployee ")]
[Authorize(Policy = "CustomPolicy")]
public ActionResult API1 ()
{
}
```

この例では、API1 を呼び出すために、ユーザーは次の条件を満たす必要があります。

- Adminstrator "*または*" PowerUser ロールであり、"*かつ*"

- RemoteEmployee ロールであり、*かつ*

- CustomPolicy 承認のためのカスタム ハンドラーを満たしている。

## <a name="implement-policy-based-authorization"></a>ポリシーベースの承認を実装する

カスタム承認規則も[承認ポリシー](https://docs.asp.net/en/latest/security/authorization/policies.html)を使用して記述できます。 ここでは、概要について説明します。 詳細については、[ASP.NET Authorization Workshop](https://github.com/blowdart/AspNetAuthorizationWorkshop) に関するページを参照してください。

カスタム承認ポリシーは、service.AddAuthorization メソッドを使用して、Startup.ConfigureServices メソッドに登録されます。 このメソッドは、AuthorizationOptions 引数を構成するデリゲートを受け取ります。

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("AdministratorsOnly", policy =>
        policy.RequireRole("Administrator"));

    options.AddPolicy("EmployeesOnly", policy =>
        policy.RequireClaim("EmployeeNumber"));

    options.AddPolicy("Over21", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});
```

例に示すように、ポリシーをさまざまな種類の要件に関連付けることができます。 ポリシーが登録された後に、ポリシーの名前を Authorize 属性の Policy 引数として渡すことで、アクションまたはコントローラーにポリシーを適用できます (たとえば、`[Authorize(Policy="EmployeesOnly")]`)。ポリシーは、これらの例に示すように 1 つだけでなく複数の要件を持つことができます。

前の例では、AddPolicy の最初の呼び出しは、ロールによる承認の代替の方法です。 `[Authorize(Policy="AdministratorsOnly")]` が API に適用される場合、Administrator ロールのユーザーのみがアクセスできます。

2 番目の <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.AddPolicy%2A> の呼び出しは、特定の要求をユーザーに提示することをユーザーに義務付けるための簡単な方法を示しています。 <xref:Microsoft.AspNetCore.Authorization.AuthorizationPolicyBuilder.RequireClaim%2A> メソッドは、また、必要に応じて、要求の予期される値を受け取ります。 値を指定する場合、ユーザーが正しい種類の要求と、指定された値のいずれかの両方を持っている場合にのみこの要件を満たします。 JWT ベアラー認証ミドルウェアを使用している場合、すべての JWT プロパティはユーザーの要求として使用可能になります。

ここで示す最も注目すべきポリシーは、カスタム承認要件を使用している 3 番目の `AddPolicy` メソッドです。 カスタム承認要件を使用すると、承認を実行する方法を細かく制御することができます。 これを機能させるには、次の種類を実装する必要があります。

- <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> から派生し、要件の詳細を指定するフィールドを格納している要件の種類。 例では、これはサンプル `MinimumAgeRequirement` の種類の年齢フィールドです。

- <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601> を実装するハンドラー。T は、ハンドラーが満たすことができる <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> の種類です。 このハンドラーは、ユーザーに関する情報を格納している指定されたコンテキストが要件を満たすかどうかをチェックする <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601.HandleRequirementAsync%2A> メソッドを実装する必要があります。

ユーザーが要件を満たす場合、`context.Succeed` への呼び出しが、ユーザーを承認されていることを示します。 ユーザーが承認要件を満たす方法が複数ある場合、複数のハンドラーを作成することができます。

カスタム ポリシーの要件の `AddPolicy` 呼び出しへの登録に加えて、依存関係の挿入 (`services.AddTransient<IAuthorizationHandler, MinimumAgeHandler>()`) を使用してカスタム要件ハンドラーも登録する必要があります。

カスタム承認要件およびユーザーの年齢 (`DateOfBirth` 要求に基づく) をチェックするハンドラーの例については、ASP.NET Core [承認ドキュメント](https://docs.asp.net/en/latest/security/authorization/policies.html)を参照してください。

## <a name="additional-resources"></a>その他のリソース

- **ASP.NET Core の認証** \
  [https://docs.microsoft.com/aspnet/core/security/authentication/identity](/aspnet/core/security/authentication/identity)

- **ASP.NET Core の承認** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/introduction](/aspnet/core/security/authorization/introduction)

- **ロールベースの承認** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/roles](/aspnet/core/security/authorization/roles)

- **カスタム ポリシーベースの承認** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/policies](/aspnet/core/security/authorization/policies)

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](developer-app-secrets-storage.md)
