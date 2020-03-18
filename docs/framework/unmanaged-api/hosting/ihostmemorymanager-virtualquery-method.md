---
title: IHostMemoryManager::VirtualQuery メソッド
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.VirtualQuery
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::VirtualQuery
helpviewer_keywords:
- IHostMemoryManager::VirtualQuery method [.NET Framework hosting]
- VirtualQuery method [.NET Framework hosting]
ms.assetid: 757af1e6-b9e8-49e7-b5db-342be3aa205f
topic_type:
- apiref
ms.openlocfilehash: 00ec0b92a9f7151ee9b831c85548c4f61d87af68
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73192031"
---
# <a name="ihostmemorymanagervirtualquery-method"></a>IHostMemoryManager::VirtualQuery メソッド
対応する Win32 関数の論理ラッパーとして機能します。 `VirtualQuery` の Win32 実装では、呼び出し元プロセスの仮想アドレス空間にあるページの範囲に関する情報を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT VirtualQuery (  
    [in]  void*    lpAddress,  
    [out] void*    lpBuffer,  
    [in]  SIZE_T   dwLength,  
    [out] SIZE_T*  pResult  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `lpAddress`  
 から照会される仮想メモリ内のアドレスへのポインター。  
  
 `lpBuffer`  
 入出力指定されたメモリ領域に関する情報を格納している構造体へのポインター。  
  
 `dwLength`  
 から`lpBuffer` が指すバッファーのサイズ (バイト単位)。  
  
 `pResult`  
 入出力情報バッファーによって返されたバイト数へのポインター。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`VirtualQuery` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>Remarks  
 `VirtualQuery` は、呼び出し元プロセスの仮想アドレス空間にあるページの範囲に関する情報を提供します。 この実装では、`pResult` パラメーターの値を、情報バッファーで返されるバイト数に設定し、HRESULT 値を返します。 Win32 `VirtualQuery` 関数では、戻り値はバッファーサイズです。 詳細については、Windows プラットフォームのドキュメントを参照してください。  
  
> [!IMPORTANT]
> オペレーティングシステムの `VirtualQuery` の実装では、デッドロックは発生せず、ユーザーコード内で中断されたランダムなスレッドで完了まで実行できます。 このメソッドのホストされたバージョンを実装する場合は、細心の注意を払ってください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IHostMemoryManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostmemorymanager-interface.md)
