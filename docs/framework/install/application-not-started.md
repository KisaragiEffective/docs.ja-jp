---
title: "\"このアプリケーションを開始できませんでした\" のトラブルシューティング"
description: "\"このアプリケーションを開始できませんでした\" のダイアログ ボックスが表示された場合の対処方法を説明します。"
ms.date: 09/05/2019
ms.openlocfilehash: 864c6ea23e9a048f060eee39d904bd4377be5084
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76965907"
---
# <a name="troubleshooting-a-this-application-could-not-be-started-error-message"></a>"このアプリケーションを開始できませんでした" のエラー メッセージのトラブルシューティング

.NET Framework 用に開発されたアプリケーションでは、通常、システムに特定のバージョンの .NET Framework がインストールされている必要があります。 場合によっては、インストールしたバージョンまたは必要なバージョンの .NET Framework が存在しない状態でアプリケーションを実行することがあります。 その結果、次のようなエラー ダイアログ ボックスが生成されることがよくあります。

![このアプリケーションを開始できませんでした。](media/application-not-started/app-could-not-be-started.png)

通常、これは次のいずれかの条件を示します。

- システムにインストールされている .NET Framework が破損しています。

- アプリケーションに必要な .NET Framework のバージョンを検出できません。

この問題を解決し、アプリケーションを実行できるようにするには、次の手順を実行します。

1. [.NET Framework 修復ツール (NetFxRepairTool.exe)](https://www.microsoft.com/download/details.aspx?id=30135) をダウンロードします。 ダウンロードが完了すると、ツールが自動的に実行されます。

1. .NET Framework 修復ツールで、次の図に示すような追加のアクションが推奨される場合は、 **[次へ]** を選択します。

   ![推奨される変更](media/application-not-started/repair-tool-recommended-changes.png)

1. 次の図に示すように、変更が完了したことを示すダイアログ ボックスが .NET Framework 修復ツールに表示されます。 アプリケーションを再実行するときは、このダイアログ ボックスを開いたままにしておきます。 .NET Framework 修復ツールによって破損した .NET Framework インストールが特定され、修正された場合は成功です。

   ![推奨される変更](media/application-not-started/repair-tool-changes-complete.png)

1. アプリケーションが正常に実行された場合は、 **[完了]** ボタンを選択します。 それ以外の場合は、 **[次へ]** ボタンを選択します。

1. **[次へ]** ボタンを選択した場合、.NET Framework 修復ツールでは次のようなダイアログ ボックスが表示されます。 **[完了]** ボタンを選択して、診断情報を Microsoft に送信します。

   ![問題を解決できない](media/application-not-started/repair-tool-no-resolution.png)

1. それでもアプリケーションを実行できない場合は、次の表に示すように、使用しているバージョンの Windows でサポートされている .NET Framework の最新バージョンをインストールします。

   |Windows のバージョン|.NET framework のインストール|
   |---|---|
   |Windows 10 Anniversary Update 以降のバージョン|[.NET Framework 4.8 ランタイム](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows 10、Windows 10 November Update|[.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)|
   |Windows 8.1|[.NET Framework 4.8 ランタイム](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows 8|[.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)|
   |Windows 7 SP1|[.NET Framework 4.8 ランタイム](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows Vista SP2|[.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)|

   > [!NOTE]
   > .NET Framework 4.8 は、Windows 10 May 2019 Update にプレインストールされています。

1. アプリケーションを起動してみます。

1. 場合によっては、次のようなダイアログ ボックスが表示され、.NET Framework 3.5 をインストールするように求められます。 **[この機能をダウンロードしてインストールする]** を選択して .NET Framework 3.5 をインストールし、アプリケーションをもう一度起動します。

   ![問題を解決できない](media/application-not-started/install-3-5.png)

## <a name="see-also"></a>参照

- [.NET Framework のシステム要件](../get-started/system-requirements.md)
- [.NET Framework のインストール ガイド](index.md)
- [.NET Framework のインストールおよびアンインストールのブロックのトラブルシューティング](troubleshoot-blocked-installations-and-uninstallations.md)
