---
description: 詳細については、「偽装と復帰」を参照してください。
title: 偽装と復帰
ms.date: 07/15/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WindowsIdentity objects, impersonating
- security [.NET], impersonating Windows accounts
- impersonating Windows accounts
ms.assetid: b93d402c-6c28-4f50-b2bc-d9607dc3e470
ms.openlocfilehash: f3e536f87ef5aa09cd727fe9674c7a40f3a09150
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99685011"
---
# <a name="impersonating-and-reverting"></a>偽装と復帰

> [!NOTE]
> この記事は、Windows に適用されます。
>
> ASP.NET Core の詳細については、「 [ASP.NET Core Security](/aspnet/core/security/)」を参照してください。

場合によっては、Windows アカウント トークンを取得して、Windows アカウントを偽装する必要があります。 たとえば、ASP.NET ベースのアプリケーションが、時間によって複数のユーザーの代わりに操作しなければならない場合があります。 その場合、アプリケーションはインターネット インフォメーション サービス (IIS) から管理者を表すトークンを受け入れて、そのユーザーを偽装し、操作を実行してから前の ID に戻ります。 続いて、IIS から管理者より権限が少ないユーザーを表すトークンを受け入れ、操作を実行してから、また元に戻ります。  
  
 アプリケーションが、IIS によって現在のスレッドにアタッチされていない Windows アカウントを偽装しなければならない場合は、そのアカウントのトークンを取得し、そのトークンを使用してアカウントをアクティブ化する必要があります。 それには、次のタスクを実行します。  
  
1. アンマネージ **LogonUser** メソッドを呼び出すことによって、特定のユーザーのアカウント トークンを取得します。 このメソッドは .NET 基底クラスライブラリには含まれていませんが、アンマネージ **advapi32.dll** にあります。 アンマネージ コード内のメソッドへのアクセスは高度な操作であり、ここでは説明しません。 詳細については、「[アンマネージ コードとの相互運用](../../framework/interop/index.md)」を参照してください。 **LogonUser** メソッドと **advapi32.dll** の詳細については、プラットフォーム SDK ドキュメントを参照してください。  
  
2. **WindowsIdentity** クラスの新しいインスタンスを作成し、トークンを渡します。 次のコードに、この呼び出しを示します。ここで、`hToken` は Windows トークンを表します。  
  
    ```csharp  
    WindowsIdentity impersonatedIdentity = new WindowsIdentity(hToken);  
    ```  
  
    ```vb  
    Dim impersonatedIdentity As New WindowsIdentity(hToken)  
    ```  
  
3. このコードに示されているように、<xref:System.Security.Principal.WindowsImpersonationContext> クラスの新しいインスタンスを作成し、そのインスタンスを、初期化されたクラスの <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A?displayProperty=nameWithType> メソッドで初期化することで、偽装を開始します。  
  
    ```csharp  
    WindowsImpersonationContext myImpersonation = impersonatedIdentity.Impersonate();  
    ```  
  
    ```vb  
    WindowsImpersonationContext myImpersonation = impersonatedIdentity.Impersonate()  
    ```  
  
4. 偽装する必要がなくなったら、以下のコードに示されているように <xref:System.Security.Principal.WindowsImpersonationContext.Undo%2A?displayProperty=nameWithType> メソッドを呼び出して、偽装を元に戻します。  
  
    ```csharp  
    myImpersonation.Undo();  
    ```  
  
    ```vb  
    myImpersonation.Undo()  
    ```  
  
 信頼されたコードが既にオブジェクトをスレッドにアタッチしている場合は、 <xref:System.Security.Principal.WindowsPrincipal> アカウントトークンを受け取らないインスタンスメソッド **Impersonate** を呼び出すことができます。 この方法が役立つのは、スレッドで **WindowsPrincipal** オブジェクトが表しているユーザーが、現在プロセスが実行されているユーザーではない場合のみです。 このような状態は、たとえば、Windows 認証を有効にして、偽装を無効にした ASP.NET を使用している場合に発生することがあります。 その場合、プロセスはインターネット インフォメーション サービス (IIS) で構成されたアカウントで実行されますが、現在のプリンシパルは、ページにアクセスしている Windows ユーザーを表しています。  
  
 **Impersonate** も **Undo** も、現在の <xref:System.Security.Principal.IPrincipal> 呼び出しコンテキストに関連付けられているプリンシパルオブジェクト () を変更しないことに注意してください。 代わりに、偽装と復帰によって、現在のオペレーティングシステムプロセスに関連付けられているトークンが変更されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Security.Principal.WindowsIdentity>
- <xref:System.Security.Principal.WindowsImpersonationContext>
- [プリンシパル オブジェクトと ID オブジェクト](principal-and-identity-objects.md)
- [アンマネージ コードとの相互運用](../../framework/interop/index.md)
- [ASP.NET Core のセキュリティ](/aspnet/core/security/)
