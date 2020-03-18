---
title: .NET Framework 配置ガイド (管理者向け)
ms.date: 04/10/2018
helpviewer_keywords:
- administrator's guide, deploying .NET Framework
- deployment [.NET Framework], administrator's guide
ms.assetid: bee14036-0436-44e8-89f5-4bc61317977a
ms.openlocfilehash: be15ce0b0bed37da6fe400e98bfdd118c48f7ba0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716536"
---
# <a name="net-framework-deployment-guide-for-administrators"></a>.NET Framework 配置ガイド (管理者向け)

この記事では、システム管理者が Microsoft Endpoint Configuration Manager を使用して .NET Framework 4.5 とそのシステムの依存関係をネットワーク経由で配置する方法を手順に沿って説明します。 ここでは、すべての対象のクライアント コンピューターが .NET Framework の最小要件を満たしていることを前提としています。 .NET Framework 4.5 のインストールに必要なソフトウェアとハードウェアの要件の一覧については、[システム要件](../get-started/system-requirements.md)に関する記事を参照してください。

> [!NOTE]
> .NET Framework 4.5、Configuration Manager、Active Directory など、このドキュメントで言及されるソフトウェアなどにはそれぞれ、ライセンス条項が適用されます。 このドキュメントの内容は、ライセンス条項がソフトウェアの適切なライセンス取得者によって確認され、同意されていることを前提にしています 記載の内容についても、ライセンス条項は効力があるものとします。
>
> .NET Framework のサポートの詳細については、Microsoft サポート オンラインの [.NET Framework の公式サポート ライフサイクル ポリシー](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)に関するページを参照してください。

このトピックには、次のセクションが含まれています。

