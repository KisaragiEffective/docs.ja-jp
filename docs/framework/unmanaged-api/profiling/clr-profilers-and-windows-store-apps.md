---
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
ms.openlocfilehash: 1a839c4cd99e21bc2a3ebd90cf3302a475c02e17
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75938136"
---
# <a name="clr-profilers-and-windows-store-apps"></a>CLR プロファイラと Windows ストア アプリ

このトピックでは、Windows ストアアプリ内で実行されているマネージコードを分析する診断ツールを記述するときに考慮する必要がある事項について説明します。 また、既存の開発ツールを Windows ストアアプリに対して実行するときに引き続き機能するように変更するためのガイドラインも提供します。 この情報を理解するには、共通言語ランタイムプロファイル API を使い慣れていることをお勧めします。 Windows デスクトップアプリケーションに対して正しく実行される診断ツールで既にこの API を使用しているので、ツールを変更することをお勧めします。Windows ストアアプリに対して正常に実行されます。

## <a name="introduction"></a>はじめに

入門用の段落を過ぎた場合は、CLR プロファイル API について理解している必要があります。 管理対象のデスクトップアプリケーションに対して適切に機能する診断ツールが既に作成されています。 ここで、ツールが管理対象の Windows ストアアプリで動作するようにするための作業について説明します。 既にこの作業を行ったことがあるかもしれませんが、これは簡単な作業ではないことを発見しました。 実際には、すべてのツール開発者にとって明らかでない可能性がある考慮事項がいくつかあります。 例:

- Windows ストアアプリは、大幅に削減されたアクセス許可を持つコンテキストで実行されます。

- Windows メタデータファイルには、従来のマネージモジュールと比較した場合の固有の特性があります。

- Windows ストアアプリは、対話機能が停止したときに自身を中断する習慣を持ちます。

- プロセス間の通信メカニズムは、さまざまな理由で機能しなくなる可能性があります。

このトピックでは、注意する必要がある事項と、それらを適切に処理する方法を示します。

CLR プロファイル API を初めて使用する場合は、このトピックの最後にあるリソースに進んで、より良い概要情報を確認してください。

特定の Windows Api の詳細とその使用方法についても、このトピックでは説明しません。 このトピックは出発点として、MSDN を参照して、ここで参照されている Windows Api の詳細を確認してください。

## <a name="architecture-and-terminology"></a>アーキテクチャと用語

通常、診断ツールには、次の図に示すようなアーキテクチャがあります。 "Profiler" という用語を使用しますが、このようなツールの多くは、コードカバレッジ、モックオブジェクトフレームワーク、タイムトラベルデバッグ、アプリケーション監視などの領域への一般的なパフォーマンスまたはメモリのプロファイリングよりも優れています。 わかりやすくするために、このトピックでは、これらすべてのツールをプロファイラーと呼びます。

このトピックでは、次の用語を使用します。

**アプリケーション**

これは、プロファイラーが分析しているアプリケーションです。 通常、このアプリケーションの開発者は、アプリケーションの問題を診断するためにプロファイラーを使用しています。 従来、このアプリケーションは Windows デスクトップアプリケーションですが、このトピックでは Windows ストアアプリについて見ていきます。

**プロファイラー DLL**

これは、分析対象のアプリケーションのプロセス空間に読み込まれるコンポーネントです。 このコンポーネント (プロファイラー "agent" とも呼ばれます) は、 [ICorProfilerCallback](icorprofilercallback-interface.md)[ICorProfilerCallback Interface](icorprofilercallback-interface.md)(2, 3 など) インターフェイスを実装し、 [ICorProfilerInfo](icorprofilerinfo-interface.md)(2、3など) インターフェイスを使用して、分析されたアプリケーションに関するデータを収集し、アプリケーションの動作の側面を変更する可能性があります。

**プロファイラー UI**

これは、プロファイラーユーザーが操作するデスクトップアプリケーションです。 アプリケーションの状態をユーザーに表示し、分析されたアプリケーションの動作を制御する手段をユーザーに与える必要があります。 このコンポーネントは、プロファイル対象のアプリケーションのプロセス空間とは別に、常に独自のプロセス空間で実行されます。 プロファイラー UI は、 [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md)メソッドを呼び出すプロセスである "アタッチトリガー" として機能することもあります。これは、プロファイラー dll が起動時に読み込まれなかった場合に、分析されたアプリケーションがプロファイラー dll を読み込むために使用します。

> [!IMPORTANT]
> プロファイラーの UI は、windows ストアアプリの制御とレポートに使用されている場合でも、Windows デスクトップアプリケーションのままにしておく必要があります。 Windows ストアに診断ツールをパッケージ化して出荷できるとは思わないでください。 ツールでは、Windows ストアアプリが実行できない処理を実行する必要があり、その多くはプロファイラー UI 内に存在します。

