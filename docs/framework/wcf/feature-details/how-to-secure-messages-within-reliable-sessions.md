---
description: '詳細情報: 方法: 信頼できるセッション内でメッセージをセキュリティで保護する'
title: '方法: 信頼できるセッション内でメッセージをセキュリティで保護する'
ms.date: 03/30/2017
ms.assetid: aee33e50-936f-4486-9ca8-c1520c19a62d
ms.openlocfilehash: 73ca0d543edc2445dc72264e7eccf6c1eb616313
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643359"
---
# <a name="how-to-secure-messages-within-reliable-sessions"></a>方法: 信頼できるセッション内でメッセージをセキュリティで保護する

ここでは、信頼できるセッション内で交換されるメッセージに対して、メッセージ レベルのセキュリティを有効にするために必要な手順について説明します。このとき、信頼できるセッションがオプションでサポートされているシステム指定のバインディングを使用します。 セキュリティで保護された信頼できるセッションを有効にするには、コードを使用して強制的に行うか、構成ファイルで宣言します。 この手順では、クライアントとサービスの構成ファイルを使用して、セキュリティで保護された信頼できるセッションを有効にします。

この手順の主な部分は、次の 3 つのタスクで構成されます。

1. クライアントとサービスのメッセージ交換は信頼できるセッション内で行うことを指定します。

1. 信頼できるセッション内でメッセージ レベルのセキュリティを要求します。

1. サービスに対する認証時にクライアントが使用する必要があるクライアント資格情報を指定します。

最初のタスクで重要なのは、エンドポイント構成要素に (この例の場合) `MessageSecurity` という名前のバインディング構成を参照する `bindingConfiguration` 属性が含まれていることです。 [ **\<binding>**](../../configure-apps/file-schema/wcf/bindings.md) 構成要素は、この名前を参照して、[ **\<reliableSession>** ](/previous-versions/ms731375(v=vs.90)) 要素の `enabled` 属性を `true` に設定することで、信頼できるセッションを有効にします。 信頼できるセッション内で使用できる順序付き配信の保証は、`ordered` 属性を `true` に設定することによって要求できます。

この構成手順のベースであるソース例については、「[WS 信頼できるセッション](../samples/ws-reliable-session.md)」を参照してください。

2 番目の作業で必要な項目は、クライアントとサービスの **\<binding>** 要素に含まれている **\<security>** 要素の `mode` 属性を `Message` に設定することによって達成できます。

3 番目の作業で必要な項目は、クライアントとサービスの **\<security>** 要素に含まれている **\<message>** 要素の `clientCredentialType` 属性を `Certificate` に設定することによって達成できます。

> [!NOTE]
> 信頼できるセッションでメッセージ セキュリティを使用するときに、信頼できるメッセージ機能は最初の失敗で例外をスローするのではなく、タイムアウトが発生するまで未認証のクライアントの認証を試みます。

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するためにサービスを WSHttpBinding で構成する

この手順については、「[方法: 信頼されたセッション内のメッセージを変換する](how-to-exchange-messages-within-a-reliable-session.md)」を参照してください。

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するためにクライアントを WSHttpBinding で構成する

この手順については、「[方法: 信頼されたセッション内のメッセージを変換する](how-to-exchange-messages-within-a-reliable-session.md)」を参照してください。

### <a name="set-the-mode-and-clientcredentialtype-in-configuration"></a>構成でモードと ClientCredentialType を設定する

1. 構成ファイルの [ **\<bindings>** ](../../configure-apps/file-schema/wcf/bindings.md) 要素に、適切なバインド要素を追加します。 次の例では [ **\<wsHttpBinding>** ](../../configure-apps/file-schema/wcf/wshttpbinding.md) 要素を追加しています。

1. **\<binding>** 要素を追加し、`name` 属性に適切な値を設定します。 この例では、`MessageSecurity` という名前を使用しています。

1. **\<security>** 要素を追加し、`mode` 属性を `Message` に設定します。

1. **\<security>** 要素内に、 **\<message>** 要素を追加し、`clientCredentialType` 属性を `Certificate` に設定します。

```xml
<wsHttpBinding>
  <binding name="MessageSecurity">
    <security mode="Message">
      <message clientCredentialType="Certificate" />
    </security>
  </binding>
</wsHttpBinding>
```
