---
title: ICorProfilerInfo3::RequestProfilerDetach メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.RequestProfilerDetach Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::RequestProfilerDetach
helpviewer_keywords:
- RequestProfilerDetach method [.NET Framework profiling]
- ICorProfilerInfo3::RequestProfilerDetach method [.NET Framework profiling]
ms.assetid: ea102e62-0454-4477-bcf3-126773acd184
topic_type:
- apiref
ms.openlocfilehash: 8520f5fc0a6ff7e71f40cd7fbb1caf68aab63197
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868513"
---
# <a name="icorprofilerinfo3requestprofilerdetach-method"></a>ICorProfilerInfo3::RequestProfilerDetach メソッド
プロファイラーをデタッチするようにランタイムに指示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT RequestProfilerDetach(  
   [in] DWORD    dwExpectedCompletionMilliseconds);  
```  
  
## <a name="parameters"></a>パラメーター  
 `dwExpectedCompletionMilliseconds`  
 [in] プロファイラーをアンロードするのが安全かどうかを確認するチェックを行うまでに共通言語ランタイム (CLR) が待機する時間 (ミリ秒)。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|デタッチ要求は有効です。またデタッチ プロシージャは別のスレッドで継続しています。 デタッチが完了すると、`ProfilerDetachSucceeded` イベントが発行されます。|  
|E_ CORPROF_E_CALLBACK3_REQUIRED|プロファイラーは、 [ICorProfilerCallback3](icorprofilercallback3-interface.md)インターフェイスの[IUnknown:: QueryInterface](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q))の試行に失敗しました。デタッチ操作をサポートするには、このインターフェイスを実装する必要があります。 デタッチは試行されませんでした。|  
|CORPROF_E_IMMUTABLE_FLAGS_SET|プロファイラーが起動時に変更できないフラグを設定しているため、デタッチできません。 デタッチは試行されませんでした。プロファイラーは完全にアタッチされたままです。|  
|CORPROF_E_IRREVERSIBLE_INSTRUMENTATION_PRESENT|プロファイラーがインストルメント化された Microsoft 中間言語 (MSIL) コードを使用したか、または `enter`/`leave` フックを挿入したため、デタッチは不可能です。 デタッチは試行されませんでした。プロファイラーは完全にアタッチされたままです。<br /><br /> **メモ**インストルメント化された MSIL はコードであり、 [SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)メソッドを使用してプロファイラーによって提供されます。|  
|CORPROF_E_RUNTIME_UNINITIALIZED|マネージド アプリケーションでランタイムがまだ初期化されていません。 (つまり、ランタイムが完全に読み込まれていません)。このエラーコードは、プロファイラーコールバックの[ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md)メソッド内でデタッチが要求された場合に返されることがあります。|  
|CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT|サポートされていないタイミングで `RequestProfilerDetach` が呼び出されました。 このエラーは、メソッドがマネージスレッドで呼び出されたが、 [ICorProfilerCallback](icorprofilercallback-interface.md)メソッド内、またはガベージコレクションを許容できない[ICorProfilerCallback](icorprofilercallback-interface.md)メソッド内から呼び出されなかった場合に発生します。 詳細については、「 [HRESULT CORPROF_E_UNSUPPORTED_CALL_SEQUENCE](corprof-e-unsupported-call-sequence-hresult.md)」を参照してください。|  
  
## <a name="remarks"></a>Remarks  
 デタッチ プロシージャの実行中に、デタッチ スレッド (プロファイラーのデタッチのために作成されたスレッド) は、すべてのスレッドがプロファイラーのコードを終了したかどうかを時々チェックします。 プロファイラーは、退避が終了するまでの予想時間を、`dwExpectedCompletionMilliseconds` パラメーターを介して提供する必要があります。 使用に適した値は、特定の `ICorProfilerCallback*` メソッド内でプロファイラーが使う通常の時間です。この値は、プロファイラーが使うと想定される最大時間の半分未満にしないでください。  
  
 デタッチ スレッドでは `dwExpectedCompletionMilliseconds` を使用して、プロファイラーのコールバック コードがすべてのスタックからポップされたかどうかをチェックする前にスリープする時間の長さを指定します。 次のアルゴリズムの詳細は今後の CLR のリリースで変更される可能性がありますが、これはプロファイラーをアンロードしても安全なタイミングを決定するために `dwExpectedCompletionMilliseconds` を利用する 1 つの方法を示しています。 デタッチ スレッドはまず `dwExpectedCompletionMilliseconds` ミリ秒間、スリープ状態になります。 復帰のスリープ状態を解除した後、プロファイラーのコールバックコードがまだ存在していることが CLR によって検出された場合、この場合は2倍の時間 `dwExpectedCompletionMilliseconds` ミリ秒間、デタッチスレッドが再びスリープ状態になります。 この 2 回目のスリープから復帰した後に、プロファイラーのコールバック コードがまだ存在することがデタッチ スレッドで検出された場合、再チェックが行われる前に 10 分間、スリープ状態になります。 デタッチ スレッドは継続して 10 分ごとに再チェックを行います。  
  
 プロファイラーが `dwExpectedCompletionMilliseconds` を 0 (ゼロ) に指定している場合、CLR は既定値 5000 を使用します。この設定では、チェックが 5 秒後に行われ、その次は 10 秒後に、それ以降は 10 分ごとに再チェックが行われます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo3 インターフェイス](icorprofilerinfo3-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
