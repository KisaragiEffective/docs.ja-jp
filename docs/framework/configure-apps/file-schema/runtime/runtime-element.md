---
title: <runtime> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#runtime
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime
helpviewer_keywords:
- <runtime> element
- runtime element
- container tags, <runtime> element
ms.assetid: 1eb2fae3-de4b-45b6-852f-517c39b751bd
ms.openlocfilehash: 3825ae7c3e35193cb835981600fe1ef83097cd2d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74430462"
---
# <a name="runtime-element"></a>\<ランタイム > 要素

アプリケーションを構成するために共通言語ランタイムによって使用される情報を提供します。

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;\<ランタイム >

## <a name="syntax"></a>構文

```xml
<runtime>
</runtime>
```

## <a name="attributes-and-elements"></a>属性と要素

次のセクションでは、子要素と親要素について説明します。

### <a name="attributes"></a>属性

[なし]。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)|偽装の実行方法に関係なく、Windows ID が常に非同期ポイント間でフローすることを指定します。|
|[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)|<xref:System.AppContext> クラスで使用される、新機能に対するオプトアウト メカニズムを指定するスイッチを 1 つまたは複数定義します。|
|[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)|プロセスにおける既定のアプリケーション ドメインのアプリケーション ドメイン マネージャーを提供するアセンブリを指定します。|
|[\<appDomainManagerType>](appdomainmanagertype-element.md)|既定のアプリケーション ドメインのアプリケーション ドメイン マネージャーの役割を果たす種類を指定します。|
|[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)|プロセスのライフサイクルにおいて、プロセスのすべてのアプリケーション ドメインの統計を収集するようにランタイムに指示します。|
|[\<assemblyBinding>](assemblybinding-element-for-runtime.md)|アセンブリ バージョンのリダイレクトおよびアセンブリの位置に関する情報が含まれます。|
|[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)|信頼されたアセンブリに対する厳密な名前の検証をバイパスするかどうかを指定します。|
|[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)|文字列比較を実行するときに、ランタイムが従来の並べ替え動作を使用するように指定します。|
|[\<developmentMode>](developmentmode-element.md)|DEVPATH 環境変数によって指定されたディレクトリで、ランタイムがアセンブリの検索を行うかどうかを指定します。|
|[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)|バインディングエラーのキャッシュ (.NET Framework バージョン2.0 での既定の動作) を無効にするかどうかを指定します。|
|[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)|スレッドの起動時にスレッド スタック全体をコミットするかどうかを指定します。|
|[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)|アプリケーション ドメインの構成設定をランタイム ホストがオーバーライドする既定の動作を無効化するかどうかを指定します。|
|[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)|日付と時刻の解析メソッドが、日、月、時間、および午前/午後のみを含む日付の文字列を解析するように調整されたルールのセットを使用するかどうかを決定します。|
|[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)|暗号化アルゴリズムが連邦情報処理規格 (FIPS: Federal Information Processing Standard) に準拠する必要があるコンピューターの構成要件を強制するかどうかを指定します。|
|[\<etwEnable>](etwenable-element.md)|共通言語ランタイム イベントで Windows イベント トレーシング (ETW) を有効にするかどうかを指定します。|
|[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)|PerfCounter.dll が、.NET Framework バージョン 1.1 のアプリケーションの CategoryOptions レジストリ設定を使用してするかどうかを指定して、カテゴリ別の共有メモリとグローバル メモリのどちらからパフォーマンス カウンター データを読み込むかを決定します。|
|[\<gcAllowVeryLargeObjects>](gcallowverylargeobjects-element.md)|64 ビット プラットフォームで、合計サイズが 2 GB (ギガバイト) を超える配列を有効にします。|
|[\<gcConcurrent>](gcconcurrent-element.md)|共通言語ランタイムがガベージコレクションを同時に実行するかどうかを指定します。|
|[\<GCCpuGroup>](gccpugroup-element.md)|ガベージ コレクションが複数の CPU グループをサポートするかどうかを指定します。|
|[\<GCHeapAffinitizeMask >](gcheapaffinitizemask-element.md)|ガベージコレクションヒープと個々のプロセッサ間の関係を定義します。|
|[\<G>](gcheapcount-element.md)|サーバーのガベージコレクションに使用するヒープまたはスレッドの数を指定します。|
|[\<GCLOHThreshold >](gclohthreshold-element.md)|ガベージコレクターが大きなオブジェクトヒープにオブジェクトを配置するしきい値のサイズを指定します。|
|[\<GCNoAffinitize >](gcnoaffinitize-element.md)|Cpu を使用してサーバーガベージコレクションスレッドを関係付けするかどうかを指定します。|
|[\<gcServer>](gcserver-element.md)|共通言語ランタイムがサーバーのガベージ コレクションを実行するかどうかを指定します。|
|[\<generatePublisherEvidence>](generatepublisherevidence-element.md)|ランタイムがコード アクセス セキュリティ (CAS) の発行元ポリシーを使用するかどうかを指定します。|
|[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)|ランタイムがアクセス違反およびその他の破損状態例外をキャッチするマネージド コードを許可するかどうかを指定します。|
|[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)|Windows ID が、現在のスレッドの実行コンテキストのフロー設定に関係なく、非同期ポイント間でフローしないことを指定します。|
|[\<loadfromRemoteSources>](loadfromremotesources-element.md)|リモート ソースからのアセンブリを完全な信頼として読み込むかどうかを指定します。|
|[\<NetFx40_LegacySecurityPolicy >](netfx40-legacysecuritypolicy-element.md)|ランタイムがレガシ コード アクセス セキュリティ (CAS) ポリシーを使用するかどうかを指定します。|
|[\<NetFx40_PInvokeStackResilience >](netfx40-pinvokestackresilience-element.md)|ランタイムが実行時の不適切なプラットフォーム呼び出し宣言を自動的に修正するかどうかを指定します。これにより、マネージド コードとアンマネージド コード間の遷移が遅くなります。|
|[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings >](netfx45-cultureawarecomparergethashcode-longstrings-element.md)|ランタイムが <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> メソッドで固定量のメモリを使用してハッシュ コードを計算するかどうかを指定します。|
|[\<PreferComInsteadOfRemoting >](prefercominsteadofmanagedremoting-element.md)|ランタイムが、アプリケーション ドメインの境界間のリモート処理ではなく COM 相互運用を使用することを指定します。|
|[\<relativeBindForResources>](relativebindforresources-element.md)|サテライト アセンブリのプローブを最適化します。|
|[\<shadowCopyVerifyByTimeStamp>](shadowcopyverifybytimestamp-element.md)|シャドウコピーで .NET Framework 4 で導入された既定の起動動作を使用するか、以前のバージョンの .NET Framework の起動動作に戻すかを指定します。|
|[\<supportPortability>](supportportability-element.md)|.NET Framework の 2 つの異なる実装にある同じアセンブリを 1 つのアプリケーションから参照できるように、既定の動作を無効にすることができます。既定の動作では、アプリケーションの移植性を高めるために、このようなアセンブリは同等のものとして扱われます。|
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|既定のメモリ内オブジェクト キャッシュの構成情報を提供します。|
|[\<Thread_UseAllCpuGroups >](thread-useallcpugroups-element.md)|ランタイムによって、すべての CPU グループにマネージド スレッドを分散するかどうかを指定します。|
|[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)|タスクがハンドルされない例外によって実行中のプロセスを終了するかどうかを指定します。|
|[\<TimeSpan_LegacyFormatMode >](timespan-legacyformatmode-element.md)|ランタイムで <xref:System.TimeSpan> の値に従来の書式を使用するかどうかを指定します。|
|[\<useLegacyJit>](uselegacyjit-element.md)|共通言語ランタイムが Just-In-Time コンパイルの従来の 64 ビット JIT コンパイラを使用するかどうかを決定します。|
|[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)|ランタイムがアプリケーション ドメインごとに文字列のハッシュ コードを計算するかどうかを指定します。|
|[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)|ランタイムが内部的に使用する特定のスレッド作成時に、既定のスタック サイズではなく明示的なスタック サイズを使用することを要求します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|

