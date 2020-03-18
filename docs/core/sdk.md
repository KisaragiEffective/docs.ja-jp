---
title: .NET Core SDK の概要
description: .NET Core プロジェクトの作成に使用するライブラリとツールのセットである .NET Core SDK について説明します。
ms.date: 07/31/2019
ms.technology: dotnet-cli
ms.openlocfilehash: c2723e0e28c889f91f79ea3c0b26aa38f69fb41c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157467"
---
# <a name="net-core-sdk-overview"></a>.NET Core SDK の概要

.NET Core SDK は一連のライブラリとツールであり、開発者はこれを利用して .NET Core のアプリケーションやライブラリを作成できます。 それには、アプリケーションのビルドと実行に使用される次のコンポーネントが含まれています。

- .NET Core CLI。
- .NET Core ライブラリとランタイム。
- `dotnet` [ドライバー](tools/index.md#driver)。

## <a name="acquiring-the-net-core-sdk"></a>.NET Core SDK を入手する

他のツールと同様に、最初にコンピューターにツールをインストールする必要があります。 シナリオに応じて、次のいずれかの方法で SDK をインストールできます。

- ネイティブ インストーラーを使う。
- インストール シェル スクリプトを使う。

開発者のコンピューターでは、ネイティブ インストーラーが利用されることが多いです。 SDK はサポートされるプラットフォームのネイティブ インストール メカニズムにより配信されます。たとえば、Ubuntu の場合は DEB パッケージ、Windows の場合は MSI バンドルです。 これらのインストーラーでは、インストール直後に、ユーザーが SDK を使うために必要な環境がインストールされて設定されます。 ただし、コンピューター上で管理者特権も必要になります。 インストールする SDK は、[.NET ダウンロード](https://dotnet.microsoft.com/download) ページで見つけることができます。

一方、インストール スクリプトでは、管理者特権は必要ありません。 ただし、いかなる前提条件もコンピューターにインストールされません。前提条件はすべてユーザーが手動でインストールする必要があります。 スクリプトは、通常、管理者特権なしでツールをインストールするとき、ビルド サーバーを設定するために利用されます (上の前提条件警告に注意してください)。 詳しくは、[インストール スクリプト リファレンス](tools/dotnet-install-script.md)に関する記事をご覧ください。 お使いの CI ビルド サーバーで SDK を設定する方法について関心がある場合は、「[継続的インテグレーション (CI) で .NET Core SDK とツールを使用する](tools/using-ci-with-cli.md)」をご覧ください。

既定では、SDK は "side-by-side" (SxS) 方式でインストールされます。つまり、複数のバージョンを 1 台のコンピューター上にいつでも共存させることができます。 CLI コマンドを実行したときにバージョンが選択される方法について詳しくは、「[使用する .NET Core のバージョンを選択する](versions/selection.md)」記事をご覧ください。

## <a name="see-also"></a>参照

- [.NET Core CLI の概要](tools/index.md)
- [.NET Core のバージョン管理の概要](versions/index.md)
- [.NET Core ランタイムと SDK を削除する方法](versions/remove-runtime-sdk-versions.md)
- [使用する .NET Core のバージョンを選択する](versions/selection.md)
