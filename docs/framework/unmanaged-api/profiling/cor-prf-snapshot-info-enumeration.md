---
title: COR_PRF_SNAPSHOT_INFO 列挙型
ms.date: 03/30/2017
api_name:
- COR_PRF_SNAPSHOT_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_SNAPSHOT_INFO
helpviewer_keywords:
- COR_PRF_SNAPSHOT_INFO enumeration [.NET Framework profiling]
ms.assetid: a5906b2a-ad4a-4cc6-a421-2d7d8adf7468
topic_type:
- apiref
ms.openlocfilehash: 97bf3e69a8ea155d53479ba6f61988e56e3bd396
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867025"
---
# <a name="cor_prf_snapshot_info-enumeration"></a>COR_PRF_SNAPSHOT_INFO 列挙型
プロファイラーの[Stacksnapshotcallback](stacksnapshotcallback-function.md)関数を呼び出すたびに、スタックスナップショットで返すデータ量を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum _COR_PRF_SNAPSHOT_INFO {  
    COR_PRF_SNAPSHOT_DEFAULT = 0x0,  
    COR_PRF_SNAPSHOT_REGISTER_CONTEXT = 0x1,  
    COR_PRF_SNAPSHOT_X86_OPTIMIZED = 0X2  
} COR_PRF_SNAPSHOT_INFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|-------------|-----------------|  
|`COR_PRF_SNAPSHOT_DEFAULT`|`context` パラメーターを除くすべての `StackSnapshotCallback` パラメーターに対して値を渡す必要があることを示します。|  
|`COR_PRF_SNAPSHOT_REGISTER_CONTEXT`|`context` パラメーターを含め、すべての `StackSnapshotCallback` パラメーターに対して値を渡す必要があることを示します。|  
|`COR_PRF_SNAPSHOT_X86_OPTIMIZED`|よりシンプルで代替のスタックウォークアルゴリズムが使用されることを示します。|  
  
## <a name="remarks"></a>コメント  
 `COR_PRF_SNAPSHOT_INFO` 列挙体によって指定された値は、 [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)メソッドにパラメーターとして渡されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [DoStackSnapshot メソッド](icorprofilerinfo2-dostacksnapshot-method.md)
- [列挙型のプロファイリング](profiling-enumerations.md)
