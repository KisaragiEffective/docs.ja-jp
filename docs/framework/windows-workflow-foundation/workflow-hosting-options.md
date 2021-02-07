---
description: 詳細については、「ワークフローホスティングオプション」を参照してください。
title: ワークフロー ホスティングのオプション
ms.date: 03/30/2017
ms.assetid: 37bcd668-9c5c-4e7c-81da-a1f1b3a16514
ms.openlocfilehash: 64d02decd3a096b4c83555729826c5fc07b13cbf
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99754889"
---
# <a name="workflow-hosting-options"></a>ワークフロー ホスティングのオプション

ほとんどの Windows Workflow Foundation (WF) のサンプルでは、コンソールアプリケーションでホストされているワークフローを使用しますが、これは実際のワークフローにとって現実的なシナリオではありません。 実際のビジネスアプリケーションのワークフローは、開発者が作成した Windows サービスまたは IIS 7.0 や AppFabric などのサーバーアプリケーションのいずれかの永続的なプロセスでホストされます。 これらの方法の違いを次に示します。

## <a name="hosting-workflows-in-iis-with-windows-appfabric"></a>Windows AppFabric を使用した IIS でのワークフローのホスト

IIS と AppFabric は、ワークフローに推奨されるホストです。 AppFabric を使用したワークフローのホスト アプリケーションは Windows プロセス アクティブ化サービスで、これは IIS を介した HTTP への依存関係のみを解消します。

## <a name="hosting-workflows-in-iis-alone"></a>IIS のみでのワークフローのホスト

実行中のアプリケーションのメンテナンスを容易にする AppFabric で使用できる管理ツールと監視ツールがあるため、IIS 7.0 だけを使用することはお勧めしません。 AppFabric への移行に関するインフラストラクチャの問題がある場合、ワークフローは IIS 7.0 のみでホストする必要があります。

> [!WARNING]
> IIS 7.0 は、さまざまな理由でアプリケーションプールを定期的にリサイクルします。 アプリケーション プールがリサイクルされると、IIS は古いプールへのメッセージの受け取りを中止し、新しいアプリケーション プールをインスタンス化して新しい要求を受け取ります。 応答を送信した後もワークフローが動作を続ける場合、IIS 7.0 は、実行されている作業を認識しないため、ホストしているアプリケーションプールをリサイクルする可能性があります。 これが発生すると、ワークフローが中止され、追跡サービスによって、"理由" フィールドが空の [1004-WorkflowInstanceAborted](1004-workflowinstanceaborted.md) メッセージが記録されます。
>
> 永続化を使用する場合、ホストは、最後の永続性ポイントから、中止されたインスタンスを明示的に再開する必要があります。
>
> AppFabric を使用する場合、永続化を使用すると、ワークフロー管理サービスは、最終的に、最後に成功した永続性ポイントからワークフローを再開します。 永続化を使用せず、ワークフローが要求/応答パターン以外の操作を実行している場合、ワークフローが中止されるとデータは失われます。

## <a name="hosting-a-workflow-in-a-custom-windows-service"></a>カスタム Windows サービスでのワークフローのホスト

カスタム ワークフロー サービスを作成してワークフローをホストする場合、開発者は AppFabric に標準搭載されている多くの機能を複製する必要がありますが、カスタム機能をより柔軟に使用できるようになります。 このオプションは、AppFabric を選択できない場合にのみ検討してください。
