---
description: '詳細については、「チュートリアル: カスタムクライアントおよびサービスの資格情報を作成する」を参照してください。'
title: 'チュートリアル: カスタム クライアントおよびサービスの資格情報を作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2b5ba5c3-0c6c-48e9-9e46-54acaec443ba
ms.openlocfilehash: 75313defafaf0d0d558c100f1e86df9e2415d993
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643957"
---
# <a name="walkthrough-creating-custom-client-and-service-credentials"></a>チュートリアル: カスタム クライアントおよびサービスの資格情報を作成する

このトピックでは、クライアントおよびサービスにカスタム資格情報を実装する方法と、これをアプリケーション コードから使用する方法について説明します。

## <a name="credentials-extensibility-classes"></a>資格情報拡張クラス

<xref:System.ServiceModel.Description.ClientCredentials>クラスと <xref:System.ServiceModel.Description.ServiceCredentials> クラスは、WINDOWS COMMUNICATION FOUNDATION (WCF) セキュリティ拡張機能への主なエントリポイントです。 これらの資格情報クラスでは、アプリケーション コードから資格情報を設定し、資格情報の種類をセキュリティ トークンに変換する API を提供します  (*セキュリティトークン* は、SOAP メッセージ内で資格情報を送信するために使用されるフォームです)。これらの資格情報クラスの役割は、次の2つの領域に分けることができます。

- アプリケーションで資格情報を設定するための API を提供します。

- <xref:System.IdentityModel.Selectors.SecurityTokenManager> 実装のファクトリとして動作します。

WCF に用意されている既定の実装では、システム指定の資格情報の種類をサポートし、これらの資格情報の種類を処理できるセキュリティトークンマネージャーを作成します。

## <a name="reasons-to-customize"></a>カスタマイズする理由

クライアントまたはサービスの資格情報クラスをカスタマイズする場合、いくつかの理由があります。 特に次のような理由から、システム指定の資格情報の種類の処理に関して、既定の WCF セキュリティ動作を変更する必要があることが最も重要です。

- 他の拡張ポイントを使用した場合には不可能な変更

- 新しい資格情報の種類の追加

- 新しいカスタム セキュリティ トークンの種類の追加

このトピックでは、カスタムのクライアント資格情報およびサービス資格情報を実装する方法と、これをアプリケーション コードから使用する方法について説明します。

## <a name="first-in-a-series"></a>最初の手順

資格情報をカスタマイズする理由は、資格情報のプロビジョニング、セキュリティトークンのシリアル化、または認証に関する WCF の動作を変更するため、カスタム資格情報クラスの作成は最初の手順にすぎません。 このセクションの他のトピックでは、カスタムのシリアライザーと認証システムを作成する方法について説明します。 カスタム資格情報クラスの作成は、このような観点からすると、一連のトピックの最初の手順になります。 これに続く処理 (カスタムのシリアライザーおよび認証システムの作成) は、カスタム資格情報の作成後にのみ可能になります。 このトピックに基づく他のトピックには、次のものがあります。

- [方法: カスタム セキュリティ トークン プロバイダーを作成する](how-to-create-a-custom-security-token-provider.md)

- [方法: カスタム セキュリティ トークン認証システムを作成する](how-to-create-a-custom-security-token-authenticator.md)

- [方法: カスタムトークンを作成](how-to-create-a-custom-token.md)する。

## <a name="procedures"></a>手順

#### <a name="to-implement-custom-client-credentials"></a>カスタム クライアント資格情報を実装するには

1. <xref:System.ServiceModel.Description.ClientCredentials> クラスから派生する新しいクラスを定義します。

2. 任意。 新しい資格情報の種類に新しいメソッドまたはプロパティを追加します。 新しい資格情報の種類を追加しない場合は、この手順を省略します。 次の例では、`CreditCardNumber` プロパティを追加します。

