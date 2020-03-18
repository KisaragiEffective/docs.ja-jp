---
title: Windows Communication Foundation サンプルの 1 回限りのセットアップの手順
ms.date: 03/30/2017
ms.assetid: a5848ffd-3eb5-432d-812e-bd948ccb6bca
ms.openlocfilehash: 1d7b5f6cb5922bde4b002611209ea3464f8e457c
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921181"
---
# <a name="one-time-setup-procedure-for-the-windows-communication-foundation-samples"></a>Windows Communication Foundation サンプルの 1 回限りのセットアップの手順

ほとんどの Windows Communication Foundation (WCF) サンプルはインターネットインフォメーションサービス (IIS) でホストされ、共通の仮想ディレクトリから実行されます。 この1回限りのセットアップ手順では、ディスク上にフォルダーを作成します。また、 **ServiceModelSamples**という名前の IIS に仮想ディレクトリを追加します。

**ServiceModelSamples**仮想ディレクトリは、IIS でホストされるサービスを使用するすべてのサンプルをビルドして実行するために使用されます。 サンプルの実行に必要な仮想ディレクトリはこれだけです。 サンプルをビルドすると、この仮想ディレクトリにある、以前に配置されたサービスがすべて置き換えられます。この仮想ディレクトリには最近ビルドされたサンプルだけが配置されるため、そのサンプルしか使用できません。

> [!NOTE]
> すべてのコマンドは、ローカル管理者アカウントで実行する必要があります。 Windows 7、Windows Vista、または Windows Server 2008 R2 を使用している場合は、管理者特権でコマンドプロンプトを実行する必要もあります。 これを行うには、コマンドプロンプトアイコンを右クリックし、 **[管理者として実行]** をクリックします。 このトピックで使用するすべてのコマンドは、適切なパスが設定されているコマンド プロンプトで実行する必要があります。  このようにするための最も簡単な方法は、Visual Studio コマンド プロンプトを使用する方法です。 このプロンプトを開くには、 **[スタート]** ボタンをクリックし、 **[すべてのプログラム]** 、 **[visual studio 2010]** の順にスクロールし、 **[Visual Studio Tools]** をクリックします。次に、 **[visual studio コマンドプロンプト (2010)]** を右クリックし、 **[管理者として実行]** をクリックします。 Visual Studio Express Editions のいずれかがインストールされている場合は、このコマンド プロンプトを使用できません。この場合、システム パスに "C:\Windows\Microsoft.Net\Framework\v4.0" を追加する必要があります。

### <a name="one-time-setup-procedure-for-wcf-samples"></a>WCF サンプルの 1 回限りのセットアップの手順

1. ASP.NET が設定されていることを確認します。 ASP.NET をセットアップする方法の詳細については、[インターネットインフォメーションサービスのホスティング手順](../../../../docs/framework/wcf/samples/internet-information-service-hosting-instructions.md)に関する説明を参照してください。

2. .NET Framework 4 がインストールされていることを確認します。 次のバージョンの v2.0 (またはそれ以降) のディレクトリを検索します: **\windows\ microsoft.net \framework**

3. Visual Studio 2012 以降がインストールされていること、またはオペレーティングシステムが Windows Server 2008 SP2 以降であることを確認します。

4. 次のコマンドを実行します。 これらのコマンドを実行する必要がある理由の詳細については、「 [IIS ホステッドサービスが失敗](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752252(v=vs.90))する」を参照してください。

    > [!WARNING]
    > IIS を再インストールした場合は、次のコマンドを再実行します。

    ```console
    "%WINDIR%\Microsoft.Net\Framework\v4.0.30319\aspnet_regiis" –i –enable
    "%WINDIR%\Microsoft.Net\Framework\v4.0.30319\ServiceModelReg.exe" -r
    ```

    > [!WARNING]
    > コマンド `aspnet_regiis –i –enable` を実行すると、既定のアプリプールが .NET Framework 4 を使用して実行されます。これにより、同じコンピューター上の他のアプリケーションに対して非互換性の問題が生じる可能性があります。

5. [ファイアウォールの指示](../../../../docs/framework/wcf/samples/firewall-instructions.md)に従って、サンプルで使用するポートを有効にします。

6. 次の既定のディレクトリを確認してください: \<InstallDrive > **:\ WF_WCF_Samples**。 サンプルが既にインストールされている場合は、これが既定のディレクトリです。

