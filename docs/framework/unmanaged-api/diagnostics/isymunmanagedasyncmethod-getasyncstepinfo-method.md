---
description: '詳細情報: ISymUnmanagedAsyncMethod::GetAsyncStepInfo メソッド'
title: ISymUnmanagedAsyncMethod::GetAsyncStepInfo メソッド
ms.date: 03/30/2017
ms.assetid: 3ef5b4b8-4ac7-4906-849b-f932c5e3db07
ms.openlocfilehash: dc255323f103b3422b927b0489402b24767dcd92
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99737845"
---
# <a name="isymunmanagedasyncmethodgetasyncstepinfo-method"></a>ISymUnmanagedAsyncMethod::GetAsyncStepInfo メソッド

「[DefineAsyncStepInfo メソッド](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md)」を参照してください。  
  
## <a name="syntax"></a>構文  
  
```idl  
HRESULT GetAsyncStepInfo(    [in] ULONG32 cStepInfo,    [out] ULONG32 *pcStepInfo,    [in, size_is(cStepInfo)] ULONG32 yieldOffsets[],    [in, size_is(cStepInfo)] ULONG32 breakpointOffset[],    [in, size_is(cStepInfo)] mdToken breakpointMethod[]);  
```  
  
## <a name="parameters"></a>パラメーター  
  
|パラメーター|説明|  
|---------------|-----------------|  
|`cStepInfo`||  
|`pcStepInfo`||  
|`yieldOffsets`||  
|`breakpointOffset`||  
|`breakpointMethod`||  
  
## <a name="return-value"></a>戻り値  

 `HRESULT` を返します。  
  
## <a name="requirements"></a>必要条件  

 **ヘッダー:** CorSym.idl、CorSym.h  
  
## <a name="see-also"></a>関連項目

- [ISymUnmanagedAsyncMethod インターフェイス](isymunmanagedasyncmethod-interface.md)
