---
title: COR_HEAPINFO 構造体
ms.date: 03/30/2017
api_name:
- COR_HEAPINFO
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_HEAPINFO
helpviewer_keywords:
- COR_HEAPINFO structure [.NET Framework debugging]
ms.assetid: bfb2cd39-3e0b-4d51-ba0c-f009755c1456
topic_type:
- apiref
ms.openlocfilehash: b6fd3682290c9752125aed7b9663c6704ade25de
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132331"
---
# <a name="cor_heapinfo-structure"></a>COR_HEAPINFO 構造体
列挙可能かどうかなど、ガベージ コレクション ヒープに関する情報が提供されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _COR_HEAPINFO {  
    BOOL areGCStructuresValid;   
    DWORD pointerSize;   
    DWORD numHeaps;  
    BOOL concurrent;   
    CorDebugGCType gcType;   
} COR_HEAPINFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`areGCStructuresValid`|ガベージコレクション構造が有効で、ヒープを列挙できるかどうかを `true` します。それ以外の場合は、`false`ます。|  
|`pointerSize`|ターゲットアーキテクチャのポインターのサイズ (バイト単位)。|  
|`numHeaps`|プロセス内の論理ガベージコレクションヒープの数。|  
|`concurrent`|同時実行 (バックグラウンド) ガベージコレクションが有効になっている場合に `TRUE` します。それ以外の場合は、`FALSE`ます。|  
|`gcType`|ガベージコレクターがワークステーションまたはサーバーのどちらで実行されているかを示す[CorDebugGCType](cordebuggctype-enumeration.md)列挙体のメンバー。|  
  
## <a name="remarks"></a>Remarks  
 [ICorDebugProcess5:: Getg](icordebugprocess5-getgcheapinformation-method.md)メソッドを呼び出すことによって、`COR_HEAPINFO` 構造体のインスタンスが返されます。  
  
 ガベージコレクションヒープのオブジェクトを列挙する前に、必ず `areGCStructuresValid` フィールドをチェックして、ヒープが列挙可能な状態であることを確認する必要があります。 詳細については、「 [ICorDebugProcess5:: Getg](icordebugprocess5-getgcheapinformation-method.md) 」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
