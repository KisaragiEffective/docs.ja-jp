---
title: 非推奨の CLR ホスト関数
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 1.1, hosting global static functions
- global static functions [.NET Framework hosting], version 2.0
- .NET Framework 2.0, hosting global static functions
- hosting global static functions [.NET Framework], version 2.0
ms.assetid: 91fbbb35-e543-4814-b806-371cebae8c5a
ms.openlocfilehash: 62773ce526b1f21c57ab85a106708589fcf92f6f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73138266"
---
# <a name="deprecated-clr-hosting-functions"></a>非推奨の CLR ホスト関数
このセクションでは、以前のバージョンのホスト API で使用されていたアンマネージ グローバル静的関数について説明します。  
  
 .NET Framework によってのみ使用されるインフラストラクチャ関数 (`_Cor*` 関数) を除き、これらの関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="activation-functions"></a>アクティブ化関数  
 [ClrCreateManagedInstance 関数](../../../../docs/framework/unmanaged-api/hosting/clrcreatemanagedinstance-function.md)  
 非推奨。 指定したマネージド型のインスタンスを作成します。  
  
 [CoInitializeCor 関数](../../../../docs/framework/unmanaged-api/hosting/coinitializecor-function.md)  
 互換性のために残されています。 共通言語ランタイム (CLR) を初期化するには、 [Corbindtoruntimeex](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimeex-function.md)または[Corbindtoの entruntime](../../../../docs/framework/unmanaged-api/hosting/corbindtocurrentruntime-function.md)を使用します。  
  
 [CoInitializeEE 関数](../../../../docs/framework/unmanaged-api/hosting/coinitializeee-function.md)  
 非推奨。 CLR 実行エンジンがプロセスに読み込まれていることを確認します。 代わりに[ICLRRuntimeHost:: Start](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-start-method.md)メソッドを使用してください。  
  
 [CorBindToCurrentRuntime 関数](../../../../docs/framework/unmanaged-api/hosting/corbindtocurrentruntime-function.md)  
 非推奨。 XML ファイルに格納されているバージョン情報を使用して、共通言語ランタイム (CLR: Common Language Runtime) をプロセスに読み込みます。  
  
 [CorBindToRuntime 関数](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntime-function.md)  
 非推奨。 アンマネージ ホストが CLR をプロセスに読み込めるようにします。  
  
 [CorBindToRuntimeByCfg 関数](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimebycfg-function.md)  
 非推奨。 XML ファイルから読み取ったバージョン情報を使用して、CLR をプロセスに読み込みます。  
  
 [CorBindToRuntimeEx 関数](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimeex-function.md)  
 非推奨。 アンマネージ ホストが CLR をプロセスに読み込めるようにします。CLR の動作を指定するフラグを設定できます。  
  
 [CorBindToRuntimeHost 関数](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimehost-function.md)  
 非推奨。 指定したバージョンの CLR をホストがプロセスに読み込めるようにします。  
  
 [GetCORRequiredVersion 関数](../../../../docs/framework/unmanaged-api/hosting/getcorrequiredversion-function.md)  
 非推奨。 必要な CLR のバージョン番号を取得します。  
  
 [GetCORSystemDirectory 関数](../../../../docs/framework/unmanaged-api/hosting/getcorsystemdirectory-function.md)  
 非推奨。 プロセスに読み込まれた CLR のインストール ディレクトリを返します。  
  
 [GetRealProcAddress 関数](../../../../docs/framework/unmanaged-api/hosting/getrealprocaddress-function.md)  
 非推奨。 最後にインストールされたバージョンの CLR からエクスポートされる、指定した関数のアドレスを取得します。  
  
 [GetRequestedRuntimeInfo 関数](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeinfo-function.md)  
 非推奨。 アプリケーションが要求した CLR についてのバージョン情報とディレクトリ情報を取得します。  
  
