---
title: アプリケーション ドメインのリソース監視
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- monitoring managed memory use by application domain
- application domains, memory use
- memory use, monitoring
- application domains, resource monitoring
ms.assetid: 318bedf8-7f35-4f00-b34a-2b7b8e3fa315
ms.openlocfilehash: 54e300bef1818fd08f27d7920eec68ee1f2c45bb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141382"
---
# <a name="application-domain-resource-monitoring"></a>アプリケーション ドメインのリソース監視

アプリケーション ドメインのリソース監視 (ARM) を使用して、ホストでアプリケーション ドメインによる CPU とメモリの使用状況を監視できます。 これは、実行時間の長いプロセスで多数のアプリケーション ドメインを使用する ASP.NET などのホストに役立ちます。 問題のあるアプリケーションを特定できる場合に限り、ホストは、プロセス全体のパフォーマンスに悪影響を与えるアプリケーションのアプリケーション ドメインをアンロードできます。 ARM は、このような意思決定を支援するために使用できる情報を提供します。

たとえば、ホスティング サービスでは、ASP.NET サーバー上で多くのアプリケーションが実行されている可能性があります。 プロセス内の 1 つのアプリケーションでメモリ消費量やプロセッサ時間が過度に増え始めた場合、ホスティング サービスは ARM を使用して問題の原因となっているアプリケーション ドメインを特定できます。

ARM は十分に軽量なので、ライブ アプリケーションで使用できます。 情報には、Windows (ETW) のイベント トレースを使用してアクセスするか、マネージド API またはネイティブ API を使用して直接アクセスできます。

## <a name="enabling-resource-monitoring"></a>リソース監視を有効にする

ARM を有効にする方法は 4 つあります。共通言語ランタイム (CLR) の起動時に構成ファイルを提供する方法、アンマネージド ホスティング API を使用する方法、マネージド コードを使用する方法、または ARM ETW イベントをリッスンする方法です。

ARM が有効になるとすぐに、プロセスのすべてのアプリケーション ドメイン上でデータの収集が開始されます。ARM が有効になる前にアプリケーション ドメインが作成された場合、アプリケーション ドメインの作成時ではなく、ARM が有効になったときに累積データが開始されます。ARM を有効にした後に無効にすることはできません。

- CLR 起動時に ARM を有効にするには、[\<appDomainResourceMonitoring>](../../../docs/framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md) 要素を構成ファイルに追加し、`enabled` 属性を `true` に設定します。 `false` の値 (既定) は、起動時に ARM は有効ではないことのみを意味します。他のアクティブ化メカニズムのいずれかを使用して、後で有効にすることができます。

- ホストは、[ICLRAppDomainResourceMonitor](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md) ホスティング インターフェイスを要求することで ARM を有効にすることができます。 このインターフェイスが正常に取得されると、ARM が有効になります。

- マネージド コードでは、static (Visual Basic では `Shared`) <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> プロパティを `true` に設定して ARM を有効にすることができます。 プロパティが設定されるとすぐに、ARM が有効になります。

- 起動後に ETW イベントをリッスンして ARM を有効にすることができます。 `AppDomainResourceManagementKeyword` キーワードを使用してパブリック プロバイダー `Microsoft-Windows-DotNETRuntime` を有効にすると、ARM が有効になり、すべてのアプリケーション ドメインのイベント生成が開始されます。 データをアプリケーション ドメインとスレッドに関連付けるには、`ThreadingKeyword` キーワードを使用して `Microsoft-Windows-DotNETRuntimeRundown` プロバイダーを有効にする操作も必要です。

## <a name="using-arm"></a>ARM の使用

ARM は、アプリケーション ドメインに使用される合計プロセッサ時間と、メモリ使用に関する 3 種類の情報を提供します。

- **アプリケーション ドメインの合計プロセッサ時間 (秒単位)** : アプリケーション ドメインの有効期間中に実行されていたすべてのスレッドについて、オペレーティング システムから報告されたスレッド時間を合計して計算されます。 ブロックされているスレッドまたはスリープ状態のスレッドは、プロセッサ時間を使用しません。 スレッドがネイティブ コードを呼び出すときに、スレッドがネイティブ コードに費やす時間は、呼び出しが行われたアプリケーション ドメインの計算に含まれます。

  - マネージド API: <xref:System.AppDomain.MonitoringTotalProcessorTime%2A?displayProperty=nameWithType> プロパティ。

  - ホスティング API: [ICLRAppDomainResourceMonitor::GetCurrentCpuTime](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentcputime-method.md) メソッド。

  - ETW イベント: `ThreadCreated`、`ThreadAppDomainEnter`、および `ThreadTerminated` イベント プロバイダーとキーワードの詳細については、「[CLR ETW イベント](../../../docs/framework/performance/clr-etw-events.md)」の「AppDomain Resource Monitoring Events」(AppDomain リソース監視イベント) を参照してください。

