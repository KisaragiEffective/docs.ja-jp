---
title: CLR_DEBUGGING_VERSION 構造体
ms.date: 03/30/2017
api_name:
- CLR_DEBUGGING_VERSION
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CLR_DEBUGGING_VERSION
helpviewer_keywords:
- CLR_DEBUGGING_VERSION structure [.NET Framework debugging]
ms.assetid: 4d821186-3ddf-405a-ac44-d79438a9f7f3
topic_type:
- apiref
ms.openlocfilehash: 651b916a0e3ca178432094428611f9bcc8f0fd17
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132419"
---
# <a name="clr_debugging_version-structure"></a>CLR_DEBUGGING_VERSION 構造体
デバッグのために共通言語ランタイム (CLR) の製品バージョンを定義します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _CLR_DEBUGGING_VERSION  
{  
    WORD wStructVersion;
    WORD wMajor;
    WORD wMinor;
    WORD wBuild;
    WORD wRevision;
} CLR_DEBUGGING_VERSION;
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`wStructVersion`|構造体のバージョン番号|  
|`wMajor`|メジャー バージョン番号。|  
|`wMinor`|マイナー バージョン番号。|  
|`wBuild`|ビルド番号。|  
|`wRevision`|リビジョン番号。|  
  
## <a name="remarks"></a>Remarks  
 `CLR_DEBUGGING_VERSION` 構造体は COR_VERSION 構造体と同じですが、`CLR_DEBUGGING_VERSION` 構造体には、追加の構造バージョンフィールド (`wStructVersion`) が用意されています。 現在、このフィールドは0に設定する必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug .idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
