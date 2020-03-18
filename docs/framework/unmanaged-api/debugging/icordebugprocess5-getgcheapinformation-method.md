---
title: ICorDebugProcess5::GetGCHeapInformation メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.GetGCHeapInformation
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::GetGCHeapInformation
helpviewer_keywords:
- ICorDebugProcess5::GetGCHeapInformation method [.NET Framework debugging]
- GetGCHeapInformation method, ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: b9538ceb-230a-4079-9cb2-903dbf5c1848
topic_type:
- apiref
ms.openlocfilehash: 703f159c5bc6b73dcd0e770bdeb61f676aae034c
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792373"
---
# <a name="icordebugprocess5getgcheapinformation-method"></a>ICorDebugProcess5::GetGCHeapInformation メソッド
現在列挙可能であるかどうかなど、ガベージコレクションヒープに関する一般的な情報を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetGCHeapInformation(  
    [out] COR_HEAPINFO *pHeapInfo  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pHeapInfo`  
 入出力ガベージコレクションヒープに関する全般的な情報を提供する[COR_HEAPINFO](cor-heapinfo-structure.md)値へのポインター。  
  
## <a name="remarks"></a>コメント  
 プロセス内のガベージコレクション構造が現在有効であることを確認するには、ヒープまたは個々のヒープ領域を列挙する前に、`ICorDebugProcess5::GetGCHeapInformation` メソッドを呼び出す必要があります。 コレクションの実行中にガベージコレクションヒープをウォークすることはできません。 それ以外の場合、列挙体は無効なガベージコレクション構造をキャプチャする可能性があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugProcess5 インターフェイス](icordebugprocess5-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
