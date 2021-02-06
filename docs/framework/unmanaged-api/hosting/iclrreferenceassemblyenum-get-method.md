---
description: '詳細について: ICLRReferenceAssemblyEnum:: Get メソッド'
title: ICLRReferenceAssemblyEnum::Get メソッド
ms.date: 03/30/2017
api_name:
- ICLRReferenceAssemblyEnum.Get
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRReferenceAssemblyEnum::Get
helpviewer_keywords:
- ICLRReferenceAssemblyEnum::Get method [.NET Framework hosting]
- Get method, ICLRReferenceAssemblyEnum interface [.NET Framework hosting]
ms.assetid: f21c1612-9c5d-4abc-a337-577086d29c17
topic_type:
- apiref
ms.openlocfilehash: ea4e71631a9ebb381f721b78f17507603891a032
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99637301"
---
# <a name="iclrreferenceassemblyenumget-method"></a>ICLRReferenceAssemblyEnum::Get メソッド

指定されたインデックス位置にあるアセンブリ id を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Get (  
    [in] DWORD dwIndex,  
    [out, size_is(*pcchBufferSize)] LPWSTR pwzBuffer,  
    [in, out] DWORD *pcchBufferSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `dwIndex`  
 から返されるアセンブリ id の0から始まるインデックス。  
  
 `pwzBuffer`  
 入出力アセンブリ id データを格納しているバッファー。  
  
 `pcchBufferSize`  
 [入力、出力]バッファーのサイズ `pwzBuffer` 。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`Get` 正常に返されました。|  
|ERROR_INSUFFICIENT_BUFFER|`pwzBuffer` が小さすぎます。|  
|ERROR_NO_MORE_ITEMS|列挙には、これ以上項目が含まれていません。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返す場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>解説  

 `Get` は通常、2回呼び出されます。 最初の呼び出しでは、に null 値を指定し、に `pwzBuffer` `pcchBufferSize` 適したサイズに設定し `pwzBuffer` ます。 2番目の呼び出しでは、適切にサイズ `pwzBuffer` を指定し、完了時に正規のアセンブリ id データを格納します。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRAssemblyReferenceList インターフェイス](iclrassemblyreferencelist-interface.md)
- [ICLRReferenceAssemblyEnum インターフェイス](iclrreferenceassemblyenum-interface.md)
