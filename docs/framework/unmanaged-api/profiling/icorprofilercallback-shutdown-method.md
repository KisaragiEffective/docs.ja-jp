---
title: ICorProfilerCallback::Shutdown メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.Shutdown
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::Shutdown
helpviewer_keywords:
- ICorProfilerCallback::Shutdown method [.NET Framework profiling]
- Shutdown method [.NET Framework profiling]
ms.assetid: 1ea194f0-a331-4855-a2ce-37393b8e5f84
topic_type:
- apiref
ms.openlocfilehash: 490f9dd5446a51bd07881cdb9825d737e380a63e
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865858"
---
# <a name="icorprofilercallbackshutdown-method"></a>ICorProfilerCallback::Shutdown メソッド
アプリケーションがシャットダウン中であることをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Shutdown();  
```  
  
## <a name="remarks"></a>Remarks  
 `Shutdown` メソッドが呼び出された後に、プロファイラーコードが[ICorProfilerInfo](icorprofilerinfo-interface.md)インターフェイスのメソッドを安全に呼び出すことはできません。 `ICorProfilerInfo` メソッドを呼び出すと、`Shutdown` メソッドから制御が戻った後に、未定義の動作が発生します。 シャットダウン後も、特定の不変イベントが発生する可能性があります。プロファイラーは、このようになるとすぐに制御を戻す必要があります。  
  
 `Shutdown` メソッドは、プロファイリングされているマネージアプリケーションがマネージコードとして開始されている場合 (つまり、プロセススタックの初期フレームが管理されている場合) にのみ呼び出されます。 アプリケーションがアンマネージコードとして起動され、後でマネージコードにジャンプし、その結果、共通言語ランタイム (CLR) のインスタンスが作成された場合、`Shutdown` は呼び出されません。 このような場合、プロファイラーは、DLL_PROCESS_DETACH 値を使用してリソースを解放し、トレースをディスクにフラッシュするなどのデータのクリーンアップ処理を実行する `DllMain` ルーチンをライブラリに組み込む必要があります。  
  
 一般に、プロファイラーは予期しないシャットダウンに対処する必要があります。 たとえば、Win32's `TerminateProcess` メソッド (Winbase. h で宣言) によってプロセスが停止される場合があります。 それ以外の場合、CLR は、特定のマネージスレッド (バックグラウンドスレッド) を停止します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [Initialize メソッド](icorprofilercallback-initialize-method.md)
