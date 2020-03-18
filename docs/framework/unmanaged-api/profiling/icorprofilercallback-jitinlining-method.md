---
title: ICorProfilerCallback::JITInlining メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITInlining
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITInlining
helpviewer_keywords:
- JITInlining method [.NET Framework profiling]
- ICorProfilerCallback::JITInlining method [.NET Framework profiling]
ms.assetid: c2f45801-dd38-4b78-b6b7-64397dc73f83
topic_type:
- apiref
ms.openlocfilehash: 3e13b17fb03530730a78f6889309f1993419574b
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866214"
---
# <a name="icorprofilercallbackjitinlining-method"></a>ICorProfilerCallback::JITInlining メソッド
Just-in-time (JIT) コンパイラが、別の関数と共に関数を挿入しようとしていることをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT JITInlining(  
    [in]  FunctionID callerId,  
    [in]  FunctionID calleeId,  
    [out] BOOL      *pfShouldInline);  
```  
  
## <a name="parameters"></a>パラメーター  
 `callerId`  
 から`calleeId` 関数が挿入される関数の ID。  
  
 `calleeId`  
 から挿入する関数の ID。  
  
 `pfShouldInline`  
 [out] 挿入を行うことができるように `true` します。それ以外の場合は、`false`ます。  
  
## <a name="remarks"></a>コメント  
 プロファイラーは `pfShouldInline` を `false` に設定して、`calleeId` 関数が `callerId` 関数に挿入されないようにすることができます。 また、プロファイラーは、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙体の COR_PRF_DISABLE_INLINING 値を使用して、インライン挿入をグローバルに無効にすることができます。  
  
 インラインで挿入された関数は、入力または終了するためのイベントを発生させません。 したがって、正確なコールグラフを生成するためには、プロファイラーで `pfShouldInline` を `false` に設定する必要があります。 `pfShouldInline` を `false` に設定すると、パフォーマンスに影響します。インライン挿入では通常、速度が向上し、挿入されたメソッドの個別の JIT コンパイルイベントの数が減少するためです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
