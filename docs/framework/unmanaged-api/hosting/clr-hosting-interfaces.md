---
title: CLR ホスト インターフェイス
ms.date: 03/30/2017
helpviewer_keywords:
- interfaces [.NET Framework hosting], version 2.0
- hosting interfaces [.NET Framework], version 2.0
- .NET Framework 2.0, hosting interfaces
ms.assetid: 703b8381-43db-4a4d-9faa-cca39302d922
ms.openlocfilehash: 66fdd97d101f5ea53a96b996a2a60e5ed65a2701
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131965"
---
# <a name="clr-hosting-interfaces"></a>CLR ホスト インターフェイス
ここでは、アンマネージホストが共通言語ランタイム (CLR) をアプリケーションに統合するために使用できるインターフェイスについて説明します。 この情報は .NET Framework バージョン2.0 以降のバージョンに関連します。 これらのインターフェイスを使用すると、ホストは、バージョン1.0 および1.1 で可能な限り多くのランタイムの側面を制御でき、CLR とホストの実行モデルとの間の統合が大幅に強化されます。  
  
 .NET Framework バージョン1.0 および1.1 では、ホストモデルによって、CLR をプロセスに読み込み、特定の設定を構成し、イベント通知を受信するために、管理されていないホストが有効になりました。 ただし、一般に、ホストと CLR は、そのプロセスで独立して実行されていました。 .NET Framework バージョン2.0 以降のバージョンでは、新しい階層の抽象化により、ホストは、Win32 アセンブリの型によって現在提供されている多くのリソースを提供し、ホストが構成できる一連の機能を拡張できます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [IActionOnCLREvent インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iactiononclrevent-interface.md)  
 登録されているイベントに対してコールバックを実行するメソッドを提供します。  
  
 [IApartmentCallback インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iapartmentcallback-interface.md)  
 アパートメント内でコールバックを作成するためのメソッドを提供します。  
  
 [IAppDomainBinding インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iappdomainbinding-interface.md)  
 実行時の構成を設定するためのメソッドを提供します。  
  
 [ICatalogServices インターフェイス](../../../../docs/framework/unmanaged-api/hosting/icatalogservices-interface.md)  
 サービスのカタログ化を行うためのメソッドを提供します。 (このインターフェイスは .NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません)。  
  
 [ICLRAssemblyIdentityManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyidentitymanager-interface.md)  
 アセンブリに関するホストと CLR 間の通信をサポートするメソッドを提供します。  
  
 [ICLRAssemblyReferenceList インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)  
 ホストではなく、CLR によって読み込まれるアセンブリの一覧を管理します。  
  
 [ICLRControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-interface.md)  
 ホストが CLR に対してアクセスを取得し、さまざまな側面を構成するためのメソッドを提供します。  
  
 [ICLRDebugManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrdebugmanager-interface.md)  
 ホストが一連のタスクを識別子とフレンドリ名に関連付けることができるようにするメソッドを提供します。  
  
 [ICLRErrorReportingManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrerrorreportingmanager-interface.md)  
 エラー報告のためにホストがカスタムヒープダンプを構成できるようにするメソッドを提供します。  
  
 [ICLRGCManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrgcmanager-interface.md)  
 ホストが CLR のガベージコレクションシステムと対話できるようにするメソッドを提供します。  
  
 [ICLRHostBindingPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrhostbindingpolicymanager-interface.md)  
 ホストがアセンブリのポリシー情報の変更を評価および通知するためのメソッドを提供します。  
  
 [ICLRHostProtectionManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrhostprotectionmanager-interface.md)  
 ホストが、部分的に信頼されたコードで実行される特定のマネージクラス、メソッド、プロパティ、およびフィールドをブロックできるようにします。  
  
 [ICLRIoCompletionManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclriocompletionmanager-interface.md)  
 ホストが、指定された i/o 要求のステータスを CLR に通知できるようにするコールバックメソッドを実装します。  
  
 [ICLRMemoryNotificationCallback インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrmemorynotificationcallback-interface.md)  
 Win32 `CreateMemoryResourceNotification` 関数と同様の方法を使用して、ホストがメモリ不足状態を報告できるようにします。  
  
 [ICLROnEventManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-interface.md)  
 ホストが CLR イベントのコールバックを登録および登録解除できるようにするメソッドを提供します。  
  
 [ICLRPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)  
 エラーとタイムアウトが発生した場合に実行されるポリシーアクションをホストが指定できるようにするメソッドを提供します。  
  
 [ICLRProbingAssemblyEnum インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrprobingassemblyenum-interface.md)  
 ホストが CLR 内部のアセンブリ id 情報を使用してアセンブリのプローブ id を取得できるようにするメソッドを提供します。このメソッドは、その id を作成したり、理解したりする必要はありません。  
  
 [ICLRReferenceAssemblyEnum インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrreferenceassemblyenum-interface.md)  
 CLR 内部のアセンブリ id データを使用して、ファイルまたはストリームによって参照されるアセンブリのセットをホストが操作できるようにするメソッドを提供します。これらの id を作成したり、理解したりする必要はありません。  
  
 [ICLRRuntimeHost インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-interface.md)  
 には、 [ICorRuntimeHost](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-interface.md)と同様の機能が用意されており、ホストコントロールインターフェイスを設定するための追加のメソッドもあります。  
  
 [ICLRSyncManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrsyncmanager-interface.md)  
 ホストが、要求されたタスクに関する情報を取得し、その同期実装でデッドロックを検出するためのメソッドを提供します。  
  
 [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)  
 ホストが CLR の要求を行うことができるようにするメソッド、または関連付けられたタスクについて CLR に通知を提供するためのメソッドを提供します。  
  
 [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)  
 CLR が新しいタスクを作成し、現在実行中のタスクを取得し、タスクの地理的言語とカルチャを設定することをホストが明示的に要求できるようにするメソッドを提供します。  
  
 [ICLRValidator インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrvalidator-interface.md)  
 ポータブル実行可能 (PE) イメージを検証し、検証エラーを報告するためのメソッドを提供します。  
  
 [ICorConfiguration インターフェイス](../../../../docs/framework/unmanaged-api/hosting/icorconfiguration-interface.md)  
 CLR を構成するためのメソッドを提供します。  
  
 [ICorThreadpool インターフェイス](../../../../docs/framework/unmanaged-api/hosting/icorthreadpool-interface.md)  
 スレッドプールにアクセスするためのメソッドを提供します。  
  
 [IDebuggerInfo インターフェイス](../../../../docs/framework/unmanaged-api/hosting/idebuggerinfo-interface.md)  
 デバッグサービスの状態に関する情報を取得するためのメソッドを提供します。  
  
 [IDebuggerThreadControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/idebuggerthreadcontrol-interface.md)  
 デバッグサービスによるスレッドのブロックおよびブロック解除についてホストに通知するためのメソッドを提供します。  
  
 [IGCHost インターフェイス](../../../../docs/framework/unmanaged-api/hosting/igchost-interface.md)  
 ガベージコレクションシステムに関する情報を取得し、ガベージコレクションのいくつかの側面を制御するためのメソッドを提供します。  
  
 [IGCHost2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/igchost2-interface.md)  
 ホストがガベージコレクションセグメントのサイズとガベージコレクションシステムのジェネレーション0の最大サイズを `DWORD`より大きい値に設定できるようにする、 [SetGCStartupLimitsEx](../../../../docs/framework/unmanaged-api/hosting/igchost2-setgcstartuplimitsex-method.md)メソッドを提供します。  
  
 [IGCHostControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/igchostcontrol-interface.md)  
 仮想メモリの制限を変更するように、ガベージコレクターがホストに要求できるようにするメソッドを提供します。  
  
 [IGCThreadControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/igcthreadcontrol-interface.md)  
 ガベージコレクションのためにブロックされるスレッドのスケジュール設定に参加するためのメソッドを提供します。  
  
 [IHostAssemblyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-interface.md)  
 CLR またはホストによって読み込まれる必要があるアセンブリのセットをホストが指定できるようにするメソッドを提供します。  
  
 [IHostAssemblyStore インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-interface.md)  
 ホストが CLR とは無関係にアセンブリおよびモジュールを読み込むことができるようにするメソッドを提供します。  
  
 [IHostAutoEvent インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostautoevent-interface.md)  
 ホストによって実装される自動リセットイベントの表現を提供します。  
  
 [IHostControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostcontrol-interface.md)  
 アセンブリの読み込みを構成し、ホストがサポートするホストインターフェイスを決定するためのメソッドを提供します。  
  
 [IHostCrst インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostcrst-interface.md)  
 スレッド処理のためのクリティカルセクションのホストの表現として機能します。  
  
 [IHostGCManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-interface.md)  
 CLR によって実装されるガベージコレクション機構でイベントのホストに通知するメソッドを提供します。  
  
 [IHostIoCompletionManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-interface.md)  
 ホストによって提供される i/o 完了ポートと CLR が対話できるようにするメソッドを提供します。  
  
 [IHostMalloc インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostmalloc-interface.md)  
 CLR がホストを介してヒープから細かい割り当てを要求するためのメソッドを提供します。  
  
 [IHostManualEvent インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostmanualevent-interface.md)  
 手動リセットイベントの表現のホストの実装を提供します。  
  
 [IHostMemoryManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostmemorymanager-interface.md)  
 標準の Win32 仮想メモリ関数を使用する代わりに、CLR がホストを介して仮想メモリ要求を行うためのメソッドを提供します。  
  
 [IHostPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostpolicymanager-interface.md)  
 中止、タイムアウト、またはエラーが発生した場合に CLR が実行するアクションをホストに通知するメソッドを提供します。  
  
 [IHostSecurityContext インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritycontext-interface.md)  
 CLR がホストによって実装されたセキュリティコンテキスト情報を維持できるようにします。  
  
 [IHostSecurityManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-interface.md)  
 現在実行中のスレッドのセキュリティコンテキストに対するアクセスと制御を可能にするメソッドを提供します。  
  
 [IHostSemaphore インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostsemaphore-interface.md)  
 ホストによって実装されるセマフォの表現を提供します。  
  
 [IHostSyncManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-interface.md)  
 Win32 同期関数を使用する代わりに、ホストを呼び出すことによって、CLR が同期プリミティブを作成するためのメソッドを提供します。  
  
 [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)  
 CLR がホストと通信してタスクを管理できるようにするメソッドを提供します。  
  
 [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)  
 標準のオペレーティングシステムのスレッド処理やファイバー関数を使用する代わりに、CLR がホストを通じてタスクを操作できるようにするメソッドを提供します。  
  
 [IHostThreadPoolManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-interface.md)  
 CLR がスレッドプールを構成し、作業項目をスレッドプールにキューするためのメソッドを提供します。  
  
 [IManagedObject インターフェイス](../../../../docs/framework/unmanaged-api/hosting/imanagedobject-interface.md)  
 マネージオブジェクトを制御するためのメソッドを提供します。  
  
 IObjectHandle  
 値渡しのマーシャリングオブジェクトを間接参照からラップ解除するためのメソッドを提供します。  
  
 [ITypeName インターフェイス](../../../../docs/framework/unmanaged-api/hosting/itypename-interface.md)  
 型名の情報を取得するためのメソッドを提供します。 (このインターフェイスは .NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません)。  
  
 [ITypeNameBuilder インターフェイス](../../../../docs/framework/unmanaged-api/hosting/itypenamebuilder-interface.md)  
 型名を構築するためのメソッドを提供します。 (このインターフェイスは .NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません)。  
  
 [ITypeNameFactory インターフェイス](../../../../docs/framework/unmanaged-api/hosting/itypenamefactory-interface.md)  
 型名を分解するためのメソッドを提供します。 (このインターフェイスは .NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません)。  
  
 IValidator  
 ポータブル実行可能 (PE) イメージを検証し、検証エラーを報告するためのメソッドを提供します。  
  
## <a name="related-sections"></a>関連項目  
 [非推奨の CLR のホスト インターフェイスおよびコクラス](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-interfaces-and-coclasses.md)  
 .NET Framework バージョン1.0 および1.1 で提供されるホストインターフェイスについて説明するトピックが含まれています。  
  
 [.NET Framework 4 および 4.5 で追加された CLR ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)  
 .NET Framework 4 で提供されるホスティングインターフェイスについて説明するトピックが含まれています。