## <a name="remarks"></a>コメント

構成ファイルの[\<runtime >](runtime-element.md)セクションの子要素は、アプリケーションの実行方法を構成するために共通言語ランタイムによって使用されます。 たとえば、 [\<gcServer >](gcserver-element.md)要素は、ガベージコレクターがワークステーションのガベージコレクションまたはサーバーのガベージコレクションを使用するかどうかを決定します。 [\<UseRandomizedStringHashAlgorithm >](userandomizedstringhashalgorithm-element.md)要素は、共通言語ランタイムが、アプリケーションごとまたはアプリケーションドメインごとに文字列のハッシュコードを計算するかどうかを決定 `AppContextSwitchOverrides` します

[\<runtime >](runtime-element.md)セクションの要素は、アプリケーションの起動時に共通言語ランタイムによって自動的に読み取られます。 <xref:System.AppDomainSetup.ConfigurationFile%2A?displayProperty=nameWithType> プロパティに名前を指定して、既定以外のアプリケーションドメインの構成ファイルを定義することもできます。アプリケーションドメインが読み込まれると、その設定が自動的に読み取られます。 ほとんどの場合、アプリケーションの構成ファイルの [ [ \<runtime>](runtime-element.md) ] セクションの設定を直接読み取る必要があります。

## <a name="see-also"></a>参照

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
