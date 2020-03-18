---
title: ICorDebugModule::GetMetaDataInterface メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugModule.GetMetaDataInterface
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule::GetMetaDataInterface
helpviewer_keywords:
- ICorDebugModule::GetMetaDatainterface method [.NET Framework debugging]
- GetMetaDatainterface method [.NET Framework debugging]
ms.assetid: 30d906f2-cf35-4fa9-9d4c-0c31b58c9f3a
topic_type:
- apiref
ms.openlocfilehash: fb96949e22b4edfc0e64780a54bb38da44eb8369
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129545"
---
# <a name="icordebugmodulegetmetadatainterface-method"></a>ICorDebugModule::GetMetaDataInterface メソッド
モジュールのメタデータを調べるために使用できるメタデータインターフェイスオブジェクトを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetMetaDataInterface (  
    [in] REFIID      riid,  
    [out] IUnknown **ppObj  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `riid`  
 からメタデータインターフェイスを指定する参照 ID。  
  
 `ppObj`  
 入出力[メタデータインターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)の1つである `T:IUnknown` オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 デバッガーでは、`GetMetaDataInterface` メソッドを使用して、モジュールの元のメタデータのコピーを作成できます。モジュールは、モジュールを編集するために実行する必要があります。 デバッガーは `GetMetaDataInterface` を呼び出してモジュールの[IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)インターフェイスオブジェクトを取得し、 [IMetaDataEmit:: savetomemory](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-savetomemory-method.md)を呼び出してモジュールのメタデータのコピーをメモリに保存します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ](../../../../docs/framework/unmanaged-api/metadata/index.md)
