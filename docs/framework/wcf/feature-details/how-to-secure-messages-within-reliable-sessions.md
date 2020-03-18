---
title: '方法 : 信頼できるセッション内でメッセージをセキュリティで保護する'
ms.date: 03/30/2017
ms.assetid: aee33e50-936f-4486-9ca8-c1520c19a62d
ms.openlocfilehash: edff27fe9649387c1922c34e72ef59d615e52b90
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141668"
---
# <a name="how-to-secure-messages-within-reliable-sessions"></a>方法 : 信頼できるセッション内でメッセージをセキュリティで保護する

ここでは、信頼できるセッション内で交換されるメッセージに対して、メッセージ レベルのセキュリティを有効にするために必要な手順について説明します。このとき、信頼できるセッションがオプションでサポートされているシステム指定のバインディングを使用します。 コードを使用するか、構成ファイルで宣言によって、セキュリティで保護された信頼できるセッションを強制的に有効にします。 この手順では、クライアントとサービスの構成ファイルを使用して、セキュリティで保護された信頼できるセッションを有効にします。

この手順の主な部分は、次の 3 つのタスクで構成されます。

1. クライアントとサービスのメッセージ交換は信頼できるセッション内で行うことを指定します。

1. 信頼できるセッション内でメッセージ レベルのセキュリティを要求します。

1. サービスに対する認証時にクライアントが使用する必要があるクライアント資格情報を指定します。

最初のタスクでは、エンドポイント構成要素に、(この例では `MessageSecurity`) という名前のバインディング構成を参照する `bindingConfiguration` 属性が含まれていることが重要です。 [ **\<binding >** ](../../configure-apps/file-schema/wcf/bindings.md) configuration 要素は、この名前を参照して、 [ **\<reliableSession >** ](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))要素の `enabled` 属性を `true`に設定することによって、信頼できるセッションを有効にします。 信頼できるセッション内で使用できる順序付き配信の保証は、`ordered` 属性を `true` に設定することによって要求できます。

この構成手順の基になっている例のソースコピーについては、「 [WS Reliable Session](../../../../docs/framework/wcf/samples/ws-reliable-session.md)」を参照してください。

2番目のタスクの重要な項目は、クライアントとサービスの **\<binding >** 要素に含まれる **\<セキュリティ >** 要素の `mode` 属性を `Message`に設定することによって実現されます。

3番目のタスクの重要な項目は、クライアントとサービスの **\<セキュリティ >** 要素に含まれる **\<メッセージ >** 要素の `clientCredentialType` 属性を `Certificate`に設定することによって実現されます。

> [!NOTE]
> 信頼できるセッションでメッセージセキュリティを使用する場合、信頼できるメッセージは、最初のエラー時に例外をスローするのではなく、タイムアウトが発生するまで、認証されていないクライアントの認証を試みます。

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するようにサービスを WSHttpBinding で構成する

この手順については[、「方法: 信頼できるセッション内でメッセージを交換](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-within-a-reliable-session.md)する」を参照してください。

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>信頼できるセッションを使用するようにクライアントを WSHttpBinding で構成する

この手順については[、「方法: 信頼できるセッション内でメッセージを交換](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-within-a-reliable-session.md)する」を参照してください。

### <a name="set-the-mode-and-clientcredentialtype-in-configuration"></a>構成にモードと ClientCredentialType を設定する

1. 構成ファイルの[ **\<バインド >** ](../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)要素に適切なバインド要素を追加します。 次の例では、 [ **\<wsHttpBinding >** ](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)要素を追加します。

1. **\<バインド >** 要素を追加し、その `name` 属性を適切な値に設定します。 この例では、`MessageSecurity`という名前を使用します。

1. **\<セキュリティ >** 要素を追加し、`mode` 属性を `Message`に設定します。

1. **\<security >** 要素内に **\<message >** 要素を追加し、`clientCredentialType` 属性を `Certificate`に設定します。

```xml
<wsHttpBinding>
  <binding name="MessageSecurity">
    <security mode="Message">
      <message clientCredentialType="Certificate" />
    </security>
  </binding>
</wsHttpBinding>
```
