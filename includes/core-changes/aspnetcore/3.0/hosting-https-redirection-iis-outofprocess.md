---
ms.openlocfilehash: eb3fa768a491f6c0ff4b15479beabd71b0670338
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937305"
---
### <a name="hosting-https-redirection-enabled-for-iis-out-of-process-apps"></a>ホスティング:IIS アウトプロセス アプリ用に HTTPS リダイレクトを有効化

IIS アウトプロセスでホストするための [ASP.NET Core モジュール (ANCM)](/aspnet/core/host-and-deploy/aspnet-core-module) のバージョン 13.0.19218.0 により、ASP.NET Core 3.0 および 2.2 アプリに既存の HTTPS リダイレクト機能が有効になります。

ディスカッションについては、[dotnet/AspNetCore#15243](https://github.com/dotnet/AspNetCore/issues/15243) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

ASP.NET Core 2.1 プロジェクト テンプレートで、<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A> や <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts%2A> などの HTTPS ミドルウェア メソッドのサポートが初めて導入されました。 開発中のアプリでは既定のポート 443 が使用されないため、HTTPS リダイレクトを有効にするには構成を追加する必要がありました。 [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) は、要求で既に HTTPS が使用されている場合にのみアクティブになります。 localhost は既定ではスキップされます。

#### <a name="new-behavior"></a>新しい動作

ASP.NET Core 3.0 では、IIS HTTPS シナリオが[強化](https://github.com/dotnet/AspNetCore/pull/4685)されました。 この強化により、アプリがサーバーの HTTPS ポートを検出し、既定で `UseHttpsRedirection` を動作させることができました。 インプロセス コンポーネントでは、`IServerAddresses` 機能を使用してポート検出が行われていました。この機能は、インプロセス ライブラリがフレームワークでバージョン管理されているため、ASP.NET Core 3.0 アプリにのみ影響します。 アウトプロセス コンポーネントが、`ASPNETCORE_HTTPS_PORT` 環境変数を自動的に追加するように変更されました。 アプトプロセス コンポーネントはグローバルに共有されるため、この変更によって ASP.NET Core 2.2 と 3.0 の両方のアプリが影響を受けました。 ASP.NET Core 2.1 アプリは、既定で以前のバージョンの ANCM を使用しているため、響を受けません。

上記の動作は、ASP.NET Core 2.x での動作の変更を元に戻すために ASP.NET Core 3.0.1 および 3.1.0 Preview 3 で変更されました。 これらの変更は、IIS アウトプロセス アプリにのみ影響します。

前に説明したように、ASP.NET Core 3.0.0 をインストールすると、ASP.NET Core 2.x アプリで `UseHttpsRedirection` ミドルウェアもアクティブになるという副作用がありました。 ASP.NET Core 3.0.1 と 3.1.0 Preview 3 では、インストールによって ASP.NET Core 2.x アプリに影響を及ぼさないよう、ANCM に変更が加えられました。 ANCM が ASP.NET Core 3.0.0 で設定した `ASPNETCORE_HTTPS_PORT` 環境変数は、ASP.NET Core 3.0.1 および 3.1.0 Preview 3 で `ASPNETCORE_ANCM_HTTPS_PORT` に変更されました。 これらのリリースでは、新しい変数と古い変数の両方を理解するように `UseHttpsRedirection` も更新されました。 ASP.NET Core 2.x は更新されません。 その結果、既定で無効になっている前の動作に戻ります。

#### <a name="reason-for-change"></a>変更理由

ASP.NET Core 3.0 の機能を向上するため。

#### <a name="recommended-action"></a>推奨アクション

すべてのクライアントで HTTPS を使用する場合は、アクションは必要ありません。 一部のクライアントに HTTP の使用を許可するには、次のいずれかの手順を行います。

* プロジェクトの `Startup.Configure` メソッドから `UseHttpsRedirection` と `UseHsts` への呼び出しを削除し、アプリを再デプロイします。
* *web.config* ファイルで、`ASPNETCORE_HTTPS_PORT` 環境変数を空の文字列に設定します。 この変更は、アプリを再デプロイしなくても、サーバー上で直接行うことができます。 次に例を示します。

    ```xml
    <aspNetCore processPath="dotnet" arguments=".\WebApplication3.dll" stdoutLogEnabled="false" stdoutLogFile="\\?\%home%\LogFiles\stdout" >
        <environmentVariables>
        <environmentVariable name="ASPNETCORE_HTTPS_PORT" value="" />
        </environmentVariables>
    </aspNetCore>
    ```

`UseHttpsRedirection` は引き続き次のことができます。

* `ASPNETCORE_HTTPS_PORT` 環境変数を適切なポート番号 (ほとんどの運用シナリオでは 443) に設定することによって ASP.NET Core 2.x で手動でアクティブ化する。
* 空の文字列値を使用して `ASPNETCORE_ANCM_HTTPS_PORT` を定義することで、ASP.NET Core 3.x で非アクティブ化する。 この値は、前の `ASPNETCORE_HTTPS_PORT` の例と同じ方法で設定されます。

ASP.NET Core 3.0.0 アプリを実行しているマシンでは、ASP.NET Core 3.1.0 Preview 3 ANCM をインストールする前に、ASP.NET Core 3.0.1 ランタイムをインストールする必要があります。 これにより、ASP.NET Core 3.0 アプリで引き続き `UseHttpsRedirection` を期待どおりに動作させることができます。

Azure App Service では、ANCM はそのグローバルな性質のため、ランタイムとは別のスケジュールでデプロイされます。 ANCM は、ASP.NET Core3.0.1 と 3.1.0 がデプロイされた後に、これらの変更とともに Azure にデプロイされました。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection(Microsoft.AspNetCore.Builder.IApplicationBuilder)?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`M:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection(Microsoft.AspNetCore.Builder.IApplicationBuilder)`

-->
