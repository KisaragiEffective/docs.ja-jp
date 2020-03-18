---
title: デバッグのワークフロー
ms.date: 03/30/2017
ms.assetid: b23b4814-ebb1-4c51-b7a9-469f4da7a96d
ms.openlocfilehash: 2bfc50215697636f1771d6bb35510fbf9e0b435d
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802636"
---
# <a name="debugging-workflows"></a>デバッグのワークフロー

[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] には、開発環境から実行中のワークフローをデバッグするオプションがいくつかあります。 ワークフローは、デザイナー、XAML、およびコードでデバッグできます。

## <a name="debugging-in-the-workflow-designer"></a>ワークフロー デザイナーでのデバッグ

ワークフローデザイナーのアクティビティにブレークポイントを設定するには、アクティビティを強調表示し、 <kbd>F9</kbd>キーを押すか、アクティビティのコンテキストメニューを使用します。 ワークフロー ホストをデバッグ モードで実行すると、ワークフローの実行は一時停止します。 次のスクリーンショットでは、ワークフローの実行はブレークポイントで一時停止します。 詳細については、「[ワークフローデザイナーを使用したワークフローのデバッグ](/visualstudio/workflow-designer/debugging-workflows-with-the-workflow-designer)」を参照してください。

## <a name="debugging-in-xaml"></a>XAML でのデバッグ

デザイナーでワークフローがブレークポイントで一時停止すると、XAML でもワークフローをデバッグできます。 XAML での実行ポイントを表示するには、ワークフローの実行が一時停止されているときに、ワークフローデザイナーで **[Xaml ビュー]** を選択します。 デバッグをデザイナーに切り替えるには、ソリューション エクスプローラーからデザイナーでワークフローを開き直します。 詳細については、「[方法: ワークフローデザイナーを使用](/visualstudio/workflow-designer/how-to-debug-xaml-with-the-workflow-designer)して XAML をデバッグする」を参照してください。

## <a name="debugging-in-code"></a>コードでのデバッグ

ブレークポイントを設定するには、コードペインの左余白をクリックするか、カーソルを設定する行にカーソルを合わせて<kbd>F9</kbd>キーを押します。

## <a name="attaching-to-a-workflow-process"></a>ワークフロー プロセスへのアタッチ

ワークフローのデバッグは、Visual Studio のインフラストラクチャを使用したプロセスへのアタッチもサポートしています。 そのため、ワークフロー作成者は、Internet Information Services (IIS) 7.0 など異なるホスト環境で実行されているワークフローをデバッグできます。

## <a name="remote-debugging"></a>Remote Debugging

Windows Workflow Foundation (WF) リモートデバッグは、他の Visual Studio コンポーネントのリモートデバッグと同じように機能します。 リモートデバッグの使用方法の詳細については、「[方法: リモートデバッグを有効](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/febz73k0(v=vs.100))にする」を参照してください。

> [!NOTE]
> ワークフローアプリケーションが x86 アーキテクチャを対象としていて、64ビットオペレーティングシステムを実行しているコンピューターでホストされている場合、リモートデバッグは、Visual Studio がリモートコンピューターにインストールされていない場合、またはワークフローアプリケーションのターゲットが**ANY CPU**に変更されている場合を除き、機能しません。

## <a name="extending-the-workflow-debugging-service"></a>ワークフロー デバッグ サービスの拡張

ワークフロー デバッガー サービスは公開されるようになり、ホストを変更したデザイナーでのモニタリング、シミュレーション、デバッグなど、カスタム アプリケーションを作成するときに使用できます。 詳細については、「<xref:System.Activities.Presentation.Debug.DebuggerService>」の記事を参照してください。
