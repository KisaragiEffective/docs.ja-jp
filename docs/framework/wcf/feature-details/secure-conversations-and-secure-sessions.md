---
title: セキュリティ保護されたメッセージ交換とセッション
ms.date: 03/30/2017
ms.assetid: 48cb104a-532d-40ae-aa57-769dae103fda
ms.openlocfilehash: 887b2e6e6553a910de046950514869907519cf35
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746450"
---
# <a name="secure-conversations-and-secure-sessions"></a>セキュリティ保護されたメッセージ交換とセッション
Windows Communication Foundation (WCF) の機能は、相互に認証を行い、暗号化とデジタル署名のプロセスに同意する2つのエンドポイント間のセキュリティで保護されたセッションを確立する機能です。 たとえば、サービス エンドポイントは、クライアント エンドポイントに対して認証のために X.509 証明書に基づいたセキュリティ トークンを送信するよう要求する場合があります。 クライアントの認証が終わると、サービス エンドポイントはセキュリティ コンテキスト トークン (SCT: Security Context Token) をクライアントに返します。このセッションにおける後続のすべてのメッセージは、このセキュリティ トークンを使用してセキュリティ保護されます。 セキュリティで保護されたセッションが確立されると、SCT には対称キーが含まれるため、2 つのエンドポイント間で交換される一連のメッセージの効率が向上します。 X.509 証明書の基盤となる非対称キーでは、デジタル署名の生成やデータの暗号化を行う場合に、対称キーに比べて非常に大きな計算能力が必要になります。  
  
 ( [Ws-securitypolicy](https://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/ws-securitypolicy-1.2-spec-os.html)標準のセクション6.2.7 で定義されている) ブートストラップポリシーには、チャネルをセキュリティで保護し、RST/SCT と RSTR/sct 交換の前にクライアントを認証するために使用されるメッセージセキュリティアサーションが含まれています。 特定の WCF 標準バインディングには、セキュリティで保護されたメッセージ交換を使用するかどうかを制御する `Security.Message.EstablishSecurityContext` プロパティがあります。 カスタムバインディングを使用する場合、ブートストラップは、構成ファイルの[\<secureConversationBootstrap >](../../../../docs/framework/configure-apps/file-schema/wcf/secureconversationbootstrap.md)を使用するか、コードで <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%2A> を呼び出すことによって、セキュリティバインド要素を入れ子にすることによって示されます。  
  
 セッションの詳細については、「[セッションの使用](../../../../docs/framework/wcf/using-sessions.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [セッション、インスタンス化、およびコンカレンシー](../../../../docs/framework/wcf/feature-details/sessions-instancing-and-concurrency.md)
- [方法 : セッションを必要とするサービスを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-that-requires-sessions.md)
