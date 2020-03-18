---
title: CLR のアクティブ化に関する問題をデバッグする方法
ms.date: 03/30/2017
helpviewer_keywords:
- CLR activation, debugging issues
ms.assetid: 4fe17546-d56e-4344-a930-6d8e4a545914
ms.openlocfilehash: 602ee3c88237a902d48339836fbe25f636ae9705
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716506"
---
# <a name="how-to-debug-clr-activation-issues"></a>CLR のアクティブ化に関する問題をデバッグする方法

正しいバージョンの共通言語ランタイム (CLR) でアプリケーションを実行して問題が発生した場合、CLR アクティベーション ログを表示し、デバッグできます。 アプリケーションで予想とは異なる CLR バージョンが読み込まれるときでも、CLR がまったく読み込まれないときでも、アクティベーション問題の根本原因を突き止めるときにこのログが大変役立ちます。 [.NET Framework 初期化エラー:ユーザー エクスペリエンスの管理](initialization-errors-managing-the-user-experience.md)では、アプリケーションに対して CLR が見つからない場合について説明されています。

HKEY_LOCAL_MACHINE レジストリ キーまたはシステム環境変数を利用すれば、CLR アクティベーション ログをシステム全体で有効にできます。 このログは、レジストリ キーまたは環境変数が削除されるまで生成されます。 あるいは、ユーザーまたはプロセスのローカル環境変数を利用し、別の範囲や期間でログを有効にできます。

CLR アクティベーション ログと[アセンブリ バインド ログ](../tools/fuslogvw-exe-assembly-binding-log-viewer.md)は完全に異なりますので混同しないでください。

## <a name="to-enable-clr-activation-logging"></a>CLR アクティベーション ログを有効にするには

### <a name="using-the-registry"></a>レジストリを利用する

1. レジストリ エディターで、HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework (32 ビット コンピューターの場合) または HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework フォルダー (64 ビット コンピューターの場合) に移動します。

2. `CLRLoadLogDir` という名前の文字列値を追加し、CLR アクティベーション ログを保存する既存ディレクトリの完全パスにそれを設定します。

アクティベーション ログは、この文字列値を削除するまで有効です。

### <a name="using-an-environment-variable"></a>環境変数を利用する

- CLR アクティベーション ログを保存する既存ディレクトリの完全パスを表す文字列に `COMPLUS_CLRLoadLogDir` 環境変数を設定します。

    環境変数の設定方法でその範囲が決まります。

  - システム レベルで設定した場合、環境変数が削除されるまで、そのコンピューターのすべての .NET Framework アプリケーションに対してアクティベーション ログが有効になります。

  - ユーザー レベルで設定した場合、現在のユーザー アカウントに対してのみ、アクティベーション ログが有効になります。 環境変数が削除されるまでログが記録されます。

  - CLR を読み込む前にプロセス内から設定した場合、プロセスが終了するまでアクティベーション ログが有効になります。

  - アプリケーションを実行する前にコマンド プロンプトで設定した場合、そのコマンド プロンプトから実行されたあらゆるアプリケーションに対してアクティベーション ログが有効になります。

    たとえば、プロセスレベルの範囲で c:\clrloadlogs ディレクトリにアクティベーション ログを保存するには、アプリケーションを実行する前にコマンド プロンプト ウィンドウを開き、次を入力します。

    ```console
    set COMPLUS_CLRLoadLogDir=c:\clrloadlogs
    ```

## <a name="example"></a>例

CLR アクティベーション ログは、CLR アクティベーションと API をホストする CLR の使用に関する大量のデータを提供します。 そのデータの大半は Microsoft が社内で利用しますが、一部のデータは、この記事で説明するように、開発者にとっても役立ちます。

このログには、API をホストする CLR が呼び出された順序が反映されます。 コンピューターで検出された一連のインストール済みランタイムに関する有用な情報も含まれています。 CLR アクティベーション ログの書式自体は記録されませんが、開発者が CLR アクティベーションの問題を解決するとき、この書式は役に立ちます。

> [!NOTE]
> CLR を使用するプロセスが終了するまで、アクティベーション ログを開くことはできません。

> [!NOTE]
> CLR アクティベーション ログはローカライズされません。常に英語で生成されます。

次のアクティベーション ログ例では、最も役に立つ情報が強調表示されており、ログの後に説明が付いています。

```output
532,205950.367,CLR Loading log for C:\Tests\myapp.exe
532,205950.367,Log started at 4:26:12 PM on 10/6/2011
532,205950.367,-----------------------------------
532,205950.382,FunctionCall: _CorExeMain
532,205950.382,FunctionCall: ClrCreateInstance, Clsid: {2EBCD49A-1B47-4A61-B13A-4A03701E594B}, Iid: {E2190695-77B2-492E-8E14-C4B3A7FDD593}
532,205950.382,MethodCall: ICLRMetaHostPolicy::GetRequestedRuntime. Version: (null), Metahost Policy Flags: 0x168, Binary: (null), Iid: {BD39D1D2-BA2F-486A-89B0-B4B0CB466891}
532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
532,205950.382,Input values for ComputeVersionString follow this line
532,205950.382,-----------------------------------
532,205950.382,Default Application Name: C:\Tests\myapp.exe
532,205950.382,IsLegacyBind is: 0
532,205950.382,IsCapped is 0
532,205950.382,SkuCheckFlags are 0
532,205950.382,ShouldEmulateExeLaunch is 0
532,205950.382,LegacyBindRequired is 0
532,205950.382,-----------------------------------
532,205950.382,Parsing config file: C:\Tests\myapp.exe
532,205950.382,UseLegacyV2RuntimeActivationPolicy is set to 0
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
532,205950.382,ERROR: Version v2.0.50727 is not present on the machine.
532,205950.398,SEM_FAILCRITICALERRORS is set to 0
532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
532,205950.398,FunctionCall: RealDllMain. Reason: 0
532,205950.398,FunctionCall: OnShimDllMainCalled. Reason: 0
```

- **CLR Loading log** には、マネージド コードを読み込んだプロセスを開始した実行可能ファイルのパスがあります。 ネイティブ ホストの可能性があることに注意してください。

    ```output
    532,205950.367,CLR Loading log for C:\Tests\myapp.exe
    ```

- **Installed Runtime** は、コンピューターにインストールされている CLR の一連のバージョンであり、アクティベーション要求の候補となります。

    ```output
    532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
    ```

- **built with version** は、[ICLRMetaHostPolicy::GetRequestedRuntime](../unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md) のようなメソッドに提供されたバイナリの構築に利用された CLR のバージョンです。

    ```output
    532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
    ```

- **feature-on-demand installation** は Windows 8 で .NET Framework 3.5 を有効にすることを指します。 このシナリオの詳細については、「[.NET Framework 初期化エラー:ユーザー エクスペリエンスの管理](initialization-errors-managing-the-user-experience.md)」を参照してください。

    ```output
    532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
    ```

## <a name="see-also"></a>関連項目

- [配置](index.md)
- [方法: .NET Framework 4 以降のバージョンをサポートするアプリを構成する](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
