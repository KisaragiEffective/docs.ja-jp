---
title: 実稼働時に機密情報を保護するために Azure Key Vault を使用する
description: .NET マイクロサービスと Web アプリケーションのセキュリティ - Azure Key Vault は、管理者が完全に制御するアプリケーションのシークレットを処理できる優れた方法です。 管理者は開発値の割り当てや取り消しを行うこともできます。開発者に処理してもらう必要はありません。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: cc95d491136c945255408cec2bd49d4d6579e29a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77501754"
---
# <a name="use-azure-key-vault-to-protect-secrets-at-production-time"></a>実稼働時にシークレットを保護するために Azure Key Vault を使用する

環境変数として保存されているシークレットまたは Secret Manager ツールによって保存されたシークレットは、マシンのローカルに暗号化されていない状態で保存されています。 シークレットを保存する上でより安全な選択肢として [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) があります。Azure Key Vault には、キーとシークレットを保存するためのセキュリティで保護された中央の場所が用意されています。

**Microsoft.Extensions.Configuration.AzureKeyVault** パッケージを使用すると、ASP.NET Core アプリケーションから Azure Key Vault の構成情報を読み取ることができます。 Azure Key Vault のシークレットを初めて使用する場合は、次の手順を実行します。

1. アプリケーションを Azure AD アプリケーションとして登録します (キー コンテナーへのアクセスは Azure AD で管理されます)。この操作は、Azure 管理ポータルで実行できます。\

   または、アプリケーションでパスワードまたはクライアント シークレットの代わりに証明書を使用して認証する場合は、[New-AzADApplication](/powershell/module/az.resources/new-azadapplication) PowerShell コマンドレットを使用できます。 Azure Key Vault に登録する証明書には、公開キーのみが必要です アプリケーションでは秘密キーが使用されます。

2. 新しいサービス プリンシパルを作成して、登録されたアプリケーションにキー コンテナーへのアクセス権を付与します。 この処理を実行するには、次の PowerShell コマンドを使用します。

   ```powershell
   $sp = New-AzADServicePrincipal -ApplicationId "<Application ID guid>"
   Set-AzKeyVaultAccessPolicy -VaultName "<VaultName>" -ServicePrincipalName $sp.ServicePrincipalNames[0] -PermissionsToSecrets all -ResourceGroupName "<KeyVault Resource Group>"
   ```

3. <xref:Microsoft.Extensions.Configuration.IConfigurationRoot> インスタンスを作成するときに <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault%2A?displayProperty=nameWithType> 拡張メソッドを呼び出して、アプリケーションの構成ソースとしてキー コンテナーを含めます。 `AddAzureKeyVault` を呼び出すには、前の手順で登録し、キー コンテナーへのアクセス権が付与されたアプリケーション ID が必要です。

   [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) パッケージへの参照を含めるだけで、クライアント シークレットの代わりに証明書を受け取る `AddAzureKeyVault` のオーバーロードを使用することもできます。

> [!IMPORTANT]
> Azure Key Vault を最後の構成プロバイダーとして登録して、以前のプロバイダーの構成値をオーバーライドできるようにすることをお勧めします。

## <a name="additional-resources"></a>その他の技術情報

- **Azure Key Vault を使用したアプリケーション シークレットの保護** \
  [https://docs.microsoft.com/azure/guidance/guidance-multitenant-identity-keyvault](/azure/guidance/guidance-multitenant-identity-keyvault)

- **開発中のアプリ シークレットの安全な保存** \
  [https://docs.microsoft.com/aspnet/core/security/app-secrets](/aspnet/core/security/app-secrets)

- **データ保護の構成** \
  [https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview](/aspnet/core/security/data-protection/configuration/overview)

- **ASP.NET Core でのデータ保護のキー管理と有効期間** \
  [https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/default-settings](/aspnet/core/security/data-protection/configuration/default-settings)

- **Microsoft.Extensions.Configuration.KeyPerFile** GitHub リポジトリ。 \
  <https://github.com/dotnet/extensions/tree/master/src/Configuration/Config.KeyPerFile>

>[!div class="step-by-step"]
>[前へ](developer-app-secrets-storage.md)
>[次へ](../key-takeaways.md)
