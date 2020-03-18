---
title: 自己ホスト型 gRPC アプリケーション - WCF 開発者向け gRPC
description: コア gRPC アプリケーションASP.NET自己ホスト型サービスとして展開します。
ms.date: 09/02/2019
ms.openlocfilehash: 00fb1453e19a02469f80af79672e0c1f72c7280f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147803"
---
# <a name="self-hosted-grpc-applications"></a>自己ホスト型 gRPC アプリケーション

ASP.NET Core 3.0 アプリケーションは Windows Server 上の IIS でホストできますが、HTTP/2 の機能の一部がサポートされていないため、現在 IIS で gRPC アプリケーションをホストすることはできません。 この機能は、Windows サーバーの将来の更新の目標です。

アプリケーションは、Windows サービスとして実行できます。 または、.NET Core 3.0 ホスティング拡張機能の新機能により[、 systemd](https://en.wikipedia.org/wiki/Systemd)によって制御される Linux サービスとして実行することもできます。

## <a name="run-your-app-as-a-windows-service"></a>アプリを Windows サービスとして実行する

Windows サービスとして実行するようにASP.NETコア アプリケーションを構成するには、NuGet から[Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices)パッケージをインストールします。 次に、 の`UseWindowsService`メソッドへの`CreateHostBuilder`呼び`Program.cs`出しを追加します。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseWindowsService() // Enable running as a Windows service
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

> [!NOTE]
> アプリケーションが Windows サービスとして実行されていない場合、`UseWindowsService`メソッドは何も実行しません。

次のいずれかの方法を使用してアプリケーションを公開します。

* プロジェクトを右クリックし、ショートカット メニューの **[発行**] を選択して、Visual Studio から。
* NET コア CLI から。

NET Core アプリケーションを発行する場合は、*フレームワークに依存*する展開または*自己完結型*の展開を作成できます。 フレームワークに依存する展開では、.NET Core 共有ランタイムを実行するホストにインストールする必要があります。 自己完結型の展開は、.NET Core ランタイムとフレームワークの完全なコピーと共に公開され、任意のホストで実行できます。 各方法の長所と短所など、詳細については[、.NET Core アプリケーションの展開](../../core/deploying/index.md)に関するドキュメントを参照してください。

NET Core 3.0 ランタイムをホストにインストールする必要のない、アプリケーションの自己完結型ビルドを発行するには、アプリケーションに含めるランタイムを指定します。 (または`-r``--runtime`) フラグを使用します。

```dotnetcli
dotnet publish -c Release -r win-x64 -o ./publish
```

フレームワークに依存するビルドを発行するには、フラグ`-r`を省略します。

```dotnetcli
dotnet publish -c Release -o ./publish
```

ディレクトリの完全な内容を`publish`インストール フォルダにコピーします。 次に[、sc ツール](/windows/desktop/services/controlling-a-service-using-sc)を使用して、実行可能ファイル用の Windows サービスを作成します。

```console
sc create MyService binPath=C:\MyService\MyService.exe
```

### <a name="log-to-the-windows-event-log"></a>Windows イベント ログにログを記録する

この`UseWindowsService`メソッドは、ログ メッセージを Windows イベント ログに書き込む[ログ](/aspnet/core/fundamentals/logging/)プロバイダを自動的に追加します。 このプロバイダのログ記録は、エントリを`EventLog`セクションまたは別の`Logging``appsettings.json`構成ソースに追加することで構成できます。

これらの設定でプロパティを設定することで、イベント ログで使用される`SourceName`ソース名を上書きできます。 名前を指定しない場合は、既定のアプリケーション名 (通常は実行可能アセンブリ名) が使用されます。

ログの詳細については、この章の最後に説明します。

## <a name="run-your-app-as-a-linux-service-with-systemd"></a>システムを使用して Linux サービスとしてアプリを実行する

ASP.NETコア アプリケーションを Linux サービス (または Linux パーランスの*デーモン*) として実行するように構成するには、NuGet から[Microsoft.Extensions.Hosting.Systemd](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.Systemd)パッケージをインストールします。 次に、 の`UseSystemd`メソッドへの`CreateHostBuilder`呼び`Program.cs`出しを追加します。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseSystemd() // Enable running as a Systemd service
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

> [!NOTE]
> アプリケーションが Linux サービスとして実行されていない場合、メソッド`UseSystemd`は何も実行しません。

次に、アプリケーションを公開します。 アプリケーションは、関連する Linux ランタイム (たとえば) に応じてフレームワークに依存`linux-x64`するか、自己完結型にすることができます。 次のいずれかの方法を使用して発行できます。

* プロジェクトを右クリックし、ショートカット メニューの **[発行**] を選択して、Visual Studio から。
* NET コア CLI から、次のコマンドを使用します。

  ```dotnetcli
  dotnet publish -c Release -r linux-x64 -o ./publish
  ```
  
ディレクトリの完全な内容を`publish`Linux ホストのインストールフォルダにコピーします。 サービスを登録するには、`/etc/systemd/system`ユニットファイルと呼ばれる特別な*ファイル*をディレクトリに追加する必要があります。 このフォルダにファイルを作成するには、ルートアクセス権が必要です。 使用する識別子と拡張子を持つファイル`systemd`に名前を付`.service`けます。 たとえば、 `/etc/systemd/system/myapp.service`を使用します。

サービス ファイルは、次の例に示すように INI 形式を使用します。

```ini
[Unit]
Description=My gRPC Application

[Service]
Type=notify
ExecStart=/usr/sbin/myapp

[Install]
WantedBy=multi-user.target
```

この`Type=notify`プロパティは`systemd`、アプリケーションが起動時とシャットダウン時に通知することを示します。 この`WantedBy=multi-user.target`設定では、Linux システムが "runlevel 2" に達するとサービスが開始されます。

前`systemd`にサービスを認識し、その構成を再読み込みする必要があります。 コマンドを`systemd`使用して制御します。 `systemctl` 再ロード後、サブコマンド`status`を使用して、アプリケーションが正常に登録されたことを確認します。

```console
sudo systemctl daemon-reload
sudo systemctl status myapp
```

サービスを正しく構成すると、次の出力が得られます。

```text
myapp.service - My gRPC Application
 Loaded: loaded (/etc/systemd/system/myapp.service; disabled; vendor preset: enabled)
 Active: inactive (dead)
```

このコマンド`start`を使用して、サービスを開始します。

```console
sudo systemctl start myapp.service
```

> [!TIP]
> を`.service`使用`systemctl start`している場合、この拡張子はオプションです。

システムの`systemd`起動時にサービスを自動的に開始するように指示するには、`enable`コマンドを使用します。

```console
sudo systemctl enable myapp
```

### <a name="log-to-journald"></a>ジャーナル処理にログ記録

Windows イベント ログに相当する`journald`Linux は、 の一部である構造化ログ`systemd`システム サービスです。 Linux デーモンによって標準出力に書き込まれたログメッセージは自動的`journald`に に に書き込まれます。 ログレベルを設定するには、ログ`Console`設定のセクションを使用します。 ホスト`UseSystemd`・ビルダー方式では、ジャーナルに合わせてコンソール出力形式が自動的に構成されます。

Linux`journald`ログの標準であるため、さまざまなツールが統合されています。 ログは、外部ログ`journald`システムに簡単にルーティングできます。 ホスト上でローカルに作業する場合は、`journalctl`コマンドラインからログを表示できます。

```console
sudo journalctl -u myapp
```

> [!TIP]
> ホストで GUI 環境を使用できる場合は、Linux で*QJournalctl*や*gnome-logs*などのいくつかのグラフィカルログビューアを利用できます。

を使用`journalctl`してコマンド ラインから`systemd`ジャーナルを照会する方法の詳細については、[マニュアル ページを参照してください](https://manpages.debian.org/buster/systemd/journalctl.1)。

## <a name="https-certificates-for-self-hosted-applications"></a>自己ホスト型アプリケーションの HTTPS 証明書

実稼働環境で gRPC アプリケーションを実行する場合は、信頼された認証局 (CA) の TLS 証明書を使用する必要があります。 この CA は、パブリック CA または組織の内部 CA である可能性があります。

Windows ホストでは、クラスを使用して、セキュリティで保護された[証明書ストア](/windows/win32/seccrypto/managing-certificates-with-certificate-stores)から<xref:System.Security.Cryptography.X509Certificates.X509Store>証明書を読み込むことができます。 一部の Linux`X509Store`ホストでは OpenSSL キー ストアでクラスを使用することもできます。

また、次のいずれかの方法で[、X509Certificate2 コンストラクタ](xref:System.Security.Cryptography.X509Certificates.X509Certificate2.%23ctor%2A)のいずれかを使用して証明書を作成することもできます。

* 強力なパスワードで保護された`.pfx`ファイルなどのファイル
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)などのセキュリティで保護されたストレージ サービスから取得されたバイナリ データ

証明書を使用するように Kestrel を構成するには、構成とコードの 2 つの方法があります。

### <a name="set-https-certificates-by-using-configuration"></a>構成を使用して HTTPS 証明書を設定する

構成方法では、証明書`.pfx`ファイルへのパスと Kestrel 構成セクションのパスワードを設定する必要があります。 では`appsettings.json`、次のようになります。

```json
{
  "Kestrel": {
    "Certificates": {
      "Default": {
        "Path": "cert.pfx",
        "Password": "DO NOT STORE PLAINTEXT PASSWORDS IN APPSETTINGS FILES"
      }
    }
  }
}
```

Azure Key Vault やハシコープ Vault などのセキュリティで保護された構成ソースを使用してパスワードを指定します。

> [!IMPORTANT]
> 暗号化されていないパスワードを設定ファイルに保存しないでください。

### <a name="set-https-certificates-in-code"></a>コードで HTTPS 証明書を設定する

コード内の Kestrel で HTTPS`ConfigureKestrel`を設定`IWebHostBuilder`するには、`Program`クラスでメソッドを on を使用します。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            webBuilder.ConfigureKestrel(kestrel =>
            {
                kestrel.ConfigureHttpsDefaults(https =>
                {
                    https.ServerCertificate = new X509Certificate2("mycert.pfx", "password");
                });
            });
        });
```

ここでも、ファイルのパスワードを`.pfx`安全な構成ソースに保存し、そのパスワードを取得してください。

>[!div class="step-by-step"]
>[前次](grpc-in-production.md)
>[Next](docker.md)
