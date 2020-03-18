---
title: '方法 : 企業内のエンドポイントをロックダウンする'
ms.date: 03/30/2017
ms.assetid: 1b7eaab7-da60-4cf7-9d6a-ec02709cf75d
ms.openlocfilehash: 35b9a1dbebce48823799689a581033d0483c3984
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937678"
---
# <a name="how-to-lock-down-endpoints-in-the-enterprise"></a>方法 : 企業内のエンドポイントをロックダウンする

大規模な企業では、多くの場合、企業のセキュリティ ポリシーに準拠してアプリケーションを開発する必要があります。 次のトピックでは、コンピューターにインストールされているすべての Windows Communication Foundation (WCF) クライアントアプリケーションの検証に使用できるクライアントエンドポイント検証コントロールを開発してインストールする方法について説明します。

この場合、このエンドポイント動作は machine.config ファイルのクライアント[\<commonBehaviors >](../../configure-apps/file-schema/wcf/commonbehaviors.md)セクションに追加されるため、検証コントロールはクライアント検証コントロールです。 WCF は、クライアントアプリケーションに対してのみ共通のエンドポイント動作を読み込み、サービスアプリケーションに対してのみ共通のサービス動作を読み込みます。 サービス アプリケーション用のこの同じ検証コントロールをインストールするには、検証コントロールがサービス動作であることが必要です。 詳細については、「 [\<commonBehaviors >](../../configure-apps/file-schema/wcf/commonbehaviors.md) 」セクションを参照してください。

> [!IMPORTANT]
> 構成ファイルの[\<commonBehaviors >](../../configure-apps/file-schema/wcf/commonbehaviors.md)セクションに追加された <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 属性 (APTCA) でマークされていないサービスまたはエンドポイントの動作は、アプリケーションが部分信頼環境で実行されている場合は実行されず、このような場合に例外はスローされません。 検証コントロールなどの共通動作を強制的に実行するには、次のいずれかを行う必要があります。
>
> - 部分信頼アプリケーションとして展開されたときに実行できるように、共通の動作を <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 属性でマークします。 APTCA でマークされたアセンブリを実行できないように、コンピューターでレジストリ エントリを設定できます。
>
> - アプリケーションが完全に信頼されたアプリケーションとして配置されている場合は、ユーザーが部分信頼環境でアプリケーションを実行するようにコードアクセスセキュリティ設定を変更できないことを確認します。 ユーザーがこのような変更を行うことができる場合、カスタム検証コントロールは実行されず、例外もスローされません。 これを確認する方法の1つとして、[コードアクセスセキュリティポリシーツール (caspol.exe)](../../tools/caspol-exe-code-access-security-policy-tool.md)を使用した `levelfinal` オプションを参照してください。
>
> 詳細については、「[部分信頼のベストプラクティス](../feature-details/partial-trust-best-practices.md)」および「[サポートされている展開シナリオ](../feature-details/supported-deployment-scenarios.md)」を参照してください。

### <a name="to-create-the-endpoint-validator"></a>エンドポイント検証コントロールを作成するには

1. <xref:System.ServiceModel.Description.IEndpointBehavior> メソッドに、必要な検証手順を備えた <xref:System.ServiceModel.Description.IEndpointBehavior.Validate%2A> を作成します。 次にコード例を示します。 (`InternetClientValidatorBehavior` は、[セキュリティ検証](../samples/security-validation.md)のサンプルから抜粋したものです)。

    [!code-csharp[LockdownValidation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/internetclientvalidatorbehavior.cs#2)]

2. 手順 1. で作成したエンドポイント検証コントロールを登録する新しい <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> を作成します。 このコード例を次に示します。 (この例の元のコードは、[セキュリティ検証](../samples/security-validation.md)のサンプルに含まれています)。

    [!code-csharp[LockdownValidation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/internetclientvalidatorelement.cs#3)]

3. コンパイル済みのアセンブリが厳密な名前で署名されていることを確認します。 詳細については、「[厳密名ツール (SN)」を参照してください。EXE)](../../tools/sn-exe-strong-name-tool.md)を使用します。

### <a name="to-install-the-validator-into-the-target-computer"></a>検証コントロールをターゲット コンピューターにインストールするには

1. 適切な機構を使用してエンドポイント検証をインストールします。 企業では、グループ ポリシーと Systems Management Server (SMS) を使用してインストールします。

2. [Gacutil.exe (グローバルアセンブリキャッシュツール)](../../tools/gacutil-exe-gac-tool.md)を使用して、厳密な名前のアセンブリをグローバルアセンブリキャッシュにインストールします。

3. <xref:System.Configuration?displayProperty=nameWithType> 名前空間の型を使用して、次の処理を行います。

    1. 完全修飾型名を使用して[\<の >](../../configure-apps/file-schema/wcf/behaviorextensions.md)セクションに拡張機能を追加し、要素をロックします。

         [!code-csharp[LockdownValidation#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#5)]

    2. [\<commonBehaviors >](../../configure-apps/file-schema/wcf/commonbehaviors.md)セクションの `EndpointBehaviors` プロパティに behavior 要素を追加し、要素をロックします。 (検証コントロールをサービスにインストールするには、バリデーターが <xref:System.ServiceModel.Description.IServiceBehavior> であり、`ServiceBehaviors` プロパティに追加されている必要があります)。次のコード例は、手順の後の適切な構成を示しています。 と b. の後の適切な構成を示しています。厳密な名前が存在しない点だけが異なります。

        [!code-csharp[LockdownValidation#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#6)]

    3. machine.config ファイルを保存します。 次のコード例では、手順 3. にあるすべてのタスクを実行しますが、変更された machine.config ファイルのコピーはローカルに保存されます。

        [!code-csharp[LockdownValidation#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#7)]

## <a name="example"></a>使用例

次のコード例では、machine.config ファイルに共通の動作を追加し、そのコピーをディスクに保存する方法を示します。 `InternetClientValidatorBehavior` は、[セキュリティ検証](../samples/security-validation.md)のサンプルから取得したものです。

[!code-csharp[LockdownValidation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#1)]

## <a name="net-framework-security"></a>.NET Framework セキュリティ

また、構成ファイルの要素を暗号化する必要がある場合もあります。 詳細については、「参照」を参照してください。

## <a name="see-also"></a>関連項目

- [DPAPI を使用した構成ファイルの要素の暗号化](https://docs.microsoft.com/previous-versions/msp-n-p/ff647398(v=pandp.10))
- [RSA を使用して構成ファイルの要素を暗号化する](https://docs.microsoft.com/previous-versions/msp-n-p/ff650304(v=pandp.10))
