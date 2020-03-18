---
title: 有害メッセージ処理
ms.date: 03/30/2017
ms.assetid: 8d1c5e5a-7928-4a80-95ed-d8da211b8595
ms.openlocfilehash: 378849815617f6556a7d9cc7e89c6697bfdd895d
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094996"
---
# <a name="poison-message-handling"></a>有害メッセージ処理
*有害なメッセージ*とは、アプリケーションへの配信試行の最大回数を超えたことを示すメッセージのことです。 この状況は、キュー ベースのアプリケーションがエラーによってメッセージを処理できないときに発生する可能性があります。 信頼性に対する要求を満たすために、キューに置かれたアプリケーションはトランザクションの下でメッセージを受信します。 キューに置かれたメッセージを受信したトランザクションを中止すると、メッセージはそのままキューに残り、新しいトランザクションの下で再試行されます。 トランザクションを中止させた問題が解決されなければ、受信側のアプリケーションは、配信試行回数の最大値を超えるまで同じメッセージの受信と中止を繰り返す悪循環に陥る可能性があり、その結果、有害メッセージが生じることになります。  
  
 メッセージは、さまざまな理由で有害メッセージになる可能性があります。 最も一般的な理由は、アプリケーションによって異なります。 たとえば、アプリケーションがキューからメッセージを読み取り、なんらかのデータベース処理を実行する場合は、アプリケーションがデータベースをロックできないことによって、トランザクションが中止されることが考えられます。 データベース トランザクションが中止されたために、メッセージはキューに残ります。これにより、アプリケーションではメッセージを再度読み取り、データベースのロックの取得を再試行します。 メッセージに無効な情報が含まれている場合にも、有害メッセージになる可能性があります。 たとえば、発注書に無効な顧客番号が含まれている場合があります。 このような場合、アプリケーションは自発的にトランザクションを中止し、メッセージを強制的に有害メッセージにすることがあります。  
  
 まれに、メッセージをアプリケーションにディスパッチできないことがあります。 メッセージに間違ったフレームが含まれている、無効なメッセージ資格情報が添付されている、無効なアクションヘッダーなど、メッセージに問題がある場合は、Windows Communication Foundation (WCF) レイヤーによってメッセージに問題が発生することがあります。 このような場合、アプリケーションはメッセージを受信しません。ただし、メッセージは有害メッセージとなるため、手動で処理できます。  
  
## <a name="handling-poison-messages"></a>有害なメッセージの処理  
 WCF では、有害メッセージの処理によって、アプリケーションにディスパッチできないメッセージを受信側のアプリケーションが処理するメカニズムが提供されますが、アプリケーション固有のエラーによって処理されることはありません。理由. 使用可能なキューに置かれている各バインドで、次のプロパティを使用して有害メッセージの処理を構成します。  
  
