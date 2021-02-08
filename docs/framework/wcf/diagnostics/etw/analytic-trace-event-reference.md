---
description: 詳細については、「分析トレースイベントのリファレンス」を参照してください。
title: 分析トレース イベント リファレンス
ms.date: 03/30/2017
helpviewer_keywords:
- analytic tracing [WCF]. reference
ms.assetid: e44540cf-44a1-4efc-b965-7fbfd2131d73
ms.openlocfilehash: 1f4773692d3481cd5fa662d4fc70905215939db3
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99798837"
---
# <a name="analytic-trace-event-reference"></a>分析トレース イベント リファレンス

次の表は、WCF 分析トレースに関連付けられているイベントレベル、識別子、およびメッセージを定義しています。  
  
## <a name="event-reference"></a>イベント リファレンス  
  
|イベント ID|イベント レベル|イベント メッセージ|Keywords|  
|--------------|-----------------|-------------------|--------------|  
|[131 - BufferPoolAllocation](131-bufferpoolallocation.md)|"詳細"|プールが %1 バイトを割り当てています。|インフラストラクチャ|  
|[132 - BufferPoolChangeQuota](132-bufferpoolchangequota.md)|"詳細"|BufferPool のサイズ: %1、クォータの変更: %2。|インフラストラクチャ|  
|[133 - ActionItemScheduled](133-actionitemscheduled.md)|"詳細"|IO スレッド スケジューラのコールバックが呼び出されました。|インフラストラクチャ|  
|[134 - ActionItemCallbackInvoked](134-actionitemcallbackinvoked.md)|"詳細"|IO スレッド スケジューラのコールバックが呼び出されました。|インフラストラクチャ|  
|[201 - ClientMessageInspectorAfterReceiveInvoked](201-clientmessageinspectorafterreceiveinvoked.md)|Information|ディスパッチャーが型 '%1' の ClientMessageInspector で 'AfterReceiveReply' を呼び出しました。|Troubleshooting、ServiceModel|  
|[202 - ClientMessageInspectorBeforeSendInvoked](202-clientmessageinspectorbeforesendinvoked.md)|Information|ディスパッチャーが型 '%1' の ClientMessageInspector で 'BeforeSendRequest' を呼び出しました。|Troubleshooting、ServiceModel|  
|[203 - ClientParameterInspectorAfterCallInvoked](203-clientparameterinspectoraftercallinvoked.md)|Information|ディスパッチャーが 型 '%1' の ClientParameterInspector で 'AfterCall' を呼び出しました。|Troubleshooting、ServiceModel|  
|[204 - ClientParameterInspectorBeforeCallInvoked](204-clientparameterinspectorbeforecallinvoked.md)|Information|ディスパッチャーが型 '%1' の ClientParameterInspector で 'BeforeCall' を呼び出しました。|Troubleshooting、ServiceModel|  
|[205 - OperationInvoked](205-operationinvoked.md)|Information|OperationInvoker が '%1' メソッドを呼び出しました。|EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[206 - ErrorHandlerInvoked](206-errorhandlerinvoked.md)|Information|ディスパッチャーが型 '%1' の ErrorHandler を呼び出し、種類 '%3' の例外がスローされました。  ErrorHandled == '%2'。|Troubleshooting、ServiceModel|  
|[207 - FaultProviderInvoked](207-faultproviderinvoked.md)|Information|ディスパッチャーが型 '%1' の FaultProvider を呼び出し、種類 '%2' の例外がスローされました。|Troubleshooting、ServiceModel|  
|[208 - MessageInspectorAfterReceiveInvoked](208-messageinspectorafterreceiveinvoked.md)|Information|ディスパッチャーが型 '%1' の MessageInspector で 'AfterReceiveReply' を呼び出しました。|Troubleshooting、ServiceModel|  
|[209 - MessageInspectorBeforeSendInvoked](209-messageinspectorbeforesendinvoked.md)|Information|ディスパッチャーが型 '%1' の MessageInspector で 'BeforeSendRequest' を呼び出しました。|Troubleshooting、ServiceModel|  
|[210 - MessageThrottleExceeded](210-messagethrottleexceeded.md)|警告|スロットル '%1' の '%2' の制限に達しました。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[211 - ParameterInspectorAfterCallInvoked](211-parameterinspectoraftercallinvoked.md)|Information|ディスパッチャーが型 '%1' の ParameterInspector で 'AfterCall' を呼び出しました。|Troubleshooting、ServiceModel|  
|[212 - ParameterInspectorBeforeCallInvoked](212-parameterinspectorbeforecallinvoked.md)|Information|ディスパッチャーが型 '%1' の ParameterInspector で 'BeforeCall' を呼び出しました。|Troubleshooting、ServiceModel|  
|[213 - ServiceHostStarted](213-servicehoststarted.md)|LogAlways|ServiceHost は '%1' で開始されています。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[214 - OperationCompleted](214-operationcompleted.md)|Information|OperationInvoker がメソッド '%1' への呼び出しを完了しました。  メソッド呼び出し時間は '%2' ミリ秒でした。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[215 - MessageReceivedByTransport](215-messagereceivedbytransport.md)|Information|トランスポートが '%1' からメッセージを受信しました。|Troubleshooting、ServiceModel|  
|[216 - MessageSentByTransport](216-messagesentbytransport.md)|Information|トランスポートが '%1' にメッセージを送信しました。|Troubleshooting、ServiceModel|  
|[217 - ClientOperationPrepared](217-clientoperationprepared.md)|Information|クライアントは '%2' コントラクトと関連付けられている Action '%1' を実行しています。 メッセージは '%3' に送信されます。|Troubleshooting、ServiceModel|  
|[218 - ClientOperationCompleted](218-clientoperationcompleted.md)|Information|クライアントは '%1' コントラクトと関連付けられている Action '%1' の実行を完了しました。 メッセージは '%3' に送信されました。|Troubleshooting、ServiceModel|  
|[219 - ServiceException](219-serviceexception.md)|エラー|メッセージの処理中に種類 '%2' のハンドルされない例外がスローされました。  完全な例外 ToString: %1。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[220 - MessageSentToTransport](220-messagesenttotransport.md)|Information|ディスパッチャーがトランスポートにメッセージを送信しました。 関連付け ID == '%1'。|EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[221 - MessageReceivedFromTransport](221-messagereceivedfromtransport.md)|Information|ディスパッチャーがトランスポートからメッセージを受信しました。 関連付け ID == '%1'。|EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[222 - OperationFailed](222-operationfailed.md)|警告|OperationInvoker によって呼び出されたメソッド '%1' で、ハンドルされない例外がスローされました。 メソッド呼び出し時間は '%2' ミリ秒でした。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[223 - OperationFaulted](223-operationfaulted.md)|警告|OperationInvoker によって呼び出されたメソッド '%1' で FaultException がスローされました。 メソッド呼び出し時間は '%2' ミリ秒でした。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[224 - MessageThrottleAtSeventyPercent](224-messagethrottleatseventypercent.md)|警告|スロットル '%1' の '%2' の制限は 70% です。|HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[226 - IdleServicesClosed](226-idleservicesclosed.md)|LogAlways|アクティブ化された合計 %2 個のサービスのうち、アイドル状態の %1 個のサービスが閉じられました。|HealthMonitoring WebHost|  
|[301 - UserDefinedErrorOccurred](301-userdefinederroroccurred.md)|エラー|名前: '%1'、参照: '%2'、ペイロード: %3|UserEvents、HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[302 - UserDefinedWarningOccurred](302-userdefinedwarningoccurred.md)|警告|名前: '%1'、参照: '%2'、ペイロード: %3|UserEvents、HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[303 - UserDefinedInformationEventOccured](303-userdefinedinformationeventoccured.md)|Information|名前: '%1'、参照: '%2'、ペイロード: %3|UserEvents、HealthMonitoring、EndToEndMonitoring、Troubleshooting、ServiceModel|  
|[401- StopSignPostEvent](401-stopsignpostevent.md)|Information|アクティビティの境界|トラブルシューティング|  
|[402 - StartSignpostEvent](402-startsignpostevent.md)|Information|アクティビティの境界|トラブルシューティング|  
|[403 - SuspendSignpostEvent](403-suspendsignpostevent.md)|Information|アクティビティの境界|トラブルシューティング|  
|[404 - ResumeSignpostEvent](404-resumesignpostevent.md)|Information|アクティビティの境界|トラブルシューティング|  
|[451 - MessageLogInfo](451-messageloginfo.md)|Information|%1|Troubleshooting、WCFMessageLogging|  
|[452 - MessageLogWarning](452-messagelogwarning.md)|警告|%1|Troubleshooting、WCFMessageLogging|  
|[499 - TransferEmitted](499-transferemitted.md)|LogAlways|転送イベントが作成されました。|Troubleshooting、UserEvents、EndToEndMonitoring、ServiceModel、WFTracking、ServiceHost、WCFMessageLogging|  
|[501 - CompilationStart](501-compilationstart.md)|Information|コンパイルを開始します|WebHost|  
|[502 - CompilationStop](502-compilationstop.md)|Information|コンパイルを終了します|WebHost|  
|[503 - ServiceHostFactoryCreationStart](503-servicehostfactorycreationstart.md)|Information|ServiceHostFactory が作成を開始します|WebHost|  
|[504 - ServiceHostFactoryCreationStop](504-servicehostfactorycreationstop.md)|Information|ServiceHostFactory が作成を終了します|WebHost|  
|[505 - CreateServiceHostStart](505-createservicehoststart.md)|Information|CreateServiceHost を開始します|WebHost|  
|[506 - CreateServiceHostStop](506-createservicehoststop.md)|Information|CreateServiceHost を終了します|WebHost|  
|[507 - HostedTransportConfigurationManagerConfigInitStart](507-hostedtransportconfigurationmanagerconfiginitstart.md)|Information|HostedTransportConfigurationManager が構成の初期化を開始します|WebHost|  
|[508 - HostedTransportConfigurationManagerConfigInitStop](508-hostedtransportconfigurationmanagerconfiginitstop.md)|Information|HostedTransportConfigurationManager が構成の初期化を終了します|WebHost|  
|[509 - ServiceHostOpenStart](509-servicehostopenstart.md)|Information|HostedTransportConfigurationManager が構成の初期化を終了します|ServiceHost|  
|[510 - ServiceHostOpenStop](510-servicehostopenstop.md)|Information|ServiceHost Open が完了しました。|ServiceHost|  
|[513 - WebHostRequestStart](513-webhostrequeststart.md)|Information|AppDomain '%1' からの仮想パス '%2' で要求を受信しました。|WebHost|  
|[514 - WebHostRequestStop](514-webhostrequeststop.md)|Information|WebHostRequest を停止します。|WebHost|  
|[601 - CBAEntryRead](601-cbaentryread.md)|"詳細"|ServiceActivation 要素の相対アドレス: '%1'、標準化相対アドレス '%2' を処理しました。||  
|[602 - CBAMatchFound](602-cbamatchfound.md)|"詳細"|受信要求が、アドレス '%1' の ServiceActivation 要素と一致します。||  
|[603 - AspNetRoutingService](603-aspnetroutingservice.md)|"詳細"|受信要求が、アドレス %1 の ASP.NET ルートで定義されている WCF サービスと一致します。|RoutingServices|  
|[604 - AspNetRoute](604-aspnetroute.md)|"詳細"|ServiceType ' %2 ' および serviceHostFactoryType ' %3 ' の新しい ASP.NET ルート ' %1 ' が追加されました。|RoutingServices|  
|[605 - IncrementBusyCount](605-incrementbusycount.md)|"詳細"|IncrementBusyCount が呼び出されました。 ソース: %1|WebHost|  
|[606 - DecrementBusyCount](606-decrementbusycount.md)|"詳細"|DecrementBusyCount が呼び出されました。 ソース: %1|WebHost|  
|[701 - ServiceChannelOpenStart](701-servicechannelopenstart.md)|"詳細"|ServiceChannelOpen を開始しました。|WebHost|  
|[702 - ServiceChannelOpenStop](702-servicechannelopenstop.md)|Information|ServiceChannelOpen が完了しました。|ServiceModel|  
|[703 - ServiceChannelCallStart](703-servicechannelcallstart.md)|Information|ServiceChannelCall を開始しました。|ServiceModel|  
|[704 - ServiceChannelBeginCallStart](704-servicechannelbegincallstart.md)|Information|ServiceChannel の非同期呼び出しを開始しました。|ServiceModel|  
|[706 - HttpSendMessageStart](706-httpsendmessagestart.md)|"詳細"|HTTP 送信要求を開始します。|HTTP|  
|[707 - HttpSendStop](707-httpsendstop.md)|"詳細"|HTTP 送信要求を停止します。|HTTP|  
|[708 - HttpMessageReceiveStart](708-httpmessagereceivestart.md)|"詳細"|HTTP トランスポートからメッセージを受信しました。|HTTP|  
|[709 - DispatchMessageStart](709-dispatchmessagestart.md)|Information|メッセージのディスパッチを開始しました。|ServiceModel|  
|[710 - HttpContextBeforeProcessAuthentication](710-httpcontextbeforeprocessauthentication.md)|"詳細"|メッセージのディスパッチの認証を開始します|ServiceModel|  
|[711 - DispatchMessageBeforeAuthorization](711-dispatchmessagebeforeauthorization.md)|"詳細"|メッセージのディスパッチの承認を開始します|ServiceModel|  
|[712 - DispatchMessageStop](712-dispatchmessagestop.md)|Information|メッセージのディスパッチが完了しました|ServiceModel|  
|[715 - ClientChannelOpenStart](715-clientchannelopenstart.md)|Information|ServiceChannel Open を開始します。|ServiceModel|  
|[716 - ClientChannelOpenStop](716-clientchannelopenstop.md)|Information|ServiceChannel Open を停止します。|ServiceModel|  
|[717 - HttpSendStreamedMessageStart](717-httpsendstreamedmessagestart.md)|Information|ストリーム メッセージの HTTP 送信を開始しました。|HTTP|  
|[1400 - ChannelInitializationTimeout](1400-channelinitializationtimeout.md)|エラー|1%|ServiceModel|  
|[1401 - CloseTimeout](1401-closetimeout.md)|エラー|1%|ServiceModel|  
|[1402 - IdleTimeout](1402-idletimeout.md)|エラー|%1 接続プール キー: %2|ServiceModel|  
|[1403 - LeaseTimeout](1403-leasetimeout.md)|Information|%1 接続プール キー: %2|ServiceModel|  
|[1405 - OpenTimeout](1405-opentimeout.md)|エラー|%1|ServiceModel|  
|[1406 - ReceiveTimeout](1406-receivetimeout.md)|エラー|%1|ServiceModel|  
|[1407 - SendTimeout](1407-sendtimeout.md)|エラー|%1|ServiceModel|  
|[1409 - InactivityTimeout](1409-inactivitytimeout.md)|Information|%1|ServiceModel|  
|[1416 - MaxReceivedMessageSizeExceeded](1416-maxreceivedmessagesizeexceeded.md)|エラー|%1|Quota|  
|[1417 - MaxSentMessageSizeExceeded](1417-maxsentmessagesizeexceeded.md)|エラー|%1|Quota|  
|[1418 - MaxOutboundConnectionsPerEndpointExceeded](1418-maxoutboundconnectionsperendpointexceeded.md)|Information|%1|Quota|  
|[1419 - MaxPendingConnectionsExceeded](1419-maxpendingconnectionsexceeded.md)|Information|%1|Quota|  
|[1420 - ReaderQuotaExceeded](1420-readerquotaexceeded.md)|エラー|%1|Quota|  
|[1422 - NegotiateTokenAuthenticatorStateCacheExceeded](1422-negotiatetokenauthenticatorstatecacheexceeded.md)|エラー|%1|Quota|  
|[1423 - NegotiateTokenAuthenticatorStateCacheRatio](1423-negotiatetokenauthenticatorstatecacheratio.md)|"詳細"|トークン認証システムのネゴシエートの状態のキャッシュ比率: %1/%2|Quota|  
|[1424 - SecuritySessionRatio](1424-securitysessionratio.md)|"詳細"|セキュリティ セッションの比率: %1/%2|Quota|  
|[1430 - PendingConnectionsRatio](1430-pendingconnectionsratio.md)|"詳細"|保留中の接続の比率: %1/%2|Quota|  
|[1431 - ConcurrentCallsRatio](1431-concurrentcallsratio.md)|"詳細"|同時セッションの比率: %1/%2|Quota|  
|[1432 - ConcurrentSessionsRatio](1432-concurrentsessionsratio.md)|"詳細"|同時セッションの比率: %1/%2|Quota|  
|[1433 - OutboundConnectionsPerEndpointRatio](1433-outboundconnectionsperendpointratio.md)|"詳細"|エンドポイントごとの送信接続の比率: %1/%2|Quota|  
|[1436 - PendingMessagesPerChannelRatio](1436-pendingmessagesperchannelratio.md)|"詳細"|チャネルごとの保留メッセージの比率: %1/%2|Quota|  
|[1438 - ConcurrentInstancesRatio](1438-concurrentinstancesratio.md)|"詳細"|同時インスタンスの比率: %1/%2|Quota|  
|[1439 - PendingAcceptsAtZero](1439-pendingacceptsatzero.md)|Information|保留中の受け入れはありません|Quota|  
|[1441 - MaxSessionSizeReached](1441-maxsessionsizereached.md)|警告|1%|Quota|  
|[1442 - ReceiveRetryCountReached](1442-receiveretrycountreached.md)|警告|ID '%1' の MSMQ メッセージが受信再試行回数に達しました|Quota|  
|[1443 - MaxRetryCyclesExceededMsmq](1443-maxretrycyclesexceededmsmq.md)|エラー|ID '%1' の MSMQ メッセージが最大再試行サイクルを超えました|Quota|  
|[1445 - ReadPoolMiss](1445-readpoolmiss.md)|"詳細"|新しい '%1' を作成しました|Quota|  
|[1446 - WritePoolMiss](1446-writepoolmiss.md)|"詳細"|新しい '%1' を作成しました|Quota|  
|[1451 - MaxRetryCyclesExceeded](1451-maxretrycyclesexceeded.md)|エラー|1%|Quota|  
|[3300 - ReceiveContextCompleteFailed](3300-receivecontextcompletefailed.md)|警告|%1 を完了できませんでした。|チャネル|  
|[3301 - ReceiveContextAbandonFailed](3301-receivecontextabandonfailed.md)|警告|%1 を破棄できませんでした。|チャネル|  
|[3303 - ReceiveContextAbandonWithException](3303-receivecontextabandonwithexception.md)|警告|受信コンテキストでエラーが発生しました。|ServiceModel|  
|[3303 - ReceiveContextAbandonWithException](3303-receivecontextabandonwithexception.md)|Information|例外 %2 が発生したため、%1 が破棄されました。|チャネル|  
|[3305 - ClientBaseCachedChannelFactoryCount](3305-clientbasecachedchannelfactorycount.md)|Information|キャッシュされたチャネル ファクトリの数は '%1' です。  最大で '%2' 個のチャネル ファクトリをキャッシュできます。|ServiceModel|  
|[3306 - ClientBaseChannelFactoryAgedOutofCache](3306-clientbasechannelfactoryagedoutofcache.md)|Information|キャッシュが上限の '%1' に達したため、チャネル ファクトリがキャッシュから削除されました。|ServiceModel|  
|[3307 - ClientBaseChannelFactoryCacheHit](3307-clientbasechannelfactorycachehit.md)|Information|キャッシュで見つかった一致するチャネル ファクトリが使用されました。|ServiceModel|  
|[3308 - ClientBaseUsingLocalChannelFactory](3308-clientbaseusinglocalchannelfactory.md)|Information|キャッシュのチャネル ファクトリは使用されません (つまり、インスタンスのキャッシュは無効になっています)。|ServiceModel|  
|[3309 - QueryCompositionExecuted](3309-querycompositionexecuted.md)|Information|'%1' を使用したクエリの構成が要求 URI: '%2' で実行されました。|ServiceModel|  
|[3310 - DispatchFailed](3310-dispatchfailed.md)|エラー|'%1' 操作のディスパッチでエラーが発生しました。|ServiceModel|  
|[3311 - DispatchSuccessful](3311-dispatchsuccessful.md)|Information|'%1' 操作が正常にディスパッチされました。|ServiceModel|  
|[3312 - MessageReadByEncoder](3312-messagereadbyencoder.md)|Information|サイズ '%1' バイトのメッセージがエンコーダーによって読み取られました。|チャネル|  
|[3312 - MessageReadByEncoder](3312-messagereadbyencoder.md)|Information|サイズ '%1' バイトのメッセージがエンコーダーによって書き込まれました。|チャネル|  
|[3314 - SessionIdleTimeout](3314-sessionidletimeout.md)|エラー|URI: '%1' へのアイドル チャネルのセッションを中止しています。|ServiceModel|  
|[3319 - SocketAcceptEnqueued](3319-socketacceptenqueued.md)|"詳細"|接続の受け入れを開始しました。|TCP|  
|[3320 - SocketAccepted](3320-socketaccepted.md)|"詳細"|ListenerId:%1 が SocketId:%2 を受け入れました。|TCP|  
|[3321 - ConnectionPoolMiss](3321-connectionpoolmiss.md)|"詳細"|%1 のプールに使用可能な接続がありません。%2 個の接続がビジー状態です。|チャネル|  
|[3322 - DispatchFormatterDeserializeRequestStart](3322-dispatchformatterdeserializerequeststart.md)|"詳細"|ディスパッチャーが要求メッセージのシリアル化解除を開始しました。|ServiceModel|  
|[3323 - DispatchFormatterDeserializeRequestStop](3323-dispatchformatterdeserializerequeststop.md)|"詳細"|ディスパッチャーが要求メッセージのシリアル化解除を完了しました。|ServiceModel|  
|[3324 - DispatchFormatterSerializeReplyStart](3324-dispatchformatterserializereplystart.md)|"詳細"|ディスパッチャーが応答メッセージのシリアル化を開始しました。|ServiceModel|  
|[3325 - DispatchFormatterSerializeReplyStop](3325-dispatchformatterserializereplystop.md)|"詳細"|ディスパッチャーが応答メッセージのシリアル化を完了しました。|ServiceModel|  
|[3326 - ClientFormatterSerializeRequestStart](3326-clientformatterserializerequeststart.md)|"詳細"|クライアント要求のシリアル化を開始しました。|ServiceModel|  
|[3327 - ClientFormatterSerializeRequestStop](3327-clientformatterserializerequeststop.md)|"詳細"|クライアントが要求メッセージのシリアル化を完了しました。|ServiceModel|  
|[3328 - ClientFormatterDeserializeReplyStart](3328-clientformatterdeserializereplystart.md)|"詳細"|クライアントが応答メッセージのシリアル化解除を開始しました。|ServiceModel|  
|[3329 - ClientFormatterDeserializeReplyStop](3329-clientformatterdeserializereplystop.md)|"詳細"|クライアントが応答メッセージのシリアル化解除を完了しました。|ServiceModel|  
|[3330 - SecurityNegotiationStart](3330-securitynegotiationstart.md)|"詳細"|セキュリティ ネゴシエーションを開始しました。|セキュリティ|  
|[3331 - SecurityNegotiationStop](3331-securitynegotiationstop.md)|"詳細"|セキュリティ ネゴシエーションが完了しました。|セキュリティ|  
|[3332 - SecurityTokenProviderOpened](3332-securitytokenprovideropened.md)|"詳細"|SecurityTokenProvider のオープンが完了しました。|セキュリティ|  
|[3333 - OutgoingMessageSecured](3333-outgoingmessagesecured.md)|"詳細"|送信メッセージがセキュリティで保護されました。|セキュリティ|  
|[3334 - IncomingMessageVerified](3334-incomingmessageverified.md)|"詳細"|受信メッセージが確認されました。|セキュリティ ServiceModel|  
|[3335 - GetServiceInstanceStart](3335-getserviceinstancestart.md)|"詳細"|サービス インスタンスの取得を開始しました。|ServiceModel|  
|[3336 - GetServiceInstanceStop](3336-getserviceinstancestop.md)|"詳細"|サービス インスタンスが取得されました。|ServiceModel|  
|[3337 - ChannelReceiveStart](3337-channelreceivestart.md)|"詳細"|ChannelHandlerId:%1 - メッセージ受信ループを開始しました。|チャネル|  
|[3338 - ChannelReceiveStop](3338-channelreceivestop.md)|"詳細"|ChannelHandlerId:%1 - メッセージ受信ループを停止しました。|チャネル|  
|[3339 - ChannelFactoryCreated](3339-channelfactorycreated.md)|"詳細"|ChannelFactory が作成されました。|ServiceModel|  
|[3340 - PipeConnectionAcceptStart](3340-pipeconnectionacceptstart.md)|"詳細"|%1 でパイプ接続の受け入れを開始しました。|チャネル|  
|[3341 - PipeConnectionAcceptStop](3341-pipeconnectionacceptstop.md)|"詳細"|パイプ接続を受け入れました。|チャネル|  
|[3342 - EstablishConnectionStart](3342-establishconnectionstart.md)|"詳細"|%1 の接続の確立を開始しました。|チャネル|  
|[3343 - EstablishConnectionStop](3343-establishconnectionstop.md)|"詳細"|接続が確立されました。|チャネル|  
|[3345 - SessionPreambleUnderstood](3345-sessionpreambleunderstood.md)|"詳細"|'%1' のセッション プリアンブルが認識されました。|チャネル|  
|[3346 - ConnectionReaderSendFault](3346-connectionreadersendfault.md)|エラー|接続リーダーがエラー '%1' を送信しています。|チャネル|  
|[3347 - SocketAcceptClosed](3347-socketacceptclosed.md)|"詳細"|ソケットの受け入れを終了しました。|TCP|  
|[3348 - ServiceHostFaulted](3348-servicehostfaulted.md)|Critical|サービス ホストが途中終了しました。|TCP|  
|[3349 - ListenerOpenStart](3349-listeneropenstart.md)|"詳細"|'%1' のリスナーを開いています。|チャネル|  
|[3350 - ListenerOpenStop](3350-listeneropenstop.md)|"詳細"|リスナーのオープンが完了しました。|チャネル|  
|[3351 - ServerMaxPooledConnectionsQuotaReached](3351-servermaxpooledconnectionsquotareached.md)|"詳細"|サーバーのプールされた接続の最大クォータに達しました。|Quota|  
|[3352 - TcpConnectionTimedOut](3352-tcpconnectiontimedout.md)|エラー|リモート アドレス %2 への SocketId:%1 がタイムアウトしました。|TCP|  
|[3353 - TcpConnectionResetError](3353-tcpconnectionreseterror.md)|警告|リモート アドレス %2 への SocketId:%1 で接続リセット エラーが発生しました。|TCP|  
|[3354 - ServiceSecurityNegotiationCompleted](3354-servicesecuritynegotiationcompleted.md)|"詳細"|サービス セキュリティ ネゴシエーションが完了しました。|セキュリティ|  
|[3355 - SecurityNegotiationProcessingFailure](3355-securitynegotiationprocessingfailure.md)|エラー|セキュリティ ネゴシエーション処理が失敗しました。|セキュリティ|  
|[3356 - SecurityIdentityVerificationSuccess](3356-securityidentityverificationsuccess.md)|"詳細"|セキュリティ検証が成功しました。|セキュリティ|  
|[3357 - SecurityIdentityVerificationFailure](3357-securityidentityverificationfailure.md)|エラー|セキュリティ検証に失敗しました。|セキュリティ|  
|[3358 - PortSharingDuplicatedSocket](3358-portsharingduplicatedsocket.md)|"詳細"|%1 のソケットが複製されました。|ActivationServices|  
|[3359 - SecurityImpersonationSuccess](3359-securityimpersonationsuccess.md)|"詳細"|セキュリティの偽装に成功しました。|セキュリティ|  
|[3360 - SecurityImpersonationFailure](3360-securityimpersonationfailure.md)|警告|セキュリティの偽装に失敗しました。|セキュリティ|  
|[3361 - HttpChannelRequestAborted](3361-httpchannelrequestaborted.md)|警告|HTTP チャネルの要求が中止されました。|HTTP|  
|[3362 - HttpChannelResponseAborted](3362-httpchannelresponseaborted.md)|警告|HTTP チャネルの応答が中止されました。|HTTP|  
|[3363 - HttpAuthFailed](3363-httpauthfailed.md)|警告|HTTP 認証に失敗しました。|HTTP|  
|[3364 - SharedListenerProxyRegisterStart](3364-sharedlistenerproxyregisterstart.md)|"詳細"|URI '%1' の SharedListenerProxy の登録を開始しました。|ActivationServices|  
|[3365 - SharedListenerProxyRegisterStop](3365-sharedlistenerproxyregisterstop.md)|"詳細"|SharedListenerProxy の登録を停止します。|ActivationServices|  
|[3366 - SharedListenerProxyRegisterFailed](3366-sharedlistenerproxyregisterfailed.md)|エラー|SharedListenerProxy の登録は状態 '%1' で失敗しました。|ActivationServices|  
|[3367 - ConnectionPoolPreambleFailed](3367-connectionpoolpreamblefailed.md)|エラー|ConnectionPoolPreambleFailed。|チャネル|  
|[3368 - SslOnInitiateUpgrade](3368-ssloninitiateupgrade.md)|"詳細"|SslOnAcceptUpgradeStart|セキュリティ|  
|[3369 - SslOnAcceptUpgrade](3369-sslonacceptupgrade.md)|"詳細"|SslOnAcceptUpgradeStop|セキュリティ|  
|[3370 - BinaryMessageEncodingStart](3370-binarymessageencodingstart.md)|"詳細"|BinaryMessageEncoder がメッセージのエンコードを開始しました。|チャネル|  
|[3371 - MtomMessageEncodingStart](3371-mtommessageencodingstart.md)|"詳細"|MtomMessageEncoder がメッセージのエンコードを開始しました。|チャネル|  
|[3372 - TextMessageEncodingStart](3372-textmessageencodingstart.md)|"詳細"|TextMessageEncoder がメッセージのエンコードを開始しました。|チャネル|  
|[3373 - BinaryMessageDecodingStart](3373-binarymessagedecodingstart.md)|"詳細"|BinaryMessageEncoder がメッセージのデコードを開始しました。|チャネル|  
|[3374 - MtomMessageDecodingStart](3374-mtommessagedecodingstart.md)|"詳細"|MtomMessageEncoder がメッセージのデコードを開始しました。|チャネル|  
|[3375 - TextMessageDecodingStart](3375-textmessagedecodingstart.md)|"詳細"|TextMessageEncoder がメッセージのデコードを開始しました。|チャネル|  
|[3376 - HttpResponseReceiveStart](3376-httpresponsereceivestart.md)|Information|HTTP トランスポートがメッセージの受信を開始しました。|HTTP|  
|[3377 - SocketReadStop](3377-socketreadstop.md)|"詳細"|SocketId:%1 が '%3' から '%2' バイトを読み取りました。|TCP|  
|[3378 - SocketAsyncReadStop](3378-socketasyncreadstop.md)|"詳細"|SocketId:%1 が '%3' から '%2' バイトを読み取りました。|TCP|  
|[3379 - SocketWriteStart](3379-socketwritestart.md)|"詳細"|SocketId:%1 が '%3' に '%2' バイトを書き込んでいます。|TCP|  
|[3380 - SocketAsyncWriteStart](3380-socketasyncwritestart.md)|"詳細"|SocketId:%1 が '%3' に '%2' バイトを書き込んでいます。|TCP|  
|[3381 - SequenceAcknowledgementSent](3381-sequenceacknowledgementsent.md)|"詳細"|SessionId:%1 の受信確認が送信されました。|チャネル|  
|[3382 - ClientReliableSessionReconnect](3382-clientreliablesessionreconnect.md)|Information|SessionId:%1 を再接続します。|チャネル|  
|[3383 - ReliableSessionChannelFaulted](3383-reliablesessionchannelfaulted.md)|Information|SessionId:%1 でエラーが発生しました。|チャネル|  
|[3384 - WindowsStreamSecurityOnInitiateUpgrade](3384-windowsstreamsecurityoninitiateupgrade.md)|"詳細"|WindowsStreamSecurity がセキュリティ アップグレードを開始しています。|セキュリティ|  
|[3385 - WindowsStreamSecurityOnAcceptUpgrade](3385-windowsstreamsecurityonacceptupgrade.md)|"詳細"|Windows ストリーミング セキュリティがアップグレードを受け入れています。|セキュリティ|  
|[3386 - SocketConnectionAbort](3386-socketconnectionabort.md)|警告|SocketId:%1 を中止しています。|TCP|  
|[3388 - HttpGetContextStart](3388-httpgetcontextstart.md)|"詳細"|HttpGetContext を開始します。|HTTP|  
|[3389 - ClientSendPreambleStart](3389-clientsendpreamblestart.md)|"詳細"|クライアントがプリアンブルの送信を開始します。|チャネル|  
|[3390 - ClientSendPreambleStop](3390-clientsendpreamblestop.md)|"詳細"|クライアントがプリアンブルの送信を停止します。|チャネル|  
|[3391 - HttpMessageReceiveFailed](3391-httpmessagereceivefailed.md)|警告|HTTP メッセージの受信に失敗しました。|HTTP|  
|[3392 - TransactionScopeCreate](3392-transactionscopecreate.md)|Information|LocalIdentifier:'%1' および DistributedIdentifier:'%2' の TransactionScope を作成しています。|ServiceModel|  
|[3393 - StreamedMessageReadByEncoder](3393-streamedmessagereadbyencoder.md)|Information|エンコーダーによってストリーム メッセージが読み取られました。|チャネル|  
|[3394 - StreamedMessageWrittenByEncoder](3394-streamedmessagewrittenbyencoder.md)|Information|エンコーダーによってストリーム メッセージが書き込まれました。|チャネル|  
|[3395 - MessageWrittenAsynchronouslyByEncoder](3395-messagewrittenasynchronouslybyencoder.md)|Information|エンコーダーによってメッセージが非同期で書き込まれました。|チャネル|  
|[3396 - BufferedAsyncWriteStart](3396-bufferedasyncwritestart.md)|Information|BufferId:%1 から基になるストリームへの '%2' バイトの書き込みが完了しました。|チャネル|  
|[3397 - BufferedAsyncWriteStop](3397-bufferedasyncwritestop.md)|Information|エンコーダーによってメッセージが非同期で書き込まれました。|チャネル|  
|[3398 - PipeSharedMemoryCreated](3398-pipesharedmemorycreated.md)|"詳細"|パイプ共有メモリが '%1' に作成されました。|チャネル|  
|[3399 - NamedPipeCreated](3399-namedpipecreated.md)|"詳細"|NamedPipe '%1' が作成されました。|チャネル|  
|[3401 - SignatureVerificationStart](3401-signatureverificationstart.md)|"詳細"|署名の検証を開始しました。|セキュリティ|  
|[3402 - SignatureVerificationSuccess](3402-signatureverificationsuccess.md)|"詳細"|署名の検証に成功しました|セキュリティ|  
|[3403 - WrappedKeyDecryptionStart](3403-wrappedkeydecryptionstart.md)|"詳細"|ラップされたキーの解読を開始しました。|セキュリティ|  
|[3404 - WrappedKeyDecryptionSuccess](3404-wrappedkeydecryptionsuccess.md)|"詳細"|ラップされたキーの解読に成功しました。|セキュリティ|  
|[3405 - EncryptedDataProcessingStart](3405-encrypteddataprocessingstart.md)|"詳細"|暗号化されたデータの処理を開始しました。|セキュリティ|  
|[3406 - EncryptedDataProcessingSuccess](3406-encrypteddataprocessingsuccess.md)|"詳細"|暗号化されたデータの処理に成功しました。|セキュリティ|  
|[3407 - HttpPipelineProcessInboundRequestStart](3407-httppipelineprocessinboundrequeststart.md)|"詳細"|http メッセージ ハンドラーは、受信要求の処理を開始しました。|HTTP|  
|[3408 - HttpPipelineBeginProcessInboundRequestStart](3408-httppipelinebeginprocessinboundrequeststart.md)|"詳細"|http メッセージ ハンドラーは、受信要求の非同期処理を開始しました。|HTTP|  
|[3409 - HttpPipelineProcessInboundRequestStop](3409-httppipelineprocessinboundrequeststop.md)|"詳細"|http メッセージ ハンドラーは、受信要求の処理を完了しました。|HTTP|  
|[3410 - HttpPipelineFaulted](3410-httppipelinefaulted.md)|警告|http メッセージ ハンドラーに障害があります。|HTTP|  
|[3411 - HttpPipelineTimeoutException](3411-httppipelinetimeoutexception.md)|エラー|WebSocket の接続がタイムアウトしました。|HTTP|  
|[3412 - HttpPipelineProcessResponseStart](3412-httppipelineprocessresponsestart.md)|"詳細"|http メッセージ ハンドラーは、応答の処理を開始しました。|HTTP|  
|[3413 - HttpPipelineBeginProcessResponseStart](3413-httppipelinebeginprocessresponsestart.md)|"詳細"|http メッセージ ハンドラーは、応答の非同期処理を開始しました。|HTTP|  
|[3414 - HttpPipelineProcessResponseStop](3414-httppipelineprocessresponsestop.md)|"詳細"|http メッセージ ハンドラーは、応答の処理を完了しました。|HTTP|  
|[3415 - WebSocketConnectionRequestSendStart](3415-websocketconnectionrequestsendstart.md)|"詳細"|'%1' への WebSocket 接続要求の送信を開始します。|HTTP|  
|[3416 - WebSocketConnectionRequestSendStop](3416-websocketconnectionrequestsendstop.md)|"詳細"|WebSocketId:%1 の接続要求を送信しました。|HTTP|  
|[3417 - WebSocketConnectionAcceptStart](3417-websocketconnectionacceptstart.md)|"詳細"|WebSocket 接続の受け入れを開始します。|HTTP|  
|[3418 - WebSocketConnectionAccepted](3418-websocketconnectionaccepted.md)|"詳細"|WebSocketId:%1 の接続を受け入れました。|HTTP|  
|[3419 - WebSocketConnectionDeclined](3419-websocketconnectiondeclined.md)|エラー|WebSocket 接続が状態コード '%1' で拒否されました|HTTP|  
|[3420 - WebSocketConnectionFailed](3420-websocketconnectionfailed.md)|エラー|WebSocket 接続要求が失敗しました: '%1'|HTTP|  
|[3421 - WebSocketConnectionAborted](3421-websocketconnectionaborted.md)|エラー|WebSocketId:%1 の接続が中止されました。|HTTP|  
|[3422 - WebSocketAsyncWriteStart](3422-websocketasyncwritestart.md)|"詳細"|WebSocketId:%1 が '%3' に '%2' バイトを書き込んでいます。|HTTP|  
|[3423 - WebSocketAsyncWriteStop](3423-websocketasyncwritestop.md)|"詳細"|WebSocketId:%1 が非同期書き込みを停止します。|HTTP|  
|[3424 - WebSocketAsyncReadStart](3424-websocketasyncreadstart.md)|"詳細"|WebSocketId:%1 が読み取りを開始します。|HTTP|  
|[3425 - WebSocketAsyncReadStop](3425-websocketasyncreadstop.md)|"詳細"|WebSocketId:%1 が '%3' から '%2' バイトを読み取りました。|HTTP|  
|[3426 - WebSocketCloseSent](3426-websocketclosesent.md)|"詳細"|WebSocketId:%1 が、終了ステータス '%3' の終了メッセージを '%2' に送信しています。|HTTP|  
|[3427 - WebSocketCloseOutputSent](3427-websocketcloseoutputsent.md)|"詳細"|WebSocketId:%1 が、終了ステータス '%3' の終了出力メッセージを '%2' に送信しています。|HTTP|  
|[3428 - WebSocketConnectionClosed](3428-websocketconnectionclosed.md)|"詳細"|WebSocketId:%1 の接続を終了しました。|HTTP|  
|[3429 - WebSocketCloseStatusReceived](3429-websocketclosestatusreceived.md)|"詳細"|WebSocketId:%1 が、状態 '%2' の接続終了メッセージを受信しました。|HTTP|  
|[3430 - WebSocketUseVersionFromClientWebSocketFactory](3430-websocketuseversionfromclientwebsocketfactory.md)|"詳細"|型 '%1' のクライアント WebSocket ファクトリから WebSocketVersion を使用しています。|HTTP|  
|[3431 - WebSocketCreateClientWebSocketWithFactory](3431-websocketcreateclientwebsocketwithfactory.md)|"詳細"|型 '%1' のファクトリでクライアント WebSocket を作成しています。|HTTP|  
|[3553 - XamlServicesLoadStart](3553-xamlservicesloadstart.md)|Information|XamlServicesLoad が開始します|WebHost|  
|[3554 - XamlServicesLoadStop](3554-xamlservicesloadstop.md)|Information|XamlServicesLoad が停止します|WebHost|  
|[3555 - CreateWorkflowServiceHostStart](3555-createworkflowservicehoststart.md)|Information|CreateWorkflowServiceHost が開始します|WebHost|  
|[3556 - CreateWorkflowServiceHostStop](3556-createworkflowservicehoststop.md)|Information|CreateWorkflowServiceHost が停止します|WebHost|  
|[3558 - ServiceActivationStart](3558-serviceactivationstart.md)|Information|サービスのアクティブ化を開始します|WebHost|  
|[3559 - ServiceActivationStop](3559-serviceactivationstop.md)|Information|サービスのアクティブ化を停止します|WebHost|  
|[3560 - ServiceActivationAvailableMemory](3560-serviceactivationavailablememory.md)|"詳細"|使用可能なメモリ (バイト): %1|Quota|  
|[3800 - RoutingServiceClosingClient](3800-routingserviceclosingclient.md)|Information|ルーティング サービスがクライアント '%1' を終了しています。|RoutingServices|  
|[3800 - RoutingServiceClosingClient](3800-routingserviceclosingclient.md)|警告|ルーティング サービスのクライアント '%1' が途中終了しました。|RoutingServices|  
|[3802 - RoutingServiceCompletingOneWay](3802-routingservicecompletingoneway.md)|Information|ルーティング サービスの一方向メッセージを完了しています。|RoutingServices|  
|[3803 - RoutingServiceProcessingFailure](3803-routingserviceprocessingfailure.md)|エラー|アドレス '%1' のエンドポイントでメッセージを処理しているときにルーティング サービスでエラーが発生しました。|RoutingServices|  
|[3804 - RoutingServiceCreatingClientForEndpoint](3804-routingservicecreatingclientforendpoint.md)|Information|ルーティング サービスが、エンドポイント: '%1' のクライアントを作成しています。|RoutingServices|  
|[3805 - RoutingServiceDisplayConfig](3805-routingservicedisplayconfig.md)|"詳細"|ルーティング サービスは、RouteOnHeadersOnly: %1、SoapProcessingEnabled: %2、EnsureOrderedDispatch: %3 に構成されています。|RoutingServices|  
|[3807 - RoutingServiceCompletingTwoWay](3807-routingservicecompletingtwoway.md)|Information|ルーティング サービスの要求応答メッセージを完了しています。|RoutingServices|  
|[3809 - RoutingServiceMessageRoutedToEndpoints](3809-routingservicemessageroutedtoendpoints.md)|"詳細"|ルーティング サービスにより、ID: '%1' のメッセージが %2 エンドポイント リストにルーティングされました。|RoutingServices|  
|[3810 - RoutingServiceConfigurationApplied](3810-routingserviceconfigurationapplied.md)|Information|新しい RoutingConfiguration がルーティング サービスに適用されました。|RoutingServices|  
|[3815 - RoutingServiceProcessingMessage](3815-routingserviceprocessingmessage.md)|Information|ルーティング サービスが、トランザクション: %4 で受信された ID: '%1'、アクション: '%2'、着信 URL: '%3' のメッセージを処理しています。|RoutingServices|  
|[3816 - RoutingServiceTransmittingMessage](3816-routingservicetransmittingmessage.md)|Information|ルーティング サービスが、ID: '%1' [operation %2] のメッセージを '%3' に転送しています。|RoutingServices|  
|[3817 - RoutingServiceCommittingTransaction](3817-routingservicecommittingtransaction.md)|Information|ルーティング サービスが、ID: '%1' のトランザクションをコミットしています。|RoutingServices|  
|[3818 - RoutingServiceDuplexCallbackException](3818-routingserviceduplexcallbackexception.md)|エラー|ルーティング サービスのコンポーネント %1 で二重コールバックの例外が発生しました。|RoutingServices|  
|[3819 - RoutingServiceMovedToBackup](3819-routingservicemovedtobackup.md)|Information|ID: '%1' [operation %2] のルーティング サービス メッセージがバックアップ エンドポイント '%3' に移動されました。|RoutingServices|  
|[3820 - RoutingServiceCreatingTransaction](3820-routingservicecreatingtransaction.md)|Information|ルーティング サービスが、メッセージを処理するために ID '%1' の新しいトランザクションを作成しました。|RoutingServices|  
|[3821 - RoutingServiceCloseFailed](3821-routingserviceclosefailed.md)|警告|発信クライアント '%1' を終了しているときにルーティング サービスでエラーが発生しました。|RoutingServices|  
|[3822 - RoutingServiceSendingResponse](3822-routingservicesendingresponse.md)|Information|ルーティング サービスが、Action '%1' を含む応答メッセージを返送しています。|RoutingServices|  
|[3823 - RoutingServiceSendingFaultResponse](3823-routingservicesendingfaultresponse.md)|警告|ルーティング サービスが、Action '%1' を含むエラー応答メッセージを返送しています。|RoutingServices|  
|[3824 - RoutingServiceCompletingReceiveContext](3824-routingservicecompletingreceivecontext.md)|"詳細"|ルーティング サービスが、ID: '%1' のメッセージに対して ReceiveContext.Complete を呼び出しています。|RoutingServices|  
|[3825 - RoutingServiceAbandoningReceiveContext](3825-routingserviceabandoningreceivecontext.md)|警告|ルーティング サービスが、ID: '%1' のメッセージに対して ReceiveContext.Abandon を呼び出しています。|RoutingServices|  
|[3826 - RoutingServiceUsingExistingTransaction](3826-routingserviceusingexistingtransaction.md)|"詳細"|ルーティング サービスは、既存のトランザクション '%1' を使用してメッセージを送信します。|RoutingServices|  
|[3827 - RoutingServiceTransmitFailed](3827-routingservicetransmitfailed.md)|警告|'%1' への送信中にルーティング サービスでエラーが発生しました。|RoutingServices|  
|[3828 - RoutingServiceFilterTableMatchStart](3828-routingservicefiltertablematchstart.md)|Information|ルーティング サービス MessageFilterTable の照合が開始します。|RoutingServices|  
|[3829 - RoutingServiceFilterTableMatchStop](3829-routingservicefiltertablematchstop.md)|Information|ルーティング サービス MessageFilterTable の照合が停止します。|RoutingServices|  
|[3830 - RoutingServiceAbortingChannel](3830-routingserviceabortingchannel.md)|"詳細"|ルーティング サービスがチャネル '%1' で中止を呼び出しています。|RoutingServices|  
|[3831 - RoutingServiceHandledException](3831-routingservicehandledexception.md)|"詳細"|ルーティング サービスが例外を処理しました。|RoutingServices|  
|[3832 - RoutingServiceTransmitSucceeded](3832-routingservicetransmitsucceeded.md)|Information|ルーティング サービスが、ID: '%1 [operation %2] のメッセージを '%3' に正常に送信しました。|RoutingServices|  
|[4001 - TransportListenerSessionsReceived](4001-transportlistenersessionsreceived.md)|"詳細"|'%1' でトランスポート リスナー セッションを受信しました|ActivationServices|  
|[4002 - FailFastException](4002-failfastexception.md)|Critical|FailFastException。|ActivationServices|  
|[4003 - ServiceStartPipeError](4003-servicestartpipeerror.md)|エラー|サービス開始パイプ エラー。|ActivationServices|  
|[4008 - DispatchSessionStart](4008-dispatchsessionstart.md)|"詳細"|セッション ディスパッチを開始しました。|ActivationServices|  
|[4008 - DispatchSessionStart](4008-dispatchsessionstart.md)|警告|'%1' のセッション ディスパッチに失敗しました。保留セッション キューがいっぱいです。保留中の項目が '%2' 個あります。|ActivationServices|  
|[4011 - MessageQueueRegisterStart](4011-messagequeueregisterstart.md)|"詳細"|メッセージ キューの登録を開始します。|ActivationServices|  
|[4012 - MessageQueueRegisterAbort](4012-messagequeueregisterabort.md)|エラー|URI:'%2' のメッセージ キューの登録が状態:'%1' で中止されました。|ActivationServices|  
|[4013 - MessageQueueUnregisterSucceeded](4013-messagequeueunregistersucceeded.md)|"詳細"|URI:'%1' のメッセージ キューの登録解除に成功しました。|ActivationServices|  
|[4014 - MessageQueueRegisterFailed](4014-messagequeueregisterfailed.md)|エラー|URI:'%1' のメッセージ キューの登録が状態:'%2' で失敗しました。|ActivationServices|  
|[4015 - MessageQueueRegisterCompleted](4015-messagequeueregistercompleted.md)|Information|URI '%1' のメッセージ キューの登録が完了しました。|ActivationServices|  
|[4016 - MessageQueueDuplicatedSocketError](4016-messagequeueduplicatedsocketerror.md)|エラー|メッセージ キューがソケットの複製に失敗しました。|ActivationServices|  
|[4019 - MessageQueueDuplicatedSocketComplete](4019-messagequeueduplicatedsocketcomplete.md)|"詳細"|MessageQueueDuplicatedSocketComplete|ActivationServices|  
|[4020 - TcpTransportListenerListeningStart](4020-tcptransportlistenerlisteningstart.md)|"詳細"|TCP トランスポート リスナーが URI: '%1' でリッスンを開始しています。|ActivationServices|  
|[4021 - TcpTransportListenerListeningStop](4021-tcptransportlistenerlisteningstop.md)|"詳細"|TCP トランスポート リスナーがリッスンしています。|ActivationServices|  
|[4022 - WebhostUnregisterProtocolFailed](4022-webhostunregisterprotocolfailed.md)|エラー|エラー コード:%1|ActivationServices|  
|[4023 - WasCloseAllListenerChannelInstancesCompleted](4023-wasclosealllistenerchannelinstancescompleted.md)|Information|WAS がすべてのリスナー チャネル インスタンスのクローズを完了しました。|ActivationServices|  
|[4024 - WasCloseAllListenerChannelInstancesFailed](4024-wasclosealllistenerchannelinstancesfailed.md)|エラー|エラー コード:%1|ActivationServices|  
|[4025 - OpenListenerChannelInstanceFailed](4025-openlistenerchannelinstancefailed.md)|エラー|エラー コード:%1|ActivationServices|  
|[4026 - WasConnected](4026-wasconnected.md)|"詳細"|WAS が接続されました。|ActivationServices|  
|[4027 - WasDisconnected](4027-wasdisconnected.md)|"詳細"|WAS の接続が解除されました。|ActivationServices|  
|[4028 - PipeTransportListenerListeningStart](4028-pipetransportlistenerlisteningstart.md)|"詳細"|パイプ トランスポート リスナーが URI:%1 でリッスンを開始します。|ActivationServices|  
|[4029 - PipeTransportListenerListeningStop](4029-pipetransportlistenerlisteningstop.md)|"詳細"|パイプ トランスポート リスナーがリッスンを停止します。|ActivationServices|  
|[4030 - DispatchSessionSuccess](4030-dispatchsessionsuccess.md)|Information|セッション ディスパッチに成功しました。|ActivationServices|  
|[4031 - DispatchSessionFailed](4031-dispatchsessionfailed.md)|エラー|セッション ディスパッチに失敗しました。|ActivationServices|  
|[4032 - WasConnectionTimedout](4032-wasconnectiontimedout.md)|Critical|WAS の接続がタイムアウトしました。|ActivationServices|  
|[4033 - RoutingTableLookupStart](4033-routingtablelookupstart.md)|"詳細"|ルーティング テーブルの参照を開始しました。|ActivationServices|  
|[4034 - RoutingTableLookupStop](4034-routingtablelookupstop.md)|"詳細"|ルーティング テーブルの参照が完了しました。|ActivationServices|  
|[4035 - PendingSessionQueueRatio](4035-pendingsessionqueueratio.md)|"詳細"|保留セッション キューの比率: %1/%2|Quota|  
|[4600 - MessageLogEventSizeExceeded](4600-messagelogeventsizeexceeded.md)|警告|メッセージが ETW イベントのサイズを上回っているため、メッセージをログに記録できませんでした|WCFMessageLogging|  
|[4801 - DiscoveryClientInClientChannelFailedToClose](4801-discoveryclientinclientchannelfailedtoclose.md)|警告|DiscoveryClientChannel 内で作成された DiscoveryClient を閉じることができず、異常終了しました。|探索|  
|[4802 - DiscoveryClientProtocolExceptionSuppressed](4802-discoveryclientprotocolexceptionsuppressed.md)|Information|DiscoveryClient を閉じているときに ProtocolException が抑制されました。 その理由として、DiscoveryService がまだ DiscoveryClient に応答を送信しようとしていることが考えられます。|探索|  
|[4803 - DiscoveryClientReceivedMulticastSuppression](4803-discoveryclientreceivedmulticastsuppression.md)|Information|DiscoveryClient は DiscoveryProxy からマルチキャスト抑制メッセージを受け取りました。|探索|  
|[4804 - DiscoveryMessageReceivedAfterOperationCompleted](4804-discoverymessagereceivedafteroperationcompleted.md)|Information|messageId='%2' の %1 メッセージは、対応する %3 操作が完了したため、DiscoveryClient によってドロップされました。|探索|  
|[4805 - DiscoveryMessageWithInvalidContent](4805-discoverymessagewithinvalidcontent.md)|警告|messageId='%2' の %1 メッセージは、無効なコンテンツがあったため、ドロップされました。|探索|  
|[4806 - DiscoveryMessageWithInvalidRelatesToOrOperationCompleted](4806-discoverymessagewithinvalidrelatestooroperationcompleted.md)|警告|messageId='%2' および relatesTo='%3' の %1 メッセージは、対応する %4 操作が完了したか、relatesTo 値が無効であるため、DiscoveryClient によってドロップされました。|探索|  
|[4807 - DiscoveryMessageWithInvalidReplyTo](4807-discoverymessagewithinvalidreplyto.md)|警告|messageId='%1' の探索要求メッセージは、無効な ReplyTo アドレスがあったため、ドロップされました。|探索|  
|[4808 - DiscoveryMessageWithNoContent](4808-discoverymessagewithnocontent.md)|警告|%1 メッセージは、コンテンツがなかったため、ドロップされました。|探索|  
|[4809 - DiscoveryMessageWithNullMessageId](4809-discoverymessagewithnullmessageid.md)|警告|%1 メッセージは、メッセージ ヘッダーに必要な MessageId プロパティが含まれていなかったため、ドロップされました。|探索|  
|[4810 - DiscoveryMessageWithNullMessageSequence](4810-discoverymessagewithnullmessagesequence.md)|警告|messageId='%2' の %1 メッセージは、DiscoveryMessageSequence プロパティがなかったため、DiscoveryClient によってドロップされました。|探索|  
|[4811 - DiscoveryMessageWithNullRelatesTo](4811-discoverymessagewithnullrelatesto.md)|警告|messageId='%2' の %1 メッセージは、メッセージ ヘッダーに必要な RelatesTo プロパティが含まれていなかったため、DiscoveryClient によってドロップされました。|探索|  
|[4812 - DiscoveryMessageWithNullReplyTo](4812-discoverymessagewithnullreplyto.md)|警告|messageId='%1' の探索要求メッセージは、ReplyTo アドレスがなかったため、ドロップされました。|探索|  
|[4813 - DuplicateDiscoveryMessage](4813-duplicatediscoverymessage.md)|警告|messageId='%2' の %1 メッセージは、重複していたため、ドロップされました。|探索|  
|[4814 - EndpointDiscoverabilityDisabled](4814-endpointdiscoverabilitydisabled.md)|Information|EndpointAddress='%1' および ListenUri='%2' のエンドポイントの探索が無効になりました。|探索|  
|[4814 - EndpointDiscoverabilityDisabled](4814-endpointdiscoverabilitydisabled.md)|Information|EndpointAddress='%1' および ListenUri='%2' のエンドポイントの探索が有効になりました。|探索|  
|[4816 - FindInitiatedInDiscoveryClientChannel](4816-findinitiatedindiscoveryclientchannel.md)|"詳細"|エンドポイントを探索するために、Find 操作が DiscoveryClientChannel で開始されました。|探索|  
|[4817 - InnerChannelCreationFailed](4817-innerchannelcreationfailed.md)|警告|DiscoveryClientChannel は、EndpointAddress='%1' および Via='%2' の探索されたエンドポイントを使用して、チャネルを作成できませんでした。 DiscoveryClientChannel は、次に使用可能な探索されたエンドポイントを使用します。|探索|  
|[4818 - InnerChannelOpenFailed](4818-innerchannelopenfailed.md)|警告|DiscoveryClientChannel は、EndpointAddress='%1' および Via='%2' の探索されたエンドポイントを使用して、チャネルを開くことができませんでした。 DiscoveryClientChannel は、次に使用可能な探索されたエンドポイントを使用します。|探索|  
|[4819 - InnerChannelOpenSucceeded](4819-innerchannelopensucceeded.md)|Information|DiscoveryClientChannel は正常にエンドポイントを探索し、それを使用してチャネルを開きました。 クライアントは EndpointAddress='%1' および Via='%2' を使用して、サービスに接続されています。|探索|  
|[4820 - SynchronizationContextReset](4820-synchronizationcontextreset.md)|Information|SynchronizationContext は DiscoveryClientChannel によって、元の値 %1 にリセットされました。|探索|  
|[4821 - SynchronizationContextSetToNull](4821-synchronizationcontextsettonull.md)|Information|SynchronizationContext は、Find 操作を開始する前に、DiscoveryClientChannel によって NULL に設定されました。|探索|  
|[5001 - DCSerializeWithSurrogateStart](5001-dcserializewithsurrogatestart.md)|"詳細"|DataContract のサロゲートによる %1 のシリアル化を開始します。|シリアル化|  
|[5002 - DCSerializeWithSurrogateStop](5002-dcserializewithsurrogatestop.md)|"詳細"|DataContract のサロゲートによるシリアル化を停止します。|シリアル化|  
|[5003 - DCDeserializeWithSurrogateStart](5003-dcdeserializewithsurrogatestart.md)|"詳細"|DataContract のサロゲートによる %1 のシリアル化解除を開始します。|シリアル化|  
|[5004 - DCDeserializeWithSurrogateStop](5004-dcdeserializewithsurrogatestop.md)|"詳細"|DataContract のサロゲートによるシリアル化解除を停止します。|シリアル化|  
|[5005 - ImportKnownTypesStart](5005-importknowntypesstart.md)|"詳細"|ImportKnownTypes を開始します。|シリアル化|  
|[5006 - ImportKnownTypesStop](5006-importknowntypesstop.md)|"詳細"|ImportKnownTypes を停止します。|シリアル化|  
|[5007 - DCResolverResolve](5007-dcresolverresolve.md)|"詳細"|DataContract リゾルバーが %1 の解決を開始します。|シリアル化|  
|[5008 - DCGenWriterStart](5008-dcgenwriterstart.md)|"詳細"|DataContract の %2 の %1 ライターの生成を開始します。|シリアル化|  
|[5009 - DCGenWriterStop](5009-dcgenwriterstop.md)|"詳細"|DataContract のライターの生成を停止します。|シリアル化|  
|[5010 - DCGenReaderStart](5010-dcgenreaderstart.md)|"詳細"|DataContract の %2 の %1 リーダーの生成を開始します。|シリアル化|  
|[5011 - DCGenReaderStop](5011-dcgenreaderstop.md)|"詳細"|DataContract の生成を停止します。|シリアル化|  
|[5012 - DCJsonGenReaderStart](5012-dcjsongenreaderstart.md)|"詳細"|Json の %2 の %1 リーダーの生成を開始します。|シリアル化|  
|[5013 - DCJsonGenReaderStop](5013-dcjsongenreaderstop.md)|"詳細"|Json のリーダーの生成を停止します。|シリアル化|  
|[5014 - DCJsonGenWriterStart](5014-dcjsongenwriterstart.md)|"詳細"|Json の %2 の %1 ライターの生成を開始します。|シリアル化|  
|[5015 - DCJsonGenWriterStop](5015-dcjsongenwriterstop.md)|"詳細"|Json のライターの生成を停止します。|シリアル化|  
|[5016 - GenXmlSerializableStart](5016-genxmlserializablestart.md)|"詳細"|'%1' の XML シリアル化が可能な要素の生成を開始します。|シリアル化|  
|[5017 - GenXmlSerializableStop](5017-genxmlserializablestop.md)|"詳細"|XML シリアル化が可能な要素の生成を停止します。|シリアル化|  
|[5203 - JsonMessageDecodingStart](5203-jsonmessagedecodingstart.md)|"詳細"|JsonMessageEncoder がメッセージのデコードを開始しました。|チャネル|  
|[5204 - JsonMessageEncodingStart](5204-jsonmessageencodingstart.md)|"詳細"|JsonMessageEncoder がメッセージのエンコードを開始しました。|チャネル|  
|[5402 - TokenValidationStarted](5402-tokenvalidationstarted.md)|"詳細"|SecurityToken (型 '%1'、ID '%2') の検証を開始しました。|セキュリティ|  
|[5403 - TokenValidationSuccess](5403-tokenvalidationsuccess.md)|"詳細"|SecurityToken (型 '%1'、ID '%2') の検証に成功しました。|セキュリティ|  
|[5404 - TokenValidationFailure](5404-tokenvalidationfailure.md)|エラー|SecurityToken (型 '%1'、ID '%2') の検証に失敗しました。 %3|セキュリティ|  
|[5405 - GetIssuerNameSuccess](5405-getissuernamesuccess.md)|"詳細"|トークン ID: %2 からの発行者名: %1 の取得に成功しました。|セキュリティ|  
|[5406 - GetIssuerNameFailure](5406-getissuernamefailure.md)|エラー|トークン ID: %1 からの発行者名の取得に失敗しました。|セキュリティ|  
|[5600 - FederationMessageProcessingStarted](5600-federationmessageprocessingstarted.md)|"詳細"|フェデレーション メッセージの処理を開始しました。|セキュリティ|  
|[5601 - FederationMessageProcessingSuccess](5601-federationmessageprocessingsuccess.md)|"詳細"|フェデレーション メッセージの処理に成功しました。|セキュリティ|  
|[5602 - FederationMessageCreationStarted](5602-federationmessagecreationstarted.md)|"詳細"|フォーム ポストからのフェデレーション メッセージの作成を開始しました。|セキュリティ|  
|[5603 - FederationMessageCreationSuccess](5603-federationmessagecreationsuccess.md)|"詳細"|フォーム ポストからのフェデレーション メッセージの作成に成功しました。|セキュリティ|  
|[5604 - SessionCookieReadingStarted](5604-sessioncookiereadingstarted.md)|"詳細"|セッション クッキーからのセッション トークンの読み取りを開始しました。|セキュリティ|  
|[5605 - SessionCookieReadingSuccess](5605-sessioncookiereadingsuccess.md)|"詳細"|セッション クッキーからのセッション トークンの読み取りに成功しました。|セキュリティ|  
|[5606 - PrincipalSettingFromSessionTokenStarted](5606-principalsettingfromsessiontokenstarted.md)|"詳細"|セッション トークンからのプリンシパルの設定を開始しました。|セキュリティ|  
|[5607 - PrincipalSettingFromSessionTokenSuccess](5607-principalsettingfromsessiontokensuccess.md)|"詳細"|セッション トークンからのプリンシパルの設定に成功しました。|セキュリティ|  
|[57393 - AppDomainUnload](57393-appdomainunload.md)|Information|AppDomain をアンロードしています。 AppDomain.FriendlyName %1、ProcessName %2、ProcessId %3。|インフラストラクチャ|  
|[57394 - HandledException](57394-handledexception.md)|Information|例外を処理しています。|インフラストラクチャ|  
|[57395 - ShipAssertExceptionMessage](57395-shipassertexceptionmessage.md)|エラー|予期しないエラーが発生しました。 アプリケーションではこのエラーを処理することはできません。 診断上の目的から、次の英語のメッセージがエラーに関連付けられています: %1。|インフラストラクチャ|  
|[57396 - ThrowingException](57396-throwingexception.md)|警告|例外をスローしています。 発生元 %1。|インフラストラクチャ|  
|[57397 - UnhandledException](57397-unhandledexception.md)|Critical|ハンドルされていない例外です。|インフラストラクチャ|  
|[57399 - TraceCodeEventLogCritical](57399-tracecodeeventlogcritical.md)|Critical|イベント ログに書き込みました。|インフラストラクチャ|  
|[57400 - TraceCodeEventLogError](57400-tracecodeeventlogerror.md)|エラー|イベント ログに書き込みました。|インフラストラクチャ|  
|[57401 - TraceCodeEventLogInfo](57401-tracecodeeventloginfo.md)|Information|イベント ログに書き込みました。|インフラストラクチャ|  
|[57402 - TraceCodeEventLogVerbose](57402-tracecodeeventlogverbose.md)|"詳細"|イベント ログに書き込みました。|インフラストラクチャ|  
|[57403 - TraceCodeEventLogWarning](57403-tracecodeeventlogwarning.md)|警告|イベント ログに書き込みました。|インフラストラクチャ|  
|[57404 - HandledExceptionWarning](57404-handledexceptionwarning.md)|警告|例外を処理しています。|インフラストラクチャ|  
|[62326 - HttpHandlerPickedForUrl](62326-httphandlerpickedforurl.md)|Information|URL '%1' は、ルート要素型 '%2' の XAML ドキュメントをホストします。 この URL に対して行われるすべての要求を処理するために、HTTP ハンドラー型 '%3' が選択されています。|WebHost|
