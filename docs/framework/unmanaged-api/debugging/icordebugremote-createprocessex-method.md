---
title: ICorDebugRemote::CreateProcessEx メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugRemote.CreateProcessEx
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugRemoteTarget::CreateProcessEx
helpviewer_keywords:
- CreateProcessEx method, ICorDebugRemote interface [.NET Framework debugging]
- ICorDebugRemote::CreateProcessEx method [.NET Framework debugging]
ms.assetid: 41af93c7-e448-4251-8d4d-413d38c635f2
topic_type:
- apiref
ms.openlocfilehash: cfec84483d387630623f77c176c668171303dd0f
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791989"
---
# <a name="icordebugremotecreateprocessex-method"></a>ICorDebugRemote::CreateProcessEx メソッド
デバッガーでリモートコンピューター上のプロセスを起動します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateProcessEx (  
    [in]  ICorDebugRemoteTarget*      pRemoteTarget,  
    [in]  LPCWSTR                     lpApplicationName,  
    [in]  LPWSTR                      lpCommandLine,  
    [in]  LPSECURITY_ATTRIBUTES       lpProcessAttributes,  
    [in]  LPSECURITY_ATTRIBUTES       lpThreadAttributes,  
    [in]  BOOL                        bInheritHandles,  
    [in]  DWORD                       dwCreationFlags,  
    [in]  PVOID                       lpEnvironment,  
    [in]  LPCWSTR                     lpCurrentDirectory,  
    [in]  LPSTARTUPINFOW              lpStartupInfo,  
    [in]  LPPROCESS_INFORMATION       lpProcessInformation,  
    [in]  CorDebugCreateProcessFlags  debuggingFlags,  
    [out] ICorDebugProcess**          ppProcess  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pRemoteTarget`  
 からツールの[ターゲットインターフェイス](icordebugremotetarget-interface.md)を指すポインター。 プロセスを起動するリモートコンピューターを決定するために使用されます。  
  
 `lpApplicationName`  
 から起動されたプロセスによって実行されるモジュールを指定する、null で終わる文字列へのポインター。 モジュールは、呼び出し元プロセスのセキュリティコンテキストで実行されます。  
  
 `lpCommandLine`  
 から起動されたプロセスによって実行されるコマンドラインを指定する、null で終わる文字列へのポインター。  
  
 `lpProcessAttributes`  
 からリモートデバッグには使用されません。  
  
 `lpThreadAttributes`  
 からリモートデバッグには使用されません。  
  
 `bInheritHandles`  
 からリモートデバッグには使用されません。  
  
 `dwCreationFlags`  
 からリモートデバッグには使用されません。  
  
 `lpEnvironment`  
 から新しいプロセスの環境ブロックへのポインター。  
  
 `lpCurrentDirectory`  
 からプロセスの現在のディレクトリへの完全パスを指定する、null で終わる文字列へのポインター。 このパラメーターが null の場合、新しいプロセスは呼び出しプロセスと同じ現在のドライブとディレクトリを持ちます。  
  
 `lpStartupInfo`  
 からリモートデバッグには使用されません。  
  
 `lpProcessInformation`  
 からリモートデバッグには使用されません。  
  
 `debuggingFlags`  
 からリモートデバッグには使用されません。  
  
 `ppProcess`  
 入出力プロセスを表す "いいプロセスインターフェイス" オブジェクトのアドレスへのポインター。  
  
## <a name="return-value"></a>戻り値  
 S_OK  
 リモートコンピューターでプロセスが正常に起動され、デバッグのために "のプロセスインターフェイス" が返されました。  
  
 E_FAIL (またはその他の E_ リターン コード)  
 リモートコンピューターでプロセスを起動できず、デバッグのために "のプロセスインターフェイス" を返します。  
  
## <a name="remarks"></a>コメント  
 混合モードのデバッグは、Silverlight ではサポートされていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug .idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>関連項目

- [ICorDebugRemote インターフェイス](icordebugremote-interface.md)
- [ICorDebug インターフェイス](icordebug-interface.md)

- [デバッグ インターフェイス](debugging-interfaces.md)
