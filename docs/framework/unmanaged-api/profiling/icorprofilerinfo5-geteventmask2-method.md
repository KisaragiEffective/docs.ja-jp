---
title: ICorProfilerInfo5::GetEventMask2 メソッド
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerInfo5.GetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: f854b68f-009c-4ffb-89cd-ca874d1c0fb7
topic_type:
- apiref
ms.openlocfilehash: f3943eef969f777b40dc51c4900b190561f14887
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868396"
---
# <a name="icorprofilerinfo5geteventmask2-method"></a>ICorProfilerInfo5::GetEventMask2 メソッド
[.NET Framework 4.5.2 以降のバージョンでのみでサポート]  
  
 現在のイベント カテゴリを取得します。プロファイラーは、これに関する通知を共通言語ランタイム (CLR) から受け取ります。  [ICorProfilerInfo:: GetEventMask](icorprofilerinfo-geteventmask-method.md)メソッドで提供されていない機能を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp
HRESULT GetEventMask2(  
        [out] DWORD* pdwEventsLow,  
        [out] DWORD* pdwEventsHigh  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pdwEventsLow`  
 [out] イベントのカテゴリを指定する 4 バイト値へのポインター。 各ビットは、異なる性能、動作、またはイベントの型を制御します。 ビットは、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙体に記述されています。  
  
 `pdwEventsHigh`  
 [out] イベントのカテゴリを指定する 4 バイト値へのポインター。  各ビットは、異なる性能、動作、またはイベントの型を制御します。 ビットは、 [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md)列挙体に記述されています。  
  
## <a name="remarks"></a>Remarks  
 `GetEventMask2` メソッドは、プロファイラーがサブスクライブしたコールバックを判断するのに使用します。 通常は、`pdwEventsLow` と `pdwEventsHigh` 値と、設定する新しいビットの論理 OR を実行し、次に[SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)メソッドを呼び出します。  
  
 このメソッドは、 [Geteventmask](icorprofilerinfo-geteventmask-method.md)メソッドの代替手段として推奨されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo5 インターフェイス](icorprofilerinfo5-interface.md)
- [SetEventMask2 メソッド](icorprofilerinfo5-seteventmask2-method.md)
