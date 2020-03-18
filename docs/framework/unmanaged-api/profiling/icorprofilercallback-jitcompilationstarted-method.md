---
title: ICorProfilerCallback::JITCompilationStarted メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCompilationStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCompilationStarted
helpviewer_keywords:
- JITCompilationStarted method [.NET Framework profiling]
- ICorProfilerCallback::JITCompilationStarted method [.NET Framework profiling]
ms.assetid: 31782b36-d311-4518-8f45-25f65385af5b
topic_type:
- apiref
ms.openlocfilehash: e05cb944ea4f3a9ca718dc22c6cd726b6a516ea9
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866235"
---
# <a name="icorprofilercallbackjitcompilationstarted-method"></a>ICorProfilerCallback::JITCompilationStarted メソッド
Just-in-time (JIT) コンパイラが関数のコンパイルを開始したことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT JITCompilationStarted(  
    [in] FunctionID functionId,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>パラメーター  
 `functionId`  
 からコンパイルを開始する関数の ID。  
  
 `fIsSafeToBlock`  
 からプロファイラーに対して、ブロックがランタイムの操作に影響を与えるかどうかを示す値。 この値は、ブロックによって、呼び出し元のスレッドがこのコールバックから戻るまでランタイムが待機する場合に `true` ます。それ以外の場合は、`false`ます。  
  
 `true` の値はランタイムに害を及ぼすことはありませんが、プロファイルの結果をスキューできます。  
  
## <a name="remarks"></a>コメント  
 ランタイムがクラスコンストラクターを処理する方法によって、各関数に対して複数の `JITCompilationStarted` と[ICorProfilerCallback:: JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)呼び出しのペアを受け取ることができます。 たとえば、ランタイムは JIT コンパイルメソッド A を開始しますが、クラス B のクラスコンストラクターを実行する必要があります。 このため、ランタイムは、クラス B のコンストラクターを JIT でコンパイルして実行します。 コンストラクターが実行されている間、メソッド a が呼び出されます。これにより、メソッド A が再度 JIT コンパイルされます。 このシナリオでは、メソッド A の最初の JIT コンパイルが停止します。 ただし、JIT コンパイルメソッド A の両方の試行は、JIT コンパイルイベントを使用して報告されます。 プロファイラーは、 [ICorProfilerInfo:: SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)メソッドを呼び出すことによってメソッド A の Microsoft 中間言語 (MSIL) コードを置き換える必要がある場合、両方の `JITCompilationStarted` イベントに対してこれを行う必要がありますが、両方に同じ msil ブロックを使用する場合があります。  
  
 2つのスレッドが同時にコールバックを作成する場合、プロファイラーは JIT コールバックのシーケンスをサポートする必要があります。 たとえば、スレッド A は `JITCompilationStarted`を呼び出します。 ただし、スレッド A が `JITCompilationFinished`を呼び出す前に、スレッド B は、スレッド A の `JITCompilationStarted` コールバックからの関数 ID を使用して[ICorProfilerCallback:: ExceptionSearchFunctionEnter](icorprofilercallback-exceptionsearchfunctionenter-method.md)を呼び出します。 `JITCompilationFinished` の呼び出しがまだプロファイラーによって受信されていないため、関数 ID がまだ有効ではないように見えることがあります。 ただし、このような場合は、関数 ID が有効です。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [JITCompilationFinished メソッド](icorprofilercallback-jitcompilationfinished-method.md)
