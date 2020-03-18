---
title: ICorProfilerCallback インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback
helpviewer_keywords:
- ICorProfilerCallback interface [.NET Framework profiling]
ms.assetid: 4bae06f7-94d7-4ba8-b250-648b2da78674
topic_type:
- apiref
ms.openlocfilehash: 891cca8ac47a3f8391bd7ab7b27b35d6318bbe0a
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866290"
---
# <a name="icorprofilercallback-interface"></a>ICorProfilerCallback インターフェイス
プロファイラーがサブスクライブしたイベントが発生したときにコードプロファイラーに通知するために、共通言語ランタイム (CLR) によって使用されるメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[AppDomainCreationFinished メソッド](icorprofilercallback-appdomaincreationfinished-method.md)|アプリケーションドメインが作成されたことをプロファイラーに通知します。|  
|[AppDomainCreationStarted メソッド](icorprofilercallback-appdomaincreationstarted-method.md)|アプリケーションドメインが作成中であることをプロファイラーに通知します。|  
|[AppDomainShutdownFinished メソッド](icorprofilercallback-appdomainshutdownfinished-method.md)|アプリケーションドメインがプロセスからアンロードされたことをプロファイラーに通知します。|  
|[AppDomainShutdownStarted メソッド](icorprofilercallback-appdomainshutdownstarted-method.md)|アプリケーションドメインがプロセスからアンロードされていることをプロファイラーに通知します。|  
|[AssemblyLoadFinished メソッド](icorprofilercallback-assemblyloadfinished-method.md)|アセンブリの読み込みが完了したことをプロファイラーに通知します。|  
|[AssemblyLoadStarted メソッド](icorprofilercallback-assemblyloadstarted-method.md)|アセンブリが読み込まれていることをプロファイラーに通知します。|  
|[AssemblyUnloadFinished メソッド](icorprofilercallback-assemblyunloadfinished-method.md)|アセンブリがアンロードされたことをプロファイラーに通知します。|  
|[AssemblyUnloadStarted メソッド](icorprofilercallback-assemblyunloadstarted-method.md)|アセンブリがアンロードされていることをプロファイラーに通知します。|  
|[ClassLoadFinished メソッド](icorprofilercallback-classloadfinished-method.md)|クラスが読み込みを完了したことをプロファイラーに通知します。|  
|[ClassLoadStarted メソッド](icorprofilercallback-classloadstarted-method.md)|クラスが読み込まれていることをプロファイラーに通知します。|  
|[ClassUnloadFinished メソッド](icorprofilercallback-classunloadfinished-method.md)|クラスのアンロードが終了したことをプロファイラーに通知します。|  
|[ClassUnloadStarted メソッド](icorprofilercallback-classunloadstarted-method.md)|クラスがアンロードされていることをプロファイラーに通知します。|  
|[COMClassicVTableCreated メソッド](icorprofilercallback-comclassicvtablecreated-method.md)|指定した IID およびクラスのランタイム呼び出し可能ラッパー (RCW) が作成されたことをプロファイラーに通知します。|  
|[COMClassicVTableDestroyed メソッド](icorprofilercallback-comclassicvtabledestroyed-method.md)|RCW が破棄されていることをプロファイラーに通知します。|  
|[ExceptionCatcherEnter メソッド](icorprofilercallback-exceptioncatcherenter-method.md)|適切な `catch` ブロックに制御が渡されていることをプロファイラーに通知します。|  
|[ExceptionCatcherLeave メソッド](icorprofilercallback-exceptioncatcherleave-method.md)|適切な `catch` ブロックから制御が渡されていることをプロファイラーに通知します。|  
|[ExceptionCLRCatcherExecute メソッド](icorprofilercallback-exceptionclrcatcherexecute-method.md)|.NET Framework バージョン2.0 で廃止されました。|  
|[ExceptionCLRCatcherFound メソッド](icorprofilercallback-exceptionclrcatcherfound-method.md)|.NET Framework 2.0 で廃止されました。|  
|[ExceptionOSHandlerEnter メソッド](icorprofilercallback-exceptionoshandlerenter-method.md)|実装されていません。 アンマネージ例外情報を必要とするプロファイラーは、他の方法でこの情報を取得する必要があります。|  
|[ExceptionOSHandlerLeave メソッド](icorprofilercallback-exceptionoshandlerleave-method.md)|実装されていません。 アンマネージ例外情報を必要とするプロファイラーは、他の方法でこの情報を取得する必要があります。|  
|[ExceptionSearchCatcherFound メソッド](icorprofilercallback-exceptionsearchcatcherfound-method.md)|例外処理の検索フェーズによってスローされた例外のハンドラーが見つかったことをプロファイラーに通知します。|  
|[ExceptionSearchFilterEnter メソッド](icorprofilercallback-exceptionsearchfilterenter-method.md)|ユーザーフィルターが実行されていることをプロファイラーに通知します。|  
|[ExceptionSearchFilterLeave メソッド](icorprofilercallback-exceptionsearchfilterleave-method.md)|ユーザーフィルターの実行が完了したことをプロファイラーに通知します。|  
|[ExceptionSearchFunctionEnter メソッド](icorprofilercallback-exceptionsearchfunctionenter-method.md)|例外処理の検索フェーズが関数に入ったことをプロファイラーに通知します。|  
|[ExceptionSearchFunctionLeave メソッド](icorprofilercallback-exceptionsearchfunctionleave-method.md)|例外処理の検索フェーズで関数の検索が終了したことをプロファイラーに通知します。|  
|[ExceptionThrown メソッド](icorprofilercallback-exceptionthrown-method.md)|例外がスローされたことをプロファイラーに通知します。|  
|[ExceptionUnwindFinallyEnter メソッド](icorprofilercallback-exceptionunwindfinallyenter-method.md)|例外処理のアンワインドフェーズが、指定された関数に含まれる `finally` 句を入力していることをプロファイラーに通知します。|  
|[ExceptionUnwindFinallyLeave メソッド](icorprofilercallback-exceptionunwindfinallyleave-method.md)|例外処理のアンワインドフェーズが `finally` 句の後にあることをプロファイラーに通知します。|  
|[ExceptionUnwindFunctionEnter メソッド](icorprofilercallback-exceptionunwindfunctionenter-method.md)|例外処理のアンワインドフェーズが関数に入ったことをプロファイラーに通知します。|  
|[ExceptionUnwindFunctionLeave メソッド](icorprofilercallback-exceptionunwindfunctionleave-method.md)|例外処理のアンワインドフェーズが関数のアンワインドを完了したことをプロファイラーに通知します。|  
|[FunctionUnloadStarted メソッド](icorprofilercallback-functionunloadstarted-method.md)|ランタイムが関数のアンロードを開始したことをプロファイラーに通知します。|  
|[Initialize メソッド](icorprofilercallback-initialize-method.md)|新しい CLR アプリケーションが開始されるたびにプロファイラーを初期化するために呼び出されます。|  
|[JITCachedFunctionSearchFinished メソッド](icorprofilercallback-jitcachedfunctionsearchfinished-method.md)|以前に Ngen.exe を使用してコンパイルされた関数の検索が終了したことをプロファイラーに通知します。|  
|[JITCachedFunctionSearchStarted メソッド](icorprofilercallback-jitcachedfunctionsearchstarted-method.md)|以前に Ngen.exe を使用してコンパイルされた関数に対して検索が開始されたことをプロファイラーに通知します。|  
|[JITCompilationFinished メソッド](icorprofilercallback-jitcompilationfinished-method.md)|JIT コンパイラが関数のコンパイルを完了したことをプロファイラーに通知します。|  
|[JITCompilationStarted メソッド](icorprofilercallback-jitcompilationstarted-method.md)|Just-in-time (JIT) コンパイラが関数のコンパイルを開始したことをプロファイラーに通知します。|  
|[JITFunctionPitched メソッド](icorprofilercallback-jitfunctionpitched-method.md)|JIT コンパイル済みの関数がメモリから削除されたことをプロファイラーに通知します。|  
|[JITInlining メソッド](icorprofilercallback-jitinlining-method.md)|JIT コンパイラが別の関数と共に関数を挿入しようとしていることをプロファイラーに通知します。|  
|[ManagedToUnmanagedTransition メソッド](icorprofilercallback-managedtounmanagedtransition-method.md)|マネージコードからアンマネージコードへの遷移が発生したことをプロファイラーに通知します。|  
|[ModuleAttachedToAssembly メソッド](icorprofilercallback-moduleattachedtoassembly-method.md)|モジュールが親アセンブリにアタッチされていることをプロファイラーに通知します。|  
|[ModuleLoadFinished メソッド](icorprofilercallback-moduleloadfinished-method.md)|モジュールが読み込みを終了したことをプロファイラーに通知します。|  
|[ModuleLoadStarted メソッド](icorprofilercallback-moduleloadstarted-method.md)|モジュールが読み込まれていることをプロファイラーに通知します。|  
|[ModuleUnloadFinished メソッド](icorprofilercallback-moduleunloadfinished-method.md)|モジュールがアンロードを終了したことをプロファイラーに通知します。|  
|[ModuleUnloadStarted メソッド](icorprofilercallback-moduleunloadstarted-method.md)|モジュールがアンロードされていることをプロファイラーに通知します。|  
|[MovedReferences メソッド](icorprofilercallback-movedreferences-method.md)|ガベージコレクション中に移動されたオブジェクト参照をプロファイラーに通知します。|  
|[ObjectAllocated メソッド](icorprofilercallback-objectallocated-method.md)|ヒープ内のメモリがオブジェクトに割り当てられたことをプロファイラーに通知します。|  
|[ObjectReferences メソッド](icorprofilercallback-objectreferences-method.md)|指定したオブジェクトによって参照されるメモリ内のオブジェクトに関する情報をプロファイラーに通知します。|  
|[ObjectsAllocatedByClass メソッド](icorprofilercallback-objectsallocatedbyclass-method.md)|前のガベージコレクションの後に作成された、指定した各クラスのインスタンスの数をプロファイラーに通知します。|  
|[RemotingClientInvocationFinished メソッド](icorprofilercallback-remotingclientinvocationfinished-method.md)|リモート処理呼び出しがクライアントで完了まで実行されたことをプロファイラーに通知します。|  
|[RemotingClientInvocationStarted メソッド](icorprofilercallback-remotingclientinvocationstarted-method.md)|リモート処理呼び出しが開始されたことをプロファイラーに通知します。|  
|[RemotingClientReceivingReply メソッド](icorprofilercallback-remotingclientreceivingreply-method.md)|リモート処理呼び出しのサーバー側の部分が完了し、クライアントが応答を受信および処理するようになったことをプロファイラーに通知します。|  
|[RemotingClientSendingMessage メソッド](icorprofilercallback-remotingclientsendingmessage-method.md)|クライアントがサーバーに要求を送信していることをプロファイラーに通知します。|  
|[RemotingServerInvocationReturned メソッド](icorprofilercallback-remotingserverinvocationreturned-method.md)|プロセスがリモートメソッド呼び出し要求に応答してメソッドの呼び出しを完了したことをプロファイラーに通知します。|  
|[RemotingServerInvocationStarted メソッド](icorprofilercallback-remotingserverinvocationstarted-method.md)|プロセスがリモートメソッド呼び出し要求に応答してメソッドを呼び出していることをプロファイラーに通知します。|  
|[RemotingServerReceivingMessage メソッド](icorprofilercallback-remotingserverreceivingmessage-method.md)|プロセスがリモートメソッド呼び出しまたはアクティブ化要求を受信していることをプロファイラーに通知します。|  
|[RemotingServerSendingReply メソッド](icorprofilercallback-remotingserversendingreply-method.md)|プロセスがリモートメソッド呼び出し要求の処理を完了したこと、およびチャネルを介して応答を送信しようとしていることをプロファイラーに通知します。|  
|[RootReferences メソッド](icorprofilercallback-rootreferences-method.md)|ガベージコレクション後のルート参照に関する情報をプロファイラーに通知します。|  
|[RuntimeResumeFinished メソッド](icorprofilercallback-runtimeresumefinished-method.md)|ランタイムがすべてのランタイムスレッドを再開し、通常の動作に戻ったことをプロファイラーに通知します。|  
|[RuntimeResumeStarted メソッド](icorprofilercallback-runtimeresumestarted-method.md)|ランタイムがすべてのランタイムスレッドを再開していることをプロファイラーに通知します。|  
|[RuntimeSuspendAborted メソッド](icorprofilercallback-runtimesuspendaborted-method.md)|実行中のランタイムの中断がランタイムによって中止されたことをプロファイラーに通知します。|  
|[RuntimeSuspendFinished メソッド](icorprofilercallback-runtimesuspendfinished-method.md)|ランタイムがすべてのランタイムスレッドの中断を完了したことをプロファイラーに通知します。|  
|[RuntimeSuspendStarted メソッド](icorprofilercallback-runtimesuspendstarted-method.md)|ランタイムがすべてのランタイムスレッドを中断しようとしていることをプロファイラーに通知します。|  
|[RuntimeThreadResumed メソッド](icorprofilercallback-runtimethreadresumed-method.md)|指定したスレッドが中断された後に再開されたことをプロファイラーに通知します。|  
|[RuntimeThreadSuspended メソッド](icorprofilercallback-runtimethreadsuspended-method.md)|指定したスレッドが中断されたか、中断されたことをプロファイラーに通知します。|  
|[Shutdown メソッド](icorprofilercallback-shutdown-method.md)|アプリケーションがシャットダウン中であることをプロファイラーに通知します。|  
|[ThreadAssignedToOSThread メソッド](icorprofilercallback-threadassignedtoosthread-method.md)|特定のオペレーティングシステム (OS) スレッドを使用してマネージスレッドが実装されていることをプロファイラーに通知します。|  
|[ThreadCreated メソッド](icorprofilercallback-threadcreated-method.md)|スレッドが作成されたことをプロファイラーに通知します。|  
|[ThreadDestroyed メソッド](icorprofilercallback-threaddestroyed-method.md)|スレッドが破棄されたことをプロファイラーに通知します。|  
|[UnmanagedToManagedTransition メソッド](icorprofilercallback-unmanagedtomanagedtransition-method.md)|アンマネージコードからマネージコードへの遷移が発生したことをプロファイラーに通知します。|  
  
