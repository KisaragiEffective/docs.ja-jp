---
description: 詳細については、「永続性データベーススキーマ」を参照してください。
title: 永続性データベース スキーマ
ms.date: 03/30/2017
ms.assetid: 34f69f4c-df81-4da7-b281-a525a9397a5c
ms.openlocfilehash: 40c47c5bfcb6c974eab6f2f2c926e0fa13054a38
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99787839"
---
# <a name="persistence-database-schema"></a>永続性データベース スキーマ

このトピックでは、SQL Workflow Instance Store でサポートされるパブリック ビューについて説明します。  
  
## <a name="instances-view"></a>Instances ビュー  

 **インスタンス** ビューには、データベース内のすべてのワークフローインスタンスに関する一般的な情報が含まれています。  
  
|列名|列の型|説明|  
|-----------------|-----------------|-----------------|  
|InstanceId|UniqueIdentifier|ワークフロー インスタンスの ID。|  
|PendingTimer|DateTime|ワークフローが Delay アクティビティでブロックされていて、タイマーが時間切れになってから再開されることを示します。 タイマーが時間切れになるまで待機するようにワークフローがブロックされていない場合は null になります。|  
|CreationTime|DateTime|ワークフローが作成された日時を示します。|  
|LastUpdatedTime|DateTime|ワークフローがデータベースに最後に永続化された日時を示します。|  
|ServiceDeploymentId|BigInt|[ServiceDeployments] ビューに対する外部キーとして機能します。 現在のワークフロー インスタンスが Web ホスト型サービスのインスタンスの場合、この列に値が設定されます。それ以外の場合は NULL に設定されます。|  
|SuspensionExceptionName|Nvarchar(450)|ワークフローの中断の原因となった例外の種類 (InvalidOperationException など) を示します。|  
|SuspensionReason|Nvarchar(max)|ワークフロー インスタンスが中断された理由を示します。 例外が原因でインスタンスが中断された場合は、例外に関連付けられているメッセージが格納されます。<br /><br /> 手動でインスタンスが中断された場合は、ユーザーが指定したインスタンスの中断理由が格納されます。|  
|ActiveBookmarks|Nvarchar(max)|ワークフロー インスタンスがアイドル状態の場合、このプロパティはインスタンスがブロックされているブックマークを示します。 インスタンスがアイドル状態でない場合は、この列は NULL になります。|  
|CurrentMachine|Nvarchar(128)|ワークフロー インスタンスを現在メモリに読み込んでいるコンピューターの名前を示します。|  
|LastMachine|Nvarchar(450)|ワークフロー インスタンスを最後に読み込んだコンピューターを示します。|  
|ExecutionStatus|Nvarchar(450)|ワークフローの現在の実行状態を示します。 **実行**、**アイドル**、**終了** などの状態があります。|  
|IsInitialized|ビット|ワークフロー インスタンスが初期化されているかどうかを示します。 初期化されたワークフロー インスタンスとは、少なくとも 1 回は永続化されているワークフロー インスタンスのことです。|  
|IsSuspended|ビット|ワークフロー インスタンスが中断されているかどうかを示します。|  
|IsCompleted|ビット|ワークフロー インスタンスの実行が完了しているかどうかを示します。 **注:**  Iif **InstanceCompletionAction** プロパティが **DeleteAll** に設定されている場合、インスタンスは完了時にビューから削除されます。|  
|EncodingOption|TinyInt|データ プロパティのシリアル化に使用されるエンコーディングを示します。<br /><br /> -0: エンコードなし<br />-1 – GzipStream|  
|ReadWritePrimitiveDataProperties|Varbinary(max)|インスタンスが読み込まれるときにワークフロー ランタイムに戻される、シリアル化されたインスタンスのデータ プロパティが格納されます。<br /><br /> プリミティブ型の各プロパティはネイティブな CLR 型です。つまり、BLOB を逆シリアル化するときに特別なアセンブリは必要ありません。|  
|WriteOnlyPrimitiveDataProperties|Varbinary(max)|インスタンスが読み込まれるときにワークフロー ランタイムに戻されない、シリアル化されたインスタンスのデータ プロパティが格納されます。<br /><br /> プリミティブ型の各プロパティはネイティブな CLR 型です。つまり、BLOB を逆シリアル化するときに特別なアセンブリは必要ありません。|  
|ReadWriteComplexDataProperties|Varbinary(max)|インスタンスが読み込まれるときにワークフロー ランタイムに戻される、シリアル化されたインスタンスのデータ プロパティが格納されます。<br /><br /> デシリアライザーで、この BLOB に格納されているすべてのオブジェクト型を認識している必要があります。|  
|WriteOnlyComplexDataProperties|Varbinary(max)|インスタンスが読み込まれるときにワークフロー ランタイムに戻されない、シリアル化されたインスタンスのデータ プロパティが格納されます。<br /><br /> デシリアライザーで、この BLOB に格納されているすべてのオブジェクト型を認識している必要があります。|  
|IdentityName|Nvarchar(max)|ワークフロー定義の名前。|  
|IdentityPackage|Nvarchar(max)|ワークフローが作成されたときに指定されたパッケージの情報 (アセンブリ名など)。|  
|Build|BigInt|ワークフロー バージョンのビルド番号。|  
|メジャー|BigInt|ワークフロー バージョンのメジャー番号。|  
|Minor|BigInt|ワークフロー バージョンのマイナー番号。|  
|リビジョン|BigInt|ワークフロー バージョンのリビジョン番号。|  
  
