---
title: Windows 上の Visual Studio Tools for Docker
description: Visual Studio 2017 バージョン 15.7 以降で使用できる Docker ツールについて説明します。
ms.date: 02/15/2019
ms.custom: vs-dotnet
ms.openlocfilehash: 2b6fdc33f9cf850cf9e52fca4a1a9754cd412567
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68673879"
---
# <a name="use-docker-tools-in-visual-studio-2017-on-windows"></a>Windows 上の Visual Studio 2017 で Docker ツールを使用する

Visual Studio 2017 バージョン 15.7 以降に含まれている Docker ツールを使用する場合の開発者ワークフローは、Visual Studio Code と Docker CLI の使用と似ています (実際、同じ Docker CLI に基づいています) が、より簡単に始めることができ、プロセスが簡単であり、ビルド、実行、および構成タスクの生産性を高めることができます。 また、Visual Studio の通常の `F5` キーと `Ctrl+F5` キーを使用してコンテナーを実行およびデバッグすることもできます。 コンテナーがソリューション レベルで同じ `docker-compose.yml` ファイルに定義されている場合は、ソリューション全体をデバッグすることもできます。

## <a name="configure-your-local-environment"></a>ローカル環境を構成する

Docker for Windows の最新バージョンでは、次の参考ドキュメントで説明されているように、設定が簡単なので、Docker アプリケーションの開発がこれまでよりも簡単になりました。