## <a name="clr-version-functions"></a>CLR バージョン関数  
 このセクションの関数は、CLR のバージョンを返します。CLR をアクティブ化することはありません。  
  
 [GetCORVersion 関数](../../../../docs/framework/unmanaged-api/hosting/getcorversion-function.md)  
 非推奨。 現在のプロセスで実行されている CLR のバージョン番号を返します。  
  
 [GetFileVersion 関数](../../../../docs/framework/unmanaged-api/hosting/getfileversion-function.md)  
 非推奨。 指定したバッファーを使用して、指定したファイルの CLR バージョン情報を取得します。  
  
 [GetRequestedRuntimeVersion 関数](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeversion-function.md)  
 非推奨。 指定したアプリケーションで要求される CLR のバージョン番号を取得します。 そのバージョンがインストールされていない場合は、要求されるバージョンより前にインストールされた最も新しいバージョンを取得します。  
  
 [GetRequestedRuntimeVersionForCLSID 関数](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeversionforclsid-function.md)  
 非推奨。 指定の CLSID を持つクラスに対応した CLR バージョンの情報を取得します。  
  
 [GetVersionFromProcess 関数](../../../../docs/framework/unmanaged-api/hosting/getversionfromprocess-function.md)  
 非推奨。 指定のプロセス ハンドルに関連付けられた CLR のバージョン番号を取得します。  
  
 [LockClrVersion 関数](../../../../docs/framework/unmanaged-api/hosting/lockclrversion-function.md)  
 非推奨。 プロセス内で使用する CLR のバージョンを、CLR を明示的に初期化する前にホストが決定できるようにします。  
  
## <a name="hosting-functions"></a>ホスト関数  
 [CallFunctionShim 関数](../../../../docs/framework/unmanaged-api/hosting/callfunctionshim-function.md)  
 非推奨。 指定したライブラリ内の関数を、名前とパラメーターを指定して呼び出します。  
  
 [CoEEShutDownCOM 関数](../../../../docs/framework/unmanaged-api/hosting/coeeshutdowncom-function.md)  
 非推奨。 プロセスから COM アセンブリをアンロードします。  
  
 [CorExitProcess 関数](../../../../docs/framework/unmanaged-api/hosting/corexitprocess-function.md)  
 非推奨。 現在のアンマネージ プロセスを終了します。  
  
 [CorLaunchApplication 関数](../../../../docs/framework/unmanaged-api/hosting/corlaunchapplication-function.md)  
 非推奨。 指定したネットワーク パスのアプリケーションを、指定したマニフェストとその他のアプリケーション データを使用して起動します。  
  
 [CorMarkThreadInThreadPool 関数](../../../../docs/framework/unmanaged-api/hosting/cormarkthreadinthreadpool-function.md)  
 非推奨。 現在実行されているスレッド プールのスレッドに、マネージド コードの実行のマークを付けます。 .NET Framework Version 2.0 以降では、この関数に効力はありません。 必ずしも必要はありませんが、この関数はコードから削除できます。  
  
 [CoUninitializeCor 関数](../../../../docs/framework/unmanaged-api/hosting/couninitializecor-function.md)  
 互換性のために残されています。 プロセスから CLR をアンロードできません。  
  
 [CoUninitializeEE 関数](../../../../docs/framework/unmanaged-api/hosting/couninitializeee-function.md)  
 互換性のために残されています。  
  
 [CreateDebuggingInterfaceFromVersion 関数](../../../../docs/framework/unmanaged-api/hosting/createdebugginginterfacefromversion-function.md)  
 非推奨。 指定されたバージョン情報に基づいて[ICorDebug](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)オブジェクトを作成します。  
  
 [CreateICeeFileGen 関数](../../../../docs/framework/unmanaged-api/hosting/createiceefilegen-function.md)  
 非推奨。 [ICeeFileGen](../../../../docs/framework/unmanaged-api/hosting/iceefilegen-class.md)オブジェクトを作成します。  
  
 [DestroyICeeFileGen 関数](../../../../docs/framework/unmanaged-api/hosting/destroyiceefilegen-function.md)  
 非推奨。 [ICeeFileGen](../../../../docs/framework/unmanaged-api/hosting/iceefilegen-class.md)オブジェクトを破棄します。  
  
 [FExecuteInAppDomainCallback 関数ポインター](../../../../docs/framework/unmanaged-api/hosting/fexecuteinappdomaincallback-function-pointer.md)  
 非推奨。 CLR がマネージド コードを実行するために呼び出す関数を指します。  
  
 [FLockClrVersionCallback 関数ポインター](../../../../docs/framework/unmanaged-api/hosting/flockclrversioncallback-function-pointer.md)  
 非推奨。 初期化が開始または完了したことをホストに通知するために CLR が呼び出す関数を指します。  
  
 [GetCLRIdentityManager 関数](../../../../docs/framework/unmanaged-api/hosting/getclridentitymanager-function.md)  
 非推奨。 CLR が ID を管理できるようにするインターフェイスへのポインターを取得します。  
  
 [LoadLibraryShim 関数](../../../../docs/framework/unmanaged-api/hosting/loadlibraryshim-function.md)  
 非推奨。 指定したバージョンの .NET Framework DLL を読み込みます。  
  
 [LoadStringRC 関数](../../../../docs/framework/unmanaged-api/hosting/loadstringrc-function.md)  
 非推奨。 現在のスレッドの既定のカルチャを使用して、HRESULT 値をエラー メッセージに変換します。  
  
 [LoadStringRCEx 関数](../../../../docs/framework/unmanaged-api/hosting/loadstringrcex-function.md)  
 非推奨。 HRESULT 値を、指定したカルチャの適切なエラー メッセージに変換します。  
  
 [LPOVERLAPPED_COMPLETION_ROUTINE 関数ポインター](../../../../docs/framework/unmanaged-api/hosting/lpoverlapped-completion-routine-function-pointer.md)  
 非推奨。 デバイスに対する重複 I/O (非同期 I/O) が完了したときに、ホストに通知する関数を指します。  
  
 [LPTHREAD_START_ROUTINE 関数ポインター](../../../../docs/framework/unmanaged-api/hosting/lpthread-start-routine-function-pointer.md)  
 非推奨。 スレッドの実行を開始したことをホストに通知する関数を指します。  
  
 [RunDll32ShimW 関数](../../../../docs/framework/unmanaged-api/hosting/rundll32shimw-function.md)  
 非推奨。 指定されたコマンドを実行します。  
  
 [WAITORTIMERCALLBACK 関数ポインター](../../../../docs/framework/unmanaged-api/hosting/waitortimercallback-function-pointer.md)  
 非推奨。 待機ハンドルがシグナル状態になったこと、またはタイムアウトしたことをホストに通知する関数を指します。  
  