- [配置プロセス](#the_deployment_process)
- [.NET Framework の配置](#deploying_in_a_test_environment)
- [コレクションの作成](#creating_a_collection)
- [パッケージとプログラムの作成](#creating_a_package)
- [配布ポイントの選択](#select_dist_point)
- [パッケージの配置](#deploying_package)
- [リソース](#resources)
- [トラブルシューティング](#troubleshooting)

<a name="the_deployment_process"></a>

## <a name="the-deployment-process"></a>配置プロセス

サポートするインフラストラクチャが整っている場合は、Configuration Manager を使用して、.NET Framework 再頒布可能パッケージをネットワーク上のコンピューターに配置します。 インフラストラクチャを構築するには、コレクション、ソフトウェアのパッケージとプログラム、配布ポイント、配置という 5 つの主要な項目を作成し定義する必要があります。

- **コレクション**は、ユーザー、ユーザー グループ、コンピューターなど、.NET Framework の配置先となる Configuration Manager リソースのグループです。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコレクションの概要](https://docs.microsoft.com/configmgr/core/clients/manage/collections/introduction-to-collections)」を参照してください。

- **パッケージとプログラム**は、通常、クライアント コンピューターにインストールされるソフトウェア アプリケーションを表しますが、個々のファイル、更新プログラム、さらには個々のコマンドが含まれることがあります。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のパッケージとプログラム](https://docs.microsoft.com/configmgr/apps/deploy-use/packages-and-programs)」を参照してください。

- **配布ポイント**は、ソフトウェアをクライアント コンピューター上で実行するために必要なファイルを格納する Configuration Manager のサイト システムの役割です。 Configuration Manager クライアントは、ソフトウェア配置を受け取って処理するときに、配布ポイントに接続してソフトウェアに関連するコンテンツをダウンロードし、インストール処理を開始します。 詳細については、Configuration Manager ドキュメント ライブラリの「[Configuration Manager でのコンテンツ管理の基本的な概念](https://docs.microsoft.com/configmgr/core/plan-design/hierarchy/fundamental-concepts-for-content-management)」を参照してください。

- **配置**は、指定されたターゲット コレクションの該当するメンバーにソフトウェア パッケージをインストールするよう指示します。

> [!IMPORTANT]
> このトピックの手順では、パッケージとプログラムを作成するための一般的な設定を使用していて、すべての可能な設定について説明していない場合があります。 その他の Configuration Manager の配置オプションについては、[Configuration Manager のドキュメント ライブラリ](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg682041%28v=technet.10%29)を参照してください。

<a name="deploying_in_a_test_environment"></a>

## <a name="deploying-the-net-framework"></a>.NET Framework の配置

Configuration Manager を使用して、インストール処理時にユーザーが操作しない NET Framework 4.5 のサイレント インストールを配置できます。 この場合は、以下の手順に従ってください。

1. [コレクションを作成します](#creating_a_collection)。

2. [.NET Framework 再頒布可能パッケージとプログラムを作成します](#creating_a_package)。

3. [配布ポイントを選択します](#select_dist_point)。

4. [パッケージを配置します](#deploying_package)。

<a name="creating_a_collection"></a>

### <a name="create-a-collection"></a>コレクションの作成

この手順では、パッケージとプログラムを配置するコンピューターを選択し、それをデバイス コレクションにグループ化します。 Configuration Manager でコレクションを作成するときは、ダイレクト メンバーシップ規則 (コレクション メンバーを手動で指定) またはクエリ規則 (指定した条件に基づいて Configuration Manager がコレクション メンバーを決定) を使用できます。 メンバーシップ規則の詳細については、クエリ規則とダイレクト規則も含めて、Configuration Manager ドキュメント ライブラリの「[Configuration Manager のコレクションの概要](https://docs.microsoft.com/configmgr/core/clients/manage/collections/introduction-to-collections)」を参照してください。

コレクションを作成するには

1. Configuration Manager コンソールで、 **[資産とコンプライアンス]** をクリックします。

2. **[資産とコンプライアンス]** ワークスペースで、 **[デバイス コレクション]** をクリックします。

3. **[ホーム]** タブの **[作成]** グループで、 **[デバイス コレクションの作成]** をクリックします。

4. **デバイス コレクションの作成ウィザード**の **[全般]** ページで、コレクションの名前を入力します。

5. **[参照]** をクリックして、限定するコレクションを指定します。

6. **[メンバーシップの規則]** ページで、 **[規則の追加]** をクリックし、 **[ダイレクト規則]** をクリックして**ダイレクト メンバーシップの規則の作成ウィザード**を開きます。 **[次へ]** を選択します。

7. **[リソースの検索]** ページで、 **[リソース クラス]** ボックスの一覧の **[システム リソース]** をクリックします。 **[属性名]** ボックスの一覧の **[名前]** をクリックします。 **[値]** フィールドに「`%`」と入力し、 **[次へ]** をクリックします。

8. **[リソースの選択]** ページで、.NET Framework を配置する各コンピューターのチェック ボックスをオンにします。 **[次へ]** をクリックして、ウィザードの操作を完了します。

9. **デバイス コレクションの作成ウィザード**の **[メンバーシップの規則]** ページで、 **[次へ]** をクリックし、ウィザードの処理を完了します。

<a name="creating_a_package"></a>

### <a name="create-a-package-and-program-for-the-net-framework-redistributable-package"></a>.NET Framework 再頒布可能パッケージとプログラムの作成

次の手順では、.NET Framework 再頒布可能パッケージを手動で作成します。 パッケージには、.NET Framework をインストールするためのパラメーター、およびターゲット コンピューターへのパッケージの配布元となる場所が含まれます。

パッケージを作成するには

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** をクリックします。

2. **[ソフトウェア ライブラリ]** ワークスペースで、 **[アプリケーション管理]** を展開し、 **[パッケージ]** をクリックします。

3. **[ホーム]** タブの **[作成]** グループで、 **[パッケージの作成]** をクリックします。

4. **パッケージとプログラムの作成ウィザード**の **[パッケージ]** ページで、次の情報を入力します。

    - 名前: `.NET Framework 4.5`

    - 製造元: `Microsoft`

    - 言語: `English (US)`

5. **[このパッケージにソース ファイルを含める]** チェック ボックスをオンにし、 **[参照]** をクリックして .NET Framework のインストール ファイルを含むローカルまたはネットワーク フォルダーを選択します。 フォルダーを選択したら、 **[OK]** をクリックし、 **[次へ]** をクリックします。

6. ウィザードの **[プログラミングの種類]** ページで、 **[標準プログラム]** をクリックし、 **[次へ]** をクリックします。

7. **パッケージとプログラムの作成ウィザード**の **[プログラム]** ページで、次の情報を入力します。

    1. **Name:** `.NET Framework 4.5`

    2. **コマンド ライン:** `dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage ADMINDEPLOYMENT` (コマンド ライン オプションの説明はこの手順の後の表にあります)

    3. **実行:** **[非表示]** をクリックします。

    4. **プログラムの実行条件:** ユーザーがログオンしているかどうかに関係なく、プログラムを実行できることを指定するオプションを選択します。

8. **[要件]** ページで、 **[次へ]** をクリックして既定値を受け入れ、ウィザードの処理を完了します。

次の表は、手順 7. で指定したコマンド ライン オプションについて説明しています。

|オプション|[説明]|
|------------|-----------------|
|**/q**|非表示モードを設定します。 ユーザー入力は必要なく、出力は表示されません。|
|**/norestart**|セットアップ プログラムが自動的に再起動しないようにします。 このオプションを使用する場合、Configuration Manager でコンピューターの再起動を処理する必要があります。|
|**/chainingpackage** *PackageName*|チェーンを行っているパッケージの名前を指定します。 この情報は、Microsoft カスタマー エクスペリエンス向上プログラム (CEIP) に申し込んだ場合のその他のインストール セッション情報と共に報告されます。 パッケージ名にスペースが含まれている場合は、区切り記号として二重引用符を使用します (例: **/chainingpackage "Chaining Product"** )。|

これらの手順によって、.NET Framework 4.5 という名前のパッケージが作成されます。 プログラムは、.NET Framework 4.5 のサイレント インストールを配置します。 サイレント インストールでは、ユーザーはインストール プロセスと対話しないので、チェーン アプリケーションがリターン コードをキャプチャし、再起動を処理する必要があります。「[Getting Progress Information from an Installation Package](https://docs.microsoft.com/previous-versions/cc825975(v=vs.100))」 (インストール パッケージからの進行状況に関する情報の取得) を参照してください。

<a name="select_dist_point"></a>

### <a name="select-a-distribution-point"></a>配布ポイントの選択

サーバーからクライアント コンピューターにパッケージとプログラムを配布するには、まずサイト システムを配布ポイントとして指定し、次にパッケージを配布ポイントに配布する必要があります。

以下の手順を使用して、前のセクションで作成した .NET Framework 4.5 パッケージの配布ポイントを選択します。

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** をクリックします。

2. **[ソフトウェア ライブラリ]** ワークスペースで、 **[アプリケーション管理]** を展開し、 **[パッケージ]** をクリックします。

3. パッケージの一覧で、前のセクションで作成したパッケージ **[.NET Framework 4.5]** を選択します。

4. **[ホーム]** タブの **[展開]** グループで、 **[コンテンツの配布]** をクリックします。

5. **コンテンツの配布ウィザード**の **[全般]** タブで、 **[次へ]** をクリックします。

6. ウィザードの **[コンテンツの配布先]** ページで、 **[追加]** をクリックし、 **[配布ポイント]** をクリックします。

7. **[配布ポイントの追加]** ダイアログ ボックスで、パッケージとプログラムをホストする配布ポイントを選択し、 **[OK]** をクリックします。

8. ウィザードを完了します。

これでパッケージには、.NET Framework 4.5 をサイレントで配置するために必要なすべての情報が含まれています。 パッケージとプログラムを配置する前に、そのパッケージが配布ポイントにインストールされていることを確認します。Configuration Manager ドキュメント ライブラリの「[Configuration Manager で配布するコンテンツを監視する](https://docs.microsoft.com/configmgr/core/servers/deploy/configure/monitor-content-you-have-distributed)」の「コンテンツのステータスの監視」セクションを参照してください。

<a name="deploying_package"></a>

### <a name="deploy-the-package"></a>パッケージをデプロイする

.NET Framework 4.5 パッケージとプログラムを配置するには

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** をクリックします。

2. **[ソフトウェア ライブラリ]** ワークスペースで、 **[アプリケーション管理]** を展開し、 **[パッケージ]** をクリックします。

3. パッケージの一覧で、作成したパッケージ **[.NET Framework 4.5]** を選択します。

4. **[ホーム]** タブの **[展開]** グループで、 **[展開]** をクリックします。

5. **ソフトウェアの展開ウィザード**の **[全般]** ページで、 **[参照]** をクリックし、前に作成したコレクションを選択します。 **[次へ]** を選択します。

6. ウィザードの **[コンテンツ]** ページで、ソフトウェアを配布するポイントが表示されていることを確認し、 **[次へ]** をクリックします。

7. ウィザードの **[展開設定]** ページで、 **[操作]** が **[インストール]** に設定され、 **[目的]** が **[必須]** に設定されていることを確認します。 これにより、ソフトウェア パッケージがターゲット コンピューターに対する必須インストールになることが保証されます。 **[次へ]** を選択します。

8. ウィザードの **[スケジュール]** ページで、.NET Framework をインストールする時期を指定します。 **[新規]** をクリックしてインストール時期を指定することも、ユーザーがログオンまたはログオフしたとき、またはできるだけ早い時期にインストールすることもできます。 **[次へ]** を選択します。

9. ウィザードの **[ユーザー側の表示と操作]** ページで、既定値を使用し、 **[次へ]** をクリックします。

    > [!WARNING]
    > 稼動環境では、配置スケジュールで異なる選択が必要になるポリシーが適用されている場合があります。 これらのオプションについて詳しくは、「[[提供情報の名前のプロパティ]の[スケジュール]タブ](https://docs.microsoft.com/previous-versions/system-center/configuration-manager-2007/bb694016%28v=technet.10%29)」をご覧ください。

10. ウィザードの **[配布ポイント]** ページで、既定値を使用し、 **[次へ]** をクリックします。

11. ウィザードを完了します。 **[監視]** ワークスペースの **[展開]** ノードで配置の進行状況を監視できます。

これで、対象のコレクションにパッケージが配置され、.NET Framework 4.5 のサイレント インストールが開始されます。 .NET Framework 4.5 のインストールのエラー コードについては、このトピックの「[リターン コード](#return_codes)」セクションを参照してください。

<a name="resources"></a>

## <a name="resources"></a>リソース

.NET Framework 4.5 再頒布可能パッケージの配置をテストするためのインフラストラクチャの詳細については、次のリソースを参照してください。

**Active Directory、DNS、DHCP:**

- [Active Directory Domain Services](/windows/desktop/ad/active-directory-domain-services)

- [ドメイン ネーム システム (DNS)](/windows-server/networking/dns/dns-top)

- [動的ホスト構成プロトコル (DHCP)](/windows-server/networking/technologies/dhcp/dhcp-top)

**SQL Server 2008:**

- [SQL Server 2008 のインストール (SQL Server ビデオ)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/dd299415(v=sql.100))

- [SQL Server 2008 のデータベース管理者向けセキュリティ概要](https://download.microsoft.com/download/a/c/d/acd8e043-d69b-4f09-bc9e-4168b65aaa71/SQL2008SecurityOverviewforAdmins.docx)

**System Center 2012 Configuration Manager (管理ポイント、配布ポイント):**

- [System Center 2012 Configuration Manager のサイト管理](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg681983%28v=technet.10%29)

- [Configuration Manager の単一サイトの計画と展開](https://docs.microsoft.com/previous-versions/system-center/configuration-manager-2007/bb680961%28v=technet.10%29)

**Windows コンピューター用の System Center 2012 Configuration Manager クライアント:**

- [System Center 2012 Configuration Manager のクライアントの展開](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg699391%28v=technet.10%29)

<a name="troubleshooting"></a>

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="log-file-locations"></a>ログ ファイルの場所

.NET Framework のセットアップ中に次のログ ファイルが作成されます。

- %temp%\Microsoft .NET Framework <*バージョン*>\*.txt
- %temp%\Microsoft .NET Framework <*バージョン*>\*.html

<*バージョン*> は、インストールしている .NET Framework のバージョンです (4.5 や 4.7.2 など)。

.NET Framework インストール コマンドの `/log` コマンド ライン オプションを使用して、ログ ファイルが書き込まれるディレクトリを指定することもできます。 詳しくは、「[.NET Framework 配置ガイド (開発者向け)](deployment-guide-for-developers.md#command-line-options)」をご覧ください。

[ログ収集ツール](https://www.microsoft.com/download/details.aspx?id=12493)を使用して .NET Framework のログ ファイルを収集し、それらのファイルのサイズを縮小した圧縮キャビネット (.cab) ファイルを作成できます。

<a name="return_codes"></a>

### <a name="return-codes"></a>リターン コード

次の表は、.NET Framework 4.5 の再頒布可能インストール プログラムから返される最も一般的なリターン コードを示しています。 これらのリターン コードは、すべてのバージョンのインストーラーで共通です。

詳細情報へのリンクについては、次の「[ダウンロードのエラー コード](#additional_error_codes)」セクションを参照してください。

|リターン コード|[説明]|
|-----------------|-----------------|
|0|インストールは正常に終了しました。|
|1602|ユーザーがインストールをキャンセルしました。|
|1603|インストール中に致命的なエラーが発生しました。|
|1641|インストールを完了するには再起動する必要があります。 このメッセージが表示された場合、操作は成功しました。|
|3010|インストールを完了するには再起動する必要があります。 このメッセージが表示された場合、操作は成功しました。|
|5100|ユーザーのコンピューターは、システム要件を満たしていません。|

<a name="additional_error_codes"></a>

### <a name="download-error-codes"></a>ダウンロードのエラー コード

- [Background Intelligent Transfer Service (BITS) のエラー コード](/windows/desktop/Bits/bits-return-values)

- [URL モニカーのエラー コード](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms775145%28v=vs.85%29)

- [WinHttp エラー コード](/windows/desktop/WinHttp/error-messages)

その他のエラー コード:

- [Windows インストーラーのエラー コード](/windows/desktop/msi/error-codes)

- [Windows Update エージェントの結果コード](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc720442(v=ws.10))

## <a name="see-also"></a>参照

- [配置ガイド (開発者向け)](deployment-guide-for-developers.md)
- [システム要件](../get-started/system-requirements.md)
