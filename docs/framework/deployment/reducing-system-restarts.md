---
title: .NET Framework 4.5 のインストール中のシステム再起動の削減
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework, reducing system restarts
- installing .NET Framework
- installation [.NET Framework]
ms.assetid: 7aa8cb72-dee9-4716-ac54-b17b9ae8218f
ms.openlocfilehash: 6261a883e7b99b7fd38da2a17ab4820c81552506
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716426"
---
# <a name="reducing-system-restarts-during-net-framework-45-installations"></a>.NET Framework 4.5 のインストール中のシステム再起動の削減
.NET Framework 4.5 インストーラーは[再起動マネージャー](/windows/win32/rstmgr/about-restart-manager)を使用して、インストール中のシステムの再起動を可能な限り回避します。 アプリケーションのセットアップ プログラムで .NET Framework をインストールする場合は、再起動マネージャーとやり取りしてこの機能を利用できます。 詳しくは、「[方法: .NET Framework 4.5 インストーラーの進行状況を表示する](how-to-get-progress-from-the-dotnet-installer.md)」をご覧ください。  
  
## <a name="reasons-for-a-restart"></a>再起動の理由  
 .NET Framework 4.5 のインストールでは、.NET Framework 4 アプリケーションがインストール中に使用されている場合は、システムの再起動が必要になります。 これは、.NET Framework 4.5 によって .NET Framework 4 のファイルが置き換えられ、インストール中にこれらのファイルが使用可能になっていることが必要とされるためです。 多くの場合、再起動は使用中の .NET Framework 4 アプリケーションをプリエンティブに検出し、終了することで回避できます。 ただし、一部のシステム アプリケーションは終了しないでください。 このような場合、再起動は回避できません。  
  
## <a name="end-user-experience"></a>エンド ユーザー エクスペリエンス  
 .NET Framework 4.5 の完全インストールを行っているエンド ユーザーには、使用中の .NET Framework 4 アプリケーションをインストーラーが検出した場合にシステムの再起動を回避する機会が与えられます。 メッセージには、実行中の .NET Framework 4 アプリケーションがすべて一覧表示され、インストール前にこれらのアプリケーションを終了するオプションが表示されます。 ユーザーが終了を確認すると、これらのアプリケーションはインストーラーによってシャットダウンされ、システムの再起動が回避されます。 一定時間内にユーザーがメッセージに応答しなかった場合、インストールはアプリケーションを終了せずに続行します。  
  
 実行中のアプリケーションが終了してもシステムの再起動が必要な状況を再起動マネージャーが検出した場合、メッセージは表示されません。  
  
 ![現在実行中のプログラムが一覧表示されるアプリケーション終了ダイアログ。](./media/reducing-system-restarts/close-application-dialog.png)  
  
## <a name="using-a-chained-installer"></a>チェーンされたインストーラーの使用  
 アプリケーションと共に .NET Framework を再配布し、ただし独自のセットアップ プログラムと UI を使用する場合は、.NET Framework のセットアップ プロセスをセットアップ プロセスにインクルード (チェーン) できます。 チェーンされたインストールについて詳しくは、「[配置ガイド (開発者向け)](deployment-guide-for-developers.md)」をご覧ください。 チェーンされたインストールでのシステムの再起動を減らすために、.NET Framework インストーラーは、終了するアプリケーションの一覧をセットアップ プログラムに提示します。 セットアップ プログラムは、メッセージ ボックスなどのユーザー インターフェイスを経由してこの情報をユーザーに提供し、ユーザーの応答を取得して、応答を .NET Framework インストーラーに渡す必要があります。 チェーンされたインストーラーの例については、「[方法: .NET Framework 4.5 インストーラーの進行状況を表示する](how-to-get-progress-from-the-dotnet-installer.md)」をご覧ください。  
  
 チェーンされたインストーラーを使用しているときに、終了するアプリケーションに対して独自のメッセージ ボックスを非表示にする場合は、.NET Framework セットアップ プロセスをチェーンするときに、コマンド ラインで `/showrmui` と `/passive` のオプションを使用できます。 これらのオプションを一緒に使用すると、システムの再起動を避けるために終了できるアプリケーションを閉じるためのメッセージ ボックスがインストーラーによって表示されます。 このメッセージ ボックスは、完全なユーザー インターフェイスの下でも、パッシブ モードでも同じように動作します。 .NET Framework 再頒布可能パッケージのコマンドライン オプションの完全なセットについては、「[配置ガイド (開発者向け)](deployment-guide-for-developers.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- [展開](index.md)
- [配置ガイド (開発者向け)](deployment-guide-for-developers.md)
- [How to: Get Progress from the .NET Framework 4.5 Installer](how-to-get-progress-from-the-dotnet-installer.md)
