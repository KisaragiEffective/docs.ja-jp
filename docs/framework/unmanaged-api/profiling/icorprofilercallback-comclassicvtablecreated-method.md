---
description: '詳細について: ICorProfilerCallback:: COMClassicVTableCreated メソッド'
title: ICorProfilerCallback::COMClassicVTableCreated メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.COMClassicVTableCreated
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::COMClassicVTableCreated
helpviewer_keywords:
- COMClassicVTableCreated method [.NET Framework profiling]
- ICorProfilerCallback::COMClassicVTableCreated method [.NET Framework profiling]
ms.assetid: 6e1834ab-c359-498a-b10b-984ae23cdda4
topic_type:
- apiref
ms.openlocfilehash: 134ca44cbcd7a275e3ad61a3dd4decaa92668b5b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99657711"
---
# <a name="icorprofilercallbackcomclassicvtablecreated-method"></a>ICorProfilerCallback::COMClassicVTableCreated メソッド

指定した IID およびクラスの COM 相互運用機能の vtable が作成されたことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT COMClassicVTableCreated(  
    [in] ClassID wrappedClassId,  
    [in] REFGUID implementedIID,  
    [in] void    *pVTable,  
    [in] ULONG   cSlots);  
```  
  
## <a name="parameters"></a>パラメーター

- `wrappedClasId`

  \[in] vtable が作成されたクラスの ID。

- `implementedIID`

  \[in] クラスによって実装されるインターフェイスの ID。 この値は、インターフェイスが内部でのみ使用されている場合は NULL になります。

- `pVTable`

  \[) vtable の先頭へのポインター。

- `cSlots`

  \[in] vtable 内のスロットの数。

## <a name="remarks"></a>解説  

 プロファイラーは、このメソッドの実装でブロックしないでください。スタックがガベージコレクションを許可する状態にならないため、プリエンプティブガベージコレクションを有効にすることはできません。 プロファイラーがここでブロックし、ガベージコレクションを実行しようとすると、このコールバックが戻るまでランタイムはブロックします。  
  
 プロファイラーによるこのメソッドの実装では、マネージコードを呼び出さないようにするか、マネージメモリ割り当てを発生させることはできません。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [COMClassicVTableDestroyed メソッド](icorprofilercallback-comclassicvtabledestroyed-method.md)
