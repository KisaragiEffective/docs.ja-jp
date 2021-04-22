---
description: '詳細情報: CLR プロファイラと Windows ストア アプリ'
title: CLR プロファイラと Windows ストア アプリ
ms.date: 03/30/2017
dev_langs:
- csharp
applies_to:
- Windows 10
- Windows 8
helpviewer_keywords:
- profiling API
- profiling API [.NET Framework]
- profiling managed code
- profiling managed code [Windows Store Apps]
ms.assetid: 1c8eb2e7-f20a-42f9-a795-71503486a0f5
ms.openlocfilehash: e864f67aff106659194b91814bc2509d50cbf701
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99649277"
---
# <a name="clr-profilers-and-windows-store-apps"></a>CLR プロファイラと Windows ストア アプリ

このトピックでは、Windows ストア アプリ内で実行されているマネージド コードを分析する診断ツールを記述する際の考慮事項について説明します。 また、既存の開発ツールを Windows ストア アプリに対して実行する際に、それらが引き続き正常に機能するようにするための変更作業に関するガイドラインも示します。 これらの情報を理解するには、次の前提条件を満たしているのが理想的です: 共通言語ランタイム プロファイル API を使い慣れている。Windows デスクトップ アプリケーションに対して正常に動作する診断ツールで、既にこの API を使用している。Windows ストア アプリに対して正常に動作するようにツールを変更することに関心がある。

## <a name="introduction"></a>はじめに

入門用の段落を既に学習し終えた方は、CLR プロファイル API について十分に理解しているはずです。 マネージド デスクトップ アプリケーションに対して適切に機能する診断ツールを、既に作成しました。 ここでは、ご使用のツールがマネージド Windows ストア アプリに対して正常に動作するようにするための作業について説明します。 既にこの作業を行ってみた方は、これが簡単な作業ではないことにお気づきかもしれません。 実際、ツール開発者が必ずしも留意するとは限らない考慮事項がいくつかあります。 次に例を示します。

- Windows ストア アプリは、アクセス許可が厳しく制限されたコンテキストで実行されます。

- Windows メタデータ ファイルには、従来のマネージド モジュールには見られない、固有の特性があります。

- Windows ストア アプリには、対話機能が停止したときに自身の実行を中断する特性があります。

- プロセス間の通信メカニズムは、さまざまな理由で機能しなくなる可能性があります。

このトピックでは、注意する必要がある事項と、それらに適切に対処する方法を示します。

CLR プロファイル API を初めて使用する場合は、このトピックの最後にあるリソースに進んで、より詳しい概要情報を確認してください。

特定の Windows API の詳細や、その使用方法についても、このトピックでは説明しません。 このトピックを開始点と捉えたうえで、ここで参照されている Windows API の詳細については、MSDN を参照してください。

## <a name="architecture-and-terminology"></a>アーキテクチャと用語

通常、診断ツールには、以下の説明に示すようなアーキテクチャがあります。 "プロファイラー" という用語が使われていますが、この種のツールの多くでは、一般的なパフォーマンスやメモリ プロファイリングの領域にとどまらず、様々な領域の概念が関係してきます (コード カバレッジ、モック オブジェクト フレームワーク、タイムトラベル デバッグ、アプリケーション監視など)。 わかりやすくするために、このトピックでは、これらすべてのツールをプロファイラーと呼びます。

この記事では、次の用語を使用します。

**Application**

これは、プロファイラーによって分析されるアプリケーションです。 通常、このアプリケーションの開発者は、アプリケーションの問題を診断するためにプロファイラーを使用します。 従来、このアプリケーションは Windows デスクトップ アプリケーションでしたが、このトピックでは Windows ストア アプリについて見ていきます。

**プロファイラー DLL**

これは、分析対象のアプリケーションのプロセス領域に読み込まれるコンポーネントです。 このコンポーネント (プロファイラー "エージェント" とも呼ばれます) は、[ICorProfilerCallback](icorprofilercallback-interface.md)[ICorProfilerCallback Interface](icorprofilercallback-interface.md)(2、3 など) インターフェイスを実装し、[ICorProfilerInfo](icorprofilerinfo-interface.md)(2、3 など) インターフェイスを使用して、分析対象のアプリケーションに関するデータを収集し、必要に応じてアプリケーションの動作を変更します。

**プロファイラー UI**

これは、プロファイラー ユーザーが操作するデスクトップ アプリケーションです。 アプリケーションの状態をユーザーに表示し、分析対象のアプリケーションの動作を制御するための手段をユーザーに提供します。 このコンポーネントは、プロファイル対象のアプリケーションのプロセス領域とは分離された、独自のプロセス空間で常に実行されます。 プロファイラー UI は、"アタッチ トリガー" としても機能します。これは、[ICLRProfiling::AttachProfiler](iclrprofiling-attachprofiler-method.md) メソッドを呼び出すプロセスで、プロファイラー DLL が起動時に読み込まれなかった場合に、分析対象のアプリケーションからプロファイラー DLL を読み込むために使用されます。

