---
title: ICorProfilerInfo::ForceGC メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.ForceGC
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::ForceGC
helpviewer_keywords:
- ICorProfilerInfo::ForceGC method [.NET Framework profiling]
- ForceGC method [.NET Framework profiling]
ms.assetid: 0da1ef80-d242-4636-87d0-43e0470b342a
topic_type:
- apiref
ms.openlocfilehash: e1fe38419cda328c919f0840d51cf6336919aa60
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76864209"
---
# <a name="icorprofilerinfoforcegc-method"></a>ICorProfilerInfo::ForceGC メソッド
共通言語ランタイム (CLR) 内で強制的にガベージコレクションを実行します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ForceGC();  
```  
  
## <a name="remarks"></a>コメント  
 `ForceGC` メソッドを呼び出すことができるのは、マネージコードを実行しておらず、そのスタックにプロファイラーコールバックがないスレッドだけです。 最も便利な実装は、通知されたときに `ForceGC` を呼び出す、プロファイラー内に別のスレッドを作成することです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
