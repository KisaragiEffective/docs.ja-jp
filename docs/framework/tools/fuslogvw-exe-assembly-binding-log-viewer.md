---
title: Fuslogvw.exe (アセンブリ バインディング ログ ビューアー)
ms.date: 03/30/2017
helpviewer_keywords:
- failed assembly binds
- Fuslogvw.exe
- displaying failed assembly bind details
- assemblies [.NET Framework], failed assembly binds
- locating assemblies
- Assembly Binding Log Viewer
ms.assetid: e32fa443-0778-4cc3-bf36-5c8ea297d296
ms.openlocfilehash: 2f0018dca6e5add2c5bc531103a4078307a8c8c6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73129848"
---
# <a name="fuslogvwexe-assembly-binding-log-viewer"></a>Fuslogvw.exe (アセンブリ バインディング ログ ビューアー)

アセンブリ バインディング ログ ビューアーは、アセンブリ バインドの詳細を表示します。 この情報は、.NET Framework が実行時にアセンブリを見つけられない原因を診断する場合に役立ちます。 通常、このようなエラーは、アセンブリが間違った位置に配置されているか、無効になったネイティブ イメージが存在するか、バージョン番号またはカルチャの不一致が存在する場合に発生します。 通常、共通言語ランタイムによるアセンブリ検出エラーは、アプリケーション内で <xref:System.TypeLoadException> として示されます。

> [!IMPORTANT]
> fuslogvw.exe は、管理者特権で実行する必要があります。

このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、管理者の資格情報で Visual Studio 用開発者コマンド プロンプト (または、Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

コマンド プロンプトに次のように入力します。

```console
fuslogvw
```

ビューアーには、失敗したアセンブリ バインドごとに 1 つのエントリが表示されます。 バインドを開始したアプリケーション、バインドの対象となるアセンブリ (名前、バージョン、カルチャ、公開キーなど)、およびエラーの日時の情報が、エラーごとにビューアーに表示されます。

### <a name="to-change-the-log-location-view"></a>ログ位置ビューを変更するには

1. **[Default]** を選択すると、すべてのアプリケーションの種類のバインド エラーが表示されます。 既定では、ログ エントリは wininet キャッシュのディスクのユーザーごとのディレクトリに格納されます。

2. **[Custom]** を選択すると、指定したカスタム ディレクトリのバインド エラーが表示されます。 **ログ設定** ダイアログの カスタム ログのパス を使用して、ランタイムがログを格納するカスタムの場所を有効なディレクトリ名に指定する必要があります。 このディレクトリはクリーンで、ランタイムが生成するファイルだけが含まれている必要があります。 このディレクトリに、ログに記録するエラーを生成する実行可能ファイルが含まれている場合は、その実行可能ファイルと同じ名前でディレクトリの作成が試行されるため、そのエラーはログに記録されません。 また、ログの位置から実行可能ファイルを実行しようとすると、失敗します。

    > [!NOTE]
    > カスタム バインド位置ではなく、既定のバインド位置を使用することをお勧めします。 ランタイムは wininet キャッシュに既定のバインド位置を格納するので、この位置は自動的に消去されます。カスタム バインド位置を指定する場合は、この位置を削除する手段を独自に組み込む必要があります。

### <a name="to-view-details-about-a-specific-failure"></a>特定のエラーの詳細を表示するには

1. ビューアーにエントリを表示するアプリケーション名を選択します。

2. **[View Log]** をクリックします。 また、選択したエントリをダブルクリックすることもできます。

    選択したバインド エラーについて、次の詳細が表示されます。

    - "ファイルが見つからない" や "バージョンの不一致" など、バインド エラーの具体的な原因。

    - バインドを開始したアプリケーションについての情報 (アプリケーション名、アプリケーションのルート ディレクトリ (AppBase)、プライベート検索パスが存在する場合にはそのパスなど)。

    - 検索対象となるアセンブリの ID。

    - Application、Publisher、または Administrator バージョンのポリシーが適用されている場合は、その説明。

    - [グローバル アセンブリ キャッシュ](../app-domains/gac.md)でのアセンブリの検出の有無。

    - すべてのプローブ URL の一覧。

失敗したアセンブリ バインドについての詳細情報を表示するサンプル ログ エントリを次に示します。

```output
*** Assembly Binder Log Entry  (3/5/2007 @ 12:54:20 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  C:\WINNT\Microsoft.NET\Framework\v2.0.50727\fusion.dll
Running under executable  C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\graphicfailtest.exe
--- A detailed error log follows.

=== Pre-bind state information ===
LOG: DisplayName = graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
 (Fully-specified)
LOG: Appbase = C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\
LOG: Initial PrivatePath = NULL
LOG: Dynamic Base = NULL
LOG: Cache Base = NULL
LOG: AppName = NULL
Calling assembly : graphicfailtest, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
===

LOG: Processing DEVPATH.
LOG: DEVPATH is not set. Falling through to regular bind.
LOG: Policy not being applied to reference at this time (private, custom, partial, or location-based assembly bind).
LOG: Post-policy reference: graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.EXE.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.EXE.
LOG: All probing URLs attempted and failed.
```

### <a name="to-delete-a-single-entry-from-the-log"></a>ログから単一のエントリを削除するには

1. ビューアーでエントリを選択します。

2. **[Delete Entry]** をクリックします。

### <a name="to-delete-all-entries-from-the-log"></a>ログからすべてのエントリを削除するには

- **[Delete All]** をクリックします。

### <a name="to-refresh-the-user-interface"></a>ユーザー インターフェイスに最新の情報を表示するには

- **[最新の情報に更新]** をクリックします。 ビューアーの実行中に新しいログ エントリが自動的に検出されることはありません。 新しいログ エントリを表示するには、 **[Refresh]** を使用する必要があります。

### <a name="to-change-the-log-settings"></a>ログ設定を変更するには、次の処理手順に従います。

- **[設定]** をクリックして **[ログ設定]** ダイアログ ボックスを表示します。

### <a name="to-view-the-about-dialog"></a>[バージョン情報] ダイアログを表示するには

- **[バージョン情報]** をクリックします。

## <a name="binding-logs-for-native-images"></a>ネイティブ イメージのバインディング ログ

既定では、Fuslogvw.exe は通常のアセンブリ バインド要求をログに記録します。 代わりに、[ネイティブ イメージ ジェネレーター (Ngen.exe)](ngen-exe-native-image-generator.md) を使用して作成されたネイティブ イメージのアセンブリ バインドをログに記録することもできます。

#### <a name="to-log-assembly-binds-for-native-images"></a>ネイティブ イメージのアセンブリ バインドをログに記録するには

- **[ログのカテゴリ]** グループで、 **[ネイティブ イメージ]** をクリックします。

次のログは、アプリケーションのネイティブ イメージの作成時には存在しなかった依存関係が原因で発生したエラーを示しています。 実行時の依存関係が Ngen.exe を実行したときの依存関係と異なる場合、ネイティブ イメージへのバインドはできません。

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:22:07 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\App.exe
--- A detailed error log follows.

LOG: Start binding of native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\App.exe.
LOG: Start validating native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency b, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
WRN: Dependency assembly was not found at ngen time, but is found at binding time. Disallow using this native image.
WRN: No matching native image found.
LOG: Bind to native image assembly did not succeed. Use IL image.
```

次のログは、アプリケーションが実行されたときのコンピューターのセキュリティ設定が、ネイティブ イメージの作成時のセキュリティ設定と異なるために発生したネイティブ イメージのバインディング エラーを示しています。

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:29:09 PM) ***

The operation failed.
Bind result: hr = 0x80004005. Unspecified error

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\Application101622.exe
--- A detailed error log follows.

LOG: Start binding of native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\Application101622.exe.
LOG: Start validating native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency Dependency101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Dependency evaluation succeeded.
LOG: Validation of dependencies succeeded.
LOG: Start loading all the dependencies into load context.
LOG: Loading of dependencies succeeded.
LOG: Bind to native image succeeded.
Native image has correct version information.
Attempting to use native image E:\Windows\assembly\NativeImages_v2.0.50727_64\Application101622\1ac7fadabec4f72575d807501e9fdc72\Application101622.ni.exe.
Rejecting native image because it failed the security check. The assembly's permissions must have changed since the time it was ngenned, or it is running with a different security context.
Discarding native image.
```

## <a name="the-log-settings-dialog"></a>[ログ設定] ダイアログ

**[ログ設定]** ダイアログを使用すると、次のようなアクションを実行できます。

#### <a name="to-disable-logging"></a>ログを無効にするには

- **[ログを無効にする]** をクリックします。  このオプションの既定値はオンです。

#### <a name="to-log-assembly-binds-in-exceptions"></a>アセンブリ バインドの例外をログに記録するには

- **[例外テキストに記録する]** をクリックします。 詳細度が最も低い fusion ログ情報だけが例外テキストに記録されます。 完全な情報を表示するには、その他の設定のいずれかを使用します。

  ドメインに中立的に読み込まれたアセンブリに関する「重要」メモを参照してください。

#### <a name="to-log-assembly-bind-failures"></a>アセンブリ バインドの失敗をログに記録するには

- **[バインドの失敗をディスクに記録する]** をクリックします。

  ドメインに中立的に読み込まれたアセンブリに関する「重要」メモを参照してください。

#### <a name="to-log-all-assembly-binds"></a>すべてのアセンブリ バインドをログに記録するには

- **[すべてのバインドをディスクに記録する]** をクリックします。

  ドメインに中立的に読み込まれたアセンブリに関する「重要」メモを参照してください。

> [!IMPORTANT]
> アセンブリがドメインに中立的に読み込まれた場合 (<xref:System.AppDomainSetup.LoaderOptimization%2A> プロパティを <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> または <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType> に設定した場合など)、ログが有効になっているとメモリがリークすることがあります。 これが起こるのは、ドメインに中立的なモジュールがアプリケーション ドメインに読み込まれているときにログ エントリが作成され、その後でアプリケーション ドメインがアンロードされた場合です。 このログ エントリは、プロセスが終了するまで解放されません。 一部のデバッガーは、自動的にログを有効にします。

#### <a name="to-enable-a-custom-log-path"></a>カスタムのログ パスを有効にするには

1. **[カスタム ログを有効にする]** をクリックします。

2. **[カスタム ログのパス]** テキスト ボックスにパスを入力します。

> [!NOTE]
> [アセンブリ バインディング ログ ビューアー (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) では、バインディング ログの格納に Internet Explorer (IE) のキャッシュを使用します。 IE キャッシュは時折破損することがあるため、[アセンブリ バインディング ログ ビューアー (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) の表示ウィンドウに新しいバインディング ログが表示されなくなることがあります。 IE キャッシュが破損した場合、.NET バインディング インフラストラクチャ (fusion) ではバインディング ログの読み書きができなくなります (この問題はカスタム ログ パスを使用している場合は発生しません)。破損を修復し、fusion でバインディング ログが再度表示されるようにするには、IE の [インターネット オプション] ダイアログで一時インターネット ファイルを削除して IE キャッシュを消去します。
>
> アンマネージ アプリケーションが、`IHostAssemblyManager` インターフェイスと `IHostAssemblyStore` インターフェイスを実装して共通言語ランタイムをホストしている場合、ログ エントリを wininet キャッシュに格納できません。  これらのインターフェイスを実装したカスタム ホストのログ エントリを表示するには、別のログ パスを指定する必要があります。

#### <a name="to-enable-logging-for-apps-running-in-the-windows-app-container"></a>Windows アプリ コンテナー内で実行するアプリに対してログを有効にするには

1. 前の手順に従って、カスタム ログのパスを有効にします。 既定では、Windows アプリ コンテナー内で実行しているアプリでは、ハード ディスクへのアクセスが制限されます。 指定するディレクトリでは、アプリ コンテナー内のすべてのアプリに対する読み取り/書き込みアクセスが与えられます。

2. **[没入型のログを有効にします]** チェック ボックスをオンにします。

    > [!NOTE]
    > このチェック ボックスは、Windows 8 以降でのみ有効になります。

## <a name="see-also"></a>関連項目

- <xref:System.TypeLoadException>
- [ツール](index.md)
- [グローバル アセンブリ キャッシュ](../app-domains/gac.md)
- [ランタイムがアセンブリを検索する方法](../deployment/how-the-runtime-locates-assemblies.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
