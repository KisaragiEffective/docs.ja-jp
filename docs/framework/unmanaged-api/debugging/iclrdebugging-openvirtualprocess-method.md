---
title: ICLRDebugging::OpenVirtualProcess メソッド
ms.date: 03/30/2017
api_name:
- ICLRDebugging.OpenVirtualProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDebugging::OpenVirtualProcess
helpviewer_keywords:
- OpenVirtualProcess method [.NET Framework debugging]
- ICLRDebugging::OpenVirtualProcess method [.NET Framework debugging]
ms.assetid: e8ab7c41-d508-4ed9-8a31-ead072b5a314
topic_type:
- apiref
ms.openlocfilehash: 585b3d605d0df9169c12ca10198846ec0a7fe6d4
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793610"
---
# <a name="iclrdebuggingopenvirtualprocess-method"></a>ICLRDebugging::OpenVirtualProcess メソッド
プロセスに読み込まれた共通言語ランタイム (CLR) モジュールに対応する、コンポーネントインターフェイスを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT OpenVirtualProcess(  
    [in] ULONG64 moduleBaseAddress,  
    [in] IUnknown * pDataTarget,  
    [in] ICLRDebuggingLibraryProvider * pLibraryProvider,  
    [in] CLR_DEBUGGING_VERSION * pMaxDebuggerSupportedVersion,  
    [in] REFIID riidProcess,  
    [out, iid_is(riidProcess)] IUnknown ** ppProcess,  
    [in, out] CLR_DEBUGGING_VERSION * pVersion,  
    [out] CLR_DEBUGGING_PROCESS_FLAGS * pdwFlags);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleBaseAddress`  
 からターゲットプロセス内のモジュールのベースアドレス。 指定したモジュールが CLR モジュールでない場合、COR_E_NOT_CLR が返されます。  
  
 `pDataTarget`  
 からマネージデバッガーがプロセスの状態を検査できるデータターゲットの抽象化。 デバッガーでは、、のように、によっては、[このインターフェイスを](icordebugdatatarget-interface.md)実装してください。 デバッグ対象の CLR がコンピューターにローカルにインストールされていないシナリオをサポートするには、 [ICLRDebuggingLibraryProvider](iclrdebugginglibraryprovider-interface.md)インターフェイスを実装する必要があります。  
  
 `pLibraryProvider`  
 からライブラリプロバイダーのコールバックインターフェイス。バージョン固有のデバッグライブラリをオンデマンドで検索し、読み込むことができます。 このパラメーターは、`ppProcess` または `pFlags` が `null`ない場合にのみ必要です。  
  
 `pMaxDebuggerSupportedVersion`  
 からこのデバッガーがデバッグできる CLR の最大バージョン。 このデバッガーでサポートされている最新の CLR バージョンからメジャー、マイナー、ビルドの各バージョンを指定し、将来の CLR サービスリリースに対応するためにリビジョン番号を65535に設定する必要があります。  
  
 `riidProcess`  
 から取得するコード処理インターフェイスの ID。 現時点で許容される値は、IID_CORDEBUGPROCESS3、IID_CORDEBUGPROCESS2、および IID_CORDEBUGPROCESS だけです。  
  
 `ppProcess`  
 入出力`riidProcess`によって識別される COM インターフェイスへのポインター。  
  
 `pVersion`  
 [入力、出力]CLR のバージョン。 入力時には、この値を `null`できます。 また、 [CLR_DEBUGGING_VERSION](clr-debugging-version-structure.md)構造体を指すこともできます。この場合、構造体の `wStructVersion` フィールドを 0 (ゼロ) に初期化する必要があります。  
  
 出力時には、返された `CLR_DEBUGGING_VERSION` 構造体に CLR のバージョン情報が格納されます。  
  
 `pdwFlags`  
 入出力指定されたランタイムに関する情報フラグ。 フラグの説明については、 [CLR_DEBUGGING_PROCESS_FLAGS](clr-debugging-process-flags-enumeration.md)のトピックを参照してください。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|メソッドは正常に終了しました。|  
|E_POINTER|`pDataTarget` は `null`です。|  
|CORDBG_E_LIBRARY_PROVIDER_ERROR|[ICLRDebuggingLibraryProvider](iclrdebugginglibraryprovider-interface.md)コールバックによってエラーが返されたか、有効なハンドルが提供されていません。|  
|CORDBG_E_MISSING_DATA_TARGET_INTERFACE|`pDataTarget` は、このバージョンのランタイムに必要なデータターゲットインターフェイスを実装していません。|  
|CORDBG_E_NOT_CLR|指定されたモジュールは CLR モジュールではありません。 この HRESULT は、メモリが破損している、モジュールが使用できない、または CLR バージョンが shim バージョンより後であるために CLR モジュールが検出できない場合にも返されます。|  
|CORDBG_E_UNSUPPORTED_DEBUGGING_MODEL|このランタイムバージョンでは、このデバッグモデルはサポートされていません。 現時点では、.NET Framework 4 より前のバージョンの CLR では、デバッグモデルはサポートされていません。 このエラーが発生した後も、`pwszVersion` 出力パラメーターは正しい値に設定されます。|  
|CORDBG_E_UNSUPPORTED_FORWARD_COMPAT|CLR のバージョンが、このデバッガーがサポートするために要求するバージョンよりも大きくなっています。 このエラーが発生した後も、`pwszVersion` 出力パラメーターは正しい値に設定されます。|  
|E_NO_INTERFACE|`riidProcess` インターフェイスは使用できません。|  
|CORDBG_E_UNSUPPORTED_VERSION_STRUCT|`CLR_DEBUGGING_VERSION` 構造体には、`wStructVersion`に対して認識される値がありません。 現時点で許容される値は0のみです。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>コメント  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
