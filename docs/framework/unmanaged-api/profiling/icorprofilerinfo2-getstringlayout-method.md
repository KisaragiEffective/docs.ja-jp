---
title: ICorProfilerInfo2::GetStringLayout メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetStringLayout
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetStringLayout
helpviewer_keywords:
- GetStringLayout method [.NET Framework profiling]
- ICorProfilerInfo2::GetStringLayout method [.NET Framework profiling]
ms.assetid: 43189651-a535-4803-a1d1-f1c427ace2ca
topic_type:
- apiref
ms.openlocfilehash: 537840ac03b4136682b78cb964950ab5670a7d7e
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868617"
---
# <a name="icorprofilerinfo2getstringlayout-method"></a>ICorProfilerInfo2::GetStringLayout メソッド
文字列オブジェクトのレイアウトに関する情報を取得します。 このメソッドは .NET Framework 4 では非推奨とされており、 [ICorProfilerInfo3:: GetStringLayout2](icorprofilerinfo3-getstringlayout2-method.md)メソッドに置き換えられています。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetStringLayout(  
    [out] ULONG *pBufferLengthOffset,  
    [out] ULONG *pStringLengthOffset,  
    [out] ULONG *pBufferOffset);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pBufferLengthOffset`  
 入出力文字列の長さを格納する、`ObjectID` ポインターを基準とした位置のオフセットを指すポインター。 長さは `DWORD`として格納されます。  
  
> [!NOTE]
> このパラメーターは、バッファーの長さではなく、文字列自体の長さを返します。 バッファーの長さは使用できなくなりました。  
  
 `PStringLengthOffset`  
 入出力文字列自体の長さを格納する、`ObjectID` ポインターを基準とした位置のオフセットを指すポインター。 長さは `DWORD`として格納されます。  
  
 `pBufferOffset`  
 入出力ワイド文字の文字列を格納する、`ObjectID` ポインターを基準としたバッファーのオフセットへのポインター。  
  
## <a name="remarks"></a>コメント  
 `GetStringLayout` メソッドは、次のが格納されている場所のオフセットを `ObjectID` ポインターを基準として取得します。  
  
- 文字列のバッファーの長さ。  
  
- 文字列自体の長さ。  
  
- ワイド文字の実際の文字列を格納するバッファー。  
  
 文字列は null で終わる可能性があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
