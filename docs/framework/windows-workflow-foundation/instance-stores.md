---
title: インスタンス ストア
ms.date: 03/30/2017
ms.assetid: f2629668-0923-4987-b943-67477131c1e0
ms.openlocfilehash: 69b50942c36406bd29147d243e0501b8048d56dc
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802558"
---
# <a name="instance-stores"></a>インスタンス ストア
インスタンス ストアは、インスタンスの論理コンテナーです。 この場所には、インスタンス データとメタデータが格納されます。 インスタンス ストアは、専用の物理的なストレージを意味しているわけではありません。 インスタンス ストアには SQL Server データベースの永続的な情報と、メモリ内の非永続的な状態の情報が含まれます。 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] には SQL Workflow Instance Store が付属しています。これはインスタンス ストアの具象実装で、ワークフローが SQL Server 2005 または SQL Server 2008 データベースにインスタンス データとメタデータを永続化できるようにします。 また、Windows Server App Fabric には、インスタンス ストアの具象実装も用意されています。 詳細については、「 [Windows Server App Fabric インスタンスストア、クエリ、およびコントロールプロバイダー](https://docs.microsoft.com/previous-versions/appfabric/ff383417(v=azure.10))」を参照してください。  
  
 永続化 API は、ホストがコマンド要求 (<xref:System.Activities.DurableInstancing.LoadWorkflowCommand> や <xref:System.Activities.DurableInstancing.SaveWorkflowCommand> など) をインスタンス ストアに送信できるようにするための、ホストとインスタンス ストア間のインターフェイスです。 この API の具象実装は、永続化プロバイダーと呼ばれます。 永続化プロバイダーはホストからの要求を受け取り、インスタンス ストアを変更します。  
  
 ホストで多くのインスタンス ストアを使用し、インスタンス ストアを多くのホストで使用できるように、ホストとインスタンス ストアはプラグ可能です。 通常、インスタンス ストアは特定のホストの使用パターンに合わせて最適化されますが、インスタンス ストアとホストはそれぞれのライフ サイクルで進化する場合があります。 たとえば、 **WorkflowServiceHost**と**SqlWorkflowInstanceStore**は、うまく連携するように設計されています。 独自のインスタンスストアを作成して、ワークフローサービスインスタンスのデータとメタデータを永続化し、そのインスタンスストアを**WorkflowServiceHost**で使用することができます。 たとえば、SQL Server データベースに保存するのではなく、OracleWorkflowInstanceStore を作成して、ワークフローに Oracle データベースに情報を永続化させることができます。  
  
 通常、ホストは保存されたオブジェクトを変更する機能を追加して拡張されます。 たとえば、インスタンス永続化システムは、ワークフローホスト、"中断" 操作をサポートする拡張機能、および SQL インスタンスストアを持つことができます。  ワークフロー ホストは保存または読み込みなどの標準的なコマンドを送信して、インスタンス ストアに対してワークフローの保存または読み込みを行ったり、インスタンス ストアにワークフローを保存したりします。 中断されたワークフロー インスタンスが読み込まれないように、中断の拡張機能によって、ワークフロー インスタンスの保存および読み込みを行うコマンドに追加のセマンティクスが追加されます。 SQL インスタンス ストアの永続化プロバイダーは、ワークフロー インスタンスの保存と読み込み用のコマンドを理解し、SQL Server データベースの永続オブジェクトのテーブルを変更する適切なストアド プロシージャを呼び出して、コマンドを実装します。  
  
 インスタンス ストア内では、ホストはインスタンスの所有者として動作します。 ホストは、同時に複数のインスタンス ストアを持つ複数のインスタンスの所有者として動作します。 ホストはインスタンスに関連付けられているインスタンス キーの GUID を提供します。 インスタンス キーは、インスタンスを識別する一意の別名です。 永続化システムは、ホストが要求したコマンドを実行するときに、インスタンスの所有者情報を作成、更新、および削除します。  
  
 ホストとインスタンス ストアとの対話に関連する重要な手順を次に示します。  
  
1. 永続化プロバイダーから**InstanceStore**を取得します。  

2. **InstanceStore**で <xref:System.Runtime.DurableInstancing.InstanceStore.CreateInstanceHandle%2A> メソッドを呼び出して、インスタンスへのハンドルを取得します。  
  
3. **InstanceStore**の <xref:System.Runtime.DurableInstancing.InstanceStore.Execute%2A> メソッドを呼び出すことによって、インスタンスハンドルに対してコマンドを呼び出します。  
  
4. コマンドの結果を確認するために、**実行**によって返される <xref:System.Runtime.DurableInstancing.InstanceView> を調べます。
