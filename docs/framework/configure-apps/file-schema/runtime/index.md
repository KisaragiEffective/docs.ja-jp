---
title: ランタイム設定スキーマ
ms.date: 03/30/2017
helpviewer_keywords:
- schema runtime settings
- configuration schema [.NET Framework], runtime settings
- runtime settings schema
ms.assetid: f04816ab-110d-4e28-9283-845d6d9a4a68
ms.openlocfilehash: d5af9f3299b48d431b43566c11610d745167b60b
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431051"
---
# <a name="run-time-settings-schema"></a>ランタイム設定スキーマ

Run-time settings are used by the common language runtime to configure applications that target the .NET Framework.

## <a name="the-runtime-section-and-its-parent-and-child-elements"></a>\<runtime> セクションとその親要素と子要素

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerType>](appdomainmanagertype-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyBinding>](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<dependentAssembly>](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<bindingRedirect>](bindingredirect-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<codeBase>](codebase-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<publisherPolicy>](publisherpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<probing>](probing-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<qualifyAssembly>](qualifyassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<supportPortability>](supportportability-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<developmentMode>](developmentmode-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<etwEnable>](etwenable-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcAllowVeryLargeObjects>](gcconcurrent-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcConcurrent>](gcconcurrent-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCCpuGroup>](gccpugroup-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapCount>](gcheapcount-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCLOHThreshold>](gclohthreshold-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCNoAffinitize>](gcnoaffinitize-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcServer>](gcserver-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<generatePublisherEvidence>](generatepublisherevidence-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<loadfromRemoteSources>](loadfromremotesources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<relativeBindForResources>](relativebindforresources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<shadowCopyVerifyByTimeStamp>](shadowcopyverifybytimestamp-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<TimeSpan_LegacyFormatMode>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<useLegacyJit>](uselegacyjit-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)\
&nbsp;&nbsp;[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<memoryCache>](memorycache-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<namedCaches>](namedcaches-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<add>](add-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<clear>](clear-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<remove>](remove-element-for-namedcaches.md)

## <a name="alphabetical-list-of-runtime-elements"></a>\<runtime> 要素のアルファベット順の一覧

