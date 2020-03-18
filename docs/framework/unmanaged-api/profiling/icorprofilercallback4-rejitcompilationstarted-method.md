---
title: ICorProfilerCallback4::ReJITCompilationStarted メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITCompilationStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITCompilationStarted
helpviewer_keywords:
- ReJITCompilationStarted method, ICorProfilerCallback4 interface [.NET Framework profiling]
- ICorProfilerCallback4::ReJITCompilationStarted method [.NET Framework profiling]
ms.assetid: 512fdd00-262a-4456-a075-365ef4133c4d
topic_type:
- apiref
ms.openlocfilehash: 81d11c87c9bc970dd5b5c9010023610cea7c0e72
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865195"
---
# <a name="icorprofilercallback4rejitcompilationstarted-method"></a>ICorProfilerCallback4::ReJITCompilationStarted メソッド
Just-in-time (JIT) コンパイラが関数の再コンパイルを開始したことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ReJITCompilationStarted(   
    [in] FunctionID functionId,  
    [in] ReJITID    rejitId,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>パラメーター  
 `functionId`  
 からJIT コンパイラが再コンパイルを開始した関数の ID。  
  
 `rejitId`  
 から関数の新しいバージョンの再コンパイル ID。  
  
 `fIsSafeToBlock`  
 [in] ブロックによって、呼び出し元のスレッドがこのコールバックから戻るまでランタイムが待機する可能性があることを示す `true` ます。`false` は、ブロックがランタイムの操作に影響を与えないことを示します。 `true` の値はランタイムに害を与えませんが、プロファイルの結果に影響を与える可能性があります。  
  
## <a name="remarks"></a>Remarks  
 ランタイムがクラスコンストラクターを処理する方法により、各関数に対して複数の `ReJITCompilationStarted` および[ReJITCompilationFinished](icorprofilercallback4-rejitcompilationfinished-method.md)メソッド呼び出しを受け取ることができます。 たとえば、ランタイムはメソッド A の再コンパイルを開始しますが、クラス B のクラスコンストラクターを実行する必要があります。 このため、ランタイムはクラス B のコンストラクターを再コンパイルして実行します。 コンストラクターが実行されている間、メソッド a が呼び出されます。これにより、メソッド A が再コンパイルされます。 このシナリオでは、メソッド A の最初の再コンパイルが停止します。 ただし、メソッド A を再コンパイルしようとすると、JIT 再コンパイルイベントで報告されます。  
  
 2つのスレッドが同時にコールバックを作成する場合、プロファイラーは JIT 再コンパイルコールバックのシーケンスをサポートする必要があります。 たとえば、スレッド A は `ReJITCompilationStarted`を呼び出します。ただし、スレッド A が[ReJITCompilationFinished](icorprofilercallback4-rejitcompilationfinished-method.md)を呼び出す前に、スレッド B はスレッド a の `ReJITCompilationStarted` コールバックからの関数 ID を使用して[ICorProfilerCallback:: ExceptionSearchFunctionEnter](icorprofilercallback-exceptionsearchfunctionenter-method.md)を呼び出します。[ReJITCompilationFinished](icorprofilercallback4-rejitcompilationfinished-method.md)の呼び出しがまだプロファイラーによって受信されていないため、関数 ID がまだ有効ではないように見えることがあります。 ただし、この場合、関数 ID は有効です。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
- [JITCompilationFinished メソッド](icorprofilercallback-jitcompilationfinished-method.md)
- [ReJITCompilationFinished メソッド](icorprofilercallback4-rejitcompilationfinished-method.md)