3. <xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> メソッドをオーバーライドします。 このメソッドは、カスタムクライアント資格情報が使用されている場合に、WCF セキュリティインフラストラクチャによって自動的に呼び出されます。 このメソッドは、<xref:System.IdentityModel.Selectors.SecurityTokenManager> クラスの実装のインスタンスを作成して返す役割を担います。

    > [!IMPORTANT]
    > カスタム セキュリティ トークン マネージャーを作成するために、<xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> メソッドがオーバーライドされることに注意する必要があります。 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> から派生したセキュリティ トークン マネージャーは、実際のセキュリティ トークンを作成するために、<xref:System.IdentityModel.Selectors.SecurityTokenProvider> から派生したカスタム セキュリティ トークン プロバイダーを返す必要があります。 このパターンに従ってセキュリティ トークンを作成しないと、<xref:System.ServiceModel.ChannelFactory> オブジェクトがキャッシュされたとき (これは、WCF クライアント プロキシの既定の動作です)、権限の昇格攻撃を受ける可能性があり、アプリケーションが正常に機能しない場合があります。 カスタム資格情報オブジェクトは、<xref:System.ServiceModel.ChannelFactory> の一部としてキャッシュされます。 ただし、カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager> がすべての呼び出し時に作成され、これにより、トークン作成ロジックが <xref:System.IdentityModel.Selectors.SecurityTokenManager> にある限り、セキュリティの脅威が軽減されます。

