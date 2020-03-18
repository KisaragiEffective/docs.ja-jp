---
title: Windows コンテナーを展開しない状況
description: Azure Cloud と Windows コンテナーで既存の .NET アプリケーションを最新化する | Windows コンテナーをデプロイしない状況
ms.date: 04/28/2018
ms.openlocfilehash: 65e793b846b495e9a1be6db9ddfa38bbf0d49445
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69577955"
---
# <a name="when-not-to-deploy-to-windows-containers"></a>Windows コンテナーを展開しない状況

一部の Windows テクノロジは、Windows コンテナーでサポートされていません。 そのような場合でも、通常は Windows と IIS だけを使用して標準 VM に移行する必要があります。

Windows コンテナーでサポートされていないケース (2018 年 5 月):

- Microsoft メッセージ キュー (MSMQ) は現在のところ、Windows Server v1803 リリースをベースとする Windows コンテナーでのみ利用できます。それ以前のリリースでは利用できません。

  - [UserVoice リクエスト フォーラム](https://windowsserver.uservoice.com/forums/304624-containers/suggestions/15719031-create-base-container-image-with-msmq-server)

  - [ディスカッション フォーラム](https://social.msdn.microsoft.com/Forums/bce99a7d-aa60-44fa-a348-450855650810/msmqserver-is-it-supported?forum=windowscontainers)

- Microsoft 分散トランザクション コーディネーター (MSDTC) は現在のところ、Windows コンテナーでサポートされていません。

  - [GitHub の問題](https://github.com/MicrosoftDocs/Virtualization-Documentation/issues/494)

- Microsoft Office では現在、コンテナーがサポートされていません。

  - [UserVoice リクエスト フォーラム](https://windowsserver.uservoice.com/forums/304624-containers/suggestions/19686220-provide-office-support-for-containers)

- UI アプリ (ビジュアル ユーザー インターフェイスのあるクライアント アプリ) は、サポートされていないシナリオです。

- Windows インフラストラクチャの役割 (DNS、DHCP、DC、NTP、PRINT、ファイル サーバー、IAM など) は、サポートされていないシナリオです。

他の未サポート シナリオやコミュニティからのリクエストについては、Windows コンテナーの UserVoice フォーラム <https://windowsserver.uservoice.com/forums/304624-containers> を参照してください。

### <a name="additional-resources"></a>その他のリソース

- **Azure 内の仮想マシンとコンテナー**

    <https://azure.microsoft.com/overview/containers/>

> [!div class="step-by-step"]
> [前へ](deploy-existing-net-apps-as-windows-containers.md)
> [次へ](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
