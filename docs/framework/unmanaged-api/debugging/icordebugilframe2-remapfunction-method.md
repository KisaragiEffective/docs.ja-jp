---
title: ICorDebugILFrame2::RemapFunction メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame2.RemapFunction
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame2::RemapFunction
helpviewer_keywords:
- ICorDebugILFrame2::RemapFunction method [.NET Framework debugging]
- RemapFunction method [.NET Framework debugging]
ms.assetid: dd639ba0-f77b-426d-9ff6-f92706840348
topic_type:
- apiref
ms.openlocfilehash: f4f73b99b4cb48690a2a8611dbf5a5420adab5d4
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794350"
---
# <a name="icordebugilframe2remapfunction-method"></a>ICorDebugILFrame2::RemapFunction メソッド
新しい Microsoft 中間言語 (MSIL) オフセットを指定して、編集された関数を再マップします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT RemapFunction (  
    [in] ULONG32      newILOffset  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `newILOffset`  
 から命令ポインターが配置されるスタックフレームの新しい MSIL オフセット。 この値は、シーケンスポイントである必要があります。  
  
 この値の有効性を保証するのは、呼び出し元の責任です。 たとえば、関数の境界の外側にある場合、MSIL オフセットは無効です。  
  
## <a name="remarks"></a>コメント  
 フレームの関数が編集されている場合、デバッガーは `RemapFunction` メソッドを呼び出して、フレームの関数の最新バージョンをスワップし、実行できるようにします。 コードの実行は、指定された MSIL オフセットから開始されます。  
  
> [!NOTE]
> `RemapFunction`を呼び出すと、[テキストボックス:: SetIP](icordebugilframe-setip-method.md)の呼び出しと同様に、スレッドのスタックトレースの生成に関連するすべてのデバッグインターフェイスがすぐに無効になります。 これらのインターフェイス[には](icordebugchain-interface.md)、次のようなインターフェイスがあります。  
  
 `RemapFunction` メソッドは、現在のフレームのコンテキストでのみ呼び出すことができ、次のいずれかの場合にのみ呼び出すことができます。  
  
- まだ継続していない[ICorDebugManagedCallback2:: FunctionRemapOpportunity](icordebugmanagedcallback2-functionremapopportunity-method.md)コールバックを受信した後。  
  
- このフレームに対して、コードの実行が停止している間に、このフレームに対しては、[というエラーが発生し](icordebugmanagedcallback-editandcontinueremap-method.md)ます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
