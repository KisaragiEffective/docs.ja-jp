---
title: ICorProfilerInfo5::SetEventMask2 メソッド
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- IcorProfilerInfo5.SetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 05dbbe2b-049c-4a60-be69-2ad7a949405e
topic_type:
- apiref
ms.openlocfilehash: 10e84b729c8af607165009a8591a69dbc1afcb1e
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868383"
---
# <a name="icorprofilerinfo5seteventmask2-method"></a>ICorProfilerInfo5::SetEventMask2 メソッド
[.NET Framework 4.5.2 以降のバージョンでのみでサポート]  
  
 プロファイラーが共通言語ランタイム (CLR) からイベント通知を受け取ることを希望するイベントの型を指定する値を設定します。 [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md)メソッドよりも多くの機能が用意されています。  
  
## <a name="syntax"></a>構文  
  
```cpp
HRESULT SetEventMask2(        [in] DWORD dwEventsLow,        [in] DWORD dwEventsHigh  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `dwEventsLow`  
 [in] イベントのカテゴリを指定する 4 バイトの値。 各ビットは、異なる性能、動作、またはイベントの型を制御します。 ビットは、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙体に記述されています。  
  
 `dwEventsHigh`  
 [in] イベントのカテゴリを指定する 4 バイトの値。  各ビットは、異なる性能、動作、またはイベントの型を制御します。 ビットは、 [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md)列挙体に記述されています。  
  
## <a name="remarks"></a>コメント  
 `SetEventMask2` メソッドは、プロファイラーが登録するコールバックを設定するために使用します。 通常は、 [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)メソッドを呼び出して、どのビットが設定されているかを判断し、その `pdwEventsLow` と `pdwEventsHigh` 値および設定する新しいビットの論理 OR を実行してから、`SetEventMask2` メソッドを呼び出します。  
  
 このメソッドは、 [Seteventmask](icorprofilerinfo-seteventmask-method.md)メソッドの代替手段として推奨されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo5 インターフェイス](icorprofilerinfo5-interface.md)
- [GetEventMask2 メソッド](icorprofilerinfo5-geteventmask2-method.md)
