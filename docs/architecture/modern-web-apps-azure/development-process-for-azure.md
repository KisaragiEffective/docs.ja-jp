---
title: Azure の開発プロセス
description: ASP.NET Core および Azure での最新の Web アプリケーションの設計 | Azure の開発プロセス
author: ardalis
ms.author: wiwagn
ms.date: 01/30/2019
ms.openlocfilehash: 7a641c1b6665af6e9e78ef182174b360041d74aa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77450045"
---
# <a name="development-process-for-azure"></a>Azure の開発プロセス

> _"クラウドを使用すれば、個人や小企業は指をパチンと鳴らすだけでエンタープライズ クラスのサービスをすぐにセットアップできます。"_  
> _- Roy Stephan_

## <a name="vision"></a>ビジョン

> *Visual Studio または dotnet CLI および Visual Studio Code または任意のエディターを使用して、好きな方法で適切に設計された ASP .NET Core アプリケーションを開発します。*

## <a name="development-environment-for-aspnet-core-apps"></a>ASP.NET Core アプリの開発環境

### <a name="development-tools-choices-ide-or-editor"></a>開発ツールの選択: IDE またはエディター

完全で強力な IDE または軽量でアジャイルなエディターのどちらを選んでも、Microsoft は ASP.NET Core アプリケーションの開発に対応できます。

**Visual Studio 2019。** Visual Studio 2019 は ASP.NET Core 用のアプリケーションを開発するためのクラス最高の IDE です。 開発者の生産性を向上させる多数の機能が用意されています。 それを使用してアプリケーションを開発し、その後、パフォーマンスやその他の特性を分析できます。 統合デバッガーを使用して、コードの実行を一時停止し、実行中のコードを前後にステップ実行できます。 組み込みのテスト ランナーを使用して、テストとその結果を整理したり、コーディング中にライブ単体テストを実行したりできます。 Live Share を使用して、他の開発者とリアルタイムで共同作業を行い、ネットワーク経由でコード セッションをシームレスに共有できます。 準備が完了すると、アプリケーションを Azure またはそれをホストする任意の場所に発行するために必要なすべてのものが Visual Studio 内に用意されます。

[Visual Studio 2019 のダウンロード](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)

**Visual Studio Code と dotnet CLI** (Mac、Linux および Windows 用のクロスプラット フォーム ツール)。 任意の開発言語をサポートする軽量なクロスプラットフォーム エディターを選択すると、Microsoft Visual Studio Code と dotnet CLI を使用することができます。 これらの製品は、開発者のワークフローを効率化する、簡単かつ堅牢性の高いエクスペリエンスを提供します。 さらに、Visual Studio Code は C\# および Web 開発の拡張機能をサポートし、エディター内での IntelliSense およびショートカット タスクを提供します。

