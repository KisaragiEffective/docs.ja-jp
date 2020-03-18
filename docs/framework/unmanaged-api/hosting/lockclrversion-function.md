---
title: LockClrVersion 関数
ms.date: 03/30/2017
api_name:
- LockClrVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- LockClrVersion
helpviewer_keywords:
- LockClrVersion function [.NET Framework hosting]
ms.assetid: 1318ee37-c43b-40eb-bbe8-88fc46453d74
topic_type:
- apiref
ms.openlocfilehash: 216852f8f051440b2814619b843a1f25013e4042
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133771"
---
# <a name="lockclrversion-function"></a>LockClrVersion 関数
CLR を明示的に初期化する前に、プロセス内で使用される共通言語ランタイム (CLR) のバージョンをホストが判断できるようにします。  
  
 この関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT LockClrVersion (  
    [in] FLockClrVersionCallback   hostCallback,  
    [in] FLockClrVersionCallback  *pBeginHostSetup,  
    [in] FLockClrVersionCallback  *pEndHostSetup  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `hostCallback`  
 から初期化時に CLR によって呼び出される関数。  
  
 `pBeginHostSetup`  
 から初期化を開始することを CLR に通知するために、ホストによって呼び出される関数。  
  
 `pEndHostSetup`  
 から初期化が完了したことを CLR に通知するために、ホストによって呼び出される関数。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の値に加えて、Winerror.h で定義されている標準の COM エラーコードを返します。  
  
|リターン コード|説明|  
|-----------------|-----------------|  
|S_OK|メソッドは正常に完了しました。|  
|E_INVALIDARG|1つ以上の引数が null です。|  
  
## <a name="remarks"></a>Remarks  
 ホストは、CLR を初期化する前に `LockClrVersion` を呼び出します。 `LockClrVersion` は3つのパラメーターを受け取ります。これらはすべて[Flockclrversioncallback](../../../../docs/framework/unmanaged-api/hosting/flockclrversioncallback-function-pointer.md)型のコールバックです。 この型は次のように定義されています。  
  
```cpp  
typedef HRESULT ( __stdcall *FLockClrVersionCallback ) ();  
```  
  
 ランタイムの初期化時には、次の手順が実行されます。  
  
1. ホストは[Corbindtoruntimeex](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimeex-function.md)またはその他のランタイム初期化関数のいずれかを呼び出します。 また、ホストは COM オブジェクトアクティベーションを使用してランタイムを初期化することもできます。  
  
2. ランタイムは、`hostCallback` パラメーターによって指定された関数を呼び出します。  
  
3. `hostCallback` によって指定された関数は、次の一連の呼び出しを行います。  
  
    - `pBeginHostSetup` パラメーターによって指定された関数。  
  
    - `CorBindToRuntimeEx` (または別のランタイム初期化関数)。  
  
    - [ICLRRuntimeHost:: SetHostControl](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-sethostcontrol-method.md)。  
  
    - [ICLRRuntimeHost:: Start](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-start-method.md)。  
  
    - `pEndHostSetup` パラメーターによって指定された関数。  
  
 `pBeginHostSetup` から `pEndHostSetup` へのすべての呼び出しは、同じ論理スタックを持つ1つのスレッドまたはファイバーに対して行われる必要があります。 このスレッドは、`hostCallback` が呼び出されたスレッドとは異なる場合があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [非推奨の CLR ホスト関数](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
