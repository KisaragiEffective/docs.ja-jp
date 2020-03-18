---
title: ICorProfilerCallback7::ModuleInMemorySymbolsUpdated Method
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback7.ModuleInMemorySymbolsUpdated
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: f362a896-3247-4894-9727-e48dbbcd2c78
ms.openlocfilehash: 6fbb86fc63a26599ae83c81817dbcb9abfb88cc8
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76864690"
---
# <a name="icorprofilercallback7moduleinmemorysymbolsupdated-method"></a>ICorProfilerCallback7::ModuleInMemorySymbolsUpdated Method
[.NET Framework 4.6.1 以降のバージョンでのみでサポート]  
  
 メモリ内モジュールに関連付けられているシンボルストリームが更新されるたびに、プロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ModuleInMemorySymbolsUpdated(  
     ModuleID moduleId  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 [入力] `moduleId`  
 シンボルストリームが更新されるメモリ内モジュールの識別子。  
  
## <a name="remarks"></a>Remarks  
 このコールバックは、 [ICorProfilerCallback5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)メソッドを呼び出すときに、 [COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED](cor-prf-high-monitor-enumeration.md)イベントマスクフラグを設定することによって制御されます。  
  
> [!NOTE]
> このイベントは、<xref:System.Reflection.Emit> Api を使用して暗黙的に作成または変更されたシンボルに対しては、現在発生していません。  
  
 アセンブリのシンボルを指定するための `rawSymbolStore` 引数を含むマネージ <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドのオーバーロードの1つを呼び出すことによって、シンボルが事前に提供されている場合でも、 [Moduleloadfinished](icorprofilercallback-moduleloadfinished-method.md)コールバックが発生するまで、ランタイムはシンボリックデータをモジュールに実際に関連付けない可能性があります。 このイベントは、後でこのようなモジュールのシンボルを収集する機会を提供します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ModuleLoadFinished メソッド](icorprofilercallback-moduleloadfinished-method.md)
- [SetEventMask2 メソッド](icorprofilerinfo5-seteventmask2-method.md)
- [ICorProfilerCallback7 インターフェイス](icorprofilercallback7-interface.md)
