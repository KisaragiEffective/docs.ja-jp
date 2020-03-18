---
title: 'ICorProfilerInfo7:: Getinmemoryシンボルの長さメソッド'
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo7.GetInMemorySymbolsLength
api_location:
- mscorwks.dll
- icorprof.idl
api_type:
- COM
ms.assetid: d62c4a4c-8a62-45aa-8f01-a8387cf36159
ms.openlocfilehash: a675cc301d2dd32f87e3864a3123e2044761ef91
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868357"
---
# <a name="icorprofilerinfo7getinmemorysymbolslength-method"></a>ICorProfilerInfo7:: Getinmemoryシンボルの長さメソッド
[.NET Framework 4.6.1 以降のバージョンでのみでサポート]  
  
 メモリ内シンボルストリームの長さを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetInMemorySymbolsLength(  
        [in] ModuleID moduleId,  
        [out] DWORD* pCountSymbolBytes  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleId`  
 からメモリ内ストリームを格納しているモジュールの識別子。  
  
 Pcountシンボルバイト数  
 入出力メソッドから制御が戻るときに、ストリーム長がバイト単位で格納されている `DWORD` 値へのポインター。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、ゼロ (0) であっても、メモリストリームの長さを特定できる場合に `S_OK` を返します。  
  
 メソッドが <xref:System.Reflection.Emit?displayProperty=nameWithType>を使用して作成された場合、メソッドは `CORPROF_E_MODULE_IS_DYNAMIC` を返します。  
  
## <a name="remarks"></a>コメント  
 モジュールにメモリ内シンボルがある場合、ストリームの長さは `pCountSymbolBytes`に配置されます。 モジュールにメモリ内シンボルがない場合は、`*pCountSymbolBytes = 0`ます。  
  
> [!NOTE]
> 現在の実装では、リフレクションはサポートされていません。 モジュールがリフレクションを使用して作成された場合、メソッドは `CORPROF_E_MODULE_IS_DYNAMIC`を返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo7 インターフェイス](icorprofilerinfo7-interface.md)
