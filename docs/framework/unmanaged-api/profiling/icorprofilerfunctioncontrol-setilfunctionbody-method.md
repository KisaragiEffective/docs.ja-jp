---
description: '詳細について: ICorProfilerFunctionControl:: SetILFunctionBody メソッド'
title: ICorProfilerFunctionControl::SetILFunctionBody メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetILFunctionBody
helpviewer_keywords:
- ICorProfilerFunctionControl::SetILFunctionBody method [.NET Framework profiling]
- SetILFunctionBody method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: 2c33f0f7-75b2-4c19-b2c7-c94b54997576
topic_type:
- apiref
ms.openlocfilehash: 470eefce5b211adcfd111951be9a004b3bd7d8fc
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99781611"
---
# <a name="icorprofilerfunctioncontrolsetilfunctionbody-method"></a>ICorProfilerFunctionControl::SetILFunctionBody メソッド

メソッドの中間共通言語 (CIL) 本体を置換します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetILFunctionBody(  
    [in]  ULONG   cbNewILMethodHeader,  
    [in, size_is(cbNewILMethodHeader)] LPCBYTE pbNewILMethodHeader);  
```  
  
## <a name="parameters"></a>パラメーター  

 `cbNewILMethodHeader`  
 [入力] ヘッダー、および本体の後に続く構造を含む、新しい CIL の合計サイズ。  
  
 `pbNewILMethodHeader`  
 [入力] 新しい CIL ヘッダーへのポインター。  
  
## <a name="return-value"></a>戻り値  

 このメソッドは、次の特定の HRESULT を返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|置換が成功しました。|  
  
## <a name="remarks"></a>解説  

 [ICorProfilerInfo:: SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)メソッドとは異なり、 `SetILFunctionBody` メソッドは新しい CIL 本体に必要なメモリを管理します。 これは、プロファイラーによって提供される CIL 本体を [Imethodmalloc](imethodmalloc-interface.md) インターフェイスを使用して割り当てたり、特定の範囲内で割り当てたりする必要がないことを意味します。 この本体は、どのヒープにも割り当てることができます。 プロファイラーは、が返された後に、その CIL 本体に使用されるメモリを解放でき `SetILFunctionBody` ます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerFunctionControl インターフェイス](icorprofilerfunctioncontrol-interface.md)
