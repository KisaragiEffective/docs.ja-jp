---
title: ICorProfilerCallback::JITCachedFunctionSearchStarted メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCachedFunctionSearchStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCachedFunctionSearchStarted
helpviewer_keywords:
- JITCachedFunctionSearchStarted method [.NET Framework profiling]
- ICorProfilerCallback::JITCachedFunctionSearchStarted method [.NET Framework profiling]
ms.assetid: 5cba642c-0d80-48ee-889d-198c5044d821
topic_type:
- apiref
ms.openlocfilehash: 964ea11b8e8c8f494066249f7da2057970d7e8f4
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866261"
---
# <a name="icorprofilercallbackjitcachedfunctionsearchstarted-method"></a>ICorProfilerCallback::JITCachedFunctionSearchStarted メソッド
以前にネイティブイメージジェネレーター (Ngen.exe) を使用してコンパイルされた関数に対して検索が開始されたことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT JITCachedFunctionSearchStarted(  
    [in]  FunctionID functionId,  
    [out] BOOL *pbUseCachedFunction);  
```  
  
## <a name="parameters"></a>パラメーター

- `functionId`

  \[] 検索を実行する関数の ID です。

- `pbUseCachedFunction`

  \[out] `true` キャッシュされたバージョンの関数を実行エンジンが使用する必要がある場合 (使用可能な場合)、それ以外の場合は `false`。 値が `false`場合、実行エンジンは、JIT コンパイルされていないバージョンを使用するのではなく、関数を JIT コンパイルします。

## <a name="remarks"></a>コメント  
 .NET Framework バージョン2.0 では、通常の NGen イメージのすべての関数に対して `JITCachedFunctionSearchStarted` および[ICorProfilerCallback:: JITCachedFunctionSearchFinished メソッド](icorprofilercallback-jitcachedfunctionsearchfinished-method.md)のコールバックは行われません。 プロファイル用に最適化された NGen イメージのみが、イメージ内のすべての関数のコールバックを生成します。 ただし、オーバーヘッドが増加するため、プロファイラーは、これらのコールバックを使用して just-in-time (JIT) コンパイルを強制的に実行する場合にのみ、プロファイラーで最適化された NGen イメージを要求する必要があります。 それ以外の場合、プロファイラーは関数情報を収集するためにレイジー戦略を使用する必要があります。  
  
 プロファイラーは、プロファイルされたアプリケーションの複数のスレッドが同時に同じメソッドを呼び出す場合をサポートする必要があります。 たとえば、スレッド A は `JITCachedFunctionSearchStarted` を呼び出し、プロファイラーは JIT コンパイルを強制するために*pbUseCachedFunction*を FALSE に設定することによって応答します。 次に、スレッド A は[ICorProfilerCallback:: JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md)と[ICorProfilerCallback:: JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)を呼び出します。  
  
 ここで、スレッド B は同じ関数の `JITCachedFunctionSearchStarted` を呼び出します。 プロファイラーは、関数を JIT コンパイルすることを意図していましたが、プロファイラーは2番目のコールバックを受け取ります。これは、プロファイラーが `JITCachedFunctionSearchStarted`へのスレッド A の呼び出しに応答する前に、スレッド B がコールバックを送信するためです。 スレッドが呼び出しを行う順序は、スレッドがどのようにスケジュールされているかによって異なります。  
  
 プロファイラーは、重複コールバックを受信するときに、`pbUseCachedFunction` によって参照される値を、重複するすべてのコールバックについて同じ値に設定する必要があります。 つまり、同じ `functionId` 値を使用して `JITCachedFunctionSearchStarted` が複数回呼び出された場合、プロファイラーは毎回同じ応答を返す必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