> [!IMPORTANT]
> プロファイラー UI は、Windows ストア アプリの制御やレポートに使用される場合でも、Windows デスクトップ アプリケーションのままにしておく必要があります。 診断ツールを Windows ストア用にパッケージ化して出品することはできないものと考えてください。 ツールでは、Windows ストア アプリが実行できない処理を実行する必要があり、その多くはプロファイラー UI 内に存在します。

このドキュメント全体を通じて、サンプル コードでは次のことを前提とします。

- プロファイラー DLL は、C++ で記述されている (CLR プロファイル API の要件に従い、ネイティブ DLL である必要があるため)。

- プロファイラー UI は、C# で記述されている。 これは必須ではありませんが、プロファイラー UI のプロセスについては言語の要件がないため、簡潔で単純な言語を選択するのが合理的です。

### <a name="windows-rt-devices"></a>Windows RT デバイス

Windows RT デバイスのサポートはほぼ終了しています。 サードパーティのプロファイラーは、これらのデバイスに読み込むことはできません。 このドキュメントでは、Windows 8 PC に焦点を当てています。

## <a name="consuming-windows-runtime-apis"></a>Windows ランタイム API の使用

以下のセクションで説明するいくつかのシナリオでは、プロファイラー UI デスクトップ アプリケーションで、いくつかの新しい Windows ランタイム API を使用する必要があります。 デスクトップ アプリケーションからどの Windows ランタイム API を使用できるかや、デスクトップ アプリケーションから呼び出された場合と Windows ストア アプリから呼び出された場合とでの動作の違いについては、ドキュメントを参照してください。

プロファイラー UI がマネージド コードで記述されている場合は、それらの Windows ランタイム API を使用しやすくするために、いくつかの手順を実行する必要があります。 詳細については、「[マネージド デスクトップ アプリと Windows ランタイム](/previous-versions/windows/apps/jj856306(v=win.10))」を参照してください。

## <a name="loading-the-profiler-dll"></a>プロファイラー DLL の読み込み

このセクションでは、プロファイラー UI によって、プロファイラー DLL が Windows ストア アプリにどのように読み込まれるかを説明します。 このセクションで説明するコードは、プロファイラー UI デスクトップ アプリに属するものです。そのため、このコードでは、デスクトップ アプリに対しては安全であっても、Windows ストア アプリに対しては必ずしも安全でない Windows API が使用されています。

プロファイラー UI では、次の 2 つの方法でプロファイラー DLL をアプリケーションのプロセス領域に読み込むことができます。

- アプリケーションの起動時に、環境変数によって制御する。

- [ICLRProfiling::AttachProfiler](iclrprofiling-attachprofiler-method.md) メソッドを呼び出すことで、起動の完了後にアプリケーションにアタッチする。

最初に直面する問題の 1 つは、プロファイラー DLL の起動時読み込みとアタッチ読み込みを Windows ストア アプリで正常に動作させることです。 どちらの読み込み方法にも共通する特別な考慮事項がいくつかあるので、まずはそこから始めていきましょう。

### <a name="common-considerations-for-startup-and-attach-loads"></a>起動時読み込みとアタッチ読み込みに共通する考慮事項

**プロファイラー DLL への署名**

Windows でプロファイラー DLL の読み込みが試行される際には、プロファイラー DLL が適切に署名されているかどうかが検証されます。 されていない場合、既定では読み込みが失敗します。 この作業を実行する 2 つの方法があります。

- プロファイラー DLL が署名されていることを確認する。

- ツールを使用する前に、開発者用ライセンスを Windows 8 コンピューターにインストールする必要があることをユーザーに通知する。 これは、Visual Studio から自動的に実行することも、コマンド プロンプトから手動で行うこともできます。 詳細については、「[開発者用ライセンスを取得する](/previous-versions/windows/apps/hh974578(v=win.10))」を参照してください。

**ファイル システムのアクセス許可**

Windows ストア アプリには、自身が存在しているファイル システム上の場所からプロファイラー DLL を読み込んで実行するためのアクセス許可が必要です。既定では、Windows ストア アプリにはほとんどのディレクトリに対してこのようなアクセス許可がありません。プロファイラー DLL の読み込みに失敗すると、Windows アプリケーションのイベント ログに次のようなエントリが生成されます。

```output
NET Runtime version 4.0.30319.17929 - Loading profiler failed during CoCreateInstance.  Profiler CLSID: '{xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'.  HRESULT: 0x80070005.  Process ID (decimal): 4688.  Message ID: [0x2504].
```

一般に、Windows ストア アプリでは、ディスク上の限られた場所に対してのみアクセスが許可されます。 各 Windows ストア アプリでは、専用のアプリケーション データ フォルダーにアクセスできるほか、すべての Windows ストア アプリに対してアクセスが許可される、ファイル システム内の他のいくつかの領域にアクセスできます。 プロファイラー DLL とその依存関係は、Program Files または Program Files (x86) の下のどこかにインストールすることをお勧めします。これらの場所については、すべての Windows ストア アプリに対して、読み取りと実行のアクセス許可が既定で付与されてます。

