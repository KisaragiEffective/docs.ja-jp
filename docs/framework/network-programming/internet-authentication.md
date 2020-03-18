---
title: インターネット認証
ms.date: 03/30/2017
helpviewer_keywords:
- authentication [.NET Framework], classes
- IAuthenticationModule interface
- ICredentialLookup interface
- CredentialCache class, about CredentialCache class
- receiving data, authentication
- AuthenticationManager class, about AuthenticationManager class
- Internet, authentication
- sending data, authentication
- network resources, authentication
- user authentication, classes for authentication
- NetworkCredential class, about NetworkCredential class
- client authentication, classes for authentication
ms.assetid: d342e87c-f672-4660-a513-41a2f2b80c4a
ms.openlocfilehash: 3e0b5cd58270cec758db5d4dad6f3ad48962921a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047914"
---
# <a name="internet-authentication"></a>インターネット認証
<xref:System.Net> クラスは、さまざまなクライアント認証メカニズムをサポートしています。これには、基本、ダイジェスト、ネゴシエート、NTLM、および Kerberos の標準のインターネット認証方法の他に、ユーザーが作成できるカスタム メソッドも含まれます。  
  
 認証の資格情報は、<xref:System.Net.NetworkCredential> インターフェイスを実装する <xref:System.Net.CredentialCache> クラスと <xref:System.Net.ICredentials> クラスに格納されています。 資格情報についてこれらのいずれかのクラスが照会されると、そのクラスが **NetworkCredential** クラスのインスタンスを返します。 認証プロセスは <xref:System.Net.AuthenticationManager> クラスで管理され、実際の認証プロセスは <xref:System.Net.IAuthenticationModule> インターフェイスを実装する認証モジュール クラスによって実行されます。 カスタム認証モジュールは、**AuthenticationManager** に登録してから使用する必要があります。基本、ダイジェスト、ネゴシエート、NTLM、および Kerberos の各認証方法は、既定で登録されています。  
  
 **NetworkCredential** は、URI で識別される 1 つのインターネット リソースに関連付けられている一連の資格情報を格納し、<xref:System.Net.NetworkCredential.GetCredential%2A> メソッドへの任意の呼び出しに応答してそれらを返します。 **NetworkCredential** クラスは通常、限定された数のインターネット リソースにアクセスするアプリケーション、またはどんな場合でも同じ資格情報のセットを使用するアプリケーションで使用されます。  
  
 **CredentialCache** クラスは、さまざまな Web リソースの資格情報のコレクションを格納します。 <xref:System.Net.CredentialCache.GetCredential%2A> メソッドが呼び出されると、**CredentialCache** は、適切な資格情報のセットを返します。これは Web リソースの URI と要求された認証スキームによって決まります。 異なる認証スキームでさまざまなインターネット リソースを使用するアプリケーションは、**CredentialCache** クラスを使用することでメリットが得られます。それは、このクラスがすべての資格情報を格納し、要求に応じてそれらを提供するからです。  
  
 インターネット リソースが認証を要求すると、<xref:System.Net.WebRequest.GetResponse%2A?displayProperty=nameWithType> メソッドは資格情報の要求と共に <xref:System.Net.WebRequest> を **AuthenticationManager** に送信します。 そして要求は、次のプロセスに従って認証されます。  
  
1. **AuthenticationManager** が登録済みの各認証モジュールで、登録された順番で <xref:System.Net.IAuthenticationModule.Authenticate%2A> メソッドを呼び出します。 **AuthenticationManager** は **null** を返さない 1 つ目のモジュールを使用して認証プロセスを実行します。 プロセスの詳細は、使用する認証モジュールの種類によって異なります。  
  
2. 認証プロセスが完了すると、認証モジュールが <xref:System.Net.Authorization> をインターネット リソースにアクセスするために必要な情報を含む **WebRequest** に返します。  
  
 一部の認証スキームでは、最初にリソースの要求を作成しなくても、ユーザーを認証することができます。 リソースでユーザーを事前認証することで、サーバーへのラウンド トリップを少なくとも 1 回減らせるため、アプリケーションが時間を節約できます。 または、後でユーザーへの応答性を高めるため、プログラムの起動中に認証を実行できます。 事前認証を使用できる認証スキームで <xref:System.Net.IAuthenticationModule.PreAuthenticate%2A> プロパティを **true** に設定します。  
  
## <a name="see-also"></a>参照

- [基本認証とダイジェスト認証](basic-and-digest-authentication.md)
- [NTLM 認証および Kerberos 認証](ntlm-and-kerberos-authentication.md)
- [ネットワーク プログラミングにおけるセキュリティ](security-in-network-programming.md)
