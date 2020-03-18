---
title: セキュリティの基本概念
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- unauthorized access
- permissions [.NET Framework]
- security [.NET Framework], about security
ms.assetid: 3cfced4f-ea02-4e66-ae98-d69286363e98
ms.openlocfilehash: b7bcb7e56ca14d129eadcaeac19452d4a443713d
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75705973"
---
# <a name="key-security-concepts"></a>セキュリティの基本概念
Microsoft .NET Framework では、モバイル コードに関するセキュリティへの対応を支援し、各ユーザーにどのような権限を与えるかをコンポーネントで決定できるようにするためのサポートを提供するロールベースのセキュリティがあります。  
  
## <a name="type-safety-and-security"></a>タイプ セーフとセキュリティ  
 タイプ セーフなコードは、アクセス権限を与えられているメモリ位置にだけアクセスします。 (この説明では、タイプセーフはメモリタイプの安全性を意味するので、より広い観点でタイプセーフと混同しないようにする必要があります)。たとえば、タイプセーフなコードでは、別のオブジェクトのプライベートフィールドから値を読み取ることはできません。 適切に定義された許容される方法でだけ、タイプにアクセスします。  
  
 ジャスト イン タイム (JIT: Just-In-Time) コンパイル時に、オプションの検査プロセスは、ネイティブなマシン コードに JIT コンパイルされるメソッドのメタデータと Microsoft Intermediate Language (MSIL) を調べて、タイプ セーフかどうかを確認します。 コードに検査を省略するためのアクセス許可がある場合、このプロセスは省略されます。 検査の詳細については、「[マネージド実行プロセス](../../../docs/standard/managed-execution-process.md)」を参照してください。  
  
 タイプ セーフの検査はマネージド コードの実行に必須ではありませんが、アセンブリの分離とセキュリティの適用において、タイプ セーフであることが重要な意味を持ちます。 コードがタイプ セーフであると、共通言語ランタイムはアセンブリを互いに完全に分離できます。 アセンブリが互いに分離していると、アセンブリどうしが悪い影響を及ぼしあうことがなく、アプリケーションの信頼性が向上します。 タイプ セーフなコンポーネントは、信頼されるレベルが異なっていても、同じプロセスで安全に実行できます。 コードがタイプ セーフでないと、望ましくない副作用が生じることがあります。 たとえば、ランタイムは、マネージド コードがネイティブ (アンマネージ) コードにアクセスしたり、不正な操作を実行することを防止できません。 コードがタイプ セーフであると、ランタイムのセキュリティ適用機構によって、必要なアクセス許可を持たないコードがネイティブ コードにアクセスすることはできません。 タイプ セーフでないすべてのコードは、列挙子メンバー <xref:System.Security.Permissions.SecurityPermission> が渡された <xref:System.Security.Permissions.SecurityPermissionAttribute.SkipVerification%2A> を与えられていないと実行できません。  
  
 詳しくは、「[コード アクセス セキュリティの基礎](../../../docs/framework/misc/code-access-security-basics.md)」をご覧ください。  
  
## <a name="principal"></a>プリンシパル  
 プリンシパルは、ユーザーの ID とロールを表し、ユーザーの代わりを務めます。 .NET Framework のロール ベース セキュリティでは、次の 3 種類のプリンシパルがサポートされています。  
  
- 汎用プリンシパルは、Windows のユーザーとロールとは無関係に存在するユーザーとロールを表します。  
  
- Windows プリンシパルは、Windows のユーザーとそのロール (または Windows グループ) を表します。 Windows プリンシパルは、他のユーザーを偽装できます。つまり、Windows プリンシパルは、あるユーザーに属する ID を提示し、そのユーザーの代わりにリソースにアクセスできます。  
  
- カスタム プリンシパルは、アプリケーションが必要に応じて自由に定義できます。 カスタム プリンシパルによって、プリンシパルの ID とロールの基本概念を拡張できます。  
  
 詳しくは、「[プリンシパル オブジェクトと ID オブジェクト](../../../docs/standard/security/principal-and-identity-objects.md)」をご覧ください。  
  
## <a name="authentication"></a>認証  
 認証とは、ユーザーの資格情報を調べ、資格情報をなんらかの権限に対して検証することによって、プリンシパルの身元 を発見および確認するプロセスです。 認証時に得られる情報は、コードで直接使用できます。 また、.NET Framework のロール ベース セキュリティを使用して、現在のユーザーを認証したり、そのプリンシパルにコードへのアクセスを許可するかどうかを判断したりできます。 特定のロールに関してプリンシパルを認証する方法の例については、<xref:System.Security.Principal.WindowsPrincipal.IsInRole%2A?displayProperty=nameWithType> メソッドのオーバーロードを参照してください。 たとえば、<xref:System.Security.Principal.WindowsPrincipal.IsInRole%28System.String%29?displayProperty=nameWithType> オーバーロードを使用すると、現在のユーザーが Administrators グループのメンバーであるかどうかを判断できます。  
  
 さまざまな認証メカニズムが現在使用されていて、その多くを .NET Framework のロール ベース セキュリティと一緒に使用できます。 最も一般に使用されている認証機構としては、基本認証、ダイジェスト認証、パスポート認証、オペレーティング システム認証 (NTLM 認証や Kerberos 認証など)、およびアプリケーション定義の認証機構などがあります。  
  
### <a name="example"></a>使用例  
 以下の例では、アクティブ プリンシパルが管理者である必要があります。 `name` パラメーターは `null` で、これにより、管理者ユーザーがこの確認要求を渡すことを許可されます。  
  
> [!NOTE]
> Windows Vista では、ユーザー アカウント制御 (UAC: User Account Control) でユーザーの権限が決定されます。 ユーザーが組み込みの Administrators グループのメンバーである場合、そのユーザーには標準ユーザー アクセス トークンおよび管理者アクセス トークンの 2 つのランタイム アクセス トークンが割り当てられています。 既定では、ユーザーは標準ユーザー ロールに所属します。 管理者であることを要求するコードを実行するには、最初に、ユーザーの権限を標準ユーザーから管理者に昇格させる必要があります。 この操作は、アプリケーションの起動時にアプリケーション アイコンを右クリックし、管理者として実行することを指定して行うことができます。  
  
 [!code-cpp[Classic PrincipalPermission Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CPP/source.cpp#1)]
 [!code-csharp[Classic PrincipalPermission Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CS/source.cs#1)]
 [!code-vb[Classic PrincipalPermission Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/VB/source.vb#1)]  
  
 次の例は、プリンシパルの ID と、プリンシパルで使用できるロールを判別する方法を示しています。 この例のアプリケーションは、現在のユーザーがアプリケーションの使用を許可するロールに含まれていることを確認できます。  
  
 [!code-cpp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CPP/source.cpp#1)]
 [!code-csharp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CS/source.cs#1)]
 [!code-vb[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/VB/source.vb#1)]  
  
## <a name="authorization"></a>認証  
 承認とは、要求されたアクションの実行をプリンシパルに許可するかどうかを判断するプロセスです。 承認は認証の後に行われ、プリンシパルの ID とロールについての情報を使用して、そのプリンシパルがアクセスできるリソースを判断します。 承認は、.NET Framework のロール ベース セキュリティを使用して実装できます。