### <a name="startup-load"></a>起動時読み込み

通常、デスクトップ アプリでは、プロファイラー UI によって必要な CLR プロファイル API 環境変数 (`COR_PROFILER`、`COR_ENABLE_PROFILING`、`COR_PROFILER_PATH` など) を含んだ環境ブロックを初期化し、その環境ブロックを使って新しいプロセスを作成することで、プロファイラー DLL の起動時読み込みが要求されます。 Windows ストア アプリについても同じことが言えますが、メカニズムが異なります。

**管理者特権で実行しない**

プロセス A で Windows ストア アプリのプロセス B を生成しようとする場合、プロセス A は、高い整合性レベルではなく (つまり、管理者特権ではなく)、中程度の整合性レベルで実行する必要があります。 つまり、プロファイラー UI を中程度の整合性レベルで実行するか、Windows ストア アプリの起動を処理するための別のデスクトップ プロセスを、中程度の整合性レベルで生成する必要があります。

**プロファイルする Windows ストア アプリの選択**

まずは、どの Windows ストア アプリを起動するかをプロファイラー ユーザーに尋ねる必要があります。 デスクトップ アプリの場合は、ファイル参照ダイアログが表示され、ユーザーは .exe ファイルを見つけて選択します。 しかし、Windows ストア アプリの場合はそれとは違って、参照ダイアログを使用することは合理的ではありません。 代わりに、インストールされている Windows ストア アプリの一覧をユーザーに表示して、ユーザーがアプリを選択できるようにするのが合理的です。

この一覧は、<xref:Windows.Management.Deployment.PackageManager> クラスを使用して生成できます。 `PackageManager` は、デスクトップ アプリで使用できる Windows ランタイム クラスです。実際、これはデスクトップ アプリで "*のみ*" 使用できます。

次のコード例は、デスクトップ アプリとして C# で記述された架空のプロファイラー UI のものですが、このコードでは、`PackageManager` を使用して Windows アプリの一覧を生成しています。

```csharp
string currentUserSID = WindowsIdentity.GetCurrent().User.ToString();
IAppxFactory appxFactory = (IAppxFactory) new AppxFactory();
PackageManager packageManager = new PackageManager();
IEnumerable<Package> packages = packageManager.FindPackagesForUser(currentUserSID);
```

**カスタム環境ブロックの指定**

新しい COM インターフェイスである [IPackageDebugSettings](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings) を使用すると、Windows ストア アプリの実行動作をカスタマイズして、特定の形式での診断を容易にすることができます。 そのメソッドの 1 つである [EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging) では、Windows ストア アプリを起動する際、アプリに環境ブロックを渡すことができるほか、自動のプロセス中断を無効にするなど、便利な効果を実装することができます。 環境ブロックでは、プロファイラー DLL を読み込むために CLR によって使用される環境変数 (`COR_PROFILER`、`COR_ENABLE_PROFILING`、および `COR_PROFILER_PATH)`) を指定する必要があります。そのため、環境ブロックは重要です。

次のコード スニペットを考えてみます。

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, debuggerCommandLine,
                                                                 (IntPtr)fixedEnvironmentPzz);
```

次の 2 つの項目について、しっかりと理解する必要があります。

- `packageFullName` は、パッケージを反復処理して `package.Id.FullName` を取得する際に確認できます。

- `debuggerCommandLine` は、さらに興味深いパラメーターです。 カスタム環境ブロックを Windows ストア アプリに渡すには、独自のシンプルなダミー デバッガーを作成する必要があります。 Windows では、中断された Windows ストア アプリが生成された後、次の例のようなコマンド ラインを使ってデバッガーを起動することで、デバッガーがアタッチされます。

    ```console
    MyDummyDebugger.exe -p 1336 -tid 1424
    ```

     `-p 1336` は、Windows ストア アプリにプロセス ID 1336 があることを意味します。`-tid 1424` は、スレッド ID 1424 が中断されているスレッドであることを意味します。 ダミー デバッガーでは、コマンド ラインから ThreadID が解析され、そのスレッドが再開された後、終了されます。

     これを行うための C++ コードの例を次に示します (エラー チェックを必ず追加してください)。

    ```cpp
    int wmain(int argc, wchar_t* argv[])
    {
        // …
        // Parse command line here
        // …

        HANDLE hThread = OpenThread(THREAD_SUSPEND_RESUME,
                                                                  FALSE /* bInheritHandle */, nThreadID);
        ResumeThread(hThread);
        CloseHandle(hThread);
        return 0;
    }
    ```

     このダミー デバッガーを診断ツールのインストールの一部として展開した後、`debuggerCommandLine` パラメーターでこのデバッガーへのパスを指定する必要があります。

**Windows ストア アプリの起動**

ようやく、Windows ストア アプリを起動する段階まで来ました。 既にこれを試したという方は、[CreateProcess](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa) が Windows ストア アプリ プロセスを作成するための手段ではないことにお気づきかもしれません。 この場合は、[IApplicationActivationManager::ActivateApplication](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) メソッドを代わりに使用する必要があります。 これを行うには、起動する Windows ストア アプリのアプリ ユーザー モデル ID を取得する必要があります。 つまり、マニフェストを参照する必要があります。

パッケージを反復処理する際には (前述の「[起動時読み込み](#startup-load)」セクションの「プロファイルする Windows ストア アプリの選択」を参照してください)、現在のパッケージのマニフェストに含まれている一連のアプリケーションを取得する必要があります。

```csharp
string manifestPath = package.InstalledLocation.Path + "\\AppxManifest.xml";

