---
title: ICorProfilerCallback::UnmanagedToManagedTransition メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.UnmanagedToManagedTransition
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition
helpviewer_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition method [.NET Framework profiling]
- UnmanagedToManagedTransition method [.NET Framework profiling]
ms.assetid: ade2cc01-9b81-4e09-a5f9-b3b9dda27e96
topic_type:
- apiref
ms.openlocfilehash: c381d4a85a1e836f1972060c8182dd698bb27550
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870536"
---
# <a name="icorprofilercallbackunmanagedtomanagedtransition-method"></a>ICorProfilerCallback::UnmanagedToManagedTransition メソッド
アンマネージコードからマネージコードへの遷移が発生したことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT UnmanagedToManagedTransition(  
    [in] FunctionID functionId,  
    [in] COR_PRF_TRANSITION_REASON reason);  
```  
  
## <a name="parameters"></a>パラメーター  
 `functionId`  
 から呼び出される関数の ID。  
  
 `reason`  
 からアンマネージコードからのマネージコードの呼び出しによって移行が発生したかどうか、またはマネージ関数によって呼び出されたアンマネージ関数からの戻りが原因で発生したかどうかを示す[COR_PRF_TRANSITION_REASON](cor-prf-transition-reason-enumeration.md)列挙体の値。  
  
## <a name="remarks"></a>コメント  
 `reason` の値が COR_PRF_TRANSITION_RETURN で `functionId` が null でない場合、関数 ID はアンマネージ関数の ID であり、just-in-time (JIT) コンパイラを使用してコンパイルされることはありません。 アンマネージ関数には、名前やメタデータなど、いくつかの基本的な情報が関連付けられています。  
  
 `reason` の値が COR_PRF_TRANSITION_CALL 場合は、呼び出された関数 (つまり、マネージ関数) がまだ JIT でコンパイルされていない可能性があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ManagedToUnmanagedTransition メソッド](icorprofilercallback-managedtounmanagedtransition-method.md)
- [C++ での明示的な PInvoke (DllImport 属性) の使用方法](/cpp/dotnet/using-explicit-pinvoke-in-cpp-dllimport-attribute)
- [C++ Interop (暗黙の PInvoke) の使用](/cpp/dotnet/using-cpp-interop-implicit-pinvoke)
