---
title: '方法 : カスタム ユーザー名およびパスワード検証を使用する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, username and password
ms.assetid: 8e08b74b-fa44-4018-b63d-0d0805f85e3f
ms.openlocfilehash: 3d01a29671f42e80fdb7ca45223007aa60273ba9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283255"
---
# <a name="how-to-use-a-custom-user-name-and-password-validator"></a>方法 : カスタム ユーザー名およびパスワード検証を使用する

既定では、ユーザー名とパスワードが認証に使用されると、Windows Communication Foundation (WCF) は Windows を使用してユーザー名とパスワードを検証します。 ただし、WCF では、カスタムのユーザー名とパスワードの認証方式 (*検証コントロール*とも呼ばれます) を使用できます。 ユーザー名およびパスワードのカスタム検証を組み込むには、<xref:System.IdentityModel.Selectors.UserNamePasswordValidator> から派生するクラスを作成して構成します。

サンプルアプリケーションについては、「[ユーザー名パスワード検証コントロール](../samples/user-name-password-validator.md)」を参照してください。

### <a name="to-create-a-custom-user-name-and-password-validator"></a>カスタムのユーザー名/パスワード検証コントロールを作成するには

1. <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> から派生するクラスを作成します。

    [!code-csharp[C_CustomUsernameAndPasswordValidator#3](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#3)]
    [!code-vb[C_CustomUsernameAndPasswordValidator#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#3)]

2. <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> メソッドをオーバーライドして、カスタム認証方式を実装します。

    次の例のコードは、<xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> メソッドをオーバーライドします。稼働環境では、このようなコードを使用しないでください。 このコードをカスタムのユーザー名/パスワード検証方式に置き換えます。この場合、ユーザー名とパスワードの組み合わせをデータベースから取得する必要があります。

    クライアントに認証エラーを返すには、<xref:System.ServiceModel.FaultException> メソッドで <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> をスローします。

    [!code-csharp[C_CustomUsernameAndPasswordValidator#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#4)]
    [!code-vb[C_CustomUsernameAndPasswordValidator#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#4)]

### <a name="to-configure-a-service-to-use-a-custom-user-name-and-password-validator"></a>カスタムのユーザー名/パスワード検証コントロールを使用するようにサービスを構成するには

1. 任意のトランスポート上のメッセージ セキュリティ、または HTTP(S) 上のトランスポート レベル セキュリティを使用するバインディングを構成します。

    メッセージセキュリティを使用する場合は、システム指定のバインディング ( [\<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)など)、またはメッセージセキュリティと `UserName` 資格情報の種類をサポートする[\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)を追加します。

    HTTP (S) に対してトランスポートレベルのセキュリティを使用する場合は、 [\<wsHttpBinding >](../../configure-apps/file-schema/wcf/wshttpbinding.md)または[\<basicHttpBinding >](../../configure-apps/file-schema/wcf/basichttpbinding.md)、 [\<NETTCPBINDING >](../../configure-apps/file-schema/wcf/nettcpbinding.md) 、または http (s) と\<認証スキームを使用する[> customBinding `Basic`](../../configure-apps/file-schema/wcf/custombinding.md)を追加します。

    > [!NOTE]
    > .NET Framework 3.5 以降のバージョンを使用する場合は、メッセージおよびトランスポートセキュリティと共にカスタムのユーザー名とパスワード検証を使用できます。 WinFX では、カスタムのユーザー名とパスワード検証コントロールは、メッセージセキュリティでのみ使用できます。

    > [!TIP]
    > このコンテキストで \<netTcpBinding > を使用する方法の詳細については、「 [\<security >](../../configure-apps/file-schema/wcf/security-of-nettcpbinding.md)」を参照してください。

    1. 構成ファイルで、 [\<system.servicemodel >](../../configure-apps/file-schema/wcf/system-servicemodel.md)要素の下に[\<バインド >](../../configure-apps/file-schema/wcf/bindings.md)要素を追加します。

    2. [\<wsHttpBinding >](../../configure-apps/file-schema/wcf/wshttpbinding.md)または[\<basicHttpBinding >](../../configure-apps/file-schema/wcf/basichttpbinding.md)要素をバインドセクションに追加します。 WCF バインド要素の作成の詳細については、「[方法: 構成でサービスバインディングを指定](../how-to-specify-a-service-binding-in-configuration.md)する」を参照してください。

    3. [\<セキュリティ >](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md)または[\<セキュリティ >](../../configure-apps/file-schema/wcf/security-of-basichttpbinding.md)の `mode` 属性を `Message`、`Transport`、または `TransportWithMessageCredential`に設定します。

    4. [\<メッセージ >](../../../../docs/framework/configure-apps/file-schema/wcf/message-of-wshttpbinding.md)または[\<トランスポート >](../../../../docs/framework/configure-apps/file-schema/wcf/transport-of-wshttpbinding.md)の `clientCredentialType` 属性を設定します。

        メッセージセキュリティを使用する場合は、 [\<メッセージ >](../../../../docs/framework/configure-apps/file-schema/wcf/message-of-wshttpbinding.md)の `clientCredentialType` 属性を `UserName`に設定します。

        HTTP (S) 経由でトランスポートレベルのセキュリティを使用する場合は、 [\<transport >](../../configure-apps/file-schema/wcf/transport-of-wshttpbinding.md)または[\<transport >](../../configure-apps/file-schema/wcf/transport-of-basichttpbinding.md)の `clientCredentialType` 属性を `Basic`に設定します。

        > [!NOTE]
        > WCF サービスがトランスポートレベルのセキュリティを使用してインターネットインフォメーションサービス (IIS) でホストされ、<xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.UserNamePasswordValidationMode%2A> プロパティが <xref:System.ServiceModel.Security.UserNamePasswordValidationMode.Custom>に設定されている場合、カスタム認証スキームは Windows 認証のサブセットを使用します。 このシナリオでは、WCF がカスタム認証システムを呼び出す前に、IIS が Windows 認証を実行します。

    WCF バインド要素の作成の詳細については、「[方法: 構成でサービスバインディングを指定](../how-to-specify-a-service-binding-in-configuration.md)する」を参照してください。

    次の例は、バインディングの構成コードを示しています。

    ```xml
    <system.serviceModel>
      <bindings>
      <wsHttpBinding>
          <binding name="Binding1">
            <security mode="Message">
              <message clientCredentialType="UserName" />
            </security>
          </binding>
        </wsHttpBinding>
      </bindings>
    </system.serviceModel>
    ```

2. 受信 <xref:System.IdentityModel.Tokens.UserNameSecurityToken> セキュリティ トークンのユーザー名とパスワードの組み合わせを検証する際に、カスタムのユーザー名/パスワード検証コントロールを使用することを指定する動作を構成します。

    1. [\<system.servicemodel >](../../configure-apps/file-schema/wcf/system-servicemodel.md)要素の子として、 [\<の動作 >](../../configure-apps/file-schema/wcf/behaviors.md)要素を追加します。

    2. [\<behavior >](../../configure-apps/file-schema/wcf/behaviors.md)要素に[\<servicebehaviors >](../../configure-apps/file-schema/wcf/servicebehaviors.md)を追加します。

    3. 要素[> の\<動作](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md)を追加し、`name` 属性を適切な値に設定します。

    4. [\<behavior >](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md)要素に[\<serviceCredentials >](../../configure-apps/file-schema/wcf/servicecredentials.md)を追加します。

    5. [\<serviceCredentials >](../../configure-apps/file-schema/wcf/servicecredentials.md)に[\<userNameAuthentication >](../../configure-apps/file-schema/wcf/usernameauthentication.md)を追加します。

    6. `userNamePasswordValidationMode` を `Custom` に設定します。

        > [!IMPORTANT]
        > `userNamePasswordValidationMode` 値が設定されていない場合、WCF はカスタムユーザー名とパスワード検証コントロールではなく Windows 認証を使用します。

    7. `customUserNamePasswordValidatorType` を、カスタムのユーザー名/パスワード検証コントロールを表す型に設定します。

    次の例は、このポイントまでの `<serviceCredentials>` フラグメントを示しています。

    ```xml
    <serviceCredentials>
      <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService.CustomUserNameValidator, service" />
    </serviceCredentials>
    ```

## <a name="example"></a>例

カスタムのユーザー名/パスワード検証コントロールを作成する方法を次のコード例に示します。 稼働環境では、<xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> メソッドをオーバーライドするコードを使用しないでください。 このコードをカスタムのユーザー名/パスワード検証方式に置き換えます。この場合、ユーザー名とパスワードの組み合わせをデータベースから取得する必要があります。

[!code-csharp[C_CustomUsernameAndPasswordValidator#1](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#1)]
[!code-vb[C_CustomUsernameAndPasswordValidator#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#1)]
[!code-csharp[C_CustomUsernameAndPasswordValidator#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#2)]
[!code-vb[C_CustomUsernameAndPasswordValidator#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#2)]

## <a name="see-also"></a>関連項目

- <xref:System.IdentityModel.Selectors.UserNamePasswordValidator>
- [方法 : ASP.NET メンバーシップ プロバイダーを使用する](how-to-use-the-aspnet-membership-provider.md)
- [認証](authentication-in-wcf.md)