AppxPackaging.IStream manifestStream;
SHCreateStreamOnFileEx(
                    manifestPath,
                    0x00000040,     // STGM_READ | STGM_SHARE_DENY_NONE
                    0,              // file creation attributes
                    false,          // fCreate
                    null,           // reserved
                    out manifestStream);

IAppxManifestReader manifestReader = appxFactory.CreateManifestReader(manifestStream);

IAppxManifestApplicationsEnumerator appsEnum = manifestReader.GetApplications();
```

1 つのパッケージには複数のアプリケーションを含めることができ、各アプリケーションには独自のアプリケーション ユーザー モデル ID があります。 そのため、どのアプリケーションをプロファイルするかをユーザーに尋ね、その特定のアプリケーションからアプリケーション ユーザー モデル ID を取得する必要があります。

```csharp
while (appsEnum.GetHasCurrent() != 0)
{
    IAppxManifestApplication app = appsEnum.GetCurrent();
    string appUserModelId = app.GetAppUserModelId();
    //...
}
```

これで、Windows ストア アプリを起動するために必要なものが準備できました。

```csharp
IApplicationActivationManager appActivationMgr = new ApplicationActivationManager();
appActivationMgr.ActivateApplication(appUserModelId, appArgs, ACTIVATEOPTIONS.AO_NONE, out pid);
```

**DisableDebugging を呼び出すのをお忘れなく**

[IPackageDebugSettings::EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging) を呼び出した際には、[IPackageDebugSettings::DisableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) メソッドを呼び出して、自分でクリーンアップを実行するというのがお約束でした。プロファイル セッションが終了した際には、必ずこれを行うようにしてください。

### <a name="attach-load"></a>アタッチ読み込み

プロファイラー UI で、既に実行を開始しているアプリケーションにプロファイラー DLL をアタッチする場合は、[ICLRProfiling::AttachProfiler](iclrprofiling-attachprofiler-method.md) を使用します。 Windows ストア アプリの場合も、このことは同様に当てはまります。 ただし、前に示した一般的な考慮事項に加えて、ターゲットの Windows ストア アプリが中断されていないことを確認するようにしてください。

**EnableDebugging**

起動時読み込みと同様に、[IPackageDebugSettings::EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging) メソッドを呼び出します。 これは環境ブロックを渡すためには必要ありませんが、他のある機能のために必要です。それは、自動のプロセス中断を無効化することです。 これを行わないと、プロファイラー UI によって [AttachProfiler](iclrprofiling-attachprofiler-method.md) が呼び出されたときに、ターゲットの Windows ストア アプリが中断される可能性があります。 実際、ユーザーがプロファイラー UI を操作しているときに、Windows ストア アプリがユーザーの画面でアクティブになっていないと、その状況が発生する可能性があります。 また、Windows ストア アプリが中断されると、プロファイラー DLL をアタッチするために CLR から送信されるシグナルに応答できなくなります。

そのため、次のようなコードを記述する必要があります。

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, null /* debuggerCommandLine */,
                                                                 IntPtr.Zero /* environment */);
```

この呼び出しは起動時読み込みの場合に行うものと基本的に同じですが、デバッガーのコマンド ラインや環境ブロックを指定しない点が異なります。

**DisableDebugging**

プロファイル セッションが完了したときには、[IPackageDebugSettings::DisableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) を必ず呼び出すようにしてください。

## <a name="running-inside-the-windows-store-app"></a>Windows ストア アプリ内での実行

これで、Windows ストア アプリにプロファイラー DLL が読み込まれました。 次は、プロファイラー DLL に対し、Windows ストア アプリで要求されるさまざまな規則の下での動作方法を教える必要があります (どの API が許可されているかや、アクセス許可の制限された状況でどのように動作するかなど)。

### <a name="stick-to-the-windows-store-app-apis"></a>Windows ストア アプリ API への準拠

Windows API のドキュメントを見ていくと、すべての API は、デスクトップ アプリ、Windows ストア アプリ、またはその両方に適用可能と記載されていることに気づきます。 たとえば、[InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount) 関数のドキュメントの「**要件**」セクションには、この関数がデスクトップ アプリのみに適用されると記載されています。 一方、[InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex) 関数は、デスクトップ アプリと Windows ストア アプリの両方で使用できます。

