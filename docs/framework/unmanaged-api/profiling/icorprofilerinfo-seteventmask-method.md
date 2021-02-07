---
description: '詳細について: ICorProfilerInfo:: SetEventMask メソッド'
title: ICorProfilerInfo::SetEventMask メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetEventMask
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetEventMask
helpviewer_keywords:
- ICorProfilerInfo::SetEventMask method [.NET Framework profiling]
- SetEventMask method [.NET Framework profiling]
ms.assetid: 44bc0f56-32fa-491e-a62d-52fc96d48125
topic_type:
- apiref
ms.openlocfilehash: e389d25abfecfc9a5dec8834e412fe618324e311
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99687195"
---
# <a name="icorprofilerinfoseteventmask-method"></a>ICorProfilerInfo::SetEventMask メソッド

共通言語ランタイム (CLR) からの通知を受け取るプロファイラーに対するイベントの種類を指定する値を設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetEventMask(  
    [in] DWORD dwEvents);  
```  
  
## <a name="parameters"></a>パラメーター  

 `dwEvents`  
 [in] イベントのカテゴリを指定する 4 バイトの値。 各ビットは、異なる性能、動作、またはイベントの型を制御します。 ビットは、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) 列挙体に記述されています。  
  
## <a name="remarks"></a>解説  
  
> [!NOTE]
> このメソッドではなく、 [SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) メソッドを呼び出す必要があります。 メソッドは `SetEventMask` 引き続きサポートされますが、 [SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) には追加機能が用意されています。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [SetEventMask2 メソッド](icorprofilerinfo5-seteventmask2-method.md)