> [!CAUTION]
> **インスタンス** ビューには、Delete トリガーも含まれています。 適切な権限を持つユーザーは、このビューに対して delete ステートメントを実行して、データベースからワークフロー インスタンスを強制的に削除することができます。 ただし、ワークフロー ランタイムからインスタンスを削除すると意図しない結果を引き起こすことがあるため、ビューから直接削除する方法は最後の手段としてのみ使用することをお勧めします。 代わりに、ワークフロー インスタンス管理エンドポイントを使用して、ワークフロー ランタイムでインスタンスを終了するようにしてください。 ビューから多数のインスタンスを削除する場合は、それらのインスタンスで稼動しているアクティブなランタイムがないことを確認してください。  
  
## <a name="servicedeployments-view"></a>ServiceDeployments ビュー  

 **Servicedeployments** ビューには、すべての WEB (IIS/WAS) でホストされるワークフローサービスの展開情報が含まれています。 Web ホストされる各ワークフローインスタンスには、このビューの行を参照する **Servicedeploymentid** が含まれます。  
  
|列名|列の型|説明|  
|-----------------|-----------------|-----------------|  
|ServiceDeploymentId|BigInt|このビューの主キー。|  
|SiteName|Nvarchar(max)|ワークフローサービス ( **既定の Web サイト** など) を含むサイトの名前を表します。|  
|RelativeServicePath|Nvarchar(max)|ワークフロー サービスの仮想パスを、サイトを基準とした相対パスで表します  など.  **/app1/PurchaseOrderService.svc**)。|  
|RelativeApplicationPath|Nvarchar(max)|ワークフロー サービスを含むアプリケーションの仮想パスを、サイトを基準とした相対パスで表します  (例: **/app1**)。|  
|ServiceName|Nvarchar(max)|ワークフロー サービスの名前を表します  (例: **PurchaseOrderService**)。|  
|ServiceNamespace|Nvarchar(max)|ワークフロー サービスの名前空間を表します  (例: **MyCompany**)。|  
  
 ServiceDeployments ビューには、Delete トリガーも含まれています。 適切な権限を持つユーザーは、このビューに対して delete ステートメントを実行して、データベースから ServiceDeployment のエントリを削除することができます。 以下の点に注意してください。  
  
1. このビューからエントリを削除するときは、実行前にデータベース全体をロックしなければならないため、この操作は高コストです。 これは、存在しない ServiceDeployment のエントリをワークフロー インスタンスが参照しないようにするために必要です。 このビューからの削除は、ダウンタイムか保守時間帯のみに行うようにしてください。  
  
2. **インスタンス** ビューのエントリによって参照されている servicedeployment 行を削除しようとすると、操作は実行されません。 削除できるのは、参照がない ServiceDeployment 行だけです。  
  
## <a name="instancepromotedproperties-view"></a>InstancePromotedProperties ビュー  

 **InstancePromotedProperties** ビューには、ユーザーによって指定されたすべての昇格させたプロパティの情報が含まれています。 昇格されたプロパティはファーストクラスのプロパティとして機能します。ユーザーは、このプロパティをクエリで使用してインスタンスを取得できます。  たとえば、注文のコストを常に **Value1** 列に格納する PurchaseOrder 昇格をユーザーが追加できます。 これにより、コストが特定の値を超えるすべての購買発注書を照会することができます。  
  
|列の型|列の型|説明|  
|-|-|-|  
|InstanceId|UniqueIdentifier|ワークフロー インスタンスの ID。|  
|EncodingOption|TinyInt|昇格されたバイナリ プロパティのシリアル化に使用されるエンコーディングを示します。<br /><br /> -0: エンコードなし<br />-1 – GZipStream|  
|PromotionName|Nvarchar(400)|このインスタンスに関連付けられた昇格の名前。 PromotionName は、この行の汎用的な列にコンテキストを追加するために必要です。<br /><br /> たとえば、PromotionName が PurchaseOrder の場合、Value1 に注文のコスト、Value2 に注文を行った顧客の名前、Value 3 に顧客の住所のように格納されることを示します。|  
|Value[1-32]|SqlVariant|Value[1-32] には、SqlVariant 列に格納できる値が格納されます。 1 つの昇格に 32 を超える SqlVariant を格納することはできません。|  
|Value[33-64]|Varbinary(max)|Value[33-64] には、シリアル化された値が格納されます。たとえば、Value33 に購入品目の JPEG を格納することができます。 1 つの昇格に 32 を超えるバイナリ プロパティを格納することはできません。|  
  
 InstancePromotedProperties ビューはスキーマ バインドであるため、このビューに対するクエリを最適化するために、1 つまたは複数の列にインデックスを追加することができます。  
  
> [!NOTE]
> インデックス付きビューを使用する場合、必要なストレージが多くなり、処理のオーバーヘッドが増加します。 詳細については、 [SQL Server 2008 のインデックス付きビューでのパフォーマンスの向上](/previous-versions/sql/sql-server-2008/dd171921(v=sql.100)) に関する説明を参照してください。