プロファイラー DLL を開発する際には、それを Windows ストア アプリであるかのように扱い、API については、ドキュメントで Windows ストア アプリに使用可能と説明されているものだけを使うようにしてください。 依存関係を分析し (たとえば、プロファイラー DLL に対して `link /dump /imports` を実行することで監査が行えます)、ドキュメントを検索して、どの依存関係が有効で、どれが無効なのかを確認します。 多くの場合、違反は、ドキュメントで安全とされている、新しい形式の API に置き換えることで修正できます (たとえば、[InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount) を [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex) に置き換えるなど)。

プロファイラー DLL で、デスクトップ アプリのみに適用される API がいくつか呼び出される場合でも、プロファイラー DLL を Windows ストア アプリ内に読み込むと、それらが正常に機能しているように見えることがあります。 しかし、Windows ストア アプリで使用できないとドキュメントに記載されている API をプロファイラー DLL で使用すると、Windows ストア アプリ内に読み込んだときに、リスクが発生するので注意してください。

- このような API は、Windows ストア アプリが実行されるコンテキストで呼び出された場合、正常に機能することが保証されていません。

- このような API は、さまざまな Windows ストア アプリ プロセス内から呼び出された場合、一貫性をもって機能しない可能性があります。

- このような API は、現在のバージョンの Windows では Windows ストア アプリから正常に動作しているように見えても、今後リリースされる Windows では、異常を起こしたり無効になったりする可能性があります。

すべての違反を修正し、リスクを回避することをお勧めします。

場合によっては、どうしても必要な API があって、Windows ストア アプリに対応した代替の API が見つからないこともあるかもしれません。 そのような場合は、少なくとも次のことを行ってください。

- その API の使用を徹底的にテストする。

- Windows の今後のリリースで Windows ストア アプリの内部から呼び出された場合に、その API が突然中断したり、非表示になる可能性があることを理解する。 Microsoft では、これを互換性の問題とは見なしません。また、その API の使用をサポートすることは、優先事項とはされません。

### <a name="reduced-permissions"></a>制限されたアクセス許可

