---
title: F# をインストールする
description: さまざまな方法でF#をインストールする方法について説明します。
ms.date: 12/20/2019
ms.openlocfilehash: 302e04f7cf3271516dff88d9d5f18f620b6ede80
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346561"
---
# <a name="install-f"></a>F\# のインストール

環境に応じて、複数の方法で F# をインストールできます。

## <a name="install-f-with-visual-studio"></a>Visual Studio で F# をインストールする

1. [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)を初めてダウンロードする場合は、最初に Visual Studio インストーラーをインストールします。 インストーラーから適切なエディションの Visual Studio をインストールします。

   Visual Studio が既にインストールされている場合は、追加F#先のエディションの横にある [変更] を選択します。

2. [ワークロード] ページで、 **ASP.NET および web 開発**ワークロードを選択F#します。これには、ASP.NET Core プロジェクトの .net Core サポートが含まれています。

3. 右下隅にある **[変更]** を選択して、選択したすべてのものをインストールします。

   次に、[Visual Studio インストーラーでF# **起動**] を選択して、Visual Studio を開くことができます。

## <a name="install-f-with-visual-studio-code"></a>Visual Studio Code F#と共にインストールする

1. [Git](https://git-scm.com/download)がインストールされ、パスに使用できることを確認します。 コマンドプロンプトで `git --version` を入力し、 **enter**キーを押すと、正しくインストールされていることを確認できます。

2. [.NET Core SDK](https://dotnet.microsoft.com/download)と[Visual Studio Code](https://code.visualstudio.com)をインストールします。

3. 拡張機能のアイコンを選択し、"Ionide" を検索します。

   Visual Studio Code でのサポートにF#必要なプラグインは、 [Ionide fsharp.core](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp)だけです。 ただし、Ionide をインストールして、[偽](https://fake.build/)[の](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-FAKE)サポートと[Ionide](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-Paket)を取得し、[パケット](https://fsprojects.github.io/Paket/)のサポートを取得することもできます。 フェイクとパケットは、 F#プロジェクトをビルドし、依存関係を管理するための追加のコミュニティツールです。

## <a name="install-f-with-visual-studio-for-mac"></a>Visual Studio for Macで F# をインストールする

F# は、選択した構成に関係なく、[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)にデフォルトでインストールされます。

インストールが完了したら、 **[Visual Studio の開始]** を選択します。 MacOS で Finder を使用して Visual Studio を開くこともできます。

## <a name="install-f-on-a-build-server"></a>ビルドF#サーバーへのインストール

.Net Core または .net SDK を使用して .NET Framework を使用している場合は、単に .NET SDK をビルドサーバーにインストールする必要があります。 必要なものはすべて揃っています。

.NET Framework を使用していて、.NET SDK を使用して**いない**場合は、 [Visual Studio Build Tools SKU](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)を Windows Server にインストールする必要があります。 インストーラーで、 **[.net デスクトップビルドツール]** を選択し、インストーラーメニューの右側にある **F#コンパイラ**コンポーネントを選択します。