このドキュメント全体で、サンプルコードでは次のことを前提としています。

- プロファイラー DLL は、CLR プロファイルC++API の要件に従って、ネイティブ dll である必要があるため、で記述されています。

- プロファイラーの UI は、でC#記述されています。 これは必要ありませんが、プロファイラー UI のプロセスの言語には要件がないため、簡潔で単純な言語を選択できないのはなぜですか。

### <a name="windows-rt-devices"></a>Windows RT デバイス

Windows RT デバイスは非常にロックダウンされています。 サードパーティのプロファイラーは、単純にそのようなデバイスに読み込むことができません。 このドキュメントでは、Windows 8 Pc に焦点を当てています。

## <a name="consuming-windows-runtime-apis"></a>Windows ランタイム Api の使用

以下のセクションで説明するいくつかのシナリオでは、Profiler UI デスクトップアプリケーションは、いくつかの新しい Windows ランタイム Api を使用する必要があります。 このドキュメントを参照して、デスクトップアプリケーションから使用できる Windows ランタイム Api と、デスクトップアプリケーションや Windows ストアアプリから呼び出されたときの動作が異なるかどうかを理解します。

プロファイラーの UI がマネージコードで記述されている場合は、これらの Windows ランタイム Api を簡単に使用できるようにするために必要な手順がいくつかあります。 詳細については、「[マネージデスクトップアプリと Windows ランタイム](https://docs.microsoft.com/previous-versions/windows/apps/jj856306(v=win.10))」を参照してください。

## <a name="loading-the-profiler-dll"></a>プロファイラー DLL を読み込んでいます

このセクションでは、プロファイラーの UI によって、Windows ストアアプリがプロファイラー DLL を読み込む方法について説明します。 このセクションで説明するコードは、Profiler UI デスクトップアプリに属しているため、デスクトップアプリには安全な Windows Api を使用しますが、Windows ストアアプリでは安全であるとは限りません。

プロファイラーの UI を使用すると、次の2つの方法でプロファイラー DLL をアプリケーションのプロセス領域に読み込むことができます。

- アプリケーションの起動時に、環境変数によって制御されます。

- [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md)メソッドを呼び出すことによって、スタートアップの完了後にアプリケーションにアタッチします。

最初の問題点の1つは、Windows ストアアプリで正常に動作するように、プロファイラー DLL のスタートアップ読み込みとアタッチ-読み込みを取得することです。 どちらの形式の読み込みでも、一般的ないくつかの特別な考慮事項が共有されるので、ここから始めましょう。

### <a name="common-considerations-for-startup-and-attach-loads"></a>スタートアップとアタッチの読み込みに関する一般的な考慮事項

**プロファイラー DLL に署名しています**

Windows がプロファイラー DLL を読み込もうとすると、プロファイラー DLL が正しく署名されているかどうかが検証されます。 それ以外の場合、既定では読み込みに失敗します。 これには、2 つの方法があります。

- プロファイラー DLL が署名されていることを確認します。

- ツールを使用する前に、Windows 8 コンピューターに開発者ライセンスをインストールする必要があることをユーザーに通知します。 これは、Visual Studio から自動的に実行することも、コマンドプロンプトから手動で行うこともできます。 詳細については、「[開発者ライセンスを取得する](https://docs.microsoft.com/previous-versions/windows/apps/hh974578(v=win.10))」を参照してください。

**ファイルシステムのアクセス許可**

Windows ストアアプリには、residesBy が既定のファイルシステム上の場所からプロファイラー DLL を読み込んで実行するためのアクセス許可が必要です。また、Windows ストアアプリには、ほとんどのディレクトリに対するこのようなアクセス許可がありません。また、プロファイラー DLL の読み込みに失敗しました。では、Windows アプリケーションイベントログに次のようなエントリが生成されます。

```output
NET Runtime version 4.0.30319.17929 - Loading profiler failed during CoCreateInstance.  Profiler CLSID: '{xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'.  HRESULT: 0x80070005.  Process ID (decimal): 4688.  Message ID: [0x2504].
```

一般に、Windows ストアアプリでは、ディスク上の限られた場所にのみアクセスできます。 各 Windows ストアアプリは、独自のアプリケーションデータフォルダーにアクセスできます。また、ファイルシステム内の他のいくつかの領域を使用して、すべての Windows ストアアプリがアクセスを許可されるようにすることもできます。 すべての Windows ストアアプリに既定で読み取りと実行のアクセス許可が付与されているので、プロファイラー DLL とその依存関係を Program Files または Program Files (x86) のどこかにインストールすることをお勧めします。

### <a name="startup-load"></a>スタートアップ読み込み

通常、デスクトップアプリでは、プロファイラーの UI は、必要な CLR プロファイル API 環境変数 (つまり、`COR_PROFILER`、`COR_ENABLE_PROFILING`、および `COR_PROFILER_PATH`) を含む環境ブロックを初期化し、その環境ブロックを使用して新しいプロセスを作成することによって、プロファイラー DLL のスタートアップ読み込みを要求します。 Windows ストアアプリでも同じことが当てはまりますが、メカニズムは異なります。

**管理者特権で実行しない**

プロセス A が Windows ストアアプリのプロセス B を生成しようとした場合、プロセス A は高整合性レベル (つまり、昇格されていない) ではなく、中程度の整合性レベルで実行する必要があります。 これは、Profiler UI が中程度の整合性レベルで実行されている必要があるか、または Windows ストアアプリの起動を処理するために、中程度の整合性レベルで別のデスクトッププロセスを生成する必要があることを意味します。

**プロファイリングする Windows ストアアプリの選択**

最初に、起動する Windows ストアアプリをプロファイラーユーザーに要求します。 デスクトップアプリの場合、ファイル参照ダイアログが表示され、ユーザーは .exe ファイルを見つけて選択します。 ただし、Windows ストアアプリは異なります。 [参照] ダイアログボックスを使用することは意味がありません。 代わりに、ユーザーが選択できるように、インストールされている Windows ストアアプリの一覧をユーザーに表示することをお勧めします。

<xref:Windows.Management.Deployment.PackageManager> クラスを使用して、このリストを生成できます。 `PackageManager` はデスクトップアプリで使用できる Windows ランタイムクラスであり、実際にはデスクトップアプリで*のみ*使用できます。

次のコード例では、でC#デスクトップアプリとして記述された仮定のプロファイラー UI から、`PackageManager` を使用して Windows アプリの一覧を生成します。

```csharp
string currentUserSID = WindowsIdentity.GetCurrent().User.ToString();
IAppxFactory appxFactory = (IAppxFactory) new AppxFactory();
PackageManager packageManager = new PackageManager();
IEnumerable<Package> packages = packageManager.FindPackagesForUser(currentUserSID);
```

**カスタム環境ブロックの指定**

新しい COM インターフェイス[Ipackagedebugsettings](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)を使用すると、Windows ストアアプリの実行動作をカスタマイズして、何らかの形式の診断を簡単に行うことができます。 [Enabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging)のメソッドの1つは、起動時に環境ブロックを Windows ストアアプリに渡すことができるほか、自動プロセス中断を無効にするなどの便利な効果があります。 プロファイラー DLL を読み込むために CLR によって使用される環境変数 (`COR_PROFILER`、`COR_ENABLE_PROFILING`、および `COR_PROFILER_PATH)`) を指定する必要があるため、環境ブロックは重要です。

次のコード スニペットを考えてみます。

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, debuggerCommandLine,
                                                                 (IntPtr)fixedEnvironmentPzz);
```

次の2つの項目を取得する必要があります。

- `packageFullName` は、パッケージを繰り返し処理して `package.Id.FullName`を取得するときに確認できます。

- `debuggerCommandLine` はもう少し興味深いものです。 カスタム環境ブロックを Windows ストアアプリに渡すために、独自の単純なダミーデバッガーを作成する必要があります。 Windows ストアアプリが中断された後、次の例のようなコマンドラインを使用してデバッガーを起動すると、デバッガーがアタッチされます。

    ```console
    MyDummyDebugger.exe -p 1336 -tid 1424
    ```

     `-p 1336` は、Windows ストアアプリにプロセス ID 1336 があることを意味します。 `-tid 1424` は、スレッド ID 1424 が中断されているスレッドであることを意味します。 ダミーデバッガーは、コマンドラインから ThreadID を解析し、そのスレッドを再開してから終了します。

     これを行うコードC++例を次に示します (エラーチェックを必ず追加してください)。

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

     このダミーデバッガーを診断ツールのインストールの一部としてデプロイし、`debuggerCommandLine` パラメーターでこのデバッガーへのパスを指定する必要があります。

**Windows ストアアプリを起動しています**

Windows ストアアプリを起動すると、最後に到着します。 これを既に試している場合は、 [CreateProcess](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa)が Windows ストアアプリプロセスを作成する方法ではないことに気が付きます。 代わりに、 [IApplicationActivationManager::::](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication)を使用する必要があります。 これを行うには、起動している Windows ストアアプリのアプリユーザーモデル ID を取得する必要があります。 これは、マニフェストを少し掘り下げていく必要があることを意味します。

パッケージを繰り返し処理しているとき (前の「[スタートアップロード](#startup-load)」セクションの「プロファイルする Windows ストアアプリを選択する」を参照してください)、現在のパッケージのマニフェストに含まれているアプリケーションのセットを取得する必要があります。

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

はい。1つのパッケージに複数のアプリケーションを含めることができ、各アプリケーションには独自のアプリケーションユーザーモデル ID があります。 そのため、プロファイルするアプリケーションをユーザーに要求し、その特定のアプリケーションからアプリケーションユーザーモデル ID を取得する必要があります。

```csharp
while (appsEnum.GetHasCurrent() != 0)
{
    IAppxManifestApplication app = appsEnum.GetCurrent();
    string appUserModelId = app.GetAppUserModelId();
    //...
}
```

最後に、Windows ストアアプリを起動するために必要なものが完成しました。

```csharp
IApplicationActivationManager appActivationMgr = new ApplicationActivationManager();
appActivationMgr.ActivateApplication(appUserModelId, appArgs, ACTIVATEOPTIONS.AO_NONE, out pid);
```

**DisableDebugging を必ず呼び出してください**

[Ipackagedebugsettings:: EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging)を呼び出したときに、 [ipackagedebugsettings::D isabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging)メソッドを呼び出して自分でクリーンアップすることを約束しました。そのため、プロファイルセッションが終了しているときは、このことを確認してください。

### <a name="attach-load"></a>読み込みのアタッチ

プロファイラーの UI が、既に実行を開始しているアプリケーションにプロファイラー DLL をアタッチする場合、 [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md)を使用します。 Windows ストアアプリでも同じことが当てはまります。 ただし、前に示した一般的な考慮事項に加えて、ターゲットの Windows ストアアプリが中断されていないことを確認してください。

**EnableDebugging**

スタートアップロードと同様に、 [Ipackagedebugsettings:: EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging)メソッドを呼び出します。 環境ブロックを渡すためには必要ありませんが、その他の機能の1つ (自動プロセス中断の無効化) が必要です。 それ以外の場合、プロファイラーの UI が[Attachprofiler](iclrprofiling-attachprofiler-method.md)を呼び出すと、ターゲットの Windows ストアアプリが中断される可能性があります。 実際には、ユーザーがプロファイラー UI を操作していて、Windows ストアアプリがユーザーの画面でアクティブになっていない場合があります。 また、Windows ストアアプリが中断されている場合は、CLR がプロファイラー DLL をアタッチするために送信するシグナルに応答できません。

次のようにします。

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, null /* debuggerCommandLine */,
                                                                 IntPtr.Zero /* environment */);
```

この呼び出しは起動時の読み込み時と同じですが、デバッガーのコマンドラインや環境ブロックを指定しない点が異なります。

**DisableDebugging**

常に、プロファイルセッションが完了したときに[Ipackagedebugsettings::D isableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging)を呼び出すことを忘れないでください。

## <a name="running-inside-the-windows-store-app"></a>Windows ストアアプリ内での実行

Windows ストアアプリは最後にプロファイラー DLL を読み込みました。 ここで、プロファイラー DLL は、許可されている Api やアクセス許可の制限を使用した実行方法など、Windows ストアアプリで必要とされるさまざまな規則によって再生する方法を教える必要があります。

### <a name="stick-to-the-windows-store-app-apis"></a>Windows ストアアプリ Api に従う

Windows API を参照すると、すべての API がデスクトップアプリ、Windows ストアアプリ、またはその両方に適用されることがわかります。 たとえば、 [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount)関数のドキュメントの「**要件**」セクションでは、関数がデスクトップアプリにのみ適用されることを示しています。 これに対し、 [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex)関数は、デスクトップアプリと Windows ストアアプリの両方で使用できます。

プロファイラー DLL を開発するときは、それを Windows ストアアプリであるかのように扱い、Windows ストアアプリで使用可能なものとしてドキュメント化されている Api のみを使用します。 依存関係を分析します (たとえば、プロファイラー DLL に対して `link /dump /imports` を実行して監査します)。その後、ドキュメントを検索して、どの依存関係が ok であるかということを確認します。 ほとんどの場合、違反を修正するには、安全として記述されている API の新しい形式を使用します (たとえば、 [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount)を[InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex)に置き換えます)。

プロファイラー DLL は、デスクトップアプリにのみ適用されるいくつかの Api を呼び出しますが、プロファイラー DLL が Windows ストアアプリ内に読み込まれていても動作しているようです。 Windows ストアアプリプロセスに読み込まれるときに、プロファイラー DLL の Windows ストアアプリで使用するためのドキュメントに記載されていない API を使用する危険性があることに注意してください。

- このような Api は、Windows ストアアプリが実行される一意のコンテキストで呼び出された場合に動作することは保証されていません。

- このような Api は、さまざまな Windows ストアアプリプロセス内から呼び出された場合、一貫して動作しない可能性があります。

- このような Api は、現在のバージョンの Windows では Windows ストアアプリから正常に動作しているように見えますが、Windows の将来のリリースでは壊れたり無効になったりする可能性があります。

すべての違反を修正し、リスクを回避することをお勧めします。

特定の API を使用しなくても、Windows ストアアプリに適した置換を見つけることができないことがわかります。 このような場合は、少なくとも次のようにします。

- その API の使用をテスト、テスト、テストします。

- Windows の今後のリリースで Windows ストアアプリの内部から呼び出された場合、API が突然中断または非表示になる可能性があることを理解してください。 これは、Microsoft による互換性の問題とは見なされません。また、の使用をサポートすることは、優先されません。

### <a name="reduced-permissions"></a>制限されたアクセス許可

Windows ストアアプリのアクセス許可とデスクトップアプリの違いについては、このトピックでは説明しません。 ただし、この動作は、プロファイラー DLL (デスクトップアプリと比較して Windows ストアアプリに読み込まれた場合) がリソースにアクセスしようとするたびに異なります。 ファイルシステムは、最も一般的な例です。 指定された Windows ストアアプリがアクセスできるようにするディスク上の場所はいくつかあります (「[ファイルのアクセスとアクセス許可 (Windows ランタイムアプリ](https://docs.microsoft.com/previous-versions/windows/apps/hh967755(v=win.10)))」を参照してください。また、プロファイラー DLL は同じ制限を受けます。 コードを十分にテストします。

### <a name="inter-process-communication"></a>プロセス間通信

このホワイトペーパーの冒頭にある図に示すように、Profiler DLL (Windows ストアアプリのプロセス領域に読み込まれます) は、独自のカスタムプロセスを通じて (別のデスクトップアプリケーションプロセス空間で実行されている) Profiler UI と通信する必要があります。通信 (IPC) チャネル。 プロファイラーの UI は、プロファイラー DLL にシグナルを送信して動作を変更します。プロファイラー DLL は、分析された Windows ストアアプリからプロファイラーの UI にデータを送信して後処理し、プロファイラーユーザーに表示します。

ほとんどのプロファイラーはこの方法で動作する必要がありますが、プロファイラー DLL が Windows ストアアプリに読み込まれるときに、IPC メカニズムの選択肢が制限されます。 たとえば、名前付きパイプは Windows ストアアプリ SDK の一部ではないため、使用できません。

しかしもちろん、ファイルは依然として存在しますが、より制限されています。 イベントも使用できます。

**ファイルを使用した通信**

ほとんどのデータは、Profiler DLL と Profiler UI の間でファイルを介して渡される可能性があります。 キーとして、プロファイラー DLL (Windows ストアアプリのコンテキスト内) とプロファイラー UI の両方がに対する読み取りと書き込みのアクセス権を持つファイルの場所を選択します。 たとえば、一時フォルダーのパスは、プロファイラー DLL とプロファイラー UI の両方がアクセスできますが、他の Windows ストアアプリパッケージがアクセスできない場所です (したがって、他の Windows ストアアプリパッケージからログに記録される情報はシールドされます)。

プロファイラー UI とプロファイラー DLL は、どちらもこのパスを個別に判別できます。 プロファイラーの UI は、現在のユーザーに対してインストールされているすべてのパッケージを反復処理する (前のサンプルコードを参照) と、`PackageId` クラスへのアクセス権を取得します。このクラスから、このスニペットと同様のコードを使用して、一時フォルダーのパスを取得できます。 (簡潔にするために、エラーチェックは省略されています)。

```csharp
// C# code for the Profiler UI.
ApplicationData appData =
    ApplicationDataManager.CreateForPackageFamily(
        packageId.FamilyName);

tempDir = appData.TemporaryFolder.Path;
```

一方、プロファイラー DLL は基本的に同じことを行うことができますが、 [Applicationdata. Current](xref:Windows.Storage.ApplicationData.Current%2A)プロパティを使用して <xref:Windows.Storage.ApplicationData> クラスに簡単にアクセスできます。

**イベントによる通信**

プロファイラーの UI とプロファイラーの DLL の間に単純なシグナル化のセマンティクスが必要な場合は、デスクトップアプリだけでなく Windows ストアアプリ内でもイベントを使用できます。

プロファイラー DLL から[Createeventex](/windows/desktop/api/synchapi/nf-synchapi-createeventexa)関数を呼び出して、任意の名前の名前付きイベントを作成できます。 例:

```cpp
// Profiler DLL in Windows Store app (C++).
CreateEventEx(
    NULL,  // Not inherited
    "MyNamedEvent"
    CREATE_EVENT_MANUAL_RESET, /* explicit ResetEvent() required; leave initial state unsignaled */
    EVENT_ALL_ACCESS);
```

その後、プロファイラーの UI は、Windows ストアアプリの名前空間の下で、その名前付きイベントを見つける必要があります。 たとえば、プロファイラーの UI で[Createeventex](/windows/desktop/api/synchapi/nf-synchapi-createeventexa)を呼び出して、イベント名を

`AppContainerNamedObjects\<acSid>\MyNamedEvent`

`<acSid>` は、Windows ストアアプリの AppContainer SID です。 このトピックの前のセクションでは、現在のユーザーに対してインストールされているパッケージを反復処理する方法を示しました。 このサンプルコードから、packageId を取得できます。 また、packageId から、次のようなコードを使用して `<acSid>` を取得できます。

```csharp
IntPtr acPSID;
DeriveAppContainerSidFromAppContainerName(packageId.FamilyName, out acPSID);

string acSid;
ConvertSidToStringSid(acPSID, out acSid);

string acDir;
GetAppContainerFolderPath(acSid, out acDir);
```

### <a name="no-shutdown-notifications"></a>シャットダウン通知がありません

Windows ストアアプリ内で実行する場合、プロファイラー dll は、windows ストアアプリが終了していることをプロファイラー dll `DLL_PROCESS_DETACH`に通知するために、 [ICorProfilerCallback:: Shutdown](icorprofilercallback-shutdown-method.md)または [DllMain](/windows/desktop/Dlls/dllmain) (を含む) のいずれにも依存しないようにする必要があります。 実際には、これらが呼び出されることはありません。 これまで、多くのプロファイラー Dll は、ディスクへのキャッシュのフラッシュ、ファイルの終了、プロファイラーの UI への通知の送信などの便利な場所として、これらの通知を使用していました。ただし、現在はプロファイラー DLL を少し異なる方法で整理する必要があります。

プロファイラー DLL は、記録された情報をログに記録する必要があります。 パフォーマンス上の理由から、バッチのサイズがしきい値を超えて増加したときに、メモリ内の情報をバッチ処理してディスクにフラッシュすることが必要になる場合があります。 しかし、ディスクにフラッシュされていない情報は失われる可能性があると仮定します。 これは、しきい値を適切に選択し、プロファイラーの DLL によって書き込まれた不完全な情報を処理するためにプロファイラー UI を強化する必要があることを意味します。

## <a name="windows-runtime-metadata-files"></a>メタデータファイルの Windows ランタイム

Windows ランタイムメタデータ (WinMD) ファイルの詳細については、このドキュメントでは説明しません。 このセクションでは、プロファイラー DLL が分析している Windows ストアアプリによって WinMD ファイルが読み込まれるときに、CLR プロファイル API がどのように反応するかについて説明します。

### <a name="managed-and-non-managed-winmds"></a>マネージドと管理されていない WinMDs

開発者が Visual Studio を使用して新しい Windows ランタイムコンポーネントプロジェクトを作成する場合、そのプロジェクトのビルドによって、開発者によって作成されたメタデータ (クラスやインターフェイスなどの型の説明) を記述する WinMD ファイルが生成されます。 このプロジェクトがまたは Visual Basic にC#記述されたマネージ言語プロジェクトである場合、同じ WinMD ファイルにこれらの型の実装も含まれます (つまり、開発者のソースコードからコンパイルされたすべての IL が含まれます)。 このようなファイルは、マネージ WinMD ファイルと呼ばれます。 Windows ランタイムメタデータと基になる実装の両方が含まれていることに注目してください。

これに対し、開発者が用にC++Windows ランタイムコンポーネントプロジェクトを作成した場合、そのプロジェクトのビルドでは、メタデータのみを含む WinMD ファイルが生成され、実装は別のネイティブ DLL にコンパイルされます。 同様に、Windows SDK に出荷される WinMD ファイルにはメタデータのみが含まれており、実装は Windows の一部として出荷される個別のネイティブ Dll にコンパイルされます。

次の情報は、メタデータと実装を含むマネージド WinMDs と、メタデータのみを含む、管理されていない WinMDs に適用されます。

### <a name="winmd-files-look-like-clr-modules"></a>WinMD ファイルは CLR モジュールのように見えます。

CLR に関する限り、すべての WinMD ファイルはモジュールです。 そのため、CLR プロファイル API は、他のマネージモジュールと同じように、WinMD ファイルが読み込まれたときと、その ModuleIDs どのようなものであるかをプロファイラー DLL に伝えます。

プロファイラー DLL は、 [ICorProfilerInfo3:: GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md)メソッドを呼び出し、 [COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md)フラグの `pdwModuleFlags` 出力パラメーターを調べることによって、他のモジュールとの WinMD ファイルを区別できます。 (ModuleID が WinMD を表している場合にのみ設定されます)。

### <a name="reading-metadata-from-winmds"></a>WinMDs からのメタデータの読み取り

WinMD ファイル (通常のモジュールなど) には、[メタデータ api](../../../../docs/framework/unmanaged-api/metadata/index.md)を使用して読み取ることができるメタデータが含まれています。 ただし、CLR は、WinMD ファイルを読み取るときに、Windows ランタイム型を .NET Framework 型にマップします。これにより、マネージコードをプログラミングして WinMD ファイルを使用する開発者は、より自然なプログラミングエクスペリエンスを実現できます。 これらのマッピングの例については、「 [Windows ストアアプリおよび Windows ランタイムの .NET Framework サポート](../../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)」を参照してください。

これにより、プロファイラーがメタデータ Api を使用したときに取得するビュー (未加工の Windows ランタイムビュー、またはマップされた .NET Framework ビュー) を確認できます。  答えは、ユーザーによって異なります。

WinMD で[ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md)メソッドを呼び出して、 [IMetaDataImport](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)などのメタデータインターフェイスを取得する場合は、`dwOpenFlags` パラメーターで[ofnotransform](../../../../docs/framework/unmanaged-api/metadata/coropenflags-enumeration.md)を設定して、このマッピングを無効にすることができます。 それ以外の場合、既定ではマッピングが有効になります。 通常、プロファイラーはマッピングを有効なままにします。これにより、プロファイラー DLL が WinMD メタデータから取得する文字列 (たとえば、型の名前) がわかりやすく、自然にプロファイラーユーザーに表示されます。

### <a name="modifying-metadata-from-winmds"></a>WinMDs からのメタデータの変更

WinMDs でのメタデータの変更はサポートされていません。 WinMD ファイルに対して[ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md)メソッドを呼び出し、`dwOpenFlags` パラメーターに[ofwrite](../../../../docs/framework/unmanaged-api/metadata/coropenflags-enumeration.md)を指定した場合、または[IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)などの書き込み可能なメタデータインターフェイスを要求した場合、 [GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md)は失敗します。 これは、インストルメンテーションをサポートするためにメタデータを変更する必要がある (たとえば、AssemblyRefs や新しいメソッドを追加する) 必要がある、プロファイラーの IL 書き換えに特に重要です。 そのため、前のセクションで説明したように[COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md)を最初に確認し、そのようなモジュールで書き込み可能なメタデータインターフェイスを要求しないようにする必要があります。

### <a name="resolving-assembly-references-with-winmds"></a>WinMDs を使用したアセンブリ参照の解決

多くのプロファイラーは、インストルメンテーションまたは型検査を支援するために、メタデータ参照を手動で解決する必要があります。 このようなプロファイラーは、CLR が WinMDs を指すアセンブリ参照を解決する方法を認識している必要があります。これは、これらの参照が標準アセンブリ参照とはまったく異なる方法で解決されるためです。

## <a name="memory-profilers"></a>メモリプロファイラー

ガベージコレクターとマネージヒープは、Windows ストアアプリとデスクトップアプリで根本的に異なります。 ただし、プロファイラーの作成者が知っておく必要がある微妙な違いがいくつかあります。

### <a name="forcegc-creates-a-managed-thread"></a>ForceGC によってマネージスレッドが作成されます。

メモリプロファイルを実行する場合、通常、プロファイラー DLL は、 [Forcegc メソッド](icorprofilerinfo-forcegc-method.md)メソッドの呼び出し元となる別のスレッドを作成します。 これは新しいものではありません。 しかし、当然のことですが、Windows ストアアプリ内でガベージコレクションを実行すると、スレッドがマネージスレッドに変換される可能性があります (たとえば、そのスレッドに対してプロファイル API ThreadID が作成されます)。

この結果を理解するには、CLR プロファイル API で定義されている同期呼び出しと非同期呼び出しの違いを理解しておくことが重要です。 これは、Windows ストアアプリの非同期呼び出しの概念とは大きく異なることに注意してください。 詳細については、ブログ記事「 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE がある理由](https://docs.microsoft.com/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence)」を参照してください。

関連するポイントは、プロファイラーによって作成されたスレッドに対して行われた呼び出しが、プロファイラー DLL の[ICorProfilerCallback](icorprofilercallback-interface.md)メソッドのいずれかの実装の外部から行われた場合でも、常に同期と見なされることです。 少なくとも、の場合はとして使用されます。 これで、 [Forcegc メソッド](icorprofilerinfo-forcegc-method.md)の呼び出しにより、CLR がプロファイラーのスレッドをマネージスレッドに変換したので、そのスレッドはプロファイラーのスレッドと見なされなくなりました。 そのため、CLR では、そのスレッドに対して同期として機能する対象をより厳密に定義しています。つまり、呼び出しは、同期として限定するために、いずれかのプロファイラー DLL の[ICorProfilerCallback](icorprofilercallback-interface.md)メソッドの内部から生成される必要があります。

これは実際に何を意味するのでしょうか。 ほとんどの[ICorProfilerInfo](icorprofilerinfo-interface.md)メソッドは、同期的に呼び出すだけで安全です。それ以外の場合は、すぐに失敗します。 そのため、プロファイラーによって作成されたスレッド (たとえば、 [RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md)、 [RequestReJIT](icorprofilerinfo4-requestrejit-method.md)、または[RequestRevert](icorprofilerinfo4-requestrevert-method.md)) で通常行われる他の呼び出しに対して、profiler DLL が[forcegc メソッド](icorprofilerinfo-forcegc-method.md)スレッドを再利用すると、問題が発生します。 [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)などの非同期セーフ関数でも、マネージスレッドから呼び出された場合、特別なルールがあります。 (詳細については、「[プロファイラースタックウォークの基礎](https://docs.microsoft.com/archive/blogs/davbr/profiler-stack-walking-basics-and-beyond)」を参照してください)。

したがって、 [Forcegc メソッド](icorprofilerinfo-forcegc-method.md)を呼び出すためにプロファイラー DLL によって作成されるすべてのスレッドは、gc をトリガーしてから gc コールバックに応答する目的で*のみ*使用することをお勧めします。 スタックサンプリングやデタッチなどの他のタスクを実行するために、プロファイル API を呼び出すことはできません。

### <a name="conditionalweaktablereferences"></a>ConditionalWeakTableReferences

.NET Framework 4.5 以降では、新しい GC コールバックである[Conditional/Tableelementreferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md)が使用されます。これにより、プロファイラーは*依存ハンドル*に関するより詳細な情報を得ることができます。 これらのハンドルは、GC 有効期間管理の目的で、ソースオブジェクトからターゲットオブジェクトへの参照を効果的に追加します。 依存ハンドルはまったく新しいものではなく、マネージコードをプログラミングする開発者は、Windows 8 と .NET Framework 4.5 の前でも、<xref:System.Runtime.CompilerServices.ConditionalWeakTable%602?displayProperty=nameWithType> クラスを使用して独自の依存ハンドルを作成できます。

ただし、マネージ XAML Windows ストアアプリでは、依存ハンドルが頻繁に使用されるようになりました。 特に、CLR では、マネージオブジェクトとアンマネージ Windows ランタイムオブジェクトの間の参照サイクルの管理を支援するために使用されます。 これは、メモリプロファイラーがこれらの依存ハンドルを通知して、ヒープグラフの残りの部分と共に視覚化できるようにするために、これまでよりも重要であることを意味します。 プロファイラー DLL は、[RootReferences2](icorprofilercallback2-rootreferences2-method.md)、 [ObjectReferences](icorprofilercallback-objectreferences-method.md)、および[Conditional tableelementreferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md)を一緒に使用して、ヒープグラフの完全なビューを形成する必要があります。

## <a name="conclusion"></a>結論

CLR プロファイル API を使用して、Windows ストアアプリ内で実行されているマネージコードを分析することができます。 実際には、開発中の既存のプロファイラーを使用して、Windows ストアアプリを対象とするように特定の変更を加えることができます。 プロファイラーの UI は、デバッグモードで Windows ストアアプリをアクティブ化するために新しい Api を使用する必要があります。 プロファイラー DLL が、Windows ストアアプリに適用可能な Api のみを使用していることを確認してください。 プロファイラー DLL とプロファイラー UI の間の通信機構は、Windows ストアアプリの API 制限を考慮して記述し、Windows ストアアプリに適用される制限付きアクセス許可を認識しておく必要があります。 プロファイラー DLL は、CLR がどのように WinMDs を処理するか、およびマネージスレッドに関してガベージコレクターの動作がどのように異なるかを認識している必要があります。

## <a name="resources"></a>リソース

**共通言語ランタイム**

- [プロファイリング (アンマネージ API リファレンス)](index.md)

- [メタデータ (アンマネージ API リファレンス)](../metadata/index.md)

**CLR と Windows ランタイムの相互作用**

- [Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)

**Windows ストア アプリ**

- [ファイルアクセスとアクセス許可 (Windows ランタイムアプリ](https://docs.microsoft.com/previous-versions/windows/apps/hh967755%28v=win.10%29)

- [開発者ライセンスを取得](https://docs.microsoft.com/previous-versions/windows/apps/hh974578%28v=win.10%29)

- [IPackageDebugSettings インターフェイス](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)
