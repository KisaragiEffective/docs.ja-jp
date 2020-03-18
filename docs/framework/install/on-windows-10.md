---
title: Windows 10 への .NET Framework のインストール
description: Windows 10 または Windows Server 2016 に .NET Framework をインストールする方法について説明します。
ms.date: 04/18/2019
ms.custom: updateeachrelease
ms.openlocfilehash: eed15b9088d6ba46d8f5bc6d16ba779dd6115b0d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76965972"
---
# <a name="install-the-net-framework-on-windows-10-and-windows-server-2016-and-later"></a>Windows 10 と Windows Server 2016 以降に .NET Framework をインストールする

.NET Framework は、Windows でさまざまなアプリケーションを実行するために必要です。 この記事の指示は、必要なバージョンの .NET Framework をインストールする際に役立ちます。 最新版は [.NET Framework 4.8](https://github.com/Microsoft/dotnet/tree/master/releases/net48) です。

このページをご覧になっている理由は、アプリケーションを実行しようとして次のようなダイアログがコンピューターに表示されたからではないでしょうか。

![このアプリケーションを開始できませんでした。](./media/this-application-could-not-be-started.png)

## <a name="net-framework-48"></a>.NET Framework 4.8

.NET Framework 4.8 は次の Windows に付属しています。

- [Windows 10 May 2019 Update](https://support.microsoft.com/help/4028685/windows-10-get-the-update)

> [!div class="button"]
> [.NET Framework 4.8 のダウンロード](https://dotnet.microsoft.com/download/dotnet-framework/net48)

[.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) は、.NET Framework 4.0 から 4.7.2 用に構築されたアプリケーションを実行するために使用できます。

[.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) は次の Windows にインストールできます。

- Windows 10 October 2018 Update (バージョン 1809)
- Windows 10 April 2018 Update (バージョン 1803)
- Windows 10 Fall Creators Update (バージョン 1709)
- Windows 10 Creators Update (バージョン 1703)
- Windows 10 Anniversary Update (バージョン 1607)
- Windows Server 2019
- Windows Server、バージョン 1809
- Windows Server、バージョン 1803
- Windows Server 2016

.NET Framework 4.8 はサポートされていません。

- Windows 10 1507
- Windows 10 1511

Windows 10 1507 または 1511 を使用しているとき、.NET Framework 4.8 をインストールする場合、それらより新しい Windows 10 バージョンに先にアップグレードする必要があります。

## <a name="net-framework-462"></a>.NET Framework 4.6.2

[.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462) は Windows 10 1507 と 1511 でサポートされている最も新しい .NET Framework バージョンです。

.NET Framework 4.6.2 は、.NET Framework 4.0 から 4.6.2 用に構築されたアプリケーションをサポートします。

## <a name="net-framework-35"></a>.NET Framework 3.5

手順に従って [.NET Framework 3.5 を Windows 10 に](dotnet-35-windows-10.md)インストールしてください。

.NET Framework 3.5 は、.NET Framework 1.0 から 3.5 用に構築されたアプリケーションをサポートします。

## <a name="additional-information"></a>追加情報

.NET Framework 4.x バージョンは、前のバージョンのインプレース更新です。 これは次のことを意味します。

- お使いのコンピューターにインストールできる .NET Framework 4.x バージョンは 1 つだけです。

- 以降のバージョンが既にインストールされている場合、前のバージョンの .NET Framework をコンピューターにインストールすることはできません。

- .NET Framework 4.x バージョンは、4.0 からそのバージョンまでの .NET Framework 用に構築されたアプリケーションを実行するために使用できます。 たとえば、.NET Framework 4.7 は、.NET Framework 4.0 から 4.7 用に構築されたアプリケーションを実行するために使用できます。 最新版 (.NET Framework 4.8) は、4.0 以降のすべての .NET Framework バージョンで構築されたアプリケーションの実行に使用できます。

ダウンロード可能な .NET Framework バージョンの一覧は、[.NET ダウンロード](https://dotnet.microsoft.com/download) ページでご覧ください。

## <a name="help"></a>ヘルプ

正しいバージョンの .NET Framework をインストールできない場合、[Microsoft にお問い合わせください](mailto:dotnet-install-help@service.microsoft.com?subject=Install-Help)。

## <a name="see-also"></a>参照

- [.NET ダウンロード](https://dotnet.microsoft.com/download)
- [.NET Framework のインストールおよびアンインストールのブロックのトラブルシューティング](troubleshoot-blocked-installations-and-uninstallations.md)
- [開発者向けの .NET Framework のインストール](guide-for-developers.md)
