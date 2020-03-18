---
title: Docker ベースのアプリケーションの開発プロセス
description: Docker ベースのアプリケーションの開発のオプションに関する概要を確認します。 マルチ プラットフォームのサポート (Windows、macOS、Linux) のため、Windows 用 Visual Studio、Visual Studio for Mac、または Visual Studio Code のうち好みのものを使います。
ms.date: 01/30/2020
ms.openlocfilehash: 799aa6fc742a8fb763ec5a7ae3cf3f70f89bed6d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77502719"
---
# <a name="development-process-for-docker-based-applications"></a>Docker ベースのアプリケーションの開発プロセス

*Visual Studio と Visual Studio Tools for Docker を使用した統合開発環境 (IDE)、または Docker CLI と Visual Studio Code を使用した CLI/エディターのお好きな方法でコンテナー化された .NET アプリケーションを開発します。*

## <a name="development-environment-for-docker-apps"></a>Docker アプリの開発環境

### <a name="development-tool-choices-ide-or-editor"></a>開発ツールの選択: IDE またはエディター

完全で強力な IDE または軽量でアジャイルなエディターのどちらを選んでも、Microsoft では Docker アプリケーションの開発に使用できるツールが用意されています。

**Visual Studio (Windows 版)。** Visual Studio での Docker ベースの .NET Core 3.1 アプリケーションの開発には、Visual Studio 2019 バージョン 16.4 以降が必要です。 Visual Studio 2019 には、既に Docker 用のツールが組み込まれています。 Tools for Docker を使用して、ターゲットの Docker 環境で直接アプリケーションの開発、実行、および検証ができます。 F5 キーを押すと、Docker ホスト内で直接アプリケーション (1 つのコンテナー、または複数のコンテナー) を実行してデバックできます。または、CTRL キーを押しながら F5 キーを押すと、コンテナーを再構築しなくても、アプリケーションを編集して更新できます。 これは、Docker ベース アプリを開発する場合の最も強力な選択肢です。

**Visual Studio for Mac。** これは macOS で動作する、Xamarin Studio を進化させた IDE です。 .NET Core 3.1 で開発する場合、バージョン 8.4 以降が必要です。 これは、強力な IDE を使用したい macOS コンピューターで作業する開発者にとって好ましい選択肢です。

**Visual Studio Code と Docker CLI**。 すべての開発言語をサポートする軽量なクロスプラット フォーム エディターがよい場合は、Visual Studio Code と Docker CLI を使用してください。 これは、macOS、Linux、Windows 向けのクロスプラット フォーム開発アプローチです。 さらに、Visual Studio Code は、Dockerfiles の IntelliSense などの Docker の拡張機能や、エディターから Docker コマンドを実行するショートカット タスクをサポートします。

[Docker Desktop Community Edition (CE)](https://hub.docker.com/search/?type=edition&offering=community) をインストールすることで、1 つの Docker CLI を使用して Windows と Linux の両方のアプリを構築することができます。

### <a name="additional-resources"></a>その他のリソース

- **Visual Studio**. 公式サイト。 \
  [https://visualstudio.microsoft.com/vs/](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)

- **Visual Studio Code**。 公式サイト。 \
  <https://code.visualstudio.com/download>

- **Docker Desktop for Windows Community Edition (CE)**  \
  [https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

- **Docker Desktop for Mac Community Edition (CE)**  \
  [https://hub.docker.com/editions/community/docker-ce-desktop-mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

## <a name="net-languages-and-frameworks-for-docker-containers"></a>.NET 言語および Docker コンテナーのフレームワーク

このガイドの前のセクションで触れたように、Docker コンテナー化された NET アプリケーションを開発する際に、.NET Framework、.NET Core、またはオープン ソースの Mono プロジェクトを使用できます。 Linux または Windows コンテナーをターゲットにする場合、使用している .NET Framework に応じて、C\#、F\#、または Visual Basic で開発できます。 .NET 言語の詳細については、ブログ投稿「[The .NET Language Strategy](https://devblogs.microsoft.com/dotnet/the-net-language-strategy/)」 (.NET 言語戦略) を参照してください。

>[!div class="step-by-step"]
>[前へ](../architect-microservice-container-applications/scalable-available-multi-container-microservice-applications.md)
>[次へ](docker-app-development-workflow.md)