Windows ストア アプリのアクセス許可がデスクトップ アプリの場合とどのように違うかについて、このトピックで詳しく説明することはしません。 ただし、その動作は、プロファイラー DLL がリソースにアクセスしようとするたびに違ったものとなります (Windows ストア アプリに読み込まれた場合の、デスクトップ アプリと比較した動作)。 その最も一般的な例が、ファイル システムです。 Windows ストア アプリがアクセスを許可されるディスク上の場所 (「[ファイル アクセスとアクセス許可 (Windows ランタイム アプリ)](/previous-versions/windows/apps/hh967755(v=win.10))」を参照してください） はいくつかありますが、その数は限られており、プロファイラー DLL もそれと同じ制限を受けます。 コードのテストは十分に行ってください。

### <a name="inter-process-communication"></a>プロセス間通信

このホワイトペーパーの冒頭の図に示しているように、プロファイラー DLL (Windows ストア アプリのプロセス領域に読み込まれます) は、専用のカスタム プロセス間通信 (IPC) チャネルを通じて、プロファイラー UI (別個のデスクトップ アプリケーション プロセス空間で実行されます) と通信する必要があります。 プロファイラー UI は、プロファイラー DLL にシグナルを送信してその動作を変更します。プロファイラー DLL は、後処理とプロファイラー ユーザー向けの表示のために、分析対象の Windows ストア アプリからプロファイラー UI にデータを送信します。

ほとんどのプロファイラーはこの方法で動作する必要がありますが、プロファイラー DLL が Windows ストア アプリに読み込まれる場合には、IPC メカニズムの選択肢がさらに制限されます。 たとえば、名前付きパイプは Windows ストア アプリの SDK の一部ではないため、使用できません。

ただし、ファイルはもちろん存在します (その方法はより制限されます)。 イベントも使用できます。

**ファイルを介した通信**

データの多くは、プロファイラー DLL とプロファイラー UI との間で、ファイルを介して渡されることになります。 鍵となるのは、プロファイラー DLL (Windows ストア アプリのコンテキスト内) とプロファイラー UI の両方が読み取りと書き込みのアクセス権を持っているファイルの場所を選ぶことです。 たとえば、一時フォルダーのパスは、プロファイラー DLL とプロファイラー UI の両方からアクセスできる場所ですが、他の Windows ストア アプリ パッケージからはアクセスできない場所です (したがって、他の Windows ストア アプリ パッケージから記録された情報はシールドされます)。

プロファイラー UI とプロファイラー DLL は、どちらも単独でこのパスを確認できます。 プロファイラー UI は、現在のユーザーに対してインストールされているすべてのパッケージを反復処理する際 (前述のサンプル コードを参照)、`PackageId` クラスへのアクセス権を取得します。そこから、次のスニペットのようなコードを使って、一時フォルダーのパスを取得できます (他の例と同様、エラー チェックは簡潔化のために省略されています)。

```csharp
// C# code for the Profiler UI.
ApplicationData appData =
    ApplicationDataManager.CreateForPackageFamily(
        packageId.FamilyName);

tempDir = appData.TemporaryFolder.Path;
```

一方、プロファイラー DLL では、基本的に同じことを行うこともできますが、[ApplicationData.Current](xref:Windows.Storage.ApplicationData.Current%2A) プロパティを使用することで、<xref:Windows.Storage.ApplicationData> クラスにより簡単にアクセスできます。

**イベントを介した通信**

プロファイラー UI とプロファイラー DLL の間でのシグナルのセマンティクスをシンプルにしたい場合は、Windows ストア アプリ内とデスクトップ アプリ内でイベントを使用することができます。

プロファイラー DLL からは、[CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa) 関数を呼び出して、任意の名前の名前付きイベントを簡単に作成できます。 次に例を示します。

```cpp
// Profiler DLL in Windows Store app (C++).
CreateEventEx(
    NULL,  // Not inherited
    "MyNamedEvent"
    CREATE_EVENT_MANUAL_RESET, /* explicit ResetEvent() required; leave initial state unsignaled */
    EVENT_ALL_ACCESS);
```

その後、プロファイラー UI では、Windows ストア アプリの名前空間の下で、その名前付きイベントを見つける必要があります。 たとえば、プロファイラー UI では、次のようなイベント名を指定して [CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa) を呼び出すことができます。

`AppContainerNamedObjects\<acSid>\MyNamedEvent`

`<acSid>` は、Windows ストア アプリの AppContainer SID です。 このトピックの前のセクションで、現在のユーザーに対してインストールされているパッケージを反復処理する方法を示しました。 そのサンプル コードから、packageId を取得できます。 そしてその packageId から、次のようなコードを使用して `<acSid>` を取得できます。

```csharp
IntPtr acPSID;
DeriveAppContainerSidFromAppContainerName(packageId.FamilyName, out acPSID);

string acSid;
ConvertSidToStringSid(acPSID, out acSid);

string acDir;
GetAppContainerFolderPath(acSid, out acDir);
```

### <a name="no-shutdown-notifications"></a>シャットダウン通知はない

Windows ストア アプリ内で実行する場合、プロファイラー DLL で、Windows ストア アプリが終了しているという通知を受けるために、[ICorProfilerCallback::Shutdown](icorprofilercallback-shutdown-method.md) や [DllMain](/windows/desktop/Dlls/dllmain) (`DLL_PROCESS_DETACH` を含む) の呼び出しを利用することはしないでください。 実際、これらが呼び出されることはないものと考えてください。 これまで、多くのプロファイラー DLL では、ディスクへのキャッシュのフラッシュ、ファイルの終了、プロファイラー UI への通知の送信などを行うための便利な場所として、これらの通知が使用されてきました。しかし今後は、プロファイラー DLL の処理体系を少し変えていく必要があります。

プロファイラー DLL では、実行中に随時情報を記録していくようにしてください。 パフォーマンス上の理由から、バッチのサイズがしきい値を超えて増加した際には、メモリ内でバッチ処理した情報をディスクにフラッシュする必要が生じることがあります。 ただし、まだディスクにフラッシュされていない情報は、失われる可能性があると考えてください。 つまり、しきい値は適切に選択する必要があります。また、プロファイラー DLL によって書き込まれた不完全な情報を処理できるように、プロファイラー UI を強化するようにしてください。

## <a name="windows-runtime-metadata-files"></a>Windows ランタイム メタデータ ファイル

Windows ランタイム メタデータ (WinMD) ファイルの詳細については、このドキュメントでは説明しません。 このセクションでは、プロファイラー DLL の分析対象である Windows ストア アプリによって WinMD ファイルが読み込まれたときに、CLR プロファイル API がどのように反応するかについて説明します。

### <a name="managed-and-non-managed-winmds"></a>マネージドとアンマネージドの WinMD

開発者が Visual Studio を使用して新しい Windows ランタイム コンポーネント プロジェクトを作成した場合、そのプロジェクトのビルド時には、開発者によって作成されたメタデータ (クラスやインターフェイスなどの型の説明) を記述する WinMD ファイルが生成されます。 そのプロジェクトが C# または Visual Basic で記述されたマネージド言語プロジェクトである場合は、それらの型の実装も、同じ WinMD ファイルに含められます (つまり、開発者のソース コードからコンパイルされたすべての IL が含められます)。 このようなファイルは、マネージド WinMD ファイルと呼ばれます。 これらのファイルの面白い点は、Windows ランタイム メタデータと、基になる実装の両方が含まれているという点です。

これとは対照的に、開発者が C++ 用の Windows ランタイム コンポーネント プロジェクトを作成した場合、そのプロジェクトのビルド時には、メタデータのみを含んだ WinMD ファイルが生成され、実装は別のネイティブ DLL へとコンパイルされます。 同様に、Windows SDK で提供される WinMD ファイルにはメタデータのみが含まれており、実装は Windows の一部として提供される個別のネイティブ DLL へとコンパイルされます。

以下の情報は、メタデータと実装を含んだマネージド WinMD と、メタデータのみを含んだアンマネージド WinMD の両方に適用されます。

### <a name="winmd-files-look-like-clr-modules"></a>WinMD ファイルは CLR モジュールと似ている

CLR に関する限り、すべての WinMD ファイルはモジュールです。 そのため、CLR プロファイル API は、他のマネージド モジュールと同じように、WinMD ファイルが読み込まれたタイミングと、その ModuleID をプロファイラー DLL に伝達します。

プロファイラー DLL では、[ICorProfilerInfo3::GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md) メソッドを呼び出し、[COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) フラグの `pdwModuleFlags` 出力パラメーターを調べることで、WinMD ファイルを他のモジュールと区別することができます (これは、ModuleID が WinMD を表している場合にのみ設定されます)。

### <a name="reading-metadata-from-winmds"></a>WinMD からのメタデータの読み取り

WinMD ファイルには、通常のモジュールと同様、[メタデータ API](../metadata/index.md) を介して読み取ることができるメタデータが含まれています。 ただし、CLR では、WinMD ファイルを読み取る際、Windows ランタイム型が .NET Framework 型にマップされます。これは、マネージド コードをプログラミングして WinMD ファイルを使用する開発者に、より自然なプログラミング エクスペリエンスを提供するためです。 これらのマッピングの例については、「[Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)」を参照してください。

さて、プロファイラーでは、メタデータ API を使用する際、未加工の Windows ランタイム ビューと、マップ済みの .NET Framework ビューのどちらが取得されるのでしょうか。  答えは、開発者によって決まります。

WinMD に対して [ICorProfilerInfo::GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) メソッドを呼び出して、[IMetaDataImport](../metadata/imetadataimport-interface.md) などのメタデータ インターフェイスを取得する際、`dwOpenFlags` パラメーターで [ofNoTransform](../metadata/coropenflags-enumeration.md) を設定すると、このマッピングを無効にすることができます。 それ以外の場合は、既定でマッピングが有効になります。 通常、プロファイラーではマッピングが有効なまま維持されます。これにより、プロファイラー DLL で WinMD メタデータから取得された文字列 (たとえば、型の名前) が、プロファイラー ユーザーにとってわかりやすく、自然なものになります。

### <a name="modifying-metadata-from-winmds"></a>WinMD からのメタデータの変更

WinMD 内のメタデータを変更することはサポートされていません。 WinMD ファイルに対して [ICorProfilerInfo::GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) メソッドを呼び出し、`dwOpenFlags` パラメーターで [ofWrite](../metadata/coropenflags-enumeration.md) を指定した場合や、[IMetaDataEmit](../metadata/imetadataemit-interface.md) などの書き込み可能なメタデータ インターフェイスを要求した場合、[GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) は失敗します。 これは、インストルメンテーションをサポートするためにメタデータを変更する必要がある (たとえば、AssemblyRefs や新規メソッドを追加する)、IL 書き換えプロファイラーにとって特に重要なことです。 そのため、前のセクションで説明したように、[COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) を最初に確認し、そのようなモジュールに対しては書き込み可能なメタデータ インターフェイスを要求しないようにしてください。

### <a name="resolving-assembly-references-with-winmds"></a>WinMD を使用したアセンブリ参照の解決

多くのプロファイラーでは、インストルメンテーションや型検査を支援するために、メタデータ参照を手動で解決する必要があります。 このようなプロファイラーでは、WinMD を指すアセンブリ参照が CLR でどのように解決されるのかを把握しておく必要があります。なぜなら、それらの参照は標準アセンブリ参照とはまったく異なる方法で解決されるからです。

## <a name="memory-profilers"></a>メモリ プロファイラー

ガベージ コレクターとマネージド ヒープは、Windows ストア アプリとデスクトップ アプリとで根本的に異なるものではありません。 ただし、プロファイラーの作成者が知っておく必要がある、いくつかの微妙な違いがあります。

### <a name="forcegc-creates-a-managed-thread"></a>ForceGC によってマネージド スレッドが作成される

メモリ プロファイルを行う場合、通常、プロファイラー DLL では、[ForceGC Method](icorprofilerinfo-forcegc-method.md) メソッドの呼び出し元となる別のスレッドが作成されます。 この操作は新しいものではありません。 ただし、Windows ストア アプリ内でガベージ コレクションを実行すると、スレッドがマネージド スレッドに変換される可能性があります (たとえば、そのスレッドに対してプロファイル API ThreadID が作成されるなど)。

この結果を理解するには、CLR プロファイル API で定義されている、同期呼び出しと非同期呼び出しの違いについて理解しておくことが重要です。 これは、Windows ストア アプリでの非同期呼び出しの概念とは大きく異なるので注意してください。 詳細については、ブログ記事「[CORPROF_E_UNSUPPORTED_CALL_SEQUENCE が存在する理由](/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence)」を参照してください。

重要なポイントは、プロファイラーによって作成されたスレッドに対する呼び出しが、常に同期と見なされるということです。これは、それらの呼び出しが、プロファイラー DLL のいずれかの [ICorProfilerCallback](icorprofilercallback-interface.md) メソッドの実装の外部から行われた場合でも同様です。 少なくとも、以前はそうでした。 ただし、[ForceGC Method](icorprofilerinfo-forcegc-method.md) への呼び出しにより、CLR でプロファイラーのスレッドがマネージド スレッドへと変換されると、そのスレッドはプロファイラーのスレッドとは見なされなくなります。 そのため、CLR ではそのスレッドについて、より厳密な定義で同期かどうかが判断されるようになります。つまり、同期と見なされるためには、呼び出しが、プロファイラー DLL のいずれかの [ICorProfilerCallback](icorprofilercallback-interface.md) メソッドの内部から生成される必要があります。

これは実際には何を意味するのでしょうか。 ほとんどの [ICorProfilerInfo](icorprofilerinfo-interface.md) メソッドは、同期的に呼び出された場合にのみ正常に機能します。それ以外の場合は、すぐに失敗します。 そのため、プロファイラーによって作成されたスレッドに対して通常行われる他の呼び出し (たとえば、[RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md)、[RequestReJIT](icorprofilerinfo4-requestrejit-method.md)、[RequestRevert](icorprofilerinfo4-requestrevert-method.md)) のために、プロファイラー DLL で [ForceGC Method](icorprofilerinfo-forcegc-method.md) スレッドが再利用されると、問題が発生します。 [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md) などの非同期セーフ関数でも、マネージド スレッドから呼び出された場合には、特別なルールが適用されます (詳細については、ブログ記事「[プロファイラー スタック ウォーク: その基本と発展](/archive/blogs/davbr/profiler-stack-walking-basics-and-beyond)」を参照してください)。

したがって、[ForceGC Method](icorprofilerinfo-forcegc-method.md) を呼び出すためにプロファイラー DLL によって作成されるすべてのスレッドは、GC をトリガーし、その後 GC コールバックに応答する目的で "*のみ*" 使用することをお勧めします。 スタック サンプリングやデタッチなどの他のタスクを実行するために、プロファイル API への呼び出しを行うことはしないでください。

### <a name="conditionalweaktablereferences"></a>ConditionalWeakTableReferences

.NET Framework 4.5 以降では、新しい GC コールバックである [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) が導入されています。これを使用すると、プロファイラーで "*依存ハンドル*" に関するより詳細な情報を取得することができます。 これらのハンドルは、GC の有効期間管理のために、ソース オブジェクトからターゲット オブジェクトへの参照を追加するものです。 依存ハンドルはまったく新しいものではなく、マネージド コードをプログラミングする開発者は、Windows 8 や .NET Framework 4.5 以前の頃も、<xref:System.Runtime.CompilerServices.ConditionalWeakTable%602?displayProperty=nameWithType> クラスを使用して独自の依存ハンドルを作成することができました。

ただし、マネージド XAML Windows ストア アプリでは、依存ハンドルが頻繁に使用されるようになりました。 特に、CLR では、マネージド オブジェクトとアンマネージド Windows ランタイム オブジェクトとの間の参照サイクル管理を支援するために、これらが使用されます。 したがって、メモリ プロファイラーでは、これらの依存ハンドルを通知して、それらをヒープ グラフの残りの部分と共に視覚化できるようにすることが、かつてないほど重要になっています。 プロファイラー DLL では、[RootReferences2](icorprofilercallback2-rootreferences2-method.md)、[ObjectReferences](icorprofilercallback-objectreferences-method.md)、および [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) を一緒に使用して、ヒープ グラフの完全なビューを形成するようにしてください。

## <a name="conclusion"></a>まとめ

CLR プロファイル API を使用すると、Windows ストア アプリ内で実行されているマネージド コードを分析することができます。 実際、現在開発している既存のプロファイラーに変更を加えて、Windows ストア アプリに対応させることもできます。 プロファイラー UI では、Windows ストア アプリをデバッグ モードでアクティブ化するために、新しい API を使用する必要があります。 プロファイラー DLL では、Windows ストア アプリに適用可能な API のみを使用するようにしてください。 プロファイラー DLL とプロファイラー UI の間の通信メカニズムを記述する際には、Windows ストア アプリの API の制限事項を考慮すると共に、Windows ストア アプリに適用される制限付きのアクセス許可を意識するようにしてください。 プロファイラー DLL では、WinMD が CLR でどのように処理されるかや、マネージド スレッドに関してガベージ コレクターの動作がどのように異なるかを考慮するようにしてください。

## <a name="resources"></a>リソース

**共通言語ランタイム**

- [プロファイル (アンマネージ API リファレンス)](index.md)

- [メタデータ (アンマネージ API リファレンス)](../metadata/index.md)

**CLR と Windows ランタイムの相互作用**

- [Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)

**Windows ストア アプリ**

- [ファイル アクセスとアクセス許可 (Windows ランタイム アプリ](/previous-versions/windows/apps/hh967755(v=win.10))

- [開発者ライセンスを取得](/previous-versions/windows/apps/hh974578(v=win.10))

- [IPackageDebugSettings インターフェイス](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)
