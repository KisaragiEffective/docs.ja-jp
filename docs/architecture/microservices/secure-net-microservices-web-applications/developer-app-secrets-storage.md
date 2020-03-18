---
title: 開発時にアプリケーションの機密情報を安全に格納する
description: .NET マイクロサービスと Web アプリケーションのセキュリティ - パスワード、接続文字列、API キーなどのアプリケーションのシークレットはソース管理に保存しないでください。ASP.NET Core で使用できるオプションを理解してください。特に、"ユーザーのシークレット" の処理方法を理解する必要があります。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: 1ef2246746b9165f1564fa7be64ff7eb28eb1d32
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77501793"
---
# <a name="store-application-secrets-safely-during-development"></a>開発時にアプリケーションのシークレットを安全に格納する

保護されたリソースおよびその他のサービスに接続する場合、ASP.NET Core アプリケーションは、通常、接続文字列やパスワードなど、機密情報を含む資格情報を使用する必要があります。 このような機密情報は、"*シークレット*" と呼ばれます。 ソース コードにシークレットを含めないこと、さらに決してソース管理にシークレットを格納しないことがベスト プラクティスです。 ASP.NET Core 構成モデルを使用して、安全な場所からシークレットを読み取る必要があります。

開発リソースおよびステージング リソースにアクセスするためのシークレットは、運用環境のリソースにアクセスするために使用するシークレットとは区別する必要があります。さまざまな人が、そのようなさまざまなシークレット セットへのアクセスを必要とするからです。 開発時に使用するシークレットを格納するには、環境変数にシークレットを格納するか、または ASP.NET Core Secret Manager ツールを使用してシークレットを格納する方法が一般的です。 運用環境でのストレージの安全性を高めるには、マイクロサービスで Azure Key Vault にシークレットを格納することができます。

## <a name="store-secrets-in-environment-variables"></a>環境変数にシークレットを格納する

ソース コードの外部にシークレットを保持する方法の 1 つは、開発者用が開発用コンピューター上で文字列ベースのシークレットを[環境変数](/aspnet/core/security/app-secrets#environment-variables)として設定するというものです。 環境変数を使用して、階層型の名前 (構成セクションで入れ子にされている名前など) を付けてシークレットを格納する場合は、そのセクションの完全な階層をコロン (:) 区切りで含めるように変数名を付ける必要があります。

たとえば、環境変数 `Logging:LogLevel:Default` を `Debug` 値に設定する場合、それは次の JSON ファイルから構成値を取得することと等価になります。

```json
{
    "Logging": {
        "LogLevel": {
            "Default": "Debug"
        }
    }
}
```

環境変数からこれらの値にアクセスするためには、アプリケーションは IConfigurationRoot オブジェクトを構築する際に ConfigurationBuilder 上で AddEnvironmentVariables を呼び出す必要があるだけです。

環境変数は一般にプレーン テキストとして格納されるため、環境変数が設定されたコンピューターまたはプロセスのセキュリティが侵害されると、環境変数の値が表示されます。

## <a name="store-secrets-with-the-aspnet-core-secret-manager"></a>ASP.NET Core Secret Manager を使用してシークレットを格納する

[開発時に](/aspnet/core/security/app-secrets#secret-manager)ソース コードの外部にシークレットを保持する別の方法として、ASP.NET Core の **Secret Manager** ツールを使用するやり方があります。 Secret Manager ツールを使用するには、プロジェクト ファイルにパッケージ **Microsoft.Extensions.Configuration.SecretManager** を組み込みます。 その依存関係が存在し、それが復元されたら、`dotnet user-secrets` コマンドを使用して、コマンド ラインからシークレットの値を設定できます。 これらのシークレットは、ユーザーのプロファイル ディレクトリにある JSON ファイルに格納され (OS によって詳細は異なる)、ソース コードから離れた場所に置かれます。

Secret Manager ツールによって設定されたシークレットは、シークレットを使用するプロジェクトの `UserSecretsId` プロパティによって編成されます。 そのため、次のスニペットに示すように、プロジェクト ファイルに UserSecretsId プロパティを設定する必要があります。 既定値は Visual Studio によって割り当てられた GUID ですが、お使いのコンピューター内で一意である限り、実際の文字列は重要ではありません。

```xml
<PropertyGroup>
    <UserSecretsId>UniqueIdentifyingString</UserSecretsId>
</PropertyGroup>
```

Secret Manager で格納されたシークレットをアプリケーションで使用するには、ConfigurationBuilder インスタンス上で `AddUserSecrets<T>` を呼び出して、アプリケーション用のシークレットをその構成に含めます。 ジェネリック パラメーター T は、UserSecretId が適用されたアセンブリからの型である必要があります。 通常は、`AddUserSecrets<Startup>` を使用しても問題ありません。

`AddUserSecrets<Startup>()`Program.cs`CreateDefaultBuilder` の  *メソッドを使用する場合、* は開発環境の既定のオプションに含まれています。

>[!div class="step-by-step"]
>[前へ](authorization-net-microservices-web-applications.md)
>[次へ](azure-key-vault-protects-secrets.md)
