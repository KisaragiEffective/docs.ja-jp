---
title: FunctionEnter3WithInfo 関数
ms.date: 03/30/2017
api_name:
- FunctionEnter3WithInfo
api_location:
- mscorwks.cll
api_type:
- COM
f1_keywords:
- FunctionEnter3WithInfo
helpviewer_keywords:
- FunctionEnter3WithInfo function [.NET Framework profiling]
ms.assetid: 277c3344-d0cb-431e-beae-eb1eeeba8eea
topic_type:
- apiref
ms.openlocfilehash: 75a9a7174f105d99e9d1c9b220cfc5bf928d46ec
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866971"
---
# <a name="functionenter3withinfo-function"></a>FunctionEnter3WithInfo 関数
コントロールが関数に渡されていることをプロファイラーに通知し、 [ICorProfilerInfo3:: GetFunctionEnter3Info メソッド](icorprofilerinfo3-getfunctionenter3info-method.md)に渡すことができるハンドルを提供して、スタックフレームと関数の引数を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
void __stdcall FunctionEnter3WithInfo(  
               [in] FunctionIDOrClientID functionIDOrClientID,  
               [in] COR_PRF_ELT_INFO eltInfo);  
```  
  
## <a name="parameters"></a>パラメーター

- `functionIDOrClientID`

  \[] には、コントロールが渡される関数の識別子を指定します。

- `eltInfo`

  \[in] 特定のスタックフレームに関する情報を表す不透明なハンドル。 このハンドルは、渡されるコールバック中にのみ有効です。

## <a name="remarks"></a>コメント  
 `FunctionEnter3WithInfo` のコールバックメソッドは、関数が呼び出されたことをプロファイラーに通知します。また、プロファイラーは[ICorProfilerInfo3:: GetFunctionEnter3Info メソッド](icorprofilerinfo3-getfunctionenter3info-method.md)を使用して引数値を検査できます。 引数情報にアクセスするには、`COR_PRF_ENABLE_FUNCTION_ARGS` フラグを設定する必要があります。 プロファイラーは、 [ICorProfilerInfo:: SetEventMask メソッド](icorprofilerinfo-seteventmask-method.md)を使用してイベントフラグを設定し、 [ICorProfilerInfo3:: SetEnterLeaveFunctionHooks3WithInfo メソッド](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)を使用してこの関数の実装を登録できます。  
  
 `FunctionEnter3WithInfo` 関数はコールバックです。実装する必要があります。 実装では、`__declspec(naked)` のストレージクラス属性を使用する必要があります。  
  
 この関数を呼び出す前に、実行エンジンはレジスタを保存しません。  
  
- 入力時には、浮動小数点単位 (FPU) に含まれるすべてのレジスタを含め、使用するすべてのレジスタを保存する必要があります。  
  
- 終了時に、呼び出し元によってプッシュされたすべてのパラメーターをポップして、スタックを復元する必要があります。  
  
 `FunctionEnter3WithInfo` の実装では、ガベージコレクションが遅延するため、ブロックしないでください。 スタックがガベージコレクションに対応していない可能性があるため、この実装ではガベージコレクションを実行しないようにする必要があります。 ガベージコレクションを実行しようとすると、ランタイムは `FunctionEnter3WithInfo` が返されるまでブロックします。  
  
 `FunctionEnter3WithInfo` 関数は、マネージコードを呼び出さないようにするか、マネージメモリの割り当てを任意の方法で発生させることはできません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [GetFunctionEnter3Info](icorprofilerinfo3-getfunctionenter3info-method.md)
- [FunctionEnter3](functionenter3-function.md)
- [FunctionLeave3](functionleave3-function.md)
- [グローバル静的関数のプロファイル](profiling-global-static-functions.md)