- **有効期間中にアプリケーション ドメインによって行われたマネージドされた割り当ての合計 (バイト単位)** : 割り当てられたオブジェクトの有効期間は短いことがあるので、割り当ての合計にアプリケーション ドメインによるメモリ使用量が反映されない場合もあります。 ただし、アプリケーションが膨大な数のオブジェクトを割り当ててから解放すると、割り当てのコストが大幅に高くなる可能性があります。

  - マネージド API: <xref:System.AppDomain.MonitoringTotalAllocatedMemorySize%2A?displayProperty=nameWithType> プロパティ。

  - ホスティング API: [ICLRAppDomainResourceMonitor::GetCurrentAllocated](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentallocated-method.md) メソッド。

  - ETW イベント: `AppDomainMemAllocated` イベント、`Allocated` フィールド。

- **アプリケーション ドメインによって参照され、最新の完全なブロッキング コレクションの後に残っているマネージド メモリ (バイト単位)** : この数値は完全なブロッキング コレクションの後にのみ正確になります。 (これは、バックグラウンドで発生し、アプリケーションをブロックしない同時コレクションとは対照的です)。たとえば、<xref:System.GC.Collect?displayProperty=nameWithType> メソッドのオーバーロードによって、完全なブロッキング コレクションが実行されます。

  - マネージド API: <xref:System.AppDomain.MonitoringSurvivedMemorySize%2A?displayProperty=nameWithType> プロパティ。

  - ホスティング API: [ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md) メソッド、`pAppDomainBytesSurvived` パラメーター。

  - ETW イベント: `AppDomainMemSurvived` イベント、`Survived` フィールド。

- **プロセスによって参照され、最新の完全なブロッキング コレクションの後に残っている合計マネージド メモリ (バイト単位)** : 個々のアプリケーション ドメインに残っている​​メモリをこの数値と比較できます。

  - マネージド API: <xref:System.AppDomain.MonitoringSurvivedProcessMemorySize%2A?displayProperty=nameWithType> プロパティ。

  - ホスティング API: [ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md) メソッド、`pTotalBytesSurvived` パラメーター。

  - ETW イベント: `AppDomainMemSurvived` イベント、`ProcessSurvived` フィールド。

### <a name="determining-when-a-full-blocking-collection-occurs"></a>完全なブロッキング コレクションが発生したときの判断

残っているメモリの計算が正確かどうかを判断するには、完全なブロッキング コレクションがいつ発生したかを把握する必要があります。 これを行う方法は、ARM 統計を調べるために使用する API によって異なります。

#### <a name="managed-api"></a>マネージド API

<xref:System.AppDomain> クラスのプロパティを使用する場合は、<xref:System.GC.RegisterForFullGCNotification%2A?displayProperty=nameWithType> メソッドを使用して完全なコレクションの通知を登録できます。 コレクションのアプローチではなく、コレクションの完了を待機するので、使用するしきい値は重要ではありません。 次に、<xref:System.GC.WaitForFullGCComplete%2A?displayProperty=nameWithType> メソッドを呼び出すことができます。このメソッドは、完全なコレクションが完了するまでブロックされます。 ループ内でメソッドを呼び出し、メソッドから復帰するたびに必要な分析を実行するスレッドを作成できます。

また、<xref:System.GC.CollectionCount%2A?displayProperty=nameWithType> メソッドを定期的に呼び出して、世代 2 コレクションの数が増加しているかどうかを確認することもできます。 ポーリング頻度によっては、この手法では完全なコレクションの発生が正確に示されない場合があります。

#### <a name="hosting-api"></a>ホスティング API

アンマネージ ホスティング API を使用する場合、ホストは CLR に [IHostGCManager](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-interface.md) インターフェイスの実装を渡す必要があります。 コレクションの実行中に中断されたスレッドの実行を再開すると、CLR は [IHostGCManager::SuspensionEnding](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) メソッドを呼び出します。 CLR は、完全なコレクションの生成をメソッドのパラメーターとして渡します。そのため、ホストはコレクションが完全か部分的かを判断できます。 [IHostGCManager::SuspensionEnding](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) メソッドを実装すると、残っているメモリのクエリを実行して、カウントが更新された場合にすぐに取得することができます。

## <a name="see-also"></a>関連項目

- <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>
- [ICLRAppDomainResourceMonitor インターフェイス](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md)
- [\<appDomainResourceMonitoring>](../../../docs/framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)
- [CLR ETW イベント](../../../docs/framework/performance/clr-etw-events.md)
