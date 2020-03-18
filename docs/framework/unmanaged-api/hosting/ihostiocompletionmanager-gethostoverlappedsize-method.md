---
title: IHostIoCompletionManager::GetHostOverlappedSize メソッド
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.GetHostOverlappedSize
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::GetHostOverlappedSize
helpviewer_keywords:
- IHostIoCompletionManager::GetHostOverlappedSize method [.NET Framework hosting]
- GetHostOverlappedSize method [.NET Framework hosting]
ms.assetid: 2902578b-d5e2-4f8d-a103-0c7b6dceda9e
topic_type:
- apiref
ms.openlocfilehash: 3c662021894acbb8eb448fa2166498254158caa4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133858"
---
# <a name="ihostiocompletionmanagergethostoverlappedsize-method"></a>IHostIoCompletionManager::GetHostOverlappedSize メソッド
ホストが i/o 要求に追加しようとしているカスタムデータのサイズを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetHostOverlappedSize (  
    [out] DWORD *pcbSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pcbSize`  
 入出力Win32 `OVERLAPPED` オブジェクトのサイズに加えて、共通言語ランタイム (CLR: common language runtime) によって割り当てられるバイト数へのポインター。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`GetHostOverlappedSize` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>Remarks  
 Windows プラットフォーム Api に対するすべての非同期 i/o 呼び出しでは、Win32 `OVERLAPPED` オブジェクトが使用されます。このオブジェクトは、ファイルポインターの位置などの情報を提供します。 状態を維持するために、非同期 i/o 呼び出しを行うアプリケーションは、通常、カスタムデータを構造に追加します。 `GetHostOverlappedSize` と[Iho/O補完 manager:: InitializeHostOverlapped](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-initializehostoverlapped-method.md)は、このようなカスタムデータをホストに含める機会を提供します。  
  
 CLR は、`GetHostOverlappedSize` メソッドを呼び出して、ホストが `OVERLAPPED` オブジェクトに追加するカスタムデータのサイズを決定します。  
  
> [!NOTE]
> `GetHostOverlappedSize` は1回だけ呼び出されます。 ホストのカスタムデータは、すべての i/o 要求に対して同じサイズである必要があります。  
  
> [!IMPORTANT]
> `OVERLAPPED` オブジェクト自体のサイズは、`pcbSize`の値には含まれません。  
  
 `OVERLAPPED` 構造の詳細については、Windows プラットフォームのドキュメントを参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Threading.NativeOverlapped>
- [ICLRIoCompletionManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclriocompletionmanager-interface.md)
- [IHostIoCompletionManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-interface.md)
