---
description: 詳細については、CorDebugGuidToTypeMapping 構造体
title: CorDebugGuidToTypeMapping 構造体
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- CorDebugGuidToTypeMapping
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugGuidToTypeMapping
helpviewer_keywords:
- CorDebugGuidToTypeMapping structure [.NET Framework debugging]
ms.assetid: 57dbccd9-b16d-4da3-ae25-7a2cf9adf679
topic_type:
- apiref
ms.openlocfilehash: 5f6e99a17483b4fc16eb36ebb5fb5fd81380944b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99801619"
---
# <a name="cordebugguidtotypemapping-structure"></a>CorDebugGuidToTypeMapping 構造体

Windows ランタイム GUID を対応するテキストオブジェクトにマップします。  
  
## <a name="syntax"></a>構文  
  
```cpp
typedef struct CorDebugGuidToTypeMapping {  
    GUID iid;  
    ICorDebugType *pType;  
} CorDebugGuidToTypeMapping;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`iid`|キャッシュされた Windows ランタイムの種類の GUID。|  
|`pType`|キャッシュされた型に関する情報を提供する、テキストオブジェクトへのポインター。|  
  
## <a name="requirements"></a>要件  

 **プラットフォーム:** Windows ランタイム。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
