---
title: ICorProfilerInfo2::GetClassLayout メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassLayout
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassLayout
helpviewer_keywords:
- ICorProfilerInfo2::GetClassLayout method [.NET Framework profiling]
- GetClassLayout method, ICorProfilerInfo2 interface [.NET Framework profiling]
ms.assetid: a3a36987-5666-4e2f-95b5-d0cb246502ec
topic_type:
- apiref
ms.openlocfilehash: 85319a45861b2b48f7690f69bb8f9f9469af014c
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76862803"
---
# <a name="icorprofilerinfo2getclasslayout-method"></a>ICorProfilerInfo2::GetClassLayout メソッド
指定したクラスで定義されるフィールドのメモリ内レイアウトに関する情報を取得します。 つまり、このメソッドはクラスのフィールドのオフセットを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetClassLayout(  
    [in]  ClassID classID,  
    [in, out] COR_FIELD_OFFSET rFieldOffset[],  
    [in]  ULONG cFieldOffset,  
    [out] ULONG *pcFieldOffset,  
    [out] ULONG *pulClassSize);  
```  
  
## <a name="parameters"></a>パラメーター  
 `classID`  
 [in] レイアウトを取得するクラスの ID。  
  
 `rFieldOffset`  
 [入力、出力][COR_FIELD_OFFSET](../../../../docs/framework/unmanaged-api/metadata/cor-field-offset-structure.md)構造体の配列。各構造体には、クラスのフィールドのトークンとオフセットが格納されます。  
  
 `cFieldOffset`  
 [in] `rFieldOffset` 配列のサイズ。  
  
 `pcFieldOffset`  
 [out] 使用できる要素の総数へのポインター。 `cFieldOffset` が 0 である場合、この値は必要な要素の数を示します。  
  
 `pulClassSize`  
 [out] クラスのサイズ (バイト単位) を含む位置へのポインター。  
  
## <a name="remarks"></a>コメント  
 `GetClassLayout` メソッドは、クラス自体によって定義されているフィールドのみを返します。 クラスの親クラスもフィールドを定義している場合、プロファイラーは親クラスで `GetClassLayout` を呼び出してそのフィールドを取得する必要があります。  
  
 `GetClassLayout` を文字列クラスと一緒に使用する場合、このメソッドは E_INVALIDARG というエラー コードで失敗します。 文字列のレイアウトに関する情報を取得するには、 [ICorProfilerInfo2:: GetStringLayout](icorprofilerinfo2-getstringlayout-method.md)を使用します。 `GetClassLayout` は、配列クラスを使用して呼び出される場合も失敗します。  
  
 `GetClassLayout` から制御が戻ったら、`rFieldOffset` バッファーのサイズが十分で、すべての使用可能な `COR_FIELD_OFFSET` 構造体を格納できたかどうかを確認する必要があります。 これを行うには、`pcFieldOffset` が指している値と、`rFieldOffset` のサイズを `COR_FIELD_OFFSET` 構造体のサイズで割った値を比較します。 `rFieldOffset` の大きさが十分でない場合は、`rFieldOffset` バッファーの割り当てを増やし、`cFieldOffset` を新しい大きいサイズに更新して、`GetClassLayout` を再度呼び出します。  
  
 別の方法として、最初に `GetClassLayout` を長さゼロの `rFieldOffset` バッファーで呼び出して、適切なバッファーのサイズを取得します。 その後、バッファーのサイズを `pcFieldOffset` で返された値に設定し、`GetClassLayout` を再度呼び出します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
