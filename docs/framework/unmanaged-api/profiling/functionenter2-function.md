---
title: FunctionEnter2 関数
ms.date: 03/30/2017
api_name:
- FunctionEnter2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionEnter2
helpviewer_keywords:
- FunctionEnter2 function [.NET Framework profiling]
ms.assetid: ce7a21f9-0ca3-4b92-bc4b-bb803cae3f51
topic_type:
- apiref
ms.openlocfilehash: 6cd35c180b8a322b3402b050c6d6840073010b1f
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866984"
---
# <a name="functionenter2-function"></a>FunctionEnter2 関数
コントロールが関数に渡されていることをプロファイラーに通知し、スタックフレームと関数の引数に関する情報を提供します。 この関数は、 [Functionenter](functionenter-function.md)関数よりも優先されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
void __stdcall FunctionEnter2 (  
    [in]  FunctionID                       funcId,   
    [in]  UINT_PTR                         clientData,   
    [in]  COR_PRF_FRAME_INFO               func,   
    [in]  COR_PRF_FUNCTION_ARGUMENT_INFO  *argumentInfo  
);  
```  
  
## <a name="parameters"></a>パラメーター

- `funcId`

  \[] には、コントロールが渡される関数の識別子を指定します。

- `clientData`

  \[] マップされていない関数の識別子。これは、プロファイラーが以前に[Functionidmapper](functionidmapper-function.md)関数を使用して指定したものです。
  
- `func`

  \[] で、スタックフレームに関する情報を指す `COR_PRF_FRAME_INFO` 値です。
  
  プロファイラーは、これを[ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md)メソッドの実行エンジンに渡すことができる不透明なハンドルとして処理する必要があります。  
  
- `argumentInfo`

  \[]、関数の引数のメモリ内の場所を指定する[COR_PRF_FUNCTION_ARGUMENT_INFO](cor-prf-function-argument-info-structure.md)構造体へのポインター。

  引数情報にアクセスするには、`COR_PRF_ENABLE_FUNCTION_ARGS` フラグを設定する必要があります。 プロファイラーは、 [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md)メソッドを使用してイベントフラグを設定できます。

## <a name="remarks"></a>コメント  
 `func` パラメーターと `argumentInfo` パラメーターの値は、値が変更されるか、または破棄される可能性があるため、`FunctionEnter2` 関数から制御が戻った後に無効になります。  
  
 `FunctionEnter2` 関数はコールバックです。実装する必要があります。 実装では、`__declspec`(`naked`) ストレージクラス属性を使用する必要があります。  
  
 この関数を呼び出す前に、実行エンジンはレジスタを保存しません。  
  
- 入力時には、浮動小数点単位 (FPU) に含まれるすべてのレジスタを含め、使用するすべてのレジスタを保存する必要があります。  
  
- 終了時に、呼び出し元によってプッシュされたすべてのパラメーターをポップして、スタックを復元する必要があります。  
  
 `FunctionEnter2` の実装では、ガベージコレクションが遅延するため、ブロックしないでください。 スタックがガベージコレクションに対応していない可能性があるため、この実装ではガベージコレクションを実行しないようにしてください。 ガベージコレクションを実行しようとすると、ランタイムは `FunctionEnter2` が返されるまでブロックします。  
  
 また、`FunctionEnter2` 関数は、マネージコードを呼び出さないようにするか、マネージメモリ割り当てを発生させることはできません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [FunctionLeave2 関数](functionleave2-function.md)
- [FunctionTailcall2 関数](functiontailcall2-function.md)
- [SetEnterLeaveFunctionHooks2 メソッド](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [グローバル静的関数のプロファイル](profiling-global-static-functions.md)
