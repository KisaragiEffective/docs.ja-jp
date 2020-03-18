---
title: CLR ETW キーワードおよびレベル
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW keywords
- CLR ETW levels
- ETW, CLR keywords
- ETW, CLR levels
ms.assetid: fdf5856d-516b-4042-849d-911c4518a6cb
ms.openlocfilehash: 929ed00c44b52dd94fc9d15e564cce7eeff1619e
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716187"
---
# <a name="clr-etw-keywords-and-levels"></a>CLR ETW キーワードおよびレベル
Windows (ETW) イベントのイベント トレースは、カテゴリとレベルによってフィルター処理できます。 イベントの [CLR ETW キーワード](#clr-etw-keywords) は、イベントをカテゴリ別にフィルタ処理できます。これらはランタイム プロバイダーとランダウン プロバイダー用に組み合わせて使用します。 [イベント レベル](#etw-event-levels) は、フラグによって識別されます。  
  
## <a name="clr-etw-keywords"></a>CLR ETW キーワード  
 キーワードは、組み合わせて値を生成できるフラグです。 実際には、コマンド ライン ユーティリティを呼び出す際に、キーワード名ではなくキーワードの 16 進数の値を使用します。  
  
 これらのキーワードの説明を次の表に示します。  
  
- [CLR ETW ランタイム キーワード](#runtime)  
  
- [CLR ETW ランダウン キーワード](#rundown)  
  
- [ランタイム プロバイダーのシンボル解決のキーワードの組み合わせ](#runtime_combo)  
  
- [ランダウン プロバイダーのシンボル解決のキーワードの組み合わせ](#rundown_combo)  
  
<a name="runtime"></a>   
### <a name="clr-etw-runtime-keywords"></a>CLR ETW ランタイム キーワード  
 CLR ETW のランタイム キーワード、その値、およびその用途を次の表に示します。  
  
|ランタイム キーワード名|Value|目的|  
|--------------------------|-----------|-------------|  
|`GCKeyword`|0x00000001|[ガベージ コレクション イベント](garbage-collection-etw-events.md)のコレクションを有効にします。|  
|`LoaderKeyword`|0x00000008|[ローダー イベント](loader-etw-events.md)のコレクションを有効にします。|  
|`JITKeyword`|0x00000010|[Just-In-Time (JIT) イベント](jit-tracing-etw-events.md)のコレクションを可能にします。|  
|`NGenKeyword`|0x00000020|ネイティブ イメージ メソッド (ネイティブ イメージ ジェネレーター、Ngen.exe によって処理されるメソッド) のイベントの収集を可能にします。 `StartEnumerationKeyword` と `EndEnumerationKeyword`で使用します。 このキーワードには高いオーバーヘッドが設定されています。 読み込まれたすべての NGen モジュールの中すべてのメソッドに対してイベントを生成します。 可能であれば、このキーワードを使用する代わりに、プロファイリング ツールによって生成されたプログラム データベース (PDB) を使用して、NGen モジュールからメソッドに関する情報を取得することをお勧めします。 この表で後述する `OverrideAndSuppressNGenEventsKeyword` も参照してください。|  
|`StartEnumerationKeyword`|0x00000040|ランタイム内のすべてのメソッドの列挙を有効にします。 `NGenKeyword`と組み合わせて使用します。|  
|`EndEnumerationKeyword`|0x00000080|ランタイム内の破棄されたすべてのメソッドの列挙を有効にします。 `JITKeyword` と `NGenKeyword`を組み合わせて使用します。|  
|`SecurityKeyword`|0x00000400|[セキュリティ イベント](security-etw-events.md)のコレクションを有効にします。|  
|`AppDomainResourceManagementKeyword`|0x00000800|アプリケーション ドメイン レベルでのリソース監視イベントのコレクションを有効にします。|  
|`JITTracingKeyword`|0x00001000|[JIT トレース イベント](jit-tracing-etw-events.md)のコレクションを有効にします。|  
|`InteropKeyword`|0x00002000|[相互運用イベント](interop-etw-events.md)のコレクションを有効にします。|  
|`ContentionKeyword`|0x00004000|[競合イベント](contention-etw-events.md)のコレクションを有効にします。|  
|`ExceptionKeyword`|0x00008000|[例外イベント](exception-thrown-v1-etw-event.md)のコレクションを有効にします。|  
|`ThreadingKeyword`|0x00010000|[スレッド プール イベント](thread-pool-etw-events.md)のコレクションを有効にします。|  
|`OverrideAndSuppressNGenEventsKeyword`|0x00040000|(.NET Framework 4.5 以降で使用できます)。高いオーバーヘッドの `NGenKeyword` キーワードを抑制し、NGen モジュール内のメソッドに対してイベントが生成されないようにします。 .NET Framework 4.5 以降では、プロファイリングツールは `OverrideAndSuppressNGenEventsKeyword` と `NGenKeyword` を一緒に使用して、NGen モジュール内のメソッドのイベントの生成を抑制する必要があります。 これにより、プロファイル ツールはより効率的な NGen PDB を使用して NGen モジュール内のメソッドに関する情報を取得できます。 .NET Framework 4 以前のバージョンの CLR では、NGen PDB の作成はサポートされていません。 これらのバージョンにおいて、CLR は `OverrideAndSuppressNGenEventsKeyword` を認識せず、 `NGenKeyword` を処理して NGen モジュール内のメソッドのイベントを生成します。|  
|`PerfTrackKeyWord`|0x2000000|`ModuleLoad` イベントおよび `ModuleRange` イベントのコレクションを有効にします。|  
|`StackKeyword`|0x40000000|CLR [スタック トレース イベント](stack-etw-event.md)のコレクションを有効にします。|  
  
<a name="rundown"></a>   
### <a name="clr-etw-rundown-keywords"></a>CLR ETW ランダウン キーワード  
 CLR ETW ランダウン キーワード、その値、およびその用途を次の表に示します。  
  
|ランダウン キーワード名|Value|目的|  
|--------------------------|-----------|-------------|  
|`LoaderRundownKeyword`|0x00000008|`StartRundownKeyword` および `EndRundownKeyword`と一緒に使用する場合のローダー イベントのコレクションを有効にします。|  
|`JitRundownKeyword`|0x00000010|`DCStart` および `DCEnd` と一緒に使用する場合の、メソッド `StartRundownKeyword` と JIT コンパイルされたメソッドの `EndRundownKeyword`イベントのコレクションを有効にします。|  
|`NGenRundownKeyword`|0x00000020|`DCStart` および `DCEnd` と一緒に使用する場合の、メソッド `StartRundownKeyword` と NGen ネイティブ イメージ メソッドの `EndRundownKeyword`イベントのコレクションを有効にします。 このキーワードには高いオーバーヘッドが設定されています。 読み込まれたすべての NGen モジュールの中すべてのメソッドに対してイベントを生成します。 可能であれば、このキーワードを使用する代わりに、プロファイリング ツールによって生成されたプログラム データベース (PDB) を使用して、NGen モジュールからメソッドに関する情報を取得することをお勧めします。 この表で後述する `OverrideAndSuppressNGenEventsRundownKeyword` も参照してください。|  
|`StartRundownKeyword`|0x00000040|開始ランダウン中のシステム状態の列挙を有効にします。|  
|`EndRundownKeyword`|0x00000100|終了ランダウン中のシステム状態の列挙を有効にします。|  
|`AppDomainResourceManagementRundownKeyword`|0x00000800|<xref:System.AppDomain> または `StartRundownKeyword` と一緒に使用した場合の、 `EndRundownKeyword`レベルでのリソース監視のイベントのコレクションを有効にします。|  
|`ThreadingKeyword`|0x00010000|スレッド プール イベントのコレクションを有効にします。|  
|`OverrideAndSuppressNGenEventsRundownKeyword`|0x00040000|(.NET Framework 4.5 以降で使用できます)。高いオーバーヘッドの `NGenRundownKeyword` キーワードを抑制し、NGen モジュール内のメソッドに対してイベントが生成されないようにします。 .NET Framework 4.5 以降では、プロファイリングツールは `OverrideAndSuppressNGenEventsRundownKeyword` と `NGenRundownKeyword` を一緒に使用して、NGen モジュール内のメソッドのイベントの生成を抑制する必要があります。 これにより、プロファイル ツールはより効率的な NGen PDB を使用して NGen モジュール内のメソッドに関する情報を取得できます。 .NET Framework 4 以前のバージョンの CLR では、NGen PDB の作成はサポートされていません。 これらのバージョンにおいて、CLR は `OverrideAndSuppressNGenEventsRundownKeyword` を認識せず、 `NGenRundownKeyword` を処理して NGen モジュール内のメソッドのイベントを生成します。|  
|`PerfTrackKeyWord`|0x2000000|`ModuleDCStart`、 `ModuleDCEnd`、 `ModuleRangeDCStart`、および `ModuleRangeDCEnd` の各イベントのコレクションを有効にします。|   
  
<a name="runtime_combo"></a>   
### <a name="keyword-combinations-for-symbol-resolution-for-the-runtime-provider"></a>ランタイム プロバイダーのシンボル解決のキーワードの組み合わせ  
  
|キーワードとフラグ|アプリケーション ドメイン、アセンブリ、モジュールのロード/アンロード イベント|メソッドのロード/アンロード イベント (動的イベントを除く)|動的メソッドのロード/破棄イベント|  
|------------------------|--------------------------------------------------------------|----------------------------------------------------------|-----------------------------------------|  
|`LoaderKeyword`|イベントをロードおよびアンロードします。|ありません。|ありません。|  
|`JITKeyword`<br /><br /> (+ `StartEnumerationKeyword` は何も追加しません)|ありません。|イベントをロードします。|イベントをロードおよびアンロードします。|  
|`JITKeyword` +<br /><br /> `EndEnumerationKeyword`|ありません。|イベントをロードおよびアンロードします。|イベントをロードおよびアンロードします。|  
|`NGenKeyword`|ありません。|ありません。|該当なし。|  
|`NGenKeyword` +<br /><br /> `StartEnumerationKeyword`|ありません。|イベントをロードします。|該当なし。|  
|`NGenKeyword` +<br /><br /> `EndEnumerationKeyword`|ありません。|イベントをアンロードします。|該当なし。|  
  
<a name="rundown_combo"></a>   
### <a name="keyword-combinations-for-symbol-resolution-for-the-rundown-provider"></a>ランダウン プロバイダーのシンボル解決のキーワードの組み合わせ  
  
|キーワードとフラグ|アプリケーション ドメイン、アセンブリ、モジュールの DCStart/DCEnd イベント|メソッドの DCStart/DCEnd イベント (イベントの動的メソッドを含む)|  
|------------------------|----------------------------------------------------------------|----------------------------------------------------------------------|  
|`LoaderRundownKeyword` +<br /><br /> `StartRundownKeyword`|`DCStart` イベント。|ありません。|  
|`LoaderRundownKeyword` +<br /><br /> `EndRundownKeyword`|`DCEnd` イベント。|ありません。|  
|`JITKeyword` +<br /><br /> `StartRundownKeyword`|ありません。|`DCStart` イベント。|  
|`JITKeyword` +<br /><br /> `EndRundownKeyword`|ありません。|`DCEnd` イベント。|  
|`NGenKeyword` +<br /><br /> `StartRundownKeyword`|ありません。|`DCStart` イベント。|  
|`NGenKeyword` +<br /><br /> `EndRundownKeyword`|ありません。|`DCEnd` イベント。|  

## <a name="etw-event-levels"></a>ETW イベントのレベル  
 ETW イベントはレベルでフィルター処理することもできます。 レベルが 0x5 に設定されている場合 0x5 以下を含むすべてのレベルのイベント (キーワードによって有効になったカテゴリに属するイベント) が発生します。 レベルが 0x2 に設定されている場合、レベル 0x2 以下に属するイベントのみが発生します。  
  
 各レベルには次のような意味があります。  
  
 0x5 - 詳細  
  
 0x4 - 情報  
  
 0x3 - 警告  
  
 0x2 - エラー  
  
 0x1 - 重大  
  
 0x0 - LogAlways  
  
## <a name="see-also"></a>関連項目

- [CLR ETW プロバイダー](clr-etw-providers.md)
- [CLR ETW イベント](clr-etw-events.md)
- [共通言語ランタイムの ETW イベント](etw-events-in-the-common-language-runtime.md)
