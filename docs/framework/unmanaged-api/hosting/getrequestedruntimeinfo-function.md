---
title: GetRequestedRuntimeInfo 関数
ms.date: 03/30/2017
api_name:
- GetRequestedRuntimeInfo
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- GetRequestedRuntimeInfo
helpviewer_keywords:
- GetRequestedRuntimeInfo function [.NET Framework hosting]
ms.assetid: 0dfd7cdc-c116-4e25-b56a-ac7b0378c942
topic_type:
- apiref
ms.openlocfilehash: cd1d9e768698115bee22e35699b044e0c3526d2d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136319"
---
# <a name="getrequestedruntimeinfo-function"></a>GetRequestedRuntimeInfo 関数
アプリケーションによって要求された共通言語ランタイム (CLR) に関するバージョンとディレクトリ情報を取得します。  
  
 この関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetRequestedRuntimeInfo (  
    [in]  LPCWSTR  pExe,   
    [in]  LPCWSTR  pwszVersion,   
    [in]  LPCWSTR  pConfigurationFile,   
    [in]  DWORD    startupFlags,   
    [in]  DWORD    runtimeInfoFlags,   
    [out] LPWSTR   pDirectory,   
    [in]  DWORD    dwDirectory,   
    [out] DWORD   *dwDirectoryLength,   
    [out] LPWSTR   pVersion,   
    [in]  DWORD    cchBuffer,   
    [out] DWORD   *dwlength  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pExe`  
 からアプリケーションの名前。  
  
 `pwszVersion`  
 からランタイムのバージョン番号を指定する文字列。  
  
 `pConfigurationFile`  
 から`pExe`に関連付けられている構成ファイルの名前。  
  
 `startupFlags`  
 から1つまたは複数の[STARTUP_FLAGS](../../../../docs/framework/unmanaged-api/hosting/startup-flags-enumeration.md)列挙値。  
  
 `runtimeInfoFlags`  
 から1つまたは複数の[RUNTIME_INFO_FLAGS](../../../../docs/framework/unmanaged-api/hosting/runtime-info-flags-enumeration.md)列挙値。  
  
 `pDirectory`  
 入出力正常に完了したときのランタイムへのディレクトリパスを格納するバッファー。  
  
 `dwDirectory`  
 からディレクトリバッファーの長さ。  
  
 `dwDirectoryLength`  
 入出力ディレクトリパス文字列の長さへのポインター。  
  
 `pVersion`  
 入出力正常に完了したときのランタイムのバージョン番号を格納するバッファー。  
  
 `cchBuffer`  
 からバージョン文字列バッファーの長さ。  
  
 `dwlength`  
 入出力バージョン文字列の長さへのポインター。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の値に加えて、Winerror.h で定義されている標準のコンポーネントオブジェクトモデル (COM) エラーコードを返します。  
  
|リターン コード|説明|  
|-----------------|-----------------|  
|S_OK|メソッドは正常に完了しました。|  
|ERROR_INSUFFICIENT_BUFFER|ディレクトリのバッファーが、ディレクトリパスを格納するのに十分な大きさではありません。<br /><br /> または<br /><br /> バージョンバッファーが、バージョン文字列を格納するのに十分な大きさではありません。|  
  
## <a name="remarks"></a>Remarks  
 `GetRequestedRuntimeInfo` メソッドは、プロセスに読み込まれたバージョンに関する実行時の情報を返します。これは、必ずしもコンピューターにインストールされている最新バージョンではありません。  
  
 .NET Framework バージョン2.0 では、次のように `GetRequestedRuntimeInfo` 方法を使用して、インストールされている最新のバージョンに関する情報を取得できます。  
  
- `pExe`、`pwszVersion`、および `pConfigurationFile` パラメーターを null として指定します。  
  
- `runtimeInfoFlags` パラメーターの `RUNTIME_INFO_FLAGS` 列挙で、RUNTIME_INFO_UPGRADE_VERSION フラグを指定します。  
  
 `GetRequestedRuntimeInfo` メソッドは、次のような状況では、最新の CLR バージョンを返しません。  
  
- 特定の CLR バージョンの読み込みを指定するアプリケーション構成ファイルが存在します。 `pConfigurationFile` パラメーターに null を指定した場合でも、.NET Framework は構成ファイルを使用することに注意してください。  
  
- [Corbindtoruntimeex](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimeex-function.md)メソッドが、以前の CLR バージョンを指定して呼び出されました。  
  
- 以前のバージョンの CLR 用にコンパイルされたアプリケーションが現在実行されています。  
  
 `runtimeInfoFlags` パラメーターでは、`RUNTIME_INFO_FLAGS` 列挙体のアーキテクチャ定数を一度に1つだけ指定できます。  
  
- RUNTIME_INFO_REQUEST_IA64  
  
- RUNTIME_INFO_REQUEST_AMD64  
  
- RUNTIME_INFO_REQUEST_X86  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [GetRequestedRuntimeVersion 関数](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeversion-function.md)
- [GetVersionFromProcess 関数](../../../../docs/framework/unmanaged-api/hosting/getversionfromprocess-function.md)
- [非推奨の CLR ホスト関数](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
