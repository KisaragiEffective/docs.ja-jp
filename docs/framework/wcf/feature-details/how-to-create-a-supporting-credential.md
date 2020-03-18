---
title: '方法 : サポート資格情報を作成する'
ms.date: 03/30/2017
ms.assetid: d0952919-8bb4-4978-926c-9cc108f89806
ms.openlocfilehash: 3f33bf5a78c575237ee4bc609a482a81fd30fc53
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964557"
---
# <a name="how-to-create-a-supporting-credential"></a>方法 : サポート資格情報を作成する
カスタムのセキュリティ スキームでは、複数の資格情報が必要になることがあります。 たとえば、サービスが、ユーザー名とパスワードだけでなく、クライアントが 18 歳以上であることを証明する資格情報もクライアントに要求することがあります。 2番目の資格情報は、*サポート資格情報*です。 このトピックでは、Windows Communication Foundation (WCF) クライアントでこのような資格情報を実装する方法について説明します。  
  
> [!NOTE]
> サポート資格情報の仕様は、WS-SecurityPolicy 仕様の一部です。 詳細については、「 [Web Services Security の仕様](https://docs.microsoft.com/previous-versions/dotnet/articles/ms951273(v=msdn.10))」を参照してください。  
  
## <a name="supporting-tokens"></a>トークンのサポート  
 簡単に言えば、メッセージセキュリティを使用する場合は、メッセージをセキュリティで保護するために、*プライマリ資格情報*(たとえば、x.509 証明書または Kerberos チケット) が常に使用されます。  
  
 仕様で定義されているように、セキュリティバインディングでは、*トークン*を使用してメッセージ交換をセキュリティで保護します。 *トークン*は、セキュリティ資格情報の表現です。  
  
 セキュリティ バインディングは、セキュリティ バインディング ポリシーで特定されたプライマリ トークンを使用して、署名を作成します。 この署名は、*メッセージ署名*と呼ばれます。  
  
 メッセージ署名に関連付けられたトークンによって提供されるクレームを増やすために、追加のトークンを指定できます。  
  
## <a name="endorsing-signing-and-encrypting"></a>保証、署名、および暗号化  
 サポート資格情報は、メッセージ内で送信される*サポートトークン*になります。 WS-SecurityPolicy 仕様では、次の表に示すように、サポート トークンをメッセージに追加する方法が 4 つ定義されています。  
  
|目的|説明|  
|-------------|-----------------|  
|署名|サポート トークンはセキュリティ ヘッダーに追加され、メッセージ署名によって署名されます。|  
|保証|保証*トークン*は、メッセージ署名に署名します。|  
|署名および保証|署名付き保証トークンは、メッセージ署名から生成された `ds:Signature` 要素全体を署名し、それ自体がこのメッセージ署名によって署名されます。つまり、両方のトークン (メッセージ署名に使用されるトークンと署名付き保証トークン) がお互いに署名します。|  
|署名および暗号化|暗号化された署名付きサポート トークンは、`wsse:SecurityHeader` に表示されたときに暗号化されている署名付きサポート トークンです。|  
  
## <a name="programming-supporting-credentials"></a>サポート資格情報のプログラミング  
 サポートトークンを使用するサービスを作成するには、 [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)を作成する必要があります。 (詳細については、「[方法: カスタムバインディングを使用してカスタムバインディングを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)する」を参照してください)。  
  
 カスタム バインドを作成する最初の手順は、次の 3 種類のいずれかのセキュリティ バインド要素を作成することです。  
  
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>  
  
 すべてのクラスは、次の 4 つの関連プロパティを備えた <xref:System.ServiceModel.Channels.SecurityBindingElement> から継承します。  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.EndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OperationSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalEndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalOperationSupportingTokenParameters%2A>  
  
#### <a name="scopes"></a>スコープ  
 サポート資格情報には、次の 2 つのスコープがあります。  
  
- エンド*ポイントサポートトークン*は、エンドポイントのすべての操作をサポートします。 つまり、サポート トークンによって表される資格情報は、エンドポイントの任意の操作が呼び出されたときにいつでも使用できます。  
  
- *操作*をサポートするトークンは、特定のエンドポイント操作だけをサポートします。  
  
 プロパティ名に示されているように、サポート資格情報は必須またはオプションのどちらかになることができます。 つまり、サポート資格情報は必須ではありませんが、存在する場合は使用され、存在しない場合も認証は失敗しません。  
  
## <a name="procedures"></a>手順  
  
#### <a name="to-create-a-custom-binding-that-includes-supporting-credentials"></a>サポート資格情報を備えたカスタム バインドを作成するには  
  
1. セキュリティ バインド要素を作成します。 次の例では、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 認証モードで `UserNameForCertificate` を作成します。 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A> メソッドを使用します。  
  
2. サポート パラメーターを対応するプロパティ (`Endorsing`、`Signed`、`SignedEncrypted`、または `SignedEndorsed`) によって返される型のコレクションに追加します。 <xref:System.ServiceModel.Security.Tokens> 名前空間に存在する型には、<xref:System.ServiceModel.Security.Tokens.X509SecurityTokenParameters> など、一般的に使用される型があります。  
  
## <a name="example"></a>使用例  
  
### <a name="description"></a>説明  
 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> のインスタンスを作成し、<xref:System.ServiceModel.Security.Tokens.KerberosSecurityTokenParameters> クラスのインスタンスを、Endorsing プロパティによって返されるコレクションに追加する例を次に示します。  
  
### <a name="code"></a>コード  
 [!code-csharp[c_SupportingCredential#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_supportingcredential/cs/source.cs#1)]  
  
## <a name="see-also"></a>関連項目

- [方法 : SecurityBindingElement を使用してカスタム バインディングを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