## <a name="remarks"></a>コメント  
 CLR は、`ICorProfilerCallback` (または[ICorProfilerCallback2](icorprofilercallback2-interface.md)) インターフェイスのメソッドを呼び出して、プロファイラーがサブスクライブしているイベントが発生したときにプロファイラーに通知します。 これは、CLR がコードプロファイラーと通信するときに使用する主要なコールバックインターフェイスです。  
  
 コードプロファイラーは、`ICorProfilerCallback` インターフェイスのメソッドを実装する必要があります。 .NET Framework バージョン2.0 以降では、プロファイラーは `ICorProfilerCallback2` メソッドも実装する必要があります。 各メソッドの実装では、成功した場合は S_OK の値を持つ HRESULT を返し、失敗した場合は E_FAIL を返す必要があります。 現在、CLR では、 [ICorProfilerCallback:: ObjectReferences](icorprofilercallback-objectreferences-method.md)を除く各コールバックによって返される HRESULT は無視されます。  
  
 Microsoft Windows レジストリでは、コードプロファイラーは、`ICorProfilerCallback` および `ICorProfilerCallback2` インターフェイスを実装するコンポーネントオブジェクトモデル (COM) オブジェクトを登録する必要があります。 コードプロファイラーは、 [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md)を呼び出して、通知を受信するイベントをサブスクライブします。 これは通常、プロファイラーによる[ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md)の実装で行われます。 その後、プロファイラーは、実行中のランタイムプロセスでイベントが発生するか、発生したばかりの場合に、ランタイムから通知を受け取ることができます。  
  
> [!NOTE]
> プロファイラーは、1つの COM オブジェクトを登録します。 プロファイラーが .NET Framework バージョン1.0 または1.1 を対象としている場合、その COM オブジェクトは `ICorProfilerCallback`のメソッドのみを実装する必要があります。 .NET Framework バージョン2.0 以降を対象としている場合、COM オブジェクトは `ICorProfilerCallback2`のメソッドも実装する必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
- [ICorProfilerCallback3 インターフェイス](icorprofilercallback3-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
