---
title: ICorProfilerCallback8::D ynamicMethodJITCompilationStarted メソッド
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8.DynamicMethodJITCompilationStarted
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 1eaf29e1c93f352facde4af2ee57910783d82e5d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136464"
---
# <a name="icorprofilercallback8dynamicmethodjitcompilationstarted-method"></a>ICorProfilerCallback8::D ynamicMethodJITCompilationStarted メソッド
[.NET Framework 4.7 以降のバージョンでサポートされています]  
  
動的メソッドの JIT コンパイルが開始されるたびにプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DynamicMethodJITCompilationStarted(  
     [in]  FunctionID  functionId,   
     [in]  BOOL        fIsSafeToBlock,   
     [in]  LPCBYTE     pILHeader,   
     [in]  LONG        cbILHeader   
);  
```  
  
## <a name="parameters"></a>パラメーター  
[入力] `functionId`  
JIT コンパイルが開始されるメモリ内関数の識別子。   

[入力] `fIsSafeToBlock`   
ブロックによって、呼び出し元のスレッドがこのコールバックから戻るまでランタイムが待機する可能性があることを示す `true` ます。`false` は、ブロックがランタイムの操作に影響を与えないことを示します。  

[入力] `pILHeader`    
メソッドの IL ヘッダーの最初のバイトへのポインター。   

[入力] `cbILHeader`    
IL ヘッダー内のバイト数。 

## <a name="remarks"></a>コメント  

このコールバックは、動的メソッドが JIT コンパイルされるたびにトリガーされます。 これには、さまざまな IL スタブおよび LCG メソッドが含まれます。 その目的は、コンパイルされたメソッドをユーザーに識別するのに十分な情報をプロファイラーライターに提供することです。

> [!NOTE]
> 動的メソッドにはメタデータがないため、`functionId` 値を使用してメタデータトークンに解決することはできません。

`pILHeader` ポインターは、コールバック中にのみ有効です。

## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  
  
## <a name="see-also"></a>参照

- [DynamicMethodJITCompilationFinished メソッド](icorprofilercallback8-dynamicmethodjitcompilationfinished-method.md)
- [ICorProfilerCallback8 インターフェイス](icorprofilercallback8-interface.md)
