---
title: COR_TYPEID 構造体
ms.date: 03/30/2017
api_name:
- COR_TYPEID
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_TYPEID
helpviewer_keywords:
- COR_TYPEID structure [.NET Framework debugging]
ms.assetid: 1e172b14-ee22-4943-b3b8-3740e7bdcd2e
topic_type:
- apiref
ms.openlocfilehash: 4f6dbe8c17bd6a91078b87a87c1055fbf4977a88
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132307"
---
# <a name="cor_typeid-structure"></a>COR_TYPEID 構造体
型識別子が含まれます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct COR_TYPEID{  
    UINT64 token1;  
    UINT64 token2;  
} COR_TYPEID;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`token1`|最初のトークン。|  
|`token2`|2番目のトークン。|  
  
## <a name="remarks"></a>コメント  
 `COR_TYPEID` 構造体は、ガベージコレクトされるオブジェクトに関する情報を提供するさまざまなデバッグメソッドによって返されます。 その後、その項目に関する追加情報を提供する他のデバッグメソッドに引数として渡すことができます。 たとえば、COR_HEAPOBJECT [ICorDebugHeapEnum](icordebugheapenum-interface.md)オブジェクトを列挙することによって、マネージヒープ上の個々のオブジェクトを表す個々の[COR_HEAPOBJECT](cor-heapobject-structure.md)オブジェクトを取得できます。 その後、`COR_TYPEID` の値を `COR_HEAPOBJECT.type` フィールドから[ICorDebugProcess5:: GetTypeForTypeID](icordebugprocess5-gettypefortypeid-method.md)メソッドに渡して、オブジェクトに関する型情報を提供する、テキストオブジェクトを取得できます。  
  
 `COR_TYPEID` オブジェクトは不透明であることが想定されています。 個々のフィールドにアクセスしたり操作したりすることはできません。 唯一の用途は、メソッドの呼び出しで `out` パラメーターとして提供され、さらに他のメソッドに渡して追加情報を提供できる識別子として使用することです。  
  
## <a name="requirements"></a>要件  
 **・** [システム要件](../../get-started/system-requirements.md)に関するページを参照してください。  
  
 **ヘッダー:** CorDebug .idl、CorDebug. h  
  
 **ライブラリ**CorGuids .lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
