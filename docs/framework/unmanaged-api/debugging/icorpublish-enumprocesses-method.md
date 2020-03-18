---
title: ICorPublish::EnumProcesses メソッド
ms.date: 03/30/2017
api_name:
- ICorPublish.EnumProcesses
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublish::EnumProcesses
helpviewer_keywords:
- ICorPublish::EnumProcesses method [.NET Framework debugging]
- EnumProcesses method [.NET Framework debugging]
ms.assetid: 4ae765f0-93b2-4b6f-aea1-7b0cf44e04a7
topic_type:
- apiref
ms.openlocfilehash: 5f785b22a3fbda6403c124ec70757b16f5335907
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790767"
---
# <a name="icorpublishenumprocesses-method"></a>ICorPublish::EnumProcesses メソッド
このコンピューター上で実行されているマネージプロセスの列挙子を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumProcesses (  
    [in] COR_PUB_ENUMPROCESS       Type,  
    [out] ICorPublishProcessEnum   **ppIEnum  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `Type`  
 取得するプロセスの種類を指定する[COR_PUB_ENUMPROCESS](cor-pub-enumprocess-enumeration.md)列挙体の値。 現在のバージョンでは、COR_PUB_MANAGEDONLY のみが有効です。  
  
 `ppIEnum`  
 プロセスの列挙子である[ICorPublishProcessEnum](icorpublishprocessenum-interface.md)インスタンスのアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 列挙子のプロセスのコレクションは、`EnumProcesses` メソッドが呼び出されたときに実行されているプロセスのスナップショットに基づいています。 列挙子には、`EnumProcesses` が呼び出された後に終了または開始されるプロセスは含まれません。  
  
 この[ICorPublish](icorpublish-interface.md)インスタンスでは、`EnumProcesses` メソッドを複数回呼び出して、新しい最新のプロセスコレクションを作成することができます。 既存のコレクションは、`EnumProcesses` メソッドの後続の呼び出しの影響を受けません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorPub .idl、CorPub .h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorPublish インターフェイス](icorpublish-interface.md)
