---
title: COR_PRF_GC_ROOT_FLAGS 列挙型
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_ROOT_FLAGS
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_ROOT_FLAGS
helpviewer_keywords:
- COR_PRF_GC_ROOT_FLAGS enumeration [.NET Framework profiling]
ms.assetid: 4611ee6f-0f05-4d84-91e1-e83d5e7dd7e4
topic_type:
- apiref
ms.openlocfilehash: 0210aca5698cd9c86979c13afd1e622b50d194df
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867186"
---
# <a name="cor_prf_gc_root_flags-enumeration"></a>COR_PRF_GC_ROOT_FLAGS 列挙型
ガベージコレクションのルートのプロパティを示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    COR_PRF_GC_ROOT_PINNING = 0x1,  
    COR_PRF_GC_ROOT_WEAKREF = 0x2,  
    COR_PRF_GC_ROOT_INTERIOR = 0x4,  
    COR_PRF_GC_ROOT_REFCOUNTED = 0x8,  
} COR_PRF_GC_ROOT_FLAGS;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_GC_ROOT_PINNING`|ルートは、ガベージコレクションがオブジェクトを移動できないようにします。|  
|`COR_PRF_GC_ROOT_WEAKREF`|ルートによってガベージコレクションが妨げられることはありません。|  
|`COR_PRF_GC_ROOT_INTERIOR`|ルートは、オブジェクト自体ではなく、オブジェクトのフィールドを参照します。|  
|`COR_PRF_GC_ROOT_REFCOUNTED`|オブジェクトの参照カウントが特定の値の場合、ルートはガベージコレクションを防止します。|  
  
## <a name="remarks"></a>コメント  
 `COR_PRF_GC_ROOT_FLAGS` は、特殊なルートに関する追加情報を提供するビットマスクです。 ただし、すべてのルートが特別であるとは限りません。 たとえば、一部のルートは、弱い参照、内部ポインター、ピン留め、または参照カウントされていません。 このようなルートの場合、伝えるフラグはありません。 したがって、 [ICorProfilerCallback2:: RootReferences2](icorprofilercallback2-rootreferences2-method.md)メソッドなど、この列挙を使用するメソッドは、フラグのビットマスクとして0を送信し、すべてのフラグがオフになっていることを示します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のプロファイリング](profiling-enumerations.md)