- [https://login.microsoftonline.com/consumers/](`ReceiveRetryCount`) アプリケーション キューからアプリケーションへのメッセージの配信を再試行する最大回数を示す整数値。 既定値は 5 です。 データベースの一時的なデッドロックなどの問題を即時再試行で解決する場合は、この値で十分です。  
  
- [https://login.microsoftonline.com/consumers/](`MaxRetryCycles`) 再試行サイクルの最大数を示す整数値。 再試行サイクルは、アプリケーション キューから再試行サブキューにメッセージを転送し、構成可能な遅延の後に再試行サブキューからアプリケーション キューにメッセージを転送して配信を再試行するプロセスで構成されます。 既定値は 2 です。 Windows Vista では、メッセージは最大で (`ReceiveRetryCount` + 1) * (`MaxRetryCycles` + 1) 回試行されます。 `MaxRetryCycles` は、Windows Server 2003 および Windows XP では無視されます。  
  
- [https://login.microsoftonline.com/consumers/](`RetryCycleDelay`) 再試行サイクル間の遅延時間。 既定値は 30 分です。 `MaxRetryCycles` と `RetryCycleDelay` の組み合わせにより、問題に対処する機構が提供されます。この場合、定期的な遅延後に再試行が行われ、問題が解決されます。 たとえば、SQL Server の保留中のトランザクションのコミットでロックされた行セットが処理されます。  
  
- [https://login.microsoftonline.com/consumers/](`ReceiveErrorHandling`) 再試行を最大回数実行しても配信できなかったメッセージに対して実行するアクションを示す列挙値。 値には、Fault、Drop、Reject、および Move の 4 つがあります。 既定のオプションは Fault です。  
  
- Fault : このオプションでは、`ServiceHost` のエラーの原因となったリスナーにエラーが送信されます。 アプリケーションでキューのメッセージの処理を継続するには、なんらかの外部機構によってアプリケーション キューからメッセージを削除する必要があります。  
  
- [削除]。 このオプションでは、有害メッセージが破棄されます。メッセージがアプリケーションに配信されることはありません。 この時点でメッセージの `TimeToLive` プロパティの有効期限が既に切れている場合は、メッセージが送信側の配信不能キューに表示されることがあります。 有効期限が切れていない場合は、どこにも表示されません。 このオプションは、メッセージが失われたときにユーザーが実行する操作が指定されていないことを示します。  
  
- Reject : このオプションは、Windows Vista でのみ使用できます。 これにより、メッセージキュー (MSMQ) は、メッセージを受信できないという否定受信確認を送信キューマネージャーに返信するように指示します。 メッセージは、送信側キュー マネージャーの配信不能キューに置かれます。  
  
- Move : このオプションは、Windows Vista でのみ使用できます。 有害メッセージを有害メッセージ キューに移動して、後で有害メッセージ処理アプリケーションで処理できるようにします。 有害メッセージ キューは、アプリケーション キューのサブキューです。 有害メッセージ処理アプリケーションは、有害キューからメッセージを読み取る WCF サービスにすることができます。 有害キューはアプリケーションキューのサブキューであり、oison://\<*マシン名*>/*applicationqueue*;p としてアドレス指定できます。ここで、*コンピューター*名はキューが存在するコンピューターの名前、 *applicationqueue*はアプリケーション固有のキューの名前です。  
  
メッセージに対して実行される配信の最大試行回数は、次のようになります。  
  
- ((ReceiveRetryCount + 1) * (MaxRetryCycles + 1)) (Windows Vista の場合)。  
  
- (ReceiveRetryCount + 1) (Windows Server 2003 および Windows XP の場合)。  
  
> [!NOTE]
> 正常に配信されたメッセージについては、再試行は行われません。  
  
 メッセージの読み取りが試行された回数を追跡するために、Windows Vista は、中断の回数と、アプリケーションキューとサブキューの間でメッセージが移動する回数をカウントする "移動数" プロパティを保持します。 WCF チャネルはこれらを使用して、受信再試行回数と再試行サイクル数を計算します。 Windows Server 2003 および Windows XP では、abort カウントは WCF チャネルによってメモリ内に保持され、アプリケーションが失敗した場合はリセットされます。 また、WCF チャネルは、最大256のメッセージの中止回数をメモリ内でいつでも保持できます。 257 番目のメッセージを読み取ると、最も古いメッセージの中止回数がリセットされます。  
  
 中止回数と移動回数のプロパティは、操作コンテキストを通じてサービス操作で使用できます。 これらのプロパティにアクセスする方法を次のコード例に示します。  
  
 [!code-csharp[S_UE_MSMQ_Poison#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/service.cs#1)]  
  
 WCF には、2つの標準キューバインドが用意されています。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.ServiceModel.NetMsmqBinding>) 他の WCF エンドポイントとのキューベースの通信を実行するのに適した .NET Framework バインディング。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>) 既存のメッセージ キュー アプリケーションとの通信に適したバインディング。  
  
> [!NOTE]
> これらのバインドのプロパティは、WCF サービスの要件に基づいて変更できます。 有害メッセージ処理機構全体は、受信側アプリケーションに対してローカルです。 このプロセスは、受信側アプリケーションが最終的に処理を停止し、送信側に否定受信確認を返信する場合を除き、送信元アプリケーションには認識されません。 この場合、メッセージは、送信元の配信不能キューに送られます。  
  
## <a name="best-practice-handling-msmqpoisonmessageexception"></a>ベスト プラクティス : MsmqPoisonMessageException の処理  
 メッセージが有害であるとサービスが判断した場合、キューに置かれたトランスポートは <xref:System.ServiceModel.MsmqPoisonMessageException> をスローします。この例外には、有害メッセージの `LookupId` が含まれます。  
  
 受信側アプリケーションでは、必要な <xref:System.ServiceModel.Dispatcher.IErrorHandler> インターフェイスを実装して、すべてのエラーを処理できます。 詳細については、「[エラー処理およびレポートに対する制御の拡張](../../../../docs/framework/wcf/samples/extending-control-over-error-handling-and-reporting.md)」を参照してください。  
  
 アプリケーションでは、有害メッセージを有害メッセージ キューに移動し、サービスがキュー内の残りのメッセージにアクセスできるようにする、なんらかの有害メッセージ自動処理を必要とする場合があります。 エラー処理機構を使用して有害メッセージの例外をリッスンする唯一のシナリオは、<xref:System.ServiceModel.Configuration.MsmqBindingElementBase.ReceiveErrorHandling%2A> の設定を <xref:System.ServiceModel.ReceiveErrorHandling.Fault> に設定した場合です。 メッセージ キュー 3.0 の有害メッセージ サンプルは、この動作を示しています。 ベスト プラクティスを含め、有害メッセージを処理するために必要な手順の概要を以下に示します。  
  
1. 有害メッセージの設定がアプリケーションの要件を反映していることを確認します。 設定を使用する場合は、Windows Vista、Windows Server 2003、および Windows XP でのメッセージキューの機能の違いを理解しておく必要があります。  
  
2. 必要に応じて、`IErrorHandler` を実装して、有害メッセージエラーを処理します。 `ReceiveErrorHandling` を `Fault` に設定する場合、有害メッセージをキューから取り除いたり、外部の従属的な問題を解決したりする手動の機構が必要となります。そのため、`IErrorHandler` を `ReceiveErrorHandling` に設定するときは、次のコードに示すように `Fault` を実装するのが一般的です。  
  
     [!code-csharp[S_UE_MSMQ_Poison#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonerrorhandler.cs#2)]  
  
3. サービス動作が使用できる `PoisonBehaviorAttribute` を作成します。 この動作により、`IErrorHandler` がディスパッチャーにインストールされます。 このコード例を次に示します。  
  
     [!code-csharp[S_UE_MSMQ_Poison#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonbehaviorattribute.cs#3)]  
  
4. サービスに有害動作属性による注釈が付けられていることを確認します。  

 また、`ReceiveErrorHandling` を `Fault` に設定した場合は、`ServiceHost` が有害メッセージを検出するとエラーになります。 この場合、faulted イベントにフックしてサービスを終了し、是正措置を講じた後に再起動できます。 たとえば、`LookupId` に伝達された <xref:System.ServiceModel.MsmqPoisonMessageException> に含まれる `IErrorHandler` は記録しておくことができます。そのため、サービス ホストでエラーが発生したときに、`System.Messaging` API を使用して、この `LookupId` に基づいてキューからメッセージを受信し、そのメッセージをキューから削除して、外部ストアまたは別のキューに格納できます。 これで `ServiceHost` を再起動して、通常の処理を再開できるようになります。 [MSMQ 4.0 での有害メッセージ処理](../../../../docs/framework/wcf/samples/poison-message-handling-in-msmq-4-0.md)は、この動作を示しています。  
  
## <a name="transaction-time-out-and-poison-messages"></a>トランザクション タイムアウトと有害メッセージ  
 キューに置かれたトランスポート チャネルとユーザー コードの間では、特定の部類のエラーが発生する可能性があります。 これらのエラーは、メッセージ セキュリティ レイヤーやサービス ディスパッチ ロジックなど、中間のレイヤーで検出される場合があります。 たとえば、SOAP セキュリティ レイヤーで X.509 証明書がないことが検出された場合やアクションがない場合は、メッセージがアプリケーションにディスパッチされます。 この場合、サービス モデルはそのメッセージを破棄します。 メッセージはトランザクションで読み取られますが、そのトランザクションの結果は提供されないため、トランザクションが最終的にタイムアウトになって中止され、メッセージはキューに戻されます。 つまり、エラーの特定のクラスについては、トランザクションがすぐに中止されるのではなく、トランザクションがタイムアウトするまで待機します。サービスのトランザクションタイムアウトを変更するには、<xref:System.ServiceModel.ServiceBehaviorAttribute>を使用します。  
  
 コンピューター全体のトランザクションタイムアウトを変更するには、machine.config ファイルを変更し、適切なトランザクションタイムアウトを設定します。トランザクションで設定されたタイムアウトによっては、トランザクションが最終的に中止されてキューに戻され、その中止回数が増加することに注意する必要があります。 最終的に、メッセージは有害メッセージになり、ユーザー設定に従って適切な処理が行われます。  
  
## <a name="sessions-and-poison-messages"></a>セッションと有害メッセージ  
 セッションでは、単一のメッセージと同じ再試行および有害メッセージ処理手順が行われます。 有害メッセージに対する上記のプロパティは、セッション全体に適用されます。 つまり、セッション全体が再試行され、メッセージは最終有害メッセージ キューに送られます。または、メッセージが拒否された場合は、送信側の配信不能キューに送られます。  
  
## <a name="batching-and-poison-messages"></a>バッチ処理と有害メッセージ  
 バッチの一部であるメッセージが有害メッセージになった場合は、バッチ全体がロールバックされ、チャネルはメッセージを 1 つずつ読み取る処理に戻ります。 バッチ処理の詳細については、「[トランザクション内のメッセージのバッチ](../../../../docs/framework/wcf/feature-details/batching-messages-in-a-transaction.md)処理」を参照してください。  
  
## <a name="poison-message-handling-for-messages-in-a-poison-queue"></a>有害キュー内のメッセージに対する有害メッセージ処理  
 有害メッセージ処理は、メッセージが有害メッセージ キューに配置された時点では終了しません。 有害メッセージ キューに置かれたメッセージは、依然として読み取り、処理する必要があります。 最終有害サブキューからメッセージを読み取るときは、有害メッセージ処理設定のサブセットを使用できます。 適用できる設定は、`ReceiveRetryCount` と `ReceiveErrorHandling` です。 `ReceiveErrorHandling` は Drop、Rreject、Fault のいずれかに設定できます。 `MaxRetryCycles` が Move に設定されている場合は、`ReceiveErrorHandling` が無視され、例外がスローされます。  
  
## <a name="windows-vista-windows-server-2003-and-windows-xp-differences"></a>Windows Vista、Windows Server 2003、および Windows XP の相違点  
 前述のように、有害メッセージの処理のすべての設定が Windows Server 2003 および Windows XP に適用されるわけではありません。 Windows Server 2003、Windows XP、および Windows Vista のメッセージキューの主な相違点は、有害メッセージの処理に関連しています。  
  
- Windows Vista のメッセージキューは、サブキューをサポートします。一方、Windows Server 2003 および Windows XP では、サブキューはサポートされません。 サブキューは、有害メッセージ処理で使用されます。 再試行キューと有害キューは、有害メッセージ処理の設定に基づいて作成されるアプリケーション キューのサブキューです。 作成する再試行サブキューの数は、`MaxRetryCycles` で指定します。 このため、Windows Server 2003 または Windows XP で実行している場合、`MaxRetryCycles` は無視され、`ReceiveErrorHandling.Move` は許可されません。  
  
- Windows Vista のメッセージキューは否定受信確認をサポートしていますが、Windows Server 2003 および Windows XP ではサポートされていません。 受信側キュー マネージャーから否定受信確認を受け取ると、送信側キュー マネージャーは拒否されたメッセージを配信不能キューに入れます。 そのため、`ReceiveErrorHandling.Reject` は、Windows Server 2003 および Windows XP では許可されていません。  
  
- Windows Vista のメッセージキューは、メッセージの配信が試行された回数を保持するメッセージプロパティをサポートしています。 この中止回数のプロパティは、Windows Server 2003 および Windows XP では使用できません。 WCF では、中止回数がメモリ内に保持されるため、同じメッセージがファーム内の複数の WCF サービスによって読み取られる場合、このプロパティに正確な値が含まれていない可能性があります。  
  
## <a name="see-also"></a>参照

- [キューの概要](../../../../docs/framework/wcf/feature-details/queues-overview.md)
- [Windows Vista、Windows Server 2003、および Windows XP におけるキュー機能の相違点](../../../../docs/framework/wcf/feature-details/diff-in-queue-in-vista-server-2003-windows-xp.md)
- [コントラクトおよびサービスのエラーの指定と処理](../../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md)
