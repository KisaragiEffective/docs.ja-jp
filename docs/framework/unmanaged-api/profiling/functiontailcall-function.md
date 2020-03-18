---
title: FunctionTailcall 関数
ms.date: 03/30/2017
api_name:
- FunctionTailcall
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionTailcall
helpviewer_keywords:
- FunctionTailcall function [.NET Framework profiling]
ms.assetid: 66347e03-9a97-41e8-8f9d-89b80803f7b5
topic_type:
- apiref
ms.openlocfilehash: bd03eccc923049c4a49062d18bd11659f3316e8a
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866824"
---
# <a name="functiontailcall-function"></a>FunctionTailcall 関数
現在実行中の関数が別の関数の末尾呼び出しを実行しようとしていることをプロファイラーに通知します。  
  
> [!NOTE]
> `FunctionTailcall` 関数は、.NET Framework バージョン2.0 では非推奨とされます。 これは引き続き機能しますが、パフォーマンスが低下します。 代わりに、 [FunctionTailcall2](functiontailcall2-function.md)関数を使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp
void __stdcall FunctionTailcall (  
    [in] FunctionID funcID  
);  
```  
  
## <a name="parameters"></a>パラメーター

- `funcID`

  \[] では、末尾呼び出しを実行しようとしている現在実行中の関数の識別子。

## <a name="remarks"></a>Remarks  
 Tail 呼び出しの対象となる関数は、現在のスタックフレームを使用し、末尾呼び出しを行った関数の呼び出し元に直接戻ります。 これは、tail 呼び出しの対象である関数に対して[Functionleave](functionleave-function.md)コールバックが発行されないことを意味します。  
  
 `FunctionTailcall` 関数はコールバックです。実装する必要があります。 実装では、`__declspec`(`naked`) ストレージクラス属性を使用する必要があります。  
  
 この関数を呼び出す前に、実行エンジンはレジスタを保存しません。  
  
- 入力時には、浮動小数点単位 (FPU) に含まれるすべてのレジスタを含め、使用するすべてのレジスタを保存する必要があります。  
  
- 終了時に、呼び出し元によってプッシュされたすべてのパラメーターをポップして、スタックを復元する必要があります。  
  
 `FunctionTailcall` の実装では、ガベージコレクションが遅延するため、ブロックしないでください。 スタックがガベージコレクションに対応していない可能性があるため、この実装ではガベージコレクションを実行しないようにしてください。 ガベージコレクションを実行しようとすると、ランタイムは `FunctionTailcall` が返されるまでブロックします。  
  
 また、`FunctionTailcall` 関数は、マネージコードを呼び出さないようにするか、マネージメモリ割り当てを発生させることはできません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 1.1、1.0  
  
## <a name="see-also"></a>関連項目

- [FunctionEnter2 関数](functionenter2-function.md)
- [FunctionLeave2 関数](functionleave2-function.md)
- [SetEnterLeaveFunctionHooks2 メソッド](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [グローバル静的関数のプロファイル](profiling-global-static-functions.md)