[.NET Core SDK をダウンロードする](https://dotnet.microsoft.com/download)

[Visual Studio Code をダウンロードする](https://code.visualstudio.com/download)

## <a name="development-workflow-for-azure-hosted-aspnet-core-apps"></a>Azure でホストされる ASP.NET Core アプリの開発ワークフロー

アプリケーション開発のライフサイクルは、各開発者のコンピューターで開始されます。その場合、アプリは開発者の好みの言語でコーディングされ、ローカルでテストされます。 開発者は好みのソース管理システムを選択できます。また、ビルド サーバーを使用するか、組み込みの Azure 機能に基づいて、継続的インテグレーション (CI) および/または継続的デリバリー/デプロイ (CD) を構成できます。

CI/CD を使用して ASP.NET Core アプリケーションの開発を開始する場合は、Azure DevOps Services または組織独自の Team Foundation Server (TFS) を使用することができます。

### <a name="initial-setup"></a>初期セットアップ

アプリのリリース パイプラインを作成するには、ソース管理でアプリケーション コードを使用する必要があります。 ローカル リポジトリをセットアップし、それをチーム プロジェクトのリモート リポジトリに接続します。 次の手順に従ってください。

- [Git と Visual Studio でコードを共有する](https://docs.microsoft.com/azure/devops/git/share-your-code-in-git-vs)、または

- [TFVC と Visual Studio でコードを共有する](https://docs.microsoft.com/azure/devops/tfvc/share-your-code-in-tfvc-vs)

アプリケーションをデプロイする Azure App Service を作成します。 Azure Portal の App Services ブレードに移動して、Web アプリを作成します。 [+追加] をクリックして Web アプリ テンプレートを選択し、[作成] をクリックして名前とその他の詳細を入力します。 Web アプリは {name}.azurewebsites.net からアクセス可能になります。

![AzureWebApp](./media/image10-2.png)

**図 10-1** Azure Portal での新しい Azure App Service Web アプリの作成。

CI ビルド プロセスでは、新しいコードがプロジェクトのソース管理リポジトリにコミットされるたびに自動ビルドが実行されます。 これにより、コードがビルドし (理想的には、自動テストに合格し)、デプロイされる可能性のあるフィードバックがすぐに返されます。 この CI ビルドでは Web デプロイ パッケージ成果物が生成され、CD プロセスで使用するために公開されます。

[CI ビルド プロセスを定義する](https://docs.microsoft.com/azure/devops/build-release/apps/aspnet/build-aspnet-core#ci)

自分のチームのメンバーが新しいコードをコミットするたびにシステムによってビルドがキューに入れられるように、必ず、継続的インテグレーションを有効にしてください。 ビルドをテストし、成果物の 1 つとして Web デプロイ パッケージが生成されることを確認します。

ビルドに成功すると、CD プロセスでは CI ビルドの結果が Azure Web アプリにデプロイされます。 これを構成するには、*リリース*を作成して構成します。これは、Azure App Service にデプロイされます。

[CD リリース プロセスを定義する](https://docs.microsoft.com/azure/devops/build-release/apps/aspnet/build-aspnet-core#cd)

CI/CD パイプラインが構成されたら、Web アプリを更新して、それをソース管理にコミットしてデプロイするだけです。

### <a name="workflow-for-developing-azure-hosted-aspnet-core-applications"></a>Azure でホストされる ASP.NET Core アプリケーションの開発ワークフロー

Azure アカウントと CI/CD プロセスを構成したら、Azure でホストされる ASP.NET Core アプリケーションの開発は簡単にできます。 以下の図 10-2 に示されているように、Web アプリとして Azure App Service でホストされる、ASP.NET Core アプリを構築する場合に通常行う基本的な手順があります。

![EndToEndDevDeployWorkflow](./media/image10-3.png)

**図 10-2** ASP.NET Core アプリを構築して Azure でホストするステップ バイ ステップのワークフロー

#### <a name="step-1-local-dev-environment-inner-loop"></a>手順 1. ローカル開発環境の内側のループ

Azure にデプロイする場合の ASP.NET Core アプリケーションの開発と、それ以外の場合のアプリケーションの開発は同じです。 使い慣れたローカル開発環境を使用します。つまり、Visual Studio 2017 または dotnet CLI および Visual Studio Code または好みのエディターを使用できます。 コードを記述し、変更を実行してデバッグし、自動テストを実行し、変更を共有ソース管理リポジトリにプッシュできるようになるまで、ソース管理へのローカル コミットを行うことができます。

#### <a name="step-2-application-code-repository"></a>手順 2. アプリケーション コード リポジトリ

チームでコードを共有できるようになった場合は、常にローカル ソース リポジトリからチームの共有ソース リポジトリに変更をプッシュする必要があります。 カスタム ブランチで作業をしていた場合、この手順では通常、(たとえば、[pull request](https://docs.microsoft.com/azure/devops/git/pull-requests)を使用して) コードを共有ブランチにマージします。

#### <a name="step-3-build-server-continuous-integration-build-test-package"></a>手順 3. ビルド サーバー: 継続的インテグレーション。 ビルド、テスト、パッケージ

共有アプリケーション コード リポジトリに新しいコミットが行われるたびに、ビルド サーバーで新しいビルドがトリガーされます。 CI プロセスの一部として、このビルドで完全にアプリケーションをコンパイルし、自動テストを実行して、すべて予期したとおりに動作していることを確認する必要があります。 CI プロセスの最終結果は、デプロイの準備ができている、パッケージ化されたバージョンの Web アプリである必要があります。

#### <a name="step-4-build-server-continuous-delivery"></a>手順 4. ビルド サーバー: 継続的デリバリー

ビルドが成功すると、CD プロセスは生成されたビルド成果物を選択します。 これには、Web デプロイ パッケージが含まれます。 ビルド サーバーは Azure App Service にこのパッケージをデプロイして、既存のサービスを新しく作成されたものに置き換えます。 通常、この手順の対象はステージング環境ですが、一部のアプリケーションは CD プロセスを通じて運用環境に直接デプロイします。

#### <a name="step-5-azure-app-service-web-app"></a>手順 5. Azure App Service Web アプリ

デプロイ後、ASP.NET Core アプリケーションは Azure App Service Web アプリのコンテキスト内で実行されます。 この Web アプリは、Azure Portal を使用して監視でき、さらに構成することができます。

#### <a name="step-6-production-monitoring-and-diagnostics"></a>手順 6. 運用の監視と診断

Web アプリの実行中に、アプリケーションの正常性を監視し、診断およびユーザー動作のデータを収集することができます。 Application Insights は Visual Studio に含まれており、ASP.NET アプリの自動実装を提供します。 使用状況、例外、要求、パフォーマンス、およびログに関する情報を提供できます。

## <a name="references"></a>References

**ASP.NET Core アプリを構築して Azure にデプロイする**  
<https://docs.microsoft.com/azure/devops/build-release/apps/aspnet/build-aspnet-core>

>[!div class="step-by-step"]
>[前へ](test-asp-net-core-mvc-apps.md)
>[次へ](azure-hosting-recommendations-for-asp-net-web-apps.md)
