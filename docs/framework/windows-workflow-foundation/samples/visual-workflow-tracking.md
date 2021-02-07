---
description: '詳細情報: ビジュアルワークフロー追跡'
title: ビジュアル ワークフロー追跡
ms.date: 03/30/2017
ms.assetid: 0143448f-2044-40a0-8a3d-941f6d12468b
ms.openlocfilehash: efa88186fe0c2d0b2e7324c2d859bed46cd4a4de
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755266"
---
# <a name="visual-workflow-tracking"></a>ビジュアル ワークフロー追跡

このサンプルでは、[!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] のデバッグ機能を使用してビジュアル ワークフロー追跡アプリケーションを作成する方法を示します。

## <a name="sample-details"></a>サンプルの詳細

 このアプリケーションは、Workflow.xaml に定義されている単純なフローチャート ワークフローを実行し、ワークフロー デザイナーを再ホストして現在実行中のワークフローを表示します。 ワークフローが実行されると、現在実行中のアクティビティが黄色の枠とデバッグ矢印で示されます。 さらに、ワークフローによって生成された追跡レコードもアプリケーション ウィンドウに表示されます。 ワークフロー追跡の詳細については、「 [ワークフローの追跡とトレース](../workflow-tracking-and-tracing.md)」を参照してください。 ワークフローデザイナーの再ホストの詳細については、「 [ワークフローデザイナーのホスト](../rehosting-the-workflow-designer.md)」を参照してください。

 このワークフロー シミュレーターは、2 つのディクショナリを保持することによって機能します。 一方のディクショナリには、現在実行中のアクティビティ オブジェクトと、そのアクティビティがインスタンス化された XAML の行番号とのマッピングが含まれています。 もう一方のディクショナリには、アクティビティ インスタンス ID とアクティビティ オブジェクトとのマッピングが含まれています。 カスタム追跡プロファイルを使用して追跡レコードが出力されるときには、現在実行中のアクティビティのインスタンス ID が特定されて、そのアクティビティをインスタンス化した XAML ファイルにマップされます。 その後、再ホストされたワークフロー デザイナーで、ワークフロー デバッガーと同じ方法を使用してそのアクティビティがデザイナー画面で強調表示されます (アクティビティが黄色の枠で囲まれ、デザイナーの左端に黄色の矢印が表示されます)。

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 のサンプルディレクトリから WorkflowSimulator .sln ファイルを開きます。

2. Ctrl + Shift + B キーを押して、ソリューションをビルドします。

3. Ctrl キーを押しながら F5 キーを押してサンプルを実行します。 再ホストされたワークフロー デザイナーのウィンドウに Workflow.xaml ファイルが表示されます。

4. [ **ファイル** ] メニューをクリックし、[ **ワークフローの実行**] を選択します。

5. 現在実行中のアクティビティが上述のように強調表示され、アプリケーション ウィンドウの右側に追跡レコードが表示されます。

6. ワークフローが完了したら、任意の追跡レコードをクリックして、どのアクティビティに対応しているのかを調べることができます。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) とサンプルをダウンロードして [!INCLUDE[wf1](../../../../includes/wf1-md.md)] ください。 このサンプルは、次のディレクトリに格納されます。  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Application\VisualWorkflowTracking`
