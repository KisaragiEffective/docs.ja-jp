---
title: Mage.exe (マニフェストの生成および編集ツール)
ms.date: 12/06/2018
helpviewer_keywords:
- Manifest Generation and Editing tool
- Mage.exe
ms.assetid: 77dfe576-2962-407e-af13-82255df725a1
ms.openlocfilehash: 3752ac7108a9fcd55b61b32b889a717ef7c0faff
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714475"
---
# <a name="mageexe-manifest-generation-and-editing-tool"></a>Mage.exe (マニフェストの生成および編集ツール)

マニフェスト生成および編集ツール (*Mage.exe*) は、アプリケーション マニフェストおよび配置マニフェストの作成と編集をサポートするコマンド ライン ツールです。 *Mage.exe* はコマンド ライン ツールであるため、バッチ スクリプトから実行したり、ASP.NET アプリケーションなどの他の Windows ベースのアプリケーションから実行したりできます。

*Mage.exe* の代わりに、グラフィカルなアプリケーションである *MageUI.exe* を使用することもできます。 詳細については、「 [MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)」を参照してください。

このツールは、Visual Studio と共に自動的にインストールされます。 ツールを実行するには、Visual Studio の開発者コマンド プロンプトを使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

Visual Studio には、2 つのバージョンの *Mage.exe* および *MageUI.exe* が含まれています。 バージョン情報を確認するには、*MageUI.exe* を実行し、 **[ヘルプ]** をクリックして、 **[バージョン情報]** をクリックします。 このドキュメントでは、バージョン 4.0.x.x の *Mage.exe* および *MageUI.exe* について説明します。

## <a name="syntax"></a>構文

```console
Mage [commands] [commandOptions]
```

## <a name="parameters"></a>パラメーター

