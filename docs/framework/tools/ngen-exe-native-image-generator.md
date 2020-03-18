---
title: Ngen.exe (ネイティブ イメージ ジェネレーター)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Native Image Generator
- images [.NET Framework], native
- side-by-side execution, native images
- assemblies [.NET Framework], native image
- Ngen.exe
- native image generation
- native image cache
- publisher policy applied for native images
- invalid images
- BypassNGenAttribute
- System.Runtime.BypassNGenAttribute
ms.assetid: 44bf97aa-a9a4-4eba-9a0d-cfaa6fc53a66
ms.openlocfilehash: 297bc3f9182e76523eda4d4be3112f4d1d7e3fee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75741792"
---
# <a name="ngenexe-native-image-generator"></a>Ngen.exe (ネイティブ イメージ ジェネレーター)

ネイティブ イメージ ジェネレーター (Ngen.exe) は、マネージド アプリケーションのパフォーマンスを向上するツールです。 Ngen.exe は、コンパイルされたプロセッサ固有のマシン コードを含むファイルであるネイティブ イメージを作成してローカル コンピューターのネイティブ イメージ キャッシュにインストールします。 ランタイムは、Just-In-Time (JIT) コンパイラを使用してオリジナルのアセンブリをコンパイルする代わりに、キャッシュにあるネイティブ イメージを使用できます。

