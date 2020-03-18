---
title: Windows 10、8.1、8 に .NET Framework 3.5 をインストールする
description: Windows 10、Windows 8.1、および Windows 8 に .NET Framework 3.5 をインストールする方法について説明します
ms.date: 07/16/2018
ms.openlocfilehash: cfe21c0821b8f3223301dcc802533e1aaf024a79
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76965946"
---
# <a name="install-the-net-framework-35-on-windows-10-windows-81-and-windows-8"></a>Windows 8、Windows 8.1、および Windows 10 への .NET Framework 3.5 のインストール

Windows 10、Windows 8.1、および Windows 8 上でアプリケーションを実行するために、.NET Framework 3.5 が必要になることがあります。 また、さらに古いバージョンの Windows にもこれらの手順を使用することもできます。

## <a name="download-the-offline-installer"></a>オフライン インストーラーをダウンロードする

.NET Framework 3.5 SP1 のオフラインイン ストーラーは、[.NET Framework 3.5 SP1 ダウンロード ページ](https://dotnet.microsoft.com/download/dotnet-framework/net35-sp1)から入手できます。これは、Windows 10 より前の Windows バージョンで使用できます。

## <a name="install-the-net-framework-35-on-demand"></a>必要に応じて .NET Framework 3.5 をインストールする

.NET Framework 3.5 が必要なアプリケーションを実行しようとすると、次の構成ダイアログが表示されることがあります。 .NET Framework 3.5 を有効にするには、 **[この機能をインストールする]** を選択します。 このオプションを使用するには、インターネット接続が必要です。

![.NET Framework インストール ダイアログのスクリーンショット。](./media/dotnet-35-windows-10/dotnet-framework-installation-dialog.png)

### <a name="why-am-i-getting-this-pop-up"></a>このポップアップが表示される理由

.NET Framework は、Microsoft によって作成されており、アプリケーションの実行環境を提供します。 異なるバージョンを利用できます。 多くの企業は .NET Framework を使用して実行するようにアプリを開発しており、それらのアプリでは特定のバージョンが対象になっています。 このポップアップが表示される場合は、実行しようとしているアプリケーションでは .NET Framework バージョン 3.5 が必要ですが、そのバージョンがシステムにインストールされていません。

## <a name="enable-the-net-framework-35-in-control-panel"></a>コントロール パネルで .NET Framework 3.5 を有効にする

Windows のコントロール パネルを使用して .NET Framework 3.5 を有効にできます。 このオプションを使用するには、インターネット接続が必要です。

1. Windows キー ![Windows キー ロゴのスクリーンショット](./media/dotnet-35-windows-10/windows-keyboard-logo.png) を押します。 キーボードで「Windows の機能」と入力し、Enter キーを押します。 **[Windows 機能の有効化または無効化]** ダイアログ ボックスが表示されます。

2. **[.NET Framework 3.5 (.NET 2.0 および 3.0 を含む)]** チェック ボックスをオンにして **[OK]** を選択し、メッセージが表示された場合はコンピューターを再起動します。

   ![[コントロール パネル] を使用した .NET のインストールを示すスクリーンショット。](./media/dotnet-35-windows-10/dotnet-control-panel.png)

   Windows Communication Foundation (WCF) 機能が必要な開発者またはサーバー管理者でない限り、 **[Windows Communication Foundation HTTP アクティブ化]** および **[Windows Communication Foundation 非 HTTP アクティブ化]** の子項目を選択する必要はありません。

## <a name="troubleshoot-the-installation-of-the-net-framework-35"></a>.NET Framework 3.5 のインストールのトラブルシューティング

インストール中にエラー 0x800f0906、0x800f0907、0x800f081f、0x800F0922 が発生することがあります。その場合は、「[.NET Framework 3.5 インストール エラー: 0x800f0906、0x800f0907、または 0x800f081f](https://support.microsoft.com/help/2734782/net-framework-3-5-installation-error-0x800f0906--0x800f081f--0x800f09)」を参照し、問題の解決方法をご確認ください。

インストールの問題をまだ解決できない場合、またはインターネット接続がない場合は、Windows のインストール メディアを使ってインストールしてみることができます。 詳細については、「[Deploy .NET Framework 3.5 by using Deployment Image Servicing and Management (DISM)](/windows-hardware/manufacture/desktop/deploy-net-framework-35-by-using-deployment-image-servicing-and-management--dism)」 (展開イメージのサービスと管理 (DISM) を利用して .NET Framework 3.5 を展開する) を参照してください。 インストール メディアがない場合は、「[Windows 用のインストール メディアを作成する](https://support.microsoft.com/help/15088/windows-create-installation-media)」を参照してください。

> [!WARNING]
> .NET Framework 3.5 のインストールのソースとして Windows Update に依存していない場合は、同じ対応する Windows オペレーティング システムのバージョンからのソースを厳密に使用していることを確認する必要があります。 同じバージョンの Windows に対応しないソース パスを使用しても、一致しないバージョンの .NET Framework 3.5 のインストールは中止されません。 ただし、これによりシステムはサポートされていない使用不能の状態になります。
