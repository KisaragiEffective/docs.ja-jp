---
title: FunctionIDMapper2 関数
ms.date: 03/30/2017
api_name:
- FunctionIDMapper2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionIDMapper2
helpviewer_keywords:
- FunctionIDMapper2 function [.NET Framework profiling]
ms.assetid: 466ad51b-8f0c-41d9-81f7-371aac3374cb
topic_type:
- apiref
ms.openlocfilehash: af0ef412395394bb660ae6ed64fb154caef41655
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866919"
---
# <a name="functionidmapper2-function"></a>FunctionIDMapper2 関数
指定された関数の識別子が、その関数の[FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)、 [FunctionTailcall3](functiontailcall3-function.md)、または[FunctionEnter3WithInfo](functionenter3withinfo-function.md)、 [FunctionLeave3WithInfo](functionleave3withinfo-function.md)、および[FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)コールバックで使用される代替 ID に再マップされる可能性があることをプロファイラーに通知します。 また `FunctionIDMapper2` により、プロファイラーはその関数のコールバックを受信するかどうかを示すことができます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
UINT_PTR __stdcall FunctionIDMapper2 (  
    [in]  FunctionID  funcId,  
    [in]  void * clientData,  
    [out] BOOL       *pbHookFunction  
);  
```  
  
## <a name="parameters"></a>パラメーター

- `funcId`

  \[] マップを再マップする関数の識別子。

- `clientData`

  に \[] ランタイム間であいまいさを解消するために使用されるデータへのポインター。

- `pbHookFunction`

  \[out] プロファイラーが `FunctionEnter3`、`FunctionLeave3`、`FunctionTailcall3`、`FunctionEnter3WithInfo`、`FunctionLeave3WithInfo`、および `FunctionTailcall3WithInfo` のコールバックを受信する場合に `true` するように設定する値へのポインター。それ以外の場合は、この値を `false`に設定します。

## <a name="return-value"></a>戻り値  
 プロファイラーは、実行エンジンが代替関数識別子として使用する値を返します。 `false` で `pbHookFunction` を返さない限り、戻り値を null にすることはできません。 これ以外の状況で戻り値を null にすると、プロセスの中止など、予測できない結果が発生します。  
  
## <a name="remarks"></a>Remarks  
 このメソッドは、クライアントデータを渡すために使用される追加のパラメーターを使用して[Functionidmapper](functionidmapper-function.md)関数を拡張します。 クライアント データを使用すると、ランタイム間のあいまいさが解消されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo::SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)
- [ICorProfilerInfo3::SetFunctionIDMapper2](icorprofilerinfo3-setfunctionidmapper2-method.md)
- [FunctionEnter3](functionenter3-function.md)
- [FunctionLeave3](functionleave3-function.md)
- [FunctionTailcall3](functiontailcall3-function.md)
- [FunctionEnter3WithInfo](functionenter3withinfo-function.md)
- [FunctionLeave3WithInfo](functionleave3withinfo-function.md)
- [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)
- [グローバル静的関数のプロファイル](profiling-global-static-functions.md)