> [!TIP]
> Docker for Windows のインストールの詳細については、(<https://docs.docker.com/docker-for-windows/>) を参照してください。

## <a name="docker-support-in-visual-studio-2017"></a>Visual Studio 2017 での Docker サポート

プロジェクトに追加できる Docker サポートには 2 つのレベルがあります。 ASP.NET Core プロジェクトでは、Docker サポートを有効にしてプロジェクトに `Dockerfile` ファイルを追加するだけです。 次のレベルはコンテナー オーケストレーションのサポートです。(まだ存在しない場合は) `Dockerfile` をプロジェクトに追加し、ソリューション レベルで `docker-compose.yml` ファイルを追加します。 Docker Compose によるコンテナー オーケストレーションのサポートは、Visual Studio 2017 バージョン 15.0 から 15.7 で既定で追加されています。 コンテナー オーケストレーションのサポートは、Visual Studio 2017 バージョン 15.8 以降のオプトイン機能です。 バージョン 15.8 以降では、Docker Compose と Service Fabric をサポートします。

**[追加] > [Docker のサポート]** と **[追加] > [コンテナー オーケストレーター サポート]** コマンドは、**ソリューション エクスプローラー**の ASP.NET Core プロジェクトのプロジェクト ノードの右クリック メニュー (またはコンテキスト メニュー) にあります。図 4-31 を参照してください。

![Visual Studio の Docker サポートの追加メニュー オプション](./media/add-docker-support-menu.png)

**図 4-31**。 Visual Studio 2017 プロジェクトに Docker サポートを追加する

### <a name="add-docker-support"></a>Docker サポートを追加する

既存の ASP.NET Core プロジェクトに Docker サポートを追加するには、**ソリューション エクスプローラー**で **[追加]**  >  **[Docker のサポート]** の順に選択します。 プロジェクトの作成中に Docker サポートを有効にするには、図 4-32 に示すように、 **[新しいプロジェクト]** ダイアログ ボックスで **[OK]** をクリックすると開く **[新しい ASP.NET Core Web アプリケーション]** ダイアログ ボックスで、 **[Docker サポートを有効にする]** を選択します。

![Visual Studio で新しい ASP.NET Core Web アプリの Docker サポートを有効にする](./media/enable-docker-support-visual-studio.png)

**図 4-32**。 Visual Studio 2017 でプロジェクトの作成中に Docker サポートを有効にする

Docker サポートを追加または有効にすると、Visual Studio によって *Dockerfile* ファイルがプロジェクトに追加されます。

> [!NOTE]
> 図 4-33 に示すように、ASP.NET プロジェクト (.NET Framework .NET Core プロジェクトではありません) 用のプロジェクトの作成中に Docker Compose のサポートを有効にすると、コンテナー オーケストレーションのサポートも追加されます。

![ASP.NET プロジェクトで Docker 構成のサポートを有効にする](media/enable-docker-compose-support.png)

**図 4-33**。 Visual Studio 2017 で ASP.NET プロジェクトに対する Docker Compose のサポートを有効にする

### <a name="add-container-orchestration-support"></a>コンテナー オーケストレーションのサポートを追加する

複数コンテナーのソリューションを構成する場合は、コンテナー オーケストレーションのサポートをプロジェクトに追加します。 これにより、コンテナーのグループ (ソリューション全体) が同じ *docker-compose.yml* ファイル内で定義されている場合、それらを同時に実行およびデバッグすることができます。

コンテナー オーケストレーションのサポートを追加するには、**ソリューション エクスプローラー**でソリューションまたはプロジェクトのノードを右クリックし、 **[追加] > [Container Orchestration Support]\(コンテナー オーケストレーション サポート\)** の順に選択します。 次に、 **[Docker Compose]** または **[Service Fabric]** を選択してコンテナーを管理します。

プロジェクトにコンテナー オーケストレーションのサポートを追加すると、図 4-34 に示すように、プロジェクトに Dockerfile が追加され、**ソリューション エクスプローラー**内のソリューションに **docker-compose** フォルダーが追加されるのを確認できます。

![Visual Studio のソリューション エクスプローラーの Docker ファイル](media/docker-support-solution-explorer.png)

**図 4-34**. Visual Studio 2017 のソリューション エクスプローラーの Docker ファイル

*docker-compose.yml* が既に存在する場合、Visual Studio により、構成コードの必要な行が単にそれに追加されます。

## <a name="configure-docker-tools"></a>Docker ツールを構成する

メイン メニューから **[ツール] > [オプション]** を選択し、 **[コンテナー ツール] > [設定]** を展開します。 コンテナー ツールの設定が表示されます。

![次が表示されている Visual Studio Docker ツールのオプション。[必要な Docker イメージをプロジェクト読み込み時に自動的にプルする]、[コンテナーをバックグラウンドで自動的に開始する]、[ソリューションを閉じる際に、コンテナーを自動的に強制終了します]、[localhost SSL 証明書を信頼するためのメッセージを表示しない]。](./media/visual-studio-docker-tools-options.png)

**図 4-35**. Docker ツールのオプション

これらのオプションの設定方法を判断する際に、次の表を参照してください。

| 名前 | 既定の設定 | 対象 | 説明 |
| -----|:---------------:|:----------:| ----------- |
| 必要な Docker イメージをプロジェクト読み込み時に自動的にプルする | オン | Docker Compose | プロジェクトを読み込むときのパフォーマンスを向上するために、Visual Studio ではバックグラウンドで Docker のプル操作が開始されます。そのため、コードを実行する準備ができたら、イメージは既にダウンロードされているかダウンロード中です。 プロジェクトを読み込んでコードを参照するだけの場合は、これをオフにして不要なコンテナー イメージをダウンロードしないようにすることができます。 |
| コンテナーをバックグラウンドで自動的に開始する | オン | Docker Compose | また、パフォーマンスを向上するために、Visual Studio では、ユーザーがコンテナーを作成して実行するときに備えてボリュームのマウントを使用してコンテナーが作成されます。 コンテナーを作成するタイミングを制御する場合は、これをオフにします。 |
| ソリューションを閉じる際に、コンテナーを自動的に強制終了します | オン | Docker Compose | ソリューションを閉じた後または Visual Studio を終了した後もソリューションのコンテナーを実行し続ける場合は、これをオフにします。 |
| localhost SSL 証明書を信頼するためのメッセージを表示しない | オフ | ASP.NET Core 2.2 プロジェクト | localhost SSL 証明書が信頼されていない場合、このチェックボックスがオンになっていない限り、プロジェクトを実行するたびに Visual Studio によって確認メッセージが表示されます。 |

> [!WARNING]
> localhost SSL 証明書が信頼されていない場合に、プロンプトが表示されないようにチェックボックスをオンにすると、HTTPS Web 要求はアプリまたはサービスの実行時に失敗する可能性があります。 その場合は、 **[メッセージを表示しない]** チェックボックスをオフにしてプロジェクトを実行し、プロンプトで信頼を示します。

> [!TIP]
> サービスの実装と、Visual Studio Tools for Docker の使用方法の詳細については、以下の記事を参照してください。
>
>ローカルの Docker コンテナーでのアプリのデバッグ: <https://docs.microsoft.com/azure/vs-azure-tools-docker-edit-and-refresh>
>
>Visual Studio を使用して ASP.NET Docker コンテナーをコンテナー レジストリにデプロイする: <https://docs.microsoft.com/azure/vs-azure-tools-docker-hosting-web-apps-in-docker>

>[!div class="step-by-step"]
>[前へ](docker-apps-inner-loop-workflow.md)
>[次へ](set-up-windows-containers-with-powershell.md)