4. <xref:System.ServiceModel.Description.ClientCredentials.CloneCore%2A> メソッドをオーバーライドします。

    [!code-csharp[c_CustomCredentials#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#1)]
    [!code-vb[c_CustomCredentials#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#1)]

#### <a name="to-implement-a-custom-client-security-token-manager"></a>カスタム クライアント セキュリティ トークン マネージャーを実装するには

1. <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> から派生する新しいクラスを定義します。

2. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenProvider%28System.IdentityModel.Selectors.SecurityTokenRequirement%29> 実装を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenProvider> メソッドをオーバーライドします。 カスタムセキュリティトークンプロバイダーの詳細については、「 [方法: カスタムセキュリティトークンプロバイダーを作成](how-to-create-a-custom-security-token-provider.md)する」を参照してください。

3. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%28System.IdentityModel.Selectors.SecurityTokenRequirement%2CSystem.IdentityModel.Selectors.SecurityTokenResolver%40%29> 実装を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> メソッドをオーバーライドします。 カスタムセキュリティトークン認証システムの詳細については、「 [方法: カスタムセキュリティトークン認証システムを作成](how-to-create-a-custom-security-token-authenticator.md)する」を参照してください。

4. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenSerializer%2A> を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenSerializer> メソッドをオーバーライドします。 カスタムセキュリティトークンおよびカスタムセキュリティトークンシリアライザーの詳細については、「 [方法: カスタムトークンを作成](how-to-create-a-custom-token.md)する」を参照してください。

    [!code-csharp[c_CustomCredentials#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#2)]
    [!code-vb[c_CustomCredentials#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#2)]

#### <a name="to-use-a-custom-client-credentials-from-application-code"></a>アプリケーション コードによるカスタム クライアント資格情報を使用するには

1. サービス インターフェイスを表す、生成されたクライアントのインスタンスを作成するか、または通信の対象となるサービスを指す <xref:System.ServiceModel.ChannelFactory> のインスタンスを作成します。

2. <xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> コレクションから、システム指定のクライアント資格情報の動作を削除します。このコレクションには、<xref:System.ServiceModel.ChannelFactory.Endpoint%2A> プロパティからアクセスできます。

3. カスタム資格情報クラスの新しいインスタンスを作成し、<xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> コレクションに追加します。このコレクションには、<xref:System.ServiceModel.ChannelFactory.Endpoint%2A> プロパティからアクセスできます。

    [!code-csharp[c_CustomCredentials#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#3)]
    [!code-vb[c_CustomCredentials#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#3)]

上記の手順は、アプリケーション コードからクライアント資格情報を使用する方法を示しています。 WCF 資格情報は、アプリケーション構成ファイルを使用して構成することもできます。 ソースの変更、再コンパイル、再展開を行うことなくアプリケーションのパラメーターを変更できるため、ハードコーディングを行うよりもアプリケーション構成ファイルの使用を一般にお勧めします。

次の手順では、カスタム資格情報の構成をサポートする方法について説明します。

#### <a name="creating-a-configuration-handler-for-custom-client-credentials"></a>カスタム クライアント資格情報の構成ハンドラーの作成

1. <xref:System.ServiceModel.Configuration.ClientCredentialsElement> から派生する新しいクラスを定義します。

2. 任意。 アプリケーション構成を通じて公開する必要があるすべての追加構成パラメーターのプロパティを追加します。 次の例では、`CreditCardNumber` という名前のプロパティを追加します。

3. <xref:System.ServiceModel.Configuration.BehaviorExtensionElement.BehaviorType%2A> プロパティをオーバーライドして、構成要素によって作成されたカスタム クライアント資格情報クラスの型を返します。

4. <xref:System.ServiceModel.Configuration.BehaviorExtensionElement.CreateBehavior%2A> メソッドをオーバーライドします。 このメソッドは、構成ファイルから読み込まれた設定に基づいてカスタム資格情報クラスのインスタンスを作成して返す役割を担います。 このメソッドから <xref:System.ServiceModel.Configuration.ClientCredentialsElement.ApplyConfiguration%28System.ServiceModel.Description.ClientCredentials%29> 基本メソッドを呼び出し、カスタム クライアント資格情報インスタンスに読み込まれたシステム指定の資格情報の設定を取得します。

5. 任意。 手順 2. で追加のプロパティを追加している場合は、<xref:System.Configuration.ConfigurationElement.Properties%2A> プロパティをオーバーライドして、追加した構成設定が構成フレームワークから認識されるよう登録します。 追加したプロパティを基本クラスのプロパティと結合して、このカスタム クライアント資格情報の構成要素を通じてシステム指定の設定が構成されるようにします。

    [!code-csharp[c_CustomCredentials#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#7)]
    [!code-vb[c_CustomCredentials#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#7)]

構成ハンドラークラスを作成したら、それを WCF 構成フレームワークに統合できます。 これにより、次の手順で示すように、カスタム クライアント資格情報をクライアント エンドポイント動作要素で使用できるようになります。

#### <a name="to-register-and-use-a-custom-client-credentials-configuration-handler-in-the-application-configuration"></a>カスタム クライアント資格情報構成ハンドラーをアプリケーション構成に登録して使用するには

1. `extensions`構成ファイルに <> 要素と <`behaviorExtensions`> 要素を追加します。

2. `add`<> 要素に <> 要素を追加 `behaviorExtensions` し、属性を `name` 適切な値に設定します。

3. `type` 属性を完全修飾型名に設定します。 また、アセンブリ名と他のアセンブリ属性を含めます。

    ```xml
    <system.serviceModel>
      <extensions>
        <behaviorExtensions>
          <add name="myClientCredentials" type="Microsoft.ServiceModel.Samples.MyClientCredentialsConfigHandler, CustomCredentials, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </behaviorExtensions>
      </extensions>
    </system.serviceModel>
    ```

4. 構成ハンドラーを登録した後は、システム指定の <> 要素ではなく、同じ構成ファイル内でカスタム資格情報要素を使用でき `clientCredentials` ます。 システム指定のプロパティと、構成ハンドラーの実装に追加した任意の新規プロパティのどちらも使用できます。 次の例では、`creditCardNumber` 属性を使用して、カスタム プロパティの値を設定します。

    ```xml
    <behaviors>
      <endpointBehaviors>
        <behavior name="myClientCredentialsBehavior">
          <myClientCredentials creditCardNumber="123-123-123"/>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    ```

#### <a name="to-implement-custom-service-credentials"></a>カスタム サービス資格情報を実装するには

1. <xref:System.ServiceModel.Description.ServiceCredentials> から派生する新しいクラスを定義します。

2. 任意。 追加している新しい資格情報の値に API を提供するために新しいプロパティを追加します。 新しい資格情報の値を追加しない場合は、この手順を省略します。 次の例では、`AdditionalCertificate` プロパティを追加します。

3. <xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> メソッドをオーバーライドします。 このメソッドは、カスタムクライアント資格情報が使用されるときに、WCF インフラストラクチャによって自動的に呼び出されます。 このメソッドは、<xref:System.IdentityModel.Selectors.SecurityTokenManager> クラスの実装のインスタンスを作成して返す役割を担います (次の手順で説明)。

4. 任意。 <xref:System.ServiceModel.Description.ServiceCredentials.CloneCore%2A> メソッドをオーバーライドします。 この手順は、カスタム クライアント資格情報の実装に新しいプロパティまたは内部フィールドを追加する場合にのみ必要になります。

    [!code-csharp[c_CustomCredentials#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#4)]
    [!code-vb[c_CustomCredentials#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#4)]

#### <a name="to-implement-a-custom-service-security-token-manager"></a>カスタム サービス セキュリティ トークン マネージャーを実装するには

1. <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> クラスから派生する新しいクラスを定義します。

2. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenProvider%2A> 実装を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenProvider> メソッドをオーバーライドします。 カスタムセキュリティトークンプロバイダーの詳細については、「 [方法: カスタムセキュリティトークンプロバイダーを作成](how-to-create-a-custom-security-token-provider.md)する」を参照してください。

3. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%2A> 実装を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> メソッドをオーバーライドします。 カスタムセキュリティトークン認証システムの詳細については、「 [方法: カスタムセキュリティトークン認証システムを作成](how-to-create-a-custom-security-token-authenticator.md) する」を参照してください。

4. 任意。 カスタム <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenSerializer%28System.IdentityModel.Selectors.SecurityTokenVersion%29> を作成する必要がある場合は、<xref:System.IdentityModel.Selectors.SecurityTokenSerializer> メソッドをオーバーライドします。 カスタムセキュリティトークンおよびカスタムセキュリティトークンシリアライザーの詳細については、「 [方法: カスタムトークンを作成](how-to-create-a-custom-token.md)する」を参照してください。

    [!code-csharp[c_CustomCredentials#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#5)]
    [!code-vb[c_CustomCredentials#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#5)]

#### <a name="to-use-custom-service-credentials-from-application-code"></a>アプリケーション コードによるカスタム サービス資格情報を使用するには

1. <xref:System.ServiceModel.ServiceHost> のインスタンスを作成します。

2. <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> コレクションから、システム指定のサービス資格情報の動作を削除します。

3. カスタム サービス資格情報クラスの新しいインスタンスを作成し、これを <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> コレクションに追加します。

    [!code-csharp[c_CustomCredentials#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#6)]
    [!code-vb[c_CustomCredentials#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#6)]

前の手順「」および「」で説明した手順に従って、構成のサポートを追加し `To create a configuration handler for custom client credentials` `To register and use a custom client credentials configuration handler in the application configuration` ます。唯一の違いは、 <xref:System.ServiceModel.Configuration.ServiceCredentialsElement> クラスの代わりにクラスを <xref:System.ServiceModel.Configuration.ClientCredentialsElement> 構成ハンドラーの基本クラスとして使用することです。 カスタム サービス資格情報要素は、システム指定の `<serviceCredentials>` 要素を使用する場合にいつでも使用できます。

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.Security.SecurityCredentialsManager>
- <xref:System.IdentityModel.Selectors.SecurityTokenManager>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement>
- [方法: カスタム セキュリティ トークン プロバイダーを作成する](how-to-create-a-custom-security-token-provider.md)
- [方法: カスタム セキュリティ トークン認証システムを作成する](how-to-create-a-custom-security-token-authenticator.md)
- [方法: カスタム トークンを作成する](how-to-create-a-custom-token.md)