7. サンプルがインストールされていない場合は、の[C#](https://go.microsoft.com/fwlink/?LinkId=190939)サンプルダウンロード場所からインストールします。

8. サンプルをインストールしたら、\<InstallDrive > **:\ WF_WCF_Samples \wcf\setup\\** にアクセスします。

9. **Setupvroot**バッチファイルを実行します。 次の手順が実行されます。

    - ServiceModelSamples という名前の仮想ディレクトリが IIS に作成されます。

    - %SystemDrive%\Inetpub\wwwroot\ServiceModelSamples and %SystemDrive%\Inetpub\wwwroot\ServiceModelSamples\bin という名前の新しいディスク ディレクトリが作成されます。

    これらのディレクトリを手動で設定する場合は、[仮想ディレクトリのセットアップ手順](../../../../docs/framework/wcf/samples/virtual-directory-setup-instructions.md)を参照してください。 この手順で行ったすべての変更を元に戻すには、サンプルの使用が終わった後で cleanupvroot.bat を実行します。

    > [!NOTE]
    > cleanupvroot.bat を実行しない限り、この手順を実行するのは、1 台のコンピューターで 1 回だけです。

10. サンプルおよび Network Service ユーザーのビルドに使用するアカウントに対し、%SystemDrive%\inetpub\wwwroot の変更権限を付与する必要があります。 ビルドの実行時に、Web ホストの一部のサンプルがコンパイル済みバイナリを前述の場所にコピーしようとする場合があり、適切な権限が設定されていないと、ビルドは破損します。 または、アクセス許可をそのままにして、SDK コマンドプロンプトまたは Visual Studio コマンドプロンプト (2012) を管理者として実行するか、Visual Studio 2012 でサンプルをビルドして、管理者として実行することもできます。

    > [!NOTE]
    > この手順を完了していない場合は、ビルドの実行時に IIS でホストされているすべてのサンプルでエラーが発生します。 アクセス許可が正しく設定されていることを確認するか、SDK コマンド プロンプトと Visual Studio コマンド プロンプト (2012) を管理者として実行してください。

11. コンピューター上に C:\logs ディレクトリを作成します (一部のサンプルで必要になることがあります)。 このフォルダーに対する書き込みアクセスが適切なアカウントに付与されていることを確認してください。 Windows 7、Windows Vista、および Windows Server 2008 R2 では、このアカウントは**Network Service**です。 Windows Server 2008 の場合、アカウントは NT Authority\Network Service です。 Windows XP および Windows Server 2003 の場合、アカウントは ASPNET です。

12. Setupcerttool.bat ファイルを実行します。 このファイルは、\<InstallPath > \ WF_WCF_Samples \WCF\Setup\ フォルダーにあります。  このスクリプトでは、次のタスクが実行されます。

    - FindPrivateKey ツールをビルドします。

    - %ProgramFiles%\ServiceModelSampleTools という名前のディレクトリを作成します。

    - 新しい FindPrivateKey ツールをこのディレクトリにコピーします。

    このツールは、証明書を使用して IIS でホストされるサンプルで必要です。

    > [!NOTE]
    > セキュリティの目的で、サンプルの使用が終わったら、このセットアップ手順で付与された仮想ディレクトリの定義とアクセス許可を必ず削除してください。削除するには、Cleanupvroot.bat という名前のバッチ ファイルを実行します。

13. 自己ホスト型の (IIS でホストされていない) サンプルでは、リッスンを行うコンピューター上で HTTP アドレスを登録するためのアクセス許可が必要です。 HTTP 名前空間予約のアクセス許可は、サンプルの実行に使用されるユーザー アカウントから提供されます。 既定では、管理者アカウントには、任意の HTTP アドレスを登録するためのアクセス許可があります。 管理者以外のアカウントの場合は、サンプルで使用される HTTP 名前空間へのアクセス許可が付与される必要があります。 名前空間の予約を構成する方法については、「[Configuring HTTP and HTTPS](../../../../docs/framework/wcf/feature-details/configuring-http-and-https.md)」 (HTTP と HTTPS を構成する) を参照してください。

14. 一部のサンプルにはメッセージ キューが必要です。 インストール手順については、「[メッセージキュー (MSMQ) のインストール](../../../../docs/framework/wcf/samples/installing-message-queuing-msmq.md)」を参照してください。

    > [!NOTE]
    > メッセージ キューが必要なサンプルを実行する場合は、MSMQ サービスを事前に開始しておいてください。

15. 一部のサンプルには証明書が必要です。 「[インターネットインフォメーションサービス (IIS) サーバー証明書のインストール手順](../../../../docs/framework/wcf/samples/iis-server-certificate-installation-instructions.md)」を参照してください。
