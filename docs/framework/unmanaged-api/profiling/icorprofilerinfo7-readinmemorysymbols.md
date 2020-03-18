---
title: 'ICorProfilerInfo7:: ReadInMemorySymbols'
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo7.ReadInMemorySymbols
api_location:
- CorProf.idl
- CorProf.h
- CorGuids.lib
api_type:
- COM
ms.assetid: 1745a0b9-8332-4777-a670-b549bff3b901
ms.openlocfilehash: 53c01d2db44f4d0adf1ba5b9cc225ab49581aa5d
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868344"
---
# <a name="icorprofilerinfo7readinmemorysymbols"></a>ICorProfilerInfo7:: ReadInMemorySymbols
[.NET Framework 4.6.1 以降のバージョンでのみでサポート]  
  
 メモリ内シンボルストリームからバイトを読み取ります。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ReadInMemorySymbols(  
        [in] ModuleID moduleId,  
        [in] DWORD symbolsReadOffset,  
        [out] BYTE* pSymbolBytes,  
        [in] DWORD countSymbolBytes,  
        [out] DWORD* pCountSymbolBytesRead  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleId`  
 からメモリ内ストリームを格納しているモジュールの識別子。  
  
 `symbolsReadOffset`  
 からメモリ内ストリーム内でバイトの読み取りを開始する位置のオフセット。  
  
 `pSymbolBytes`  
 入出力データがコピーされるバッファーへのポインター。 バッファーには、使用可能な領域の `countSymbolBytes` が必要です。  
  
 `countSymbolBytes`  
 からコピーするバイト数。  
  
 `pCountSymbolBytesRead`  
 入出力メソッドから制御が戻るときに、実際に読み取られたバイト数を格納します。  
  
## <a name="return-value"></a>戻り値  
 0以外のバイト数が読み取られた場合は `S_OK`。  
  
 モジュールが <xref:System.Reflection.Emit>を使用して作成された場合は `CORPROF_E_MODULE_IS_DYNAMIC`。  
  
## <a name="remarks"></a>コメント  
 `ReadInMemorySymbols` メソッドは、インメモリストリーム内のオフセット `symbolsReadOffset` で始まるデータの `countSymbolBytes` の読み取りを試みます。 データは `pSymbolBytes`にコピーされます。これには、使用可能な領域の `countSymbolBytes` があることが予想されます。     `pCountSymbolsBytesRead` には実際に読み取られたバイト数が含まれます。ストリームの末尾に到達した場合、`countSymbolBytes` よりも小さくなることがあります。  
  
> [!NOTE]
> 現在の実装では、リフレクションはサポートされていません。 モジュールがリフレクションを使用して作成された場合、メソッドは `CORPROF_E_MODULE_IS_DYNAMIC`を返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo7 インターフェイス](icorprofilerinfo7-interface.md)