次の表に、*Mage.exe* でサポートされているコマンドを示します。 これらのコマンドでサポートされるオプションの詳細については、「[New コマンド オプションと Update コマンド オプション](#new-and-update-command-options)」および「[Sign コマンド オプション](#sign-command-options)」を参照してください。

|コマンド|説明|
|-------------|-----------------|
|**-cc, ClearApplicationCache**|すべてのオンライン専用アプリケーションのダウンロード アプリケーション キャッシュをクリアします。|
|**-n, -New** *fileType [newOptions]*|指定された種類のファイルを新規作成します。 有効な種類は、次のとおりです。<br /><br /> -   `Deployment`:配置マニフェストを新規作成します。<br />-   `Application`:アプリケーション マニフェストを新規作成します。<br /><br /> このコマンドで他のパラメーターを指定しなかった場合は、適切な既定のタグおよび属性値を使用して、適切な種類のファイルが作成されます。<br /><br /> **-ToFile** オプション (次の表を参照) を使用して、新しいファイルの名前とパスを指定します。<br /><br /> マニフェストの \<dependency> セクションに追加されたアプリケーション用のすべてのアセンブリを使用して、アプリケーション マニフェストを作成する場合は、 **-FromDirectory** オプション (次の表を参照) を使用します。|
|**-u, -Update** *[filePath] [updateOptions]*|1 つ以上の変更をマニフェスト ファイルに加えます。 編集中のファイルの種類を指定する必要はありません。 Mage.exe は、一連のヒューリスティックを使用してファイルを調査し、配置マニフェストであるのか、アプリケーション マニフェストであるのかを判定します。<br /><br /> ファイルが証明書で署名済みの場合に **-Update** を指定すると、キー署名ブロックが削除されます。 これは、キー署名にはファイルのハッシュが含まれているため、ファイルを変更するとハッシュが無効になるためです。<br /><br /> **-ToFile** オプション (次の表を参照) を使用して、既存のテキストに上書きするのではなく、新しいファイルの名前とパスを指定します。|
|**-s, -Sign** `[signOptions]`|キー ペアまたは X509 証明書を使用してファイルに署名します。 署名は、XML 要素としてファイルに挿入されます。<br /><br /> **-TimestampUri** 値を指定するマニフェストに署名するときには、インターネットに接続している必要があります。|
|**-ver, -Verify** *[manifest-filename]*|マニフェストが正しく署名されていることを確認します。 他のコマンドと組み合わせることはできません。 <br/><br/>**.NET Framework 4.7 以降のバージョンで使用できます。**|
|**-h, -?, -Help** *[verbose]*|使用可能なすべてのコマンドおよびそのオプションの説明が表示されます。 `verbose` を指定して、詳細なヘルプを表示します。|

## <a name="new-and-update-command-options"></a>New コマンド オプションと Update コマンド オプション

次の表は、`-New` コマンドおよび `-Update` コマンドでサポートされているオプションを示しています。

|オプション|既定値|対象|説明|
|-------------|-------------------|----------------|-----------------|
|**-a, -Algorithm**|sha1RSA|アプリケーション マニフェスト<br /><br /> 配置マニフェスト|アルゴリズムの依存関係のダイジェストを生成するように指定します。 値は、"sha256RSA" または "sha1RSA" であることが必要です。<br /><br /> "-Update" オプションでのみ使用します。 "-Sign" オプションを使用した場合、このオプションは無視されます|
|**-appc, -AppCodeBase** `manifestReference`||配置マニフェスト|アプリケーション マニフェスト ファイルへの URL またはファイル パス参照を挿入します。 この値は、アプリケーション マニフェストへの完全パスである必要があります。|
|**-appm, -AppManifest** `manifestPath`||配置マニフェスト|配置のアプリケーション マニフェストへの参照を、その配置マニフェストに挿入します。<br /><br /> `manifestPath` に指定したファイルは存在する必要があります。存在しない場合、*Mage.exe* はエラーを発行します。 `manifestPath` の参照先のファイルがアプリケーション マニフェストでない場合、*Mage.exe* はエラーを発行します。|
|**-cf, -CertFile** `filePath`||すべてのファイルの種類|マニフェストまたはライセンス ファイルに署名するための X509 デジタル証明書の場所を指定します。 Personal Information Exchange (PFX) ファイル用のパスワードが証明書に必要な場合は、このオプションを **-Password** オプションと共に使用できます。 .NET Framework 4.7 以降、ファイルに秘密キーが含まれない場合は、オプション **-CryptoProvider** と **-KeyContainer** を組み合わせる必要があります。<br/><br/>.NET Framework 4.6.2 以降、*Mage.exe* では CAPI と同様に CNG 証明書を使ってマニフェストに署名します。|
|**-ch, -CertHash** `hashSignature`||すべてのファイルの種類|クライアント コンピューターの個人証明書ストアに格納されているデジタル証明書のハッシュです。 これは、Windows の証明書コンソールに表示されるデジタル証明書のサムプリント文字列に対応しています。<br /><br /> `hashSignature` には、大文字も小文字も使用できます。また、1 つの文字列で指定することも、サムプリント文字の各オクテット (8 ビット) を空白で区切り、サムプリント全体を引用符で囲んで指定することもできます。|
|**-csp, -CryptoProvider** `provider-name`||すべてのファイルの種類|秘密キー コンテナーを含む暗号化サービス プロバイダー (CSP) の名前を指定します。 このオプションには **-KeyContainer** オプションが必要です。<br/><br/>このオプションは、.NET Framework 4.7 以降で使用できます。|
|**-fd, -FromDirectory** `directoryPath`||アプリケーション マニフェスト|`directoryPath`およびそのすべてのサブディレクトリで見つかったすべてのアセンブリとファイルの説明を含んでいるアプリケーション マニフェストを作成します。ここで、 `directoryPath` は、配置対象のアプリケーションが格納されているディレクトリです。 *Mage.exe* では、ディレクトリ内のファイルごとに、そのファイルがアセンブリなのかスタティック ファイルなのかが判断されます。 アセンブリの場合は、 `<dependency>` タグおよび `installFrom` 属性が、アセンブリ名、コード ベースおよびバージョンと共に、アプリケーションに追加されます。 スタティック ファイルの場合は、 `<file>` タグが追加されます。 また、*Mage.exe* は、簡単な一連のヒューリスティックによってアプリケーションのメイン実行可能ファイルを検出し、この実行可能ファイルを ClickOnce アプリケーションのエントリ ポイントとしてマニフェストでマークします。<br /><br /> *Mage.exe* では、ファイルが "データ" ファイルとして自動的にマークされることはありません。 これは手動で行う必要があります。 詳細については、[ClickOnce アプリケーションにデータ ファイルを含める](/visualstudio/deployment/how-to-include-a-data-file-in-a-clickonce-application)」を参照してください。<br /><br /> *Mage.exe* では、各ファイルのサイズに基づいて、各ファイルのハッシュも生成されます。 ClickOnce では、マニフェスト作成後に、配置の各ファイルが改ざんされていないことを確認するためにこれらのハッシュを使用します。 配置対象のファイルに変更を加えた場合には、*Mage.exe* を **-Update** コマンドおよび **-FromDirectory** オプションを付けて実行すると、参照している全ファイルのハッシュおよびアセンブリ バージョンを更新できます。<br /><br /> **-FromDirectory** を指定すると、 `directoryPath`で見つかった全サブディレクトリの全ファイルが含まれます。<br /><br /> **-FromDirectory** を **-Update** コマンドと共に使用すると、アプリケーション マニフェスト内のファイルのうち、このディレクトリに存在しなくなったファイルが *Mage.exe* によってマニフェストから削除されます。|
|**-if, -IconFile**  `filePath`||アプリケーション マニフェスト|.ICO アイコン ファイルの完全パスを指定します。 このアイコンは、[スタート] メニューおよび [プログラムの追加と削除] エントリのアプリケーション名の横に表示されます。 アイコンを指定しない場合は、既定のアイコンが使用されます。|
|**-ip, -IncludeProviderURL**  `url`|true|配置マニフェスト|**-ProviderURL**で設定された更新プログラムの場所の値が配置マニフェストに含まれているかどうかを示します。|
|**-i, -Install** `willInstall`|true|配置マニフェスト|ClickOnce アプリケーションをローカル コンピューターにインストールするのか、Web サイトで実行するのかを指定します。 アプリケーションをインストールすると、アプリケーションは Windows の **[スタート]** メニューに表示されます。 有効な値は、"true" または "t"、および "false" または "f" です。<br /><br /> **-MinVersion** オプションを指定し、ユーザーが **-MinVersion** より前のバージョンをインストールしていた場合は、 **-Install**に渡した値に関係なく、アプリケーションがインストールされます。<br /><br /> このオプションは、 **-BrowserHosted** オプションと一緒に使用することはできません。 1 つのマニフェストに両方のオプションを使用しようとすると、エラーが発生します。|
|**-kc, -KeyContainer** `name`||すべてのファイルの種類|秘密キーの名前を含むキー コンテナーを指定します。 このオプションには **CryptoProvider** オプションが必要です。<br/><br/>このオプションは、.NET Framework 4.7 以降で使用できます。|
|**-mv, -MinVersion**  `[version]`|**-Version** フラグで指定された、ClickOnce 配置マニフェストに示されたバージョン。|配置マニフェスト|ユーザーが実行できる、このアプリケーションの最小バージョンです。 このフラグで指定したバージョンが、アプリケーションの最低限必要なバージョンになります。 大幅に変更された更新プログラムを含むバージョンや、重大なセキュリティ問題を解決したバージョンの製品をリリースした場合に、このフラグを使用すると、この更新バージョンを必ずインストールするように指定でき、ユーザーは古いバージョンを実行できなくなります。<br /><br /> `version` は、 **-Version** フラグの引数と同じ意味を持ちます。|
|**-n, -Name** `nameString`|配置|すべてのファイルの種類|アプリケーションを識別するための名前です。 ClickOnce では、 **[スタート]** メニュー (アプリケーションをインストールする場合) やアクセス許可の昇格のダイアログ ボックスでこのアプリケーションを示す名前として、この名前が使用されます。 **注:** 既存のマニフェストを更新するときにこのオプションで発行者名を指定しない場合、*Mage.exe* はコンピューターで定義された組織名でマニフェストを更新します。 別の名前を使用するには、必ずこのオプションを使用して希望する発行者名を指定してください。|
|**-pwd, -Password** `passwd`||すべてのファイルの種類|デジタル証明書でマニフェストに署名する場合に使用するパスワードです。 **-CertFile** オプションと共に使用する必要があります。|
|**-p, Processor** `processorValue`|Msil|アプリケーション マニフェスト<br /><br /> 配置マニフェスト|このアプリケーションを実行するマイクロプロセッサのアーキテクチャです。 特定のマイクロプロセッサ用にアセンブリがプリコンパイルされている、1 つ以上のアプリケーションを配置する場合は、この値を指定する必要があります。 有効な値には、 `msil`、 `x86`、 `ia64`、および `amd64`があります。 `msil` (Microsoft Intermediate Language) は、すべてのアセンブリがどのプラットフォームにも依存していないことを示します。この場合、共通言語ランタイム (CLR: Common Language Runtime) は、アプリケーションの最初の実行時にアセンブリの Just-In-Time コンパイルを行います。|
|**-pu,** **-ProviderURL** `url`||配置マニフェスト|ClickOnce でアプリケーションの更新プログラムのチェックを行う場合に参照する URL を指定します。|
|**-pub, -Publisher** `publisherName`||アプリケーション マニフェスト<br /><br /> 配置マニフェスト|配置マニフェストとアプリケーション マニフェストのいずれかの description 要素に発行者名を追加します。 アプリケーション マニフェストで使用する場合、 **-UseManifestForTrust** も指定する必要があります (値は "true" または "t" に設定します)。指定しないと、エラーが発生します。|
|**-s, -SupportURL**  `url`||アプリケーション マニフェスト<br /><br /> 配置マニフェスト|[プログラムの追加と削除] で、ClickOnce アプリケーションのエントリに表示されるリンクを指定します。|
|**-ti, -TimestampUri** `uri`||アプリケーション マニフェスト<br /><br /> 配置マニフェスト|デジタル タイムスタンプ サービスの URL です。 マニフェストにタイムスタンプを設定すると、マニフェストに再署名することを防止できますが、アプリケーションの次のバージョンを配置する前にデジタル証明書の有効期限が切れるようにする必要があります。 詳細については、 [Windows ルート証明書プログラムのメンバー](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265983(v=ws.11))を参照してください。|
|**-t, -ToFile** `filePath`|-   New:<br />-   配置マニフェストの場合: deploy.application<br />-   アプリケーション マニフェストの場合: application.exe.manifest<br />-   Update コマンド オプション:<br />-   入力ファイル|すべてのファイルの種類|作成または変更されたファイルの出力パスを指定します。<br /><br /> **-New** を使用するときに **-ToFile**を指定しなかった場合、出力ファイルは現在の作業ディレクトリに書き込まれます。 **-Update** を使用する場合に **-ToFile** を指定しなかった場合、*Mage.exe* では、入力ファイルにファイルが書き戻されます。|
|**-tr, -TrustLevel** `level`|アプリケーション URL が存在するゾーンに基づいて。|アプリケーション マニフェスト|クライアント コンピューター上のアプリケーションに与える信頼のレベルです。 有効な値には、"Internet"、"Intranet"、および "FullTrust" が含まれます。|
|**-um, -UseManifestForTrust** `willUseForTrust`|False|アプリケーション マニフェスト|アプリケーションをクライアントで実行するときに、アプリケーション マニフェストのデジタル署名を信頼の決定に使用するかどうかを指定します。 "true" または "t" を指定すると、信頼の決定にアプリケーション マニフェストが使用されます。 "false" または "f" を指定すると、信頼の決定に配置マニフェストの署名が使用されます。|
|**-v, -Version** `versionNumber`|1.0.0.0|アプリケーション マニフェスト<br /><br /> 配置マニフェスト|配置のバージョンです。 引数は "*N.N.N.N*" という形式の有効なバージョン文字列である必要があります。ここで "*N*" は、32 ビット符号なし整数です。|
|**-wpf, -WPFBrowserApp**  `isWPFApp`|False|アプリケーション マニフェスト<br /><br /> 配置マニフェスト|このフラグは、アプリケーションが Internet Explorer の内部でホストされ、スタンドアロンの実行可能ファイルではない Windows Presentation Foundation (WPF) アプリケーションである場合にのみ使用します。 有効な値は、"true" または "t"、および "false" または "f" です。<br /><br /> アプリケーション マニフェストの場合、そのアプリケーション マニフェストの `hostInBrowser` 要素に `entryPoint` 属性を追加します。<br /><br /> 配置マニフェストの場合、 `install` 要素の `deployment` 属性に false を設定し、.xbap 拡張子を指定して配置マニフェストを保存します。 この引数を **-Install** 引数と共に指定すると、ブラウザーによってホストされるアプリケーションをインストールできず、アプリケーションがオフラインになるため、エラーが発生します。|

## <a name="sign-command-options"></a>Sign コマンド オプション

次の表は、 `-Sign` コマンドでサポートされているオプションを示しています。このコマンドは、すべての種類のファイルに適用されます。

|オプション|説明|
|-------------|-----------------|
|**-cf, -CertFile** `filePath`|マニフェストに署名するための証明書の場所を指定します。 Personal Information Exchange (PFX) ファイル用のパスワードが証明書に必要な場合は、このオプションを **-Password** オプションと共に使用できます。 .NET Framework 4.7 以降、ファイルに秘密キーが含まれない場合は、オプション **-CryptoProvider** と **-KeyContainer** を組み合わせる必要があります。<br/><br/>.NET Framework 4.6.2 以降、*Mage.exe* では CAPI と同様に CNG 証明書を使ってマニフェストに署名します。|
|**-ch, -CertHash** `hashSignature`|クライアント コンピューターの個人証明書ストアに格納されているデジタル証明書のハッシュです。 これは、Windows の証明書コンソールに表示されるデジタル証明書の拇印プロパティに対応しています。<br /><br /> `hashSignature` には、大文字も小文字も使用できます。また、1 つの文字列で指定することも、サムプリント文字の各オクテット (8 ビット) を空白で区切り、サムプリント全体を引用符で囲んで指定することもできます。|
**-csp, -CryptoProvider** `provider-name`|秘密キー コンテナーを含む暗号化サービス プロバイダー (CSP) の名前を指定します。 このオプションには **-KeyContainer** オプションが必要です。<br/><br/>このオプションは、.NET Framework 4.7 以降で使用できます。|
|**-kc, -KeyContainer** `name`|秘密キーの名前を含むキー コンテナーを指定します。 このオプションには **CryptoProvider** オプションが必要です。<br/><br/>このオプションは、.NET Framework 4.7 以降で使用できます。|
|**-pwd, -Password** `passwd`|デジタル証明書でマニフェストに署名する場合に使用するパスワードです。 **-CertFile** オプションと共に使用する必要があります。|
|**-t, -ToFile** `filePath`|作成または変更されたファイルの出力パスを指定します。|

## <a name="remarks"></a>解説

*Mage.exe* では、どの引数も大文字と小文字が区別されません。 コマンドおよびオプションの前には、ダッシュ (-) またはスラッシュ (/) を付けることができます。

**-Sign** コマンドと共に使用できる引数は、すべて、 **-New** コマンドまたは **-Update** コマンドでも、常に使用できます。 次のコマンドは同等です。

```console
mage -Sign c:\HelloWorldDeployment\HelloWorld.deploy -CertFile cert.pfx
mage -Update c:\HelloWorldDeployment\HelloWorld.deploy -CertFile cert.pfx
```

> [!NOTE]
> .NET framework version 4.6.2 以降では、CNG 証明書もサポートされます。

 署名済みのファイルでは、文書のシグネチャが有効であるかどうかがファイルのハッシュによって確認されるため、署名は最後に行う必要があります。 署名済みのファイルに変更を加えた場合には、再度署名する必要があります。 署名済みの文書に再度署名した場合、*Mage.exe* では、古い署名が新しい署名で置換されます。

 **-AppManifest** オプションを使用して配置マニフェストを事前設定した場合、*Mage.exe* は、配置マニフェストと同じディレクトリにアプリケーション マニフェストが存在し、そのディレクトリが含まれるサブディレクトリには現在の配置のバージョンから名前が付けてあると仮定して、配置マニフェストを適切に構成します。 アプリケーション マニフェストを別の場所に格納する場合は、 **-AppCodeBase** オプションを使用して、他の場所を設定します。

 配置マニフェストおよびアプリケーション マニフェストは、アプリケーションを配置する前に、署名される必要があります。 マニフェストに署名する方法については、「 [Trusted Application Deployment Overview](/visualstudio/deployment/trusted-application-deployment-overview)」を参照してください。

 アプリケーション マニフェストに **-TrustLevel** オプションを指定すると、アプリケーションをクライアント コンピューターで実行するために必要なアクセス許可セットが表示されます。 既定では、アプリケーションの URL が存在する *ゾーン* に基づいてアプリケーションに信頼レベルが割り当てられます。 会社のネットワーク上にアプリケーションを配置する場合、アプリケーションは通常、イントラネット ゾーンに格納されます。一方、インターネット上にアプリケーションを配置する場合には、インターネット ゾーンに格納されます。 どちらのセキュリティ ゾーンでも、アプリケーションからローカル リソースへのアクセスは制限されますが、イントラネット ゾーンはインターネット ゾーンに比べ、制限が少し緩くなっています。 FullTrust ゾーンのアプリケーションの場合には、コンピューターのローカル リソースに制限なしでアクセスできます。 **-TrustLevel** オプションを使用してアプリケーションのゾーンを指定すると、高いレベルの信頼を与えるかどうかを決定するように求めるメッセージが、CLR の Trust Manager コンポーネントによってユーザーに表示されます。 会社のネットワーク上にアプリケーションを配置する場合は、信頼されたアプリケーションの配置を使用すると、ユーザーにメッセージで確認することなく、アプリケーションの信頼レベルを引き上げることができます。

 アプリケーション マニフェストでは、カスタム信頼セクションもサポートされています。 この機能では、アプリケーションの実行に必要な特定のアクセス許可だけを要求するマニフェストを構成できるため、最低限のアクセス許可を要求するセキュリティ プリンシパルにアプリケーションを従わせることができます。 *Mage.exe* では、カスタム信頼セクションの追加は直接サポートされていません。 これを追加するには、テキスト エディター、XML パーサー、またはグラフィカルなツールである *MageUI.exe* を使用してください。 *MageUI.exe* を使用してカスタム信頼セクションを追加する方法の詳細については、「[MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)」を参照してください。

Visual Studio 2017 には *Mage.exe* のバージョン 4.6.1 が含まれています。 このバージョンの *Mage.exe* で作成されたマニフェストは、.NET Framework 4 をターゲットにします。 旧バージョンの .NET Framework を対象にする場合は、旧バージョンの *Mage.exe* を使用してください。

既存のマニフェストに対してアセンブリの追加または削除を実行しても、既存のマニフェストに再署名しても、*Mage.exe* は .NET Framework 4 を対象にするようにマニフェストを更新しません。

これらの機能および制限事項を次の表に示します。

|マニフェストのバージョン|操作|Mage v2.0|Mage v4.0|
|----------------------|---------------|---------------|---------------|
|.NET Framework 2.0 または 3.x を対象とするアプリケーションのマニフェスト|開く|OK|OK|
||閉じる|OK|OK|
||保存|OK|OK|
||再署名|OK|OK|
||新規作成|OK|サポートなし|
||更新 (以下を参照)|OK|OK|
|.NET Framework 4 を対象とするアプリケーションのマニフェスト|開く|OK|OK|
||閉じる|OK|OK|
||保存|OK|OK|
||再署名|OK|OK|
||新規作成|サポートなし|OK|
||更新 (以下を参照)|サポートなし|OK|

|マニフェストのバージョン|更新操作の詳細|Mage v2.0|Mage v4.0|
|----------------------|------------------------------|---------------|---------------|
|.NET Framework 2.0 または 3.x を対象とするアプリケーションのマニフェスト|アセンブリの変更|OK|OK|
||アセンブリの追加|OK|OK|
||アセンブリの削除|OK|OK|
|.NET Framework 4 を対象とするアプリケーションのマニフェスト|アセンブリの変更|サポートなし|OK|
||アセンブリの追加|サポートなし|OK|
||アセンブリの削除|サポートなし|OK|

 Mage.exe によって、.NET Framework 4 Client Profile をターゲットとする新しいマニフェストが作成されます。 NET Framework 4 Client Profile と完全版の .NET Framework 4 の両方で、.NET Framework 4 Client Profile をターゲットとする ClickOnce アプリケーションを実行できます。 アプリケーションのターゲットが完全版の .NET Framework 4 であり、.NET Framework 4 Client Profile 上で実行できない場合は、テキスト エディターを使用してクライアントの `<framework>` 要素を削除し、マニフェストに再署名します。

.NET Framework 4 Client Profile をターゲットとする `<framework>` 要素の例を次に示します。

```xml
<framework targetVersion="4.0" profile="client" supportedRuntime="4.0.20506" />
```

## <a name="examples"></a>使用例

Mage 用のユーザー インターフェイス (*MageUI.exe*) を開く例を次に示します。

```console
mage
```

既定の配置マニフェストおよびアプリケーション マニフェストを作成する例を次に示します。 これらのファイルは、すべて現在の作業ディレクトリに作成され、それぞれ deploy.application と application.exe.manifest という名前が付けられます。

```console
mage -New Deployment
mage -New Application
```

現在のディレクトリにあるすべてのアセンブリとリソース ファイルを含めたアプリケーション マニフェストを作成する例を次に示します。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0
```

次の例は、前の例に続いて、配置名および対象のマイクロプロセッサを指定します。 ここでは、ClickOnce が更新プログラムを確認する対象の URL も指定します。

```console
mage -New Application -FromDirectory . -Name "Hello, World! Application" -Version 1.0.0.0 -Processor "x86" -ProviderUrl http://internalserver/HelloWorld/
```

Internet Explorer でホストされる WPF アプリケーションを配置するための一組のマニフェストを作成する方法を次の例に示します。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0 -WPFBrowserApp true
mage -New Deployment -AppManifest 1.0.0.0\application.manifest -WPFBrowserApp true
```

現在のディレクトリにあるすべてのアセンブリとリソース ファイル、およびを署名を含めたアプリケーション マニフェストを作成する例を次に示します。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0 -KeyContainer keypair.snk -CryptoProvider "Microsoft Enhanced Cryptographic Provider v1.0"
```

アプリケーション マニフェストから取得した情報で配置マニフェストを更新し、アプリケーション マニフェストの場所のコード ベースを設定する例を次に示します。

```console
mage -Update HelloWorld.deploy -AppManifest 1.0.0.0\application.manifest -AppCodeBase http://internalserver/HelloWorld.deploy
```

ユーザーがインストールしているバージョンを強制的に更新するように、配置マニフェストを更新する例を次に示します。

```console
mage -Update c:\HelloWorldDeployment\HelloWorld.deploy -MinVersion 1.1.0.0
```

他のディレクトリからアプリケーション マニフェストを取得するように、配置マニフェストに指定する例を次に示します。

```console
mage -Update HelloWorld.deploy -AppCodeBase http://anotherserver/HelloWorld/1.1.0.0/
```

現在の作業ディレクトリにあるデジタル証明書を使用して、既存の配置マニフェストに署名する例を次に示します。

```console
mage -Sign deploy.application -CertFile cert.pfx -Password <passwd>
```

現在の作業ディレクトリにあるデジタル証明書と秘密キーを使用して、既存の配置マニフェストに署名する例を次に示します。

```console
mage -Sign deploy.application -CertFile cert.pfx -KeyContainer keyfile.snk -CryptoProvider "Microsoft Enhanced Cryptographic Provider v1.0"
```

## <a name="see-also"></a>関連項目

- [ClickOnce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)
- [チュートリアル: ClickOnce アプリケーションを手動で配置する](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)
- [信頼されたアプリケーションの配置の概要](/visualstudio/deployment/trusted-application-deployment-overview)
- [MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