|要素|説明|
|-------------|-----------------|
|[\<add>](add-element-for-namedcaches.md)|名前付きキャッシュを、メモリ キャッシュの `namedCaches` コレクションに追加します。|
|[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)|偽装の実行方法に関係なく、Windows ID が常に非同期ポイント間でフローすることを指定します。|
|[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)|<xref:System.AppContext> クラスで使用される、新機能に対するオプトアウト メカニズムを指定するスイッチを 1 つまたは複数定義します。|
|[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)|プロセスにおける既定のアプリケーション ドメインのアプリケーション ドメイン マネージャーを提供するアセンブリを指定します。|
|[\<appDomainManagerType>](appdomainmanagertype-element.md)|既定のアプリケーション ドメインのアプリケーション ドメイン マネージャーの役割を果たす種類を指定します。|
|[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)|プロセスのライフサイクルにおいて、プロセスのすべてのアプリケーション ドメインの統計を収集するようにランタイムに指示します。|
|[\<assemblyBinding>](assemblybinding-element-for-runtime.md)|アセンブリ バージョンのリダイレクトおよびアセンブリの位置に関する情報が含まれます。|
|[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)|アセンブリに関する識別情報が含まれています。|
|[\<bindingRedirect>](bindingredirect-element.md)|1 つのアセンブリ バージョンを別のバージョンにリダイレクトします。|
|[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)|信頼されたアセンブリに対する厳密な名前の検証をバイパスするかどうかを指定します。|
|[\<clear>](clear-element-for-namedcaches.md)|メモリ キャッシュの `namedCaches` コレクションを消去します。|
|[\<codeBase>](codebase-element.md)|ランタイムがアセンブリを検索する場所を指定します。|
|[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)|文字列比較の実行時に、ランタイムがレガシ並べ替え動作を使用するように指定します。|
|[\<dependentAssembly>](dependentassembly-element.md)|各アセンブリのバインディング ポリシーとアセンブリの場所をカプセル化します。|
|[\<developmentMode>](developmentmode-element.md)|DEVPATH 環境変数によって指定されたディレクトリで、ランタイムがアセンブリの検索を行うかどうかを指定します。|
|[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)|Specifies whether the caching of binding failures, which is the default behavior in the .NET Framework 2.0, is disabled.|
|[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)|スレッドの起動時にスレッド スタック全体をコミットするかどうかを指定します。|
|[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)|アプリケーション ドメインの構成設定をランタイム ホストがオーバーライドする既定の動作を無効化するかどうかを指定します。|
|[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)|日付と時刻の解析メソッドが、日、月、時間、および午前/午後のみを含む日付の文字列を解析するように調整されたルールのセットを使用するかどうかを決定します。|
|[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)|暗号化アルゴリズムが連邦情報処理規格 (FIPS: Federal Information Processing Standard) に準拠する必要があるコンピューターの構成要件を強制するかどうかを指定します。|
|[\<etwEnable>](etwenable-element.md)|共通言語ランタイム イベントで Windows イベント トレーシング (ETW) を有効にするかどうかを指定します。|
|[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)|PerfCounter.dll が、.NET Framework バージョン 1.1 のアプリケーションの CategoryOptions レジストリ設定を使用してするかどうかを指定して、カテゴリ別の共有メモリとグローバル メモリのどちらからパフォーマンス カウンター データを読み込むかを決定します。|
|[\<gcAllowVeryLargeObjects>](gcallowverylargeobjects-element.md)|64 ビット プラットフォームで、合計サイズが 2 GB (ギガバイト) を超える配列を有効にします。|
|[\<gcConcurrent>](gcconcurrent-element.md)|ランタイムがガベージ コレクションを並列に実行するかどうかを指定します。|
|[\<GCCpuGroup>](gccpugroup-element.md)|ガベージ コレクションが複数の CPU グループをサポートするかどうかを指定します。|
|[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)|Defines the affinity between GC heaps and individual processors.|
|[\<GCHeapCount>](gcheapcount-element.md)|Specifies the number of heaps/threads to use for server garbage collection.  |
|[\<GCLOHThreshold>](gclohthreshold-element.md)|Specifies the threshold size that causes objects to go on the large object heap (LOH).|
|[\<GCNoAffinitize>](gcnoaffinitize-element.md)|Specifies whether or not to affinitize server GC threads with CPUs.|
|[\<gcServer>](gcserver-element.md)|共通言語ランタイムがサーバーのガベージ コレクションを実行するかどうかを指定します。|
|[\<generatePublisherEvidence>](generatepublisherevidence-element.md)|ランタイムがコード アクセス セキュリティ (CAS) の発行元ポリシーを使用するかどうかを指定します。|
|[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)|ランタイムがアクセス違反およびその他の破損状態例外をキャッチするマネージド コードを許可するかどうかを指定します。|
|[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)|Windows ID が、現在のスレッドの実行コンテキストのフロー設定に関係なく、非同期ポイント間でフローしないことを指定します。|
|[\<loadfromRemoteSources>](loadfromremotesources-element.md)|リモート ソースからのアセンブリを完全な信頼として読み込むかどうかを指定します。|
|[\<memoryCache>](memorycache-element-cache-settings.md)|<xref:System.Runtime.Caching.MemoryCache> クラスに基づくキャッシュを構成するために使用される要素を定義します。|
|[\<namedCaches>](namedcaches-element-cache-settings.md)|`namedCache` インスタンスの構成設定のコレクションが含まれます。|
|[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)|ランタイムがレガシ コード アクセス セキュリティ (CAS) ポリシーを使用するかどうかを指定します。|
|[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)|ランタイムが実行時の不適切なプラットフォーム呼び出し宣言を自動的に修正するかどうかを指定します。これにより、マネージド コードとアンマネージド コード間の遷移が遅くなります。|
|[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)|ランタイムが <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> メソッドで固定量のメモリを使用してハッシュ コードを計算するかどうかを指定します。|
|[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)|ランタイムが、アプリケーション ドメインの境界間のリモート処理ではなく COM 相互運用を使用することを指定します。|
|[\<probing>](probing-element.md)|アセンブリの読み込み時にランタイムが検索するサブディレクトリを指定します。|
|[\<publisherPolicy>](publisherpolicy-element.md)|ランタイムが発行元ポリシーを適用するかどうかを指定します。|
|[\<qualifyAssembly>](qualifyassembly-element.md)|部分名が使用された場合に動的に読み込む必要があるアセンブリの完全名を指定します。|
|[\<relativeBindForResources>](relativebindforresources-element.md)|サテライト アセンブリのプローブを最適化します。|
|[\<remove>](remove-element-for-namedcaches.md)|名前付きキャッシュ エントリを、メモリ キャッシュの `namedCaches` コレクションから削除します。|
|[\<runtime>](runtime-element.md)|アセンブリのバインディングとガベージ コレクションの動作に関する情報が含まれています。|
|[\<shadowCopyTimeStampVerification>](shadowcopyverifybytimestamp-element.md)|Specifies whether shadow copying uses the default startup behavior introduced in the .NET Framework 4, or reverts to the startup behavior of earlier versions of the .NET Framework.|
|[\<supportPortability>](supportportability-element.md)|.NET Framework の 2 つの異なる実装にある同じアセンブリを 1 つのアプリケーションから参照できるように、既定の動作を無効にすることができます。既定の動作では、アプリケーションの移植性を高めるために、このようなアセンブリは同等のものとして扱われます。|
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|既定のメモリ内オブジェクト キャッシュの構成情報を提供します。|
|[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)|ランタイムによって、すべての CPU グループにマネージド スレッドを分散するかどうかを指定します。|
|[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)|タスクがハンドルされない例外によって実行中のプロセスを終了するかどうかを指定します。|
|[\<TimeSpan_LegacyFormatMode>](runtime-element.md)|ランタイムで <xref:System.TimeSpan> の値に従来の書式を使用するかどうかを指定します。|
|[\<useLegacyJit>](uselegacyjit-element.md)|共通言語ランタイムが Just-In-Time コンパイルの従来の 64 ビット JIT コンパイラを使用するかどうかを決定します。|
|[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)|ランタイムがアプリケーション ドメインごとに文字列のハッシュ コードを計算するかどうかを指定します。|
|[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)|ランタイムが内部的に使用する特定のスレッド作成時に、既定のスタック サイズではなく明示的なスタック サイズを使用することを要求します。|

## <a name="see-also"></a>関連項目

- [構成ファイル スキーマ](../index.md)
- [To disable concurrent garbage collection](gcconcurrent-element.md#to-disable-background-garbage-collection)
- [アセンブリ バージョンのリダイレクト](../../redirect-assembly-versions.md)