## <a name="infrastructure-functions"></a>インフラストラクチャ関数  
 このセクションの関数は、.NET Framework によってのみ使用されます。  
  
 [_CorDllMain 関数](../../../../docs/framework/unmanaged-api/hosting/cordllmain-function.md)  
 CLR を初期化し、DLL アセンブリの CLR ヘッダーでマネージド エントリ ポイントを検索して、その実行を開始します。  
  
 [_CorExeMain 関数](../../../../docs/framework/unmanaged-api/hosting/corexemain-function.md)  
 CLR を初期化し、実行可能アセンブリの CLR ヘッダーでマネージド エントリ ポイントを検索して、その実行を開始します。  
  
 [_CorExeMain2 関数](../../../../docs/framework/unmanaged-api/hosting/corexemain2-function.md)  
 指定されたメモリ マップト コードのエントリ ポイントを実行します。 この関数は、オペレーティング システム ローダーによって呼び出されます。  
  
 [_CorImageUnloading 関数](../../../../docs/framework/unmanaged-api/hosting/corimageunloading-function.md)  
 マネージド モジュール イメージがアンロードされたときに、ローダーに通知します。  
  
 [_CorValidateImage 関数](../../../../docs/framework/unmanaged-api/hosting/corvalidateimage-function.md)  
 マネージド モジュール イメージを検証し、それらが読み込まれると、オペレーティング システム ローダーに通知します。  
  
## <a name="see-also"></a>関連項目

- [.NET Framework 4 ホスト グローバル静的関数](../../../../docs/framework/unmanaged-api/hosting/net-framework-4-hosting-global-static-functions.md)