> [!NOTE]
> Ngen.exe によって、.NET Framework のみをターゲットとするアセンブリのネイティブ イメージがコンパイルされます。 .Net Core 用の同等のネイティブ イメージ ジェネレーターは、[CrossGen](https://github.com/dotnet/runtime/blob/master/docs/workflow/building/coreclr/crossgen.md) です。

.NET Framework 4 では、次の変更が Ngen.exe に加えられました。

- Ngen.exe によるアセンブリのコンパイルが完全信頼で行われるようになり、コード アクセス セキュリティ (CAS: Code Access Security) ポリシーが評価されなくなりました。

- Ngen.exe で生成されたネイティブ イメージを、部分信頼で実行されているアプリケーションに読み込むことができなくなりました。

.NET Framework Version 2.0 では、次の変更が Ngen.exe に加えられました。

- アセンブリと共にその依存関係もインストールされることで、Ngen.exe の構文が簡略化されました。

- ネイティブ イメージをアプリケーション ドメイン間で共有できるようになりました。

- 新しいアクションの `update` は、無効になっていたイメージを再作成します。

- アクションの実行は、コンピューターのアイドル時間を使用してイメージを生成してインストールするサービスによって遅らせることができます。

- イメージを無効化する原因の一部が解決されました。

Windows 8 の場合、「[ネイティブ イメージ タスク](#native-image-task)」を参照してください。

Ngen.exe とネイティブ イメージ サービスの使用に関する追加情報については、「[ネイティブ イメージ サービス](#native-image-service)」を参照してください。

> [!NOTE]
> .NET Framework バージョン 1.0 とバージョン 1.1 の Ngen.exe 構文は、「[ネイティブ イメージ ジェネレーター (Ngen.exe) のレガシ構文](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms165073(v=vs.100))」に記されています。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

## <a name="syntax"></a>構文

```console
ngen action [options]
```

```console
ngen /? | /help
```

## <a name="actions"></a>アクション

各 `action` の構文を示す表を次に示します。 `action` の各部分の説明については、「[引数](#ArgumentTable)」、「[優先順位](#PriorityTable)」、「[シナリオ](#ScenarioTable)」、および「[構成](#ConfigTable)」の各表を参照してください。 `options` およびヘルプ スイッチについては、「[オプション](#OptionTable)」の表を参照してください。

|アクション|説明|
|------------|-----------------|
|`install` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`] [`/queue`[`:`{`1`&#124;`2`&#124;`3`}]]|アセンブリおよびそれに依存する項目のネイティブ イメージを生成してネイティブ イメージ キャッシュにインストールします。<br /><br /> `/queue` を指定すると、アクションはネイティブ イメージ サービスのキューに置かれます。 既定の優先順位は 3 です。 「[優先順位](#PriorityTable)」の表を参照してください。|
|`uninstall` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`]|アセンブリのネイティブ イメージとその依存関係をネイティブ イメージ キャッシュから削除します。<br /><br /> 単一のイメージとその依存関係をアンインストールするには、そのイメージをインストールしたときと同じコマンド ライン引数を使用します。 **注:** .NET Framework 4 以降では、アクション `uninstall` * はサポートされなくなりました。|
|`update` [`/queue`]|無効になったネイティブ イメージを更新します。<br /><br /> `/queue` を指定すると、更新はネイティブ イメージ サービスのキューに置かれます。 更新は常に優先順位 3 でスケジュールされるため、コンピューターがアイドル状態のときに実行されます。|
|`display` [`assemblyName` &#124; `assemblyPath`]|アセンブリのネイティブ イメージとその依存関係の状態を表示します。<br /><br /> 引数を指定しなければ、ネイティブ イメージ キャッシュのすべての内容が表示されます。|
|`executeQueuedItems` [<code>1&#124;2&#124;3</code>]<br /><br /> \- または -<br /><br /> `eqi` [1&#124;2&#124;3]|キューに置かれているコンパイル ジョブを実行します。<br /><br /> 優先順位を指定すると、優先順位が高いかまたは同じコンパイル ジョブが実行されます。 優先順位を指定しなければ、キューに置かれているすべてのコンパイル ジョブが実行されます。|
|`queue` {`pause` &#124; `continue` &#124; `status`}|ネイティブ イメージ サービスを一時停止するか、停止しているサービスを再開するか、またはサービスの状態を照会します。|

<a name="ArgumentTable"></a>

## <a name="arguments"></a>引数

|引数|説明|
|--------------|-----------------|
|`assemblyName`|アセンブリの完全な表示名。 たとえば、`"myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5"` のようにします。 **注:** `myAssembly` アクションおよび `display` アクションに対しては、`uninstall` などのように部分的なアセンブリ名を指定できます。 <br /><br /> Ngen.exe の各コマンド ラインに指定できるアセンブリは 1 つだけです。|
|`assemblyPath`|アセンブリの明示的なパスを返します。 フル パスまたは相対パスを指定できます。<br /><br /> パスを指定せずにファイル名を指定する場合、アセンブリは現在のディレクトリに存在する必要があります。<br /><br /> Ngen.exe の各コマンド ラインに指定できるアセンブリは 1 つだけです。|

<a name="PriorityTable"></a>

## <a name="priority-levels"></a>優先順位

|優先度|説明|
|--------------|-----------------|
|`1`|ネイティブ イメージが生成され、アイドル時間まで待たずに直ちにインストールされます。|
|`2`|ネイティブ イメージが生成され、アイドル時間まで待たずにインストールされますが、優先順位 1 のすべてのアクション (およびその依存関係) が完了した後にインストールされます。|
|`3`|コンピューターがアイドル状態になったことをネイティブ イメージ サービスが検出するとネイティブ イメージがインストールされます。 「[ネイティブ イメージ サービス](#native-image-service)」を参照してください。|

<a name="ScenarioTable"></a>

## <a name="scenarios"></a>シナリオ

|シナリオ|説明|
|--------------|-----------------|
|`/Debug`|デバッガーで使用できるネイティブ イメージを生成します。|
|`/Profile`|プロファイラーで使用できるネイティブ イメージを生成します。|
|`/NoDependencies`|指定されたシナリオのオプションに必要なネイティブ イメージの最小数を生成します。|

<a name="ConfigTable"></a>

## <a name="config"></a>構成

|構成|説明|
|-------------------|-----------------|
|`/ExeConfig:` `exePath`|指定された実行可能アセンブリの構成を使用します。<br /><br /> Ngen.exe は、依存関係にバインドする際に、ローダーと同じ決定をする必要があります。 <xref:System.Reflection.Assembly.Load%2A> メソッドを使用して共有コンポーネントが実行時に読み込まれると、読み込まれた依存関係のバージョンなどの共有コンポーネント用に読み込まれた依存関係はアプリケーションの構成ファイルによって決定されます。 `/ExeConfig` スイッチは、実行時にどの依存関係を読み込むかに関するガイダンスを Ngen.exe に提供します。|
|`/AppBase:` `directoryPath`|依存関係を検索するときは、アプリケーション ベースに指定されているディレクトリを使用します。|

<a name="OptionTable"></a>

## <a name="options"></a>オプション

|オプション|説明|
|------------|-----------------|
|`/nologo`|Microsoft 著作権情報を表示しません。|
|`/silent`|成功メッセージを表示しません。|
|`/verbose`|デバッグの詳細情報を表示します。 **注:** オペレーティング システムの制限により、Windows 98 と Windows Millennium Edition では追加情報は表示されません。|
|`/help`、`/?`|現在のリリースのコマンド構文とオプションを表示します。|

## <a name="remarks"></a>Remarks

Ngen.exe を実行するには、管理特権が必要です。

> [!CAUTION]
> 完全に信頼されていないアセンブリで Ngen.exe を実行しないでください。 .NET Framework 4 以降では、Ngen.exe によるアセンブリのコンパイルが完全信頼で行われるようになり、コード アクセス セキュリティ (CAS) ポリシーが評価されなくなりました。

.NET Framework 4 以降では、Ngen.exe で生成されたネイティブ イメージを、部分信頼で実行されているアプリケーションに読み込むことができなくなりました。 代わりに、Just-In-Time (JIT) コンパイラが呼び出されます。

Ngen.exe は、`install` アクションへの `assemblyname` 引数で指定されたアセンブリおよびそのすべての依存項目のネイティブ イメージを生成します。 依存関係は、アセンブリ マニフェストを参照して決定されます。 依存関係を別個にインストールする必要があるのは、アプリケーションがリフレクションを使用して依存関係を読み込む場合だけです。たとえば <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを呼び出して依存関係を読み込む場合などです。

> [!IMPORTANT]
> ネイティブ イメージに <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> メソッドは使用しないでください。 実行コンテキストの他のアセンブリは、このメソッドを使用して読み込まれるイメージを使用できません。

Ngen.exe は依存関係のカウントを保持します。 たとえば、`MyAssembly.exe` と `YourAssembly.exe` がネイティブ イメージ キャッシュにインストールされており、この両方とも `OurDependency.dll` を参照するとします。 `MyAssembly.exe` がアンインストールされても、`OurDependency.dll` はアンインストールされません。 これが削除されるのは、`YourAssembly.exe` もアンインストールされた場合だけです。

アセンブリのネイティブ イメージをグローバル アセンブリ キャッシュに生成している場合は表示名を指定します。 以下を参照してください。<xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType>

Ngen.exe が生成するネイティブ イメージは、アプリケーション ドメイン間で共有できます。 つまり、アプリケーション ドメイン間でアセンブリを共有する必要のあるアプリケーション シナリオでは Ngen.exe を使用できます。 ドメイン中立性は、次の手順で指定します。

- アプリケーションに <xref:System.LoaderOptimizationAttribute> 属性を適用します。

- 新しいアプリケーション ドメインのセットアップ情報を作成するときは、<xref:System.AppDomainSetup.LoaderOptimization%2A?displayProperty=nameWithType> プロパティを設定します。

複数のアプリケーション ドメインに同じアセンブリを読み込むときは、必ずドメイン中立のコードを使用します。 共有ドメインに読み込んだ後に非共有アプリケーション ドメインに読み込んだネイティブ イメージは使用できません。

> [!NOTE]
> ドメイン中立コードはアンロードできません。また、特に静的メンバーにアクセスする際にパフォーマンスがわずかに低下します。

「解説」セクションの内容:

- [別のシナリオのためのイメージの生成](#Scenarios)

- [ネイティブ イメージを使用する状況の決定](#WhenToUse)

  - [メモリ使用の改善](#Memory)

  - [アプリケーションの起動の高速化](#Startup)

  - [使用における考慮事項のまとめ](#UsageSummary)

- [アセンブリのベース アドレスの重要性](#BaseAddresses)

- [ハード バインディング](#HardBinding)

  - [依存関係のバインディング ヒントの指定](#DependencyHint)

  - [アセンブリの既定のバインディング ヒントの指定](#AssemblyHint)

- [遅延処理](#Deferred)

- [ネイティブ イメージと JIT コンパイル](#JITCompilation)

  - [無効なイメージ](#InvalidImages)

- [トラブルシューティング](#Troubleshooting)

  - [アセンブリ バインディング ログ ビューアー](#Fusion)

  - [JITCompilationStart マネージド デバッグ アシスタント](#MDA)

  - [ネイティブ イメージ生成の中止](#OptOut)

<a name="Scenarios"></a>

## <a name="generating-images-for-----different-scenarios"></a>別のシナリオのためのイメージの生成

アセンブリのネイティブ イメージが生成された後でそのアセンブリを実行する場合、ランタイムは、常にこのネイティブ イメージを自動的に検索して使用しようとします。 使用シナリオによっては、複数のイメージを生成できます。

たとえば、デバッグまたはプロファイルの目的でアセンブリを実行する場合、ランタイムは、`/Debug` または `/Profile` の各オプションを指定して生成されたネイティブ イメージを検索します。 一致するネイティブ イメージが見つからない場合、ランタイムは標準の JIT コンパイルに戻ります。 ネイティブ イメージをデバッグする唯一の方法は、`/Debug` オプションを使用してネイティブ イメージを作成することです。

`uninstall` アクションもシナリオを認識するため、すべてのシナリオまたは指定したシナリオだけをアンインストールできます。

<a name="WhenToUse"></a>

## <a name="determining-when-to-use-native-images"></a>ネイティブ イメージを使用する状況の決定

ネイティブ イメージによって、メモリ使用の改善と起動時間の短縮の 2 つの領域においてパフォーマンスが向上します。

> [!NOTE]
> ネイティブ イメージのパフォーマンスは、コード、データ アクセス パターン、モジュール境界を超える呼び出しの数、他のアプリケーションによって既に読み込まれている依存関係の数などの多くの要素に依存するため分析が困難です。 ネイティブ イメージがアプリケーションの利益になるかどうかを確認する唯一の方法は、主な配置シナリオにおいて慎重にパフォーマンスを測定することです。

<a name="Memory"></a>

### <a name="improved-memory-use"></a>メモリ使用の改善

プロセス間でコードを共有する場合は、ネイティブ イメージによってメモリ使用を大いに改善できます。 ネイティブ イメージは Windows PE ファイルのため、.dll ファイルの単一のコピーを複数のプロセスで共有できます。これに対して、JIT コンパイラが生成するネイティブ コードは、プライベート メモリに格納されるため共有できません。

ターミナル サービスで実行されるアプリケーションも、共有されたコード ページの恩恵を受けることができます。

さらに、JIT コンパイラを読み込まないため、各アプリケーション インスタンスが使用する固定メモリの量も減らすことができます。

<a name="Startup"></a>

### <a name="faster-application-startup"></a>アプリケーションの起動の高速化

Ngen.exe によってアセンブリをプリコンパイルすることにより、一部のアプリケーションの起動時間が短縮されます。 一般に、最初のアプリケーションが起動されると、その後のアプリケーションが使用する共有コンポーネントが読み込まれるため、アプリケーションがコンポーネント アセンブリを共有している場合にアプリケーションの起動時間が短縮されます。 アプリケーションのすべてのアセンブリをハード ディスクから読み込む必要があるコールド スタートでは、ハード ディスクのアクセス時間の影響の方が大きいため、ネイティブ イメージから受け取る恩恵はそれほど大きくありません。

メイン アプリケーション アセンブリにハード バインディングされているすべてのイメージは同時に読み込む必要があるため、ハード バインディングは起動時間に影響します。

> [!NOTE]
> .NET Framework 3.5 Service Pack 1 より前のバージョンでは、厳密な名前付きの共有コンポーネントをグローバル アセンブリ キャッシュに配置する必要があります。これは、グローバル アセンブリ キャッシュ以外に配置されている厳密な名前付きのアセンブリに対してローダーが余分な検証を行うため、起動時間は短縮されず、実質的にネイティブ イメージを使用する意味がなくなります。 .NET Framework 3.5 SP1 で導入された最適化により、この余分な検証は削除されました。

<a name="UsageSummary"></a>

### <a name="summary-of-usage-considerations"></a>使用における考慮事項のまとめ

次の一般的な考慮事項とアプリケーションの考慮事項は、アプリケーションにネイティブ イメージを使用するかどうかを評価する際に役立ちます。

- ネイティブ イメージでは、JIT コンパイル、タイプ セーフ検証などの多くの起動時アクティビティが不要なため、MSIL より読み込みが高速になります。

- ネイティブ イメージでは JIT コンパイラが不要なため、必要な初期作業セットが少なくてすみます。

- ネイティブ イメージによって、プロセス間でコードを共有できます。

- ネイティブ イメージは MSIL アセンブリよりも多くのハード ディスク容量を必要とし、生成にかなりの時間を要する場合があります。

- ネイティブ イメージは保持する必要があります。

  - 元のアセンブリまたはその依存関係のいずれかに対してサービスを提供する場合は、イメージを再生成する必要があります。

  - アプリケーションまたはシナリオごとに異なるネイティブ イメージを使用する場合、1 つのアセンブリで複数のネイティブ イメージが必要な場合があります。 たとえば、2 つのアプリケーションの構成情報で、同じ依存アセンブリに対してバインディング決定が異なる場合です。

  - ネイティブ イメージは管理者が作成する必要があります。つまり Administrators グループの Windows アカウントから作成します。

このような一般的な考慮事項に加えて、ネイティブ イメージによってパフォーマンスが向上するかどうかを検討する場合は、アプリケーションの特性も考慮する必要があります。

- 多くの共有コンポーネントを使用する環境でアプリケーションを実行する場合は、ネイティブ イメージによってコンポーネントを複数のプロセスで共有できます。

- アプリケーションが複数のアプリケーション ドメインを使用する場合は、ネイティブ イメージによってドメイン間でコード ページを共有できます。

    > [!NOTE]
    > .NET Framework Version 1.0 と 1.1 では、アプリケーション ドメイン間でネイティブ イメージを共有することはできません。 Version 2.0 以降では可能です。

- アプリケーションをターミナル サーバーで実行する場合は、ネイティブ イメージによってコード ページを共有できます。

- サイズの大きなアプリケーションでは、一般にネイティブ イメージにコンパイルすることでパフォーマンスが向上します。 サイズの小さなアプリケーションでは、一般にあまり有効ではありません。

- 長時間実行するアプリケーションでは、ランタイム JIT コンパイルのパフォーマンスの方がネイティブ イメージよりも若干優れています。 ハード バインディングによって、このパフォーマンスの違いをある程度緩和できます。

<a name="BaseAddresses"></a>

## <a name="importance-of-assembly-base-addresses"></a>アセンブリのベース アドレスの重要性

ネイティブ イメージは Windows PE ファイルのため、他の実行可能ファイルと同様に再ベースの問題があります。 ハード バインディングを使用する場合、再配置のパフォーマンス コストはさらに増大します。

ネイティブ イメージにベース アドレスを設定するには、コンパイラの適切なオプションを使用してアセンブリのベース アドレスを設定します。 Ngen.exe は、このベース アドレスをネイティブ イメージに使用します。

> [!NOTE]
> ネイティブ イメージは、元になるマネージド アセンブリより大きくなります。 ベース アドレスは、この大きなサイズを考慮して算出する必要があります。

dumpbin.exe などのツールを使用すると、ネイティブ イメージの推奨ベース アドレスを表示できます。

<a name="HardBinding"></a>

## <a name="hard-binding"></a>ハード バインディング

ハード バインディングによってスループットが向上し、ネイティブ イメージの作業セットのサイズが小さくなります。 ハード バインディングの短所は、アセンブリを読み込む際にアセンブリにハード バインディングされているすべてのイメージを読み込む必要があることです。 これによって、大きなアプリケーションの起動時間がかなり長くなります。

ハード バインディングは、すべてのアプリケーションのパフォーマンスが重要な意味を持つシナリオで読み込まれる依存関係に適しています。 ネイティブ イメージの使用のすべての局面において言えることですが、ハード バインディングによってアプリケーションのパフォーマンスが向上するかどうかを確認する唯一の方法は、パフォーマンスを慎重に測定することです。

<xref:System.Runtime.CompilerServices.DependencyAttribute> 属性と <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 属性によって、ハード バインディング ヒントを Ngen.exe に提供できます。

> [!NOTE]
> この 2 つの属性は、コマンドではなく Ngen.exe へのヒントです。 これらの属性を使用してもハード バインディングは保証されません。 属性の意味は、今後のリリースで変更される可能性があります。

<a name="DependencyHint"></a>

### <a name="specifying-a-binding-hint-for-a-dependency"></a>依存関係のバインディング ヒントの指定

アセンブリに <xref:System.Runtime.CompilerServices.DependencyAttribute> を適用して、指定された依存関係が読み込まれるかどうかを示します <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> は、ハード バインディングが適切であることを示します。<xref:System.Runtime.CompilerServices.LoadHint.Default> は、依存関係の既定値を使用する必要があることを示します。<xref:System.Runtime.CompilerServices.LoadHint.Sometimes> は、ハード バインディングが適切ではないことを示します。

2 つの依存関係があるアセンブリのための属性のコードを次に示します。 最初の依存関係 (Assembly1) はハード バインディングに適しており、2 番目 (Assembly2) は適していません。

```vb
Imports System.Runtime.CompilerServices
<Assembly:DependencyAttribute("Assembly1", LoadHint.Always)>
<Assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)>
```

```csharp
using System.Runtime.CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)]
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)]
```

```cpp
using namespace System::Runtime::CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)];
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)];
```

アセンブリ名には、ファイル名拡張子は含めません。 表示名を使用できます。

<a name="AssemblyHint"></a>

### <a name="specifying-a-default-binding-hint-for-an-assembly"></a>アセンブリの既定のバインディング ヒントの指定

既定のバインディング ヒントは、依存するアプリケーションが即座に頻繁に使用するアセンブリだけに必要です。 そのようなアセンブリには、<xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> と共に <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> を適用して、ハード バインディングを使用することを指定します。

> [!NOTE]
> <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 以外の値を使用してこの属性を適用しても、この属性をまったく適用しない場合と結果は同じため、このカテゴリに属さない .dll アセンブリに <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> を適用する理由はありません。

Microsoft では、<xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> を使用して、mscorlib.dll などのごく一部の .NET Framework のアセンブリの既定値としてハード バインディングを指定します。

<a name="Deferred"></a>

## <a name="deferred-processing"></a>遅延処理

非常に大きいアプリケーションのネイティブ イメージの生成には、かなりの時間がかかります。 同様に、共有コンポーネントへの変更またはコンピューター設定への変更には、多くのネイティブ イメージの更新が必要になります。 `install` アクションと `update` アクションには `/queue` オプションがあり、これによって、ネイティブ イメージ サービスによる遅延実行をキューに置くことができます。 さらに、Ngen.exe には、サービスを制御するための `queue` アクションと `executeQueuedItems` アクションがあります。 詳細については、「[ネイティブ イメージ サービス](#native-image-service)」を参照してください。

<a name="JITCompilation"></a>

## <a name="native-images-and-jit-compilation"></a>ネイティブ イメージと JIT コンパイル

生成できないメソッドがアセンブリ内にある場合、Ngen.exe が生成するネイティブ イメージにそのメソッドは含まれません。 ランタイムは、このアセンブリの実行中に JIT コンパイルに戻し、ネイティブ イメージから除外されたこれらのメソッドを処理します。

さらに、アセンブリがアップグレードされた場合、または何らかの理解によってイメージが無効になった場合、ネイティブ イメージは使用されません。

<a name="InvalidImages"></a>

### <a name="invalid-images"></a>無効なイメージ

Ngen.exe を使用してアセンブリのネイティブ イメージを作成する場合、出力は指定したコマンド ライン オプションおよび使用するコンピューター上の設定に依存します。 影響のある設定は次のとおりです。

- .NET Framework Version。

- オペレーティング システムのバージョン (Windows 9x ファミリから Windows NT ファミリに変更した場合)。

- アセンブリの正確な ID (ID は再コンパイル時に変更されます)。

- アセンブリが参照するすべてのアセンブリの正確な ID (ID は再コンパイル時に変更されます)。

- セキュリティ関連の要因。

Ngen.exe は、ネイティブ イメージを生成するときに上記の情報を記録します。 アセンブリが実行されると、ランタイムは、コンピューターの現在の環境と一致するオプションおよび設定を指定して生成されたネイティブ イメージを検索します。 一致するネイティブ イメージが見つからない場合、ランタイムはアセンブリを JIT コンパイルに戻します。 コンピューターの設定および環境が次のように変更されると、ネイティブ イメージは無効になります。

- .NET Framework Version。

     更新プログラムを .NET Framework に適用すると、Ngen.exe を使用して作成したすべてのネイティブ イメージは、無効になります。 したがって、.NET Framework の更新では、常に `Ngen Update` コマンドが実行されてすべてのネイティブ イメージが再生成されます。 .NET Framework は、インストールする .NET Framework ライブラリの新しいネイティブ イメージを自動的に作成します。

- オペレーティング システムのバージョン (Windows 9x ファミリから Windows NT ファミリに変更した場合)。

     たとえば、コンピューターで稼動しているオペレーティング システムのバージョンが Windows 98 から Windows XP に変更されると、ネイティブ イメージ キャッシュに格納されたすべてのネイティブ イメージは無効になります。 ただし、オペレーティング システムを Windows 2000 から Windows XP に変更した場合は、イメージは無効になりません。

- アセンブリの正確な ID。

     アセンブリを再コンパイルすると、アセンブリの対応するネイティブ イメージは無効になります。

- アセンブリが参照するすべてのアセンブリの正確な ID。

     マネージド アセンブリを更新する場合、そのアセンブリに直接または間接に依存するすべてのネイティブ イメージが無効になるため、これらを再生成する必要があります。 これには、通常の参照と強くバインドされた依存関係も含まれます。 ソフトウェア更新プログラムを適用するときには、インストール プログラムで `Ngen Update` コマンドを実行して、依存するすべてのネイティブ イメージを再生成する必要があります。

- セキュリティ関連の要因。

     アセンブリに事前に与えられていたアクセス許可を制限するマシン セキュリティ ポリシーを変更すると、そのアセンブリのコンパイル済みのネイティブ イメージが無効になることがあります。

     共通言語ランタイムがコード アクセス セキュリティを管理する方法と、アクセス許可を使用する方法の詳細については、「[コード アクセス セキュリティ](../misc/code-access-security.md)」を参照してください。

<a name="Troubleshooting"></a>

## <a name="troubleshooting"></a>トラブルシューティング

次のトラブルシューティングのトピックでは、アプリケーションで使用されているネイティブ イメージと使用できないネイティブ イメージ、JIT コンパイラがメソッドのコンパイルを開始するタイミングの判別、および指定されたメソッドのネイティブ イメージ コンパイルを中止する方法を示します。

<a name="Fusion"></a>

### <a name="assembly-binding-log-viewer"></a>アセンブリ バインディング ログ ビューアー

アプリケーションでネイティブ イメージが使用されているかどうかを確認するには、[Fuslogvw.exe (アセンブリ バインディング ログ ビューアー)](fuslogvw-exe-assembly-binding-log-viewer.md) を使用できます。 バインディング ログ ビューアーのウィンドウの **[ログのカテゴリ]** ボックスで、 **[ネイティブ イメージ]** をクリックします。 Fuslogvw.exe は、ネイティブ イメージが拒否された理由に関する情報を提供します。

<a name="MDA"></a>

### <a name="the-jitcompilationstart-managed-debugging-assistant"></a>JITCompilationStart マネージド デバッグ アシスタント

[jitCompilationStart](../debug-trace-profile/jitcompilationstart-mda.md) マネージド デバッグ アシスタント (MDA) を使用すると、JIT コンパイラが関数のコンパイルを開始するタイミングを判別できます。

<a name="OptOut"></a>

### <a name="opting-out-of-native-image-generation"></a>ネイティブ イメージ生成の中止

場合によっては、NGen.exe が特定のメソッドのネイティブ イメージを生成することが困難であったり、メソッドをネイティブ イメージにコンパイルするのではなく JIT コンパイルしたいことがあります。 そのような場合、`System.Runtime.BypassNGenAttribute` 属性を使用して、NGen.exe が特定のメソッドのネイティブ イメージを生成するのを防ぐことができます。 この属性は、ネイティブ イメージに含めたくないコードを持つ各メソッドに個別に適用する必要があります。 NGen.exe はこの属性を認識し、対応するメソッドのネイティブ イメージにコードを生成しません。

ただし、`BypassNGenAttribute` は .NET Framework クラス ライブラリ内の型としては定義されていません。 コード内で属性を使用するには、最初に次のように定義する必要があります。

[!code-csharp[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#1)]
[!code-vb[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#1)]

この後、メソッドごとに属性を適用できるようになります。 次の例は、`ExampleClass.ToJITCompile` メソッドのネイティブ イメージを生成しないようにネイティブ イメージ ジェネレーターに指示しています。

[!code-csharp[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#2)]
[!code-vb[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#2)]

## <a name="examples"></a>使用例

次のコマンドは、現在のディレクトリにある `ClientApp.exe` のネイティブ イメージを生成し、ネイティブ イメージ キャッシュにインストールします。 アセンブリの構成ファイルが存在する場合、Ngen.exe はその構成ファイルを使用します。 さらに、ネイティブ イメージは、`ClientApp.exe` が参照するあらゆる .dll ファイルに対して生成されます。

```console
ngen install ClientApp.exe
```

Ngen.exe によってインストールされるイメージは、ルートとも呼ばれます。 ルートは、アプリケーションまたは共有コンポーネントです。

指定したパスにある `MyAssembly.exe` のネイティブ イメージを生成するコマンドを次に示します。

```console
ngen install c:\myfiles\MyAssembly.exe
```

アセンブリおよびその依存関係を検索する場合、Ngen.exe は共通言語ランタイムと同じプローブ ロジックを使用します。 既定では、`ClientApp.exe` が格納されているディレクトリがアプリケーションのベース ディレクトリとして使用され、すべてのアセンブリのプローブはこのディレクトリから始まります。 この動作は、`/AppBase` オプションを使用してオーバーライドできます。

> [!NOTE]
> この動作は、アプリケーション ベースが現在のディレクトリに設定される .NET Framework Version 1.0 と 1.1 における Ngen.exe から変更されています。

アセンブリは、参照を伴わない依存関係を持つことができます。たとえば、<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドを使用して .dll ファイルを読み込む場合などです。 アプリケーション アセンブリの構成情報と `/ExeConfig` オプションを使用して、このような .dll ファイルのネイティブ イメージを作成できます。 `MyLib.dll,` の構成情報を使用して `MyApp.exe` のネイティブ イメージを生成するコマンドを次に示します。

```console
ngen install c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

この方法でインストールされたアセンブリは、アプリケーションと共に削除されません。

依存関係をアンインストールするには、それをインストールしたときと同じコマンド ライン オプションを使用します。 前の例から `MyLib.dll` をアンインストールするコマンドを次に示します。

```console
ngen uninstall c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

グローバル アセンブリ キャッシュにアセンブリのネイティブ イメージを作成するには、アセンブリの表示名を使用します。 次に例を示します。

```console
ngen install "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
```

NGen.exe は、インストールする各シナリオに対して個別のイメージ セットを生成します。 たとえば、次のコマンドはネイティブ イメージの通常操作用の完全なセット、デバッグ用の完全なセット、およびプロファイル用の第 3 のセットをインストールします。

```console
ngen install MyApp.exe
ngen install MyApp.exe /debug
ngen install MyApp.exe /profile
```

### <a name="displaying-the-native-image-cache"></a>ネイティブ イメージ キャッシュの表示

ネイティブ イメージは、キャッシュにインストールすると、Ngen.exe を使用して表示できます。 ネイティブ イメージ キャッシュ内のすべてのネイティブ イメージを表示するコマンドを次に示します。

```console
ngen display
```

`display` アクションは最初にすべてのルート アセンブリを表示し、次にコンピューター上のすべてのネイティブ イメージの一覧を表示します。

アセンブリの情報だけを表示する場合は、アセンブリの簡易名を使用します。 次のコマンドは、部分名 `MyAssembly` に一致するネイティブ イメージ キャッシュのすべてのネイティブ イメージ、その依存関係、および `MyAssembly` に依存するすべてのルートを表示します。

```console
ngen display MyAssembly
```

共有コンポーネント アセンブリに依存するルートを知ることは、共有コンポーネントがアップグレードされた後の `update` アクションの影響を予測する際に役立ちます。

アセンブリのファイル拡張子を指定する場合は、パスを指定するか、またはアセンブリが格納されているディレクトリから Ngen.exe を実行する必要があります。

```console
ngen display c:\myApps\MyAssembly.exe
```

ネイティブ イメージ キャッシュ内で `MyAssembly` という名前を持ち、バージョン 1.0.0.0 であるすべてのネイティブ イメージを表示するコマンドを次に示します。

```console
ngen display "myAssembly, version=1.0.0.0"
```

### <a name="updating-images"></a>イメージの更新

一般に、共有コンポーネントがアップグレードされると、イメージが更新されます。 イメージまたは依存関係が変更されたすべてのネイティブ イメージを更新するには、引数なしで `update` アクションを使用します。

```console
ngen update
```

すべてのイメージを更新するプロセスは、長くなることがあります。 ネイティブ イメージ サービスによる更新は、`/queue` オプションを使用してキューに置くことができます。 `/queue` オプションとインストールの優先順位の詳細については、「[ネイティブ イメージ サービス](#native-image-service)」を参照してください。

```console
ngen update /queue
```

### <a name="uninstalling-images"></a>イメージのアンインストール

Ngen.exe は、共有コンポーネントに依存するすべてのアセンブリが削除された場合だけ共有コンポーネントが削除されるように依存関係の一覧を維持します。 また、ルートとしてインストールされた共有コンポーネントは削除されません。

次のコマンドは、ルートの `ClientApp.exe` のすべてのシナリオをアンインストールします。

```console
ngen uninstall ClientApp
```

`uninstall` アクションは、特定のシナリオを削除するために使用できます。 次のコマンドは、ルートの `ClientApp.exe` のすべてのデバッグ シナリオをアンインストールします。

```console
ngen uninstall ClientApp /debug
```

> [!NOTE]
> `/debug` シナリオをアンインストールしても、`/profile` と `/debug.` の両方を含むシナリオはアンインストールされません。

次のコマンドは、`ClientApp.exe` の特定のバージョンのすべてのシナリオをアンインストールします。

```console
ngen uninstall "ClientApp, Version=1.0.0.0"
```

次のコマンドは、`"ClientApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL",` のすべてのシナリオまたはアセンブリのデバッグ シナリオだけをアンインストールします。

```console
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL" /debug
```

`install` アクションと同様に、拡張子を指定する場合は、アセンブリが格納されているディレクトリから Ngen.exe を実行するか、またはフル パスを指定する必要があります。

ネイティブ イメージ サービスに関連する例については、「[ネイティブ イメージ サービス](#native-image-service)」を参照してください。

## <a name="native-image-task"></a>ネイティブ イメージ タスク

ネイティブ イメージ タスクは、ネイティブ イメージを生成および保持する Windows タスクです。 ネイティブ イメージ タスクは、サポートされるシナリオでネイティブ イメージを自動的に生成し、解放します。 また、インストーラーが、[Ngen.exe (ネイティブ イメージ ジェネレーター)](ngen-exe-native-image-generator.md) を使用して、遅延時にネイティブ イメージを生成および更新できるようにします。

各アーキテクチャを対象とするアプリケーションのコンパイルを許可するために、ネイティブ イメージ タスクはコンピューターでサポートされる CPU アーキテクチャごとに一度登録されます。

|タスク名|32 ビット コンピューター|64 ビット コンピューター|
|---------------|----------------------|----------------------|
|NET Framework NGEN v4.0.30319|はい|はい|
|NET Framework NGEN v4.0.30319 64|いいえ|はい|

ネイティブ イメージ タスクは、Windows 8 以降の実行時に .NET Framework 4.5 以降のバージョンで使用できます。 Windows の以前のバージョンでは、.NET Framework は[ネイティブ イメージ サービス](#native-image-service)を使用します。

### <a name="task-lifetime"></a>タスクの有効期間

一般に、Windows タスク スケジューラは、毎晩、コンピューターがアイドル状態のときにネイティブ イメージ タスクを開始します。 このタスクでは、アプリケーション インストーラーによってキューに入れられている遅延作業、遅延されているネイティブ イメージ更新要求、自動イメージ作成がないかどうかをチェックします。 このタスクは未完了の作業項目を完了すると、停止します。 このタスクの実行中にコンピューターがアイドル状態になると、タスクは停止します。

また、ネイティブ イメージ タスクは手動で開始することもできます。その場合には、タスク スケジューラ UI を使用するか、NGen.exe を手動で呼び出します。 このいずれかの方法でこのタスクを開始すると、コンピューターがアイドル状態でなくなると実行を続行します。 NGen.exe を使用して手動で作成されたイメージは、アプリケーション インストーラーで予測可能な動作を有効にするために優先順位が設定されます。

## <a name="native-image-service"></a>ネイティブ イメージ サービス

ネイティブ イメージ サービスは、ネイティブ イメージを生成および保持する Windows サービスです。 ネイティブ イメージ サービスによって、開発者はコンピューターがアイドル状態になるまでネイティブ イメージのインストールと更新を遅延できます。

通常ネイティブ イメージ サービスは、アプリケーションまたはアップデートのインストール プログラム (インストーラー) によって開始されます。 優先順位 3 のアクションでは、サービスはコンピューターのアイドル時間に実行します。 このサービスは自身の状態を保存するため、再起動を繰り返しても必要に応じて続けて実行できます。 複数のイメージ コンパイルをキューに配置できます。

このサービスは、手動の Ngen.exe コマンドともやり取りします。 手動で実行するコマンドは、バックグラウンド アクティビティより優先されます。

> [!NOTE]
> Windows Vista では、ネイティブ イメージ サービスに表示される名前は "Microsoft.NET Framework NGEN v2.0.50727_X86" または "Microsoft.NET Framework NGEN v2.0.50727_X64" です。 これより前のすべてのバージョンの Microsoft Windows では、名前は ".NET Runtime Optimization Service v2.0.50727_X86" または ".NET Runtime Optimization Service v2.0.50727_X64" です。

### <a name="launching-deferred-operations"></a>遅延の操作の起動

インストールまたはアップグレードを開始する前に、サービスを停止する必要があります。 これによって、インストーラーがファイルをコピーするか、またはアセンブリをグローバル アセンブリ キャッシュに格納している間に、サービスが実行されないようにできます。 次の Ngen.exe のコマンド ラインによって、サービスを停止します。

```console
ngen queue pause
```

すべての遅延操作がキューに置かれた後に、次のコマンドによってサービスを再開します。

```console
ngen queue continue
```

新しいアプリケーションをインストールする場合または共有コンポーネントを更新する場合にネイティブ イメージの生成を遅延するには、`install` アクションまたは `update` アクションで `/queue` オプションを使用します。 次の Ngen.exe のコマンド ラインは、共有コンポーネントのネイティブ イメージをインストールし、影響を受けるすべてのルートの更新を実行します。

```console
ngen install MyComponent /queue
ngen update /queue
```

`update` アクションは、`MyComponent` を使用するネイティブ イメージだけでなく、無効になったすべてのネイティブ イメージを再作成します。

多くのルートで構成されるアプリケーションでは、遅延されたアクションの優先順位を指定できます。 次のコマンドは、次の 3 つのルートのインストールをキューに置きます。 `Assembly1` がアイドル時間まで待たずに最初にインストールされます。 `Assembly2` もアイドル時間まで待たずにインストールされますが、優先順位 1 のアクションがすべてインストールされた後にインストールされます。 `Assembly3` は、コンピューターがアイドル状態になったことをサービスが検出するとインストールされます。

```console
ngen install Assembly1 /queue:1
ngen install Assembly2 /queue:2
ngen install Assembly3 /queue:3
```

`executeQueuedItems` アクションを使用すると、キューに置かれているアクションを同時に強制的に実行できます。 オプションの優先順位を指定すると、このアクションは同等またはそれ以下の優先順位を持っているキュー内のアクションだけに適用されます。 既定の優先順位は 3 であるため、次の Ngen.exe コマンドは、キューに置かれているすべてのアクションを即座に処理し、その処理が完了するまで戻りません。

```console
ngen executeQueuedItems
```

Ngen.exe は同期コマンドを実行し、ネイティブ イメージ サービスは使用しません。 ネイティブ イメージ サービスの実行中に Ngen.exe を使用してアクションを実行できます。

### <a name="service-shutdown"></a>サービスのシャットダウン

`/queue` オプションを組み込んだ Ngen.exe コマンドの実行によって開始されたサービスは、すべてのアクションが完了するまでバックグラウンドで実行されます。 このサービスは自身の状態を保存するため、再起動を繰り返しても必要に応じて処理を継続できます。 キューに待機しているアクションがないことを検出したサービスは、次回コンピューターの再起動のときに再起動されないように自身の状態をリセットしてから、シャットダウンします。

### <a name="service-interaction-with-clients"></a>クライアントとサービスとのやり取り

.NET Framework Version 2.0 では、必ず Ngen.exe というコマンド ライン ツールを使用してネイティブ イメージ サービスとやり取りします。 インストール スクリプトでコマンド ライン ツールを使用してネイティブ イメージ サービスのアクションをキューに置いたり、サービスとやり取りしたりします。

## <a name="see-also"></a>関連項目

- [ツール](index.md)
- [マネージド実行プロセス](../../standard/managed-execution-process.md)
- [ランタイムがアセンブリを検索する方法](../deployment/how-the-runtime-locates-assemblies.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
