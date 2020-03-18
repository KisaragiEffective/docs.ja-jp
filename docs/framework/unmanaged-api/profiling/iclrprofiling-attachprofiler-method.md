---
title: ICLRProfiling::AttachProfiler メソッド
ms.date: 03/30/2017
api_name:
- IClrProfiling.AttachProfiler Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- IClrProfiling::AttachProfiler
helpviewer_keywords:
- AttachProfiler method [.NET Framework profiling]
- IClrProfiling::AttachProfiler method [.NET Framework profiling]
ms.assetid: 535a6839-c443-405b-a6f4-e2af90725d5b
topic_type:
- apiref
ms.openlocfilehash: 29aecd530d18b931420467e9127bcbf96d3a4a5f
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866765"
---
# <a name="iclrprofilingattachprofiler-method"></a>ICLRProfiling::AttachProfiler メソッド
指定されたプロファイラーを、指定されたプロセスにアタッチします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT AttachProfiler(  
  [in] DWORD dwProfileeProcessID,  
  [in] DWORD dwMillisecondsMax,                     // optional  
  [in] const CLSID * pClsidProfiler,  
  [in] LPCWSTR wszProfilerPath,                     // optional  
  [in] size_is(cbClientData)] void * pvClientData,  // optional  
  [in] UINT cbClientData);                          // optional  
```  
  
## <a name="parameters"></a>パラメーター

- `dwProfileeProcessID`

  [\[]] プロファイラーをアタッチするプロセスのプロセス ID。 64 ビット コンピューターでは、プロファイル対象のプロセスのビット数は `AttachProfiler` を呼び出しているトリガー プロセスのビット数に一致する必要があります。 `AttachProfiler` が呼び出されるユーザー アカウントに管理特権がある場合は、システム上のどのプロセスもターゲット プロセスにすることができます。 それ以外の場合は、同じユーザー アカウントがターゲット プロセスを所有している必要があります。

- `dwMillisecondsMax`

  \[] `AttachProfiler` が完了するまでの時間 (ミリ秒単位)。 トリガー プロセスは、特定のプロファイラーが初期化を完了するために十分であることがわかっているタイムアウトを渡す必要があります。
  
- `pClsidProfiler`

  \[] には、読み込むプロファイラーの CLSID へのポインターを指定します。 トリガー プロセスでは、`AttachProfiler` が戻った後にこのメモリを再利用できます。

- `wszProfilerPath`

  \[] を使用します。プロファイラーの DLL ファイルの完全なパスを読み込むことができます。 この文字列に含める文字数は、null 終端文字も含めて 260 文字以内にする必要があります。 `wszProfilerPath` が null または空の文字列である場合、共通言語ランタイム (CLR: Common Language Runtime) は、`pClsidProfiler` が示す CLSID のレジストリ内を探してプロファイラーの DLL ファイルの場所を見つけることを試みます。

- `pvClientData`

  \[] [ICorProfilerCallback3:: InitializeForAttach](icorprofilercallback3-initializeforattach-method.md)メソッドによってプロファイラーに渡されるデータへのポインター。 トリガー プロセスでは、`AttachProfiler` が戻った後にこのメモリを再利用できます。 `pvClientData` が null の場合、`cbClientData` を 0 (ゼロ) にする必要があります。

- `cbClientData`

  \[] `pvClientData` が指すデータのサイズ (バイト単位)。

## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT を返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|指定されたプロファイラーは、正常にターゲット プロセスにアタッチされました。|  
|CORPROF_E_PROFILER_ALREADY_ACTIVE|アクティブなプロファイラーまたはターゲット プロセスにアタッチされているプロファイラーが既に存在します。|  
|CORPROF_E_PROFILER_NOT_ATTACHABLE|指定されたプロファイラーはアタッチをサポートしていません。 トリガー プロセスは、別のプロファイラーへのアタッチを試みる可能性があります。|  
|CORPROF_E_PROFILEE_INCOMPATIBLE_WITH_TRIGGER|ターゲット プロセスのバージョンが、`AttachProfiler` を呼び出している現在のプロセスと互換性がないため、プロファイラーのアタッチを要求できません。|  
|HRESULT_FROM_WIN32(ERROR_ACCESS_DENIED)|トリガー プロセスのユーザーは、ターゲット プロセスにアクセスできません。|  
|HRESULT_FROM_WIN32(ERROR_PRIVILEGE_NOT_HELD)|トリガー プロセスのユーザーは、プロファイラーを特定のターゲット プロセスにアタッチするために必要な特権を持っていません。 アプリケーション イベント ログには、詳細情報が含まれている可能性があります。|  
|CORPROF_E_IPC_FAILED|ターゲット プロセスとやり取りする際にエラーが発生しました。 これは、通常、ターゲット プロセスがシャットダウンしていた場合に発生します。|  
|HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND)|ターゲット プロセスは存在していないか、アタッチをサポートする CLR を実行していません。 これは、ランタイムの列挙型メソッドの呼び出し以降に CLR がアンロードされたことを示している可能性があります。|  
|HRESULT_FROM_WIN32(ERROR_TIMEOUT)|プロファイラーの読み込みを開始せずにタイムアウトの時間切れになりました。 アタッチ操作は再試行できます。 ターゲット プロセスのファイナライザーがタイムアウト値よりも長く実行されると、タイムアウトが発生します。|  
|E_INVALIDARG|1 つ以上のパラメーターの値が無効です。|  
|E_FAIL|他の何らかの未指定のエラーが発生しました。|  
|その他のエラー コード|プロファイラーの[ICorProfilerCallback3:: InitializeForAttach](icorprofilercallback3-initializeforattach-method.md)メソッドが失敗を示す hresult を返した場合、`AttachProfiler` は同じ hresult を返します。 この場合、E_NOTIMPL は CORPROF_E_PROFILER_NOT_ATTACHABLE に変換されます。|  
  
## <a name="remarks"></a>Remarks  
  
## <a name="memory-management"></a>メモリ管理  
 COM 規則に従うと、`pvClientData` パラメーターが示すデータのメモリの割り当てと割り当て解除の責任は `AttachProfiler` の呼び出し元 (たとえば、プロファイラーの開発者が作成したトリガー コード) にあります。 CLR は `AttachProfiler` の呼び出しを実行するときに、`pvClientData` が示すメモリをコピーし、それを対象プロセスに送信します。 対象プロセス内の CLR が `pvClientData` ブロックのコピーを受信すると、`InitializeForAttach` メソッドを通じてプロファイラーにそのブロックを渡してから、対象プロセスから `pvClientData` ブロックのコピーを解放します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerInfo3 インターフェイス](icorprofilerinfo3-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
