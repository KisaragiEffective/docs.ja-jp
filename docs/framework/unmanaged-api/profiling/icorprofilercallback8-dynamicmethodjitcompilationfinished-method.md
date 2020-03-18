---
title: ICorProfilerCallback8::D ynamicMethodJITCompilationFinished メソッド
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8.DynamicMethodJITCompilationFinished
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 0e04459614ca697908fb9b71ecc3931ac305a838
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136581"
---
# <a name="icorprofilercallback8dynamicmethodjitcompilationfinished-method"></a>ICorProfilerCallback8::D ynamicMethodJITCompilationFinished メソッド
[.NET Framework 4.7 以降のバージョンでサポートされています]  
  
動的メソッドの JIT コンパイルが完了するたびにプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DynamicMethodJITCompilationFinished(  
     [in]  FunctionID  functionId,   
     [in]  BOOL        hrStatus,   
     [in]  BOOL        fIsSafeToBlock   
);  
```  
  
## <a name="parameters"></a>パラメーター  
[入力] `functionId`  
JIT コンパイルが開始されるメモリ内関数の識別子。   

[入力] `hrStatus`   
JIT コンパイルが成功したかどうかを示す値。

[入力] `fIsSafeToBlock`   
ブロックによって、呼び出し元のスレッドがこのコールバックから戻るまでランタイムが待機する可能性があることを示す `true` ます。`false` は、ブロックがランタイムの操作に影響を与えないことを示します。  

## <a name="remarks"></a>コメント  

このコールバックは、動的メソッドの JIT コンパイルが完了するたびにトリガーされます。 これには、さまざまな IL スタブおよび LCG メソッドが含まれます。 その目的は、コンパイルされたメソッドをユーザーに識別するのに十分な情報をプロファイラーライターに提供することです。

> [!NOTE]
> 動的メソッドにはメタデータがないため、`functionId` 値を使用してメタデータトークンに解決することはできません。

## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  
  
## <a name="see-also"></a>参照

- [DynamicMethodJITCompilationStarted メソッド](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)
- [ICorProfilerCallback8 インターフェイス](icorprofilercallback8-interface.md)
