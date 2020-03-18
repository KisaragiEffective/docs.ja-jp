---
title: COR_HEAPOBJECT 構造体
ms.date: 03/30/2017
api_name:
- COR_HEAPOBJECT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_HEAPOBJECT
helpviewer_keywords:
- COR_HEAPOBJECT structure [.NET Framework debugging]
ms.assetid: a92fdf95-492b-49ae-a741-2186e5c1d7c5
topic_type:
- apiref
ms.openlocfilehash: 270360a8950197eca14e02a60554659e5ac7b91c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73099079"
---
# <a name="cor_heapobject-structure"></a>COR_HEAPOBJECT 構造体
マネージド ヒープ上のオブジェクトに関する情報が提供されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _COR_HEAPOBJECT {  
    CORDB_ADDRESS address;    
    ULONG64 size;             
    COR_TYPEID type;          
} COR_HEAPOBJECT;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`address`|メモリ内のオブジェクトのアドレス。|  
|`size`|オブジェクトの合計サイズ (バイト単位)。|  
|`type`|オブジェクトの型を表す[COR_TYPEID](cor-typeid-structure.md)トークンです。|  
  
## <a name="remarks"></a>Remarks  
 `COR_HEAPOBJECT` インスタンスは、 [ICorDebugProcess5:: EnumerateHeap](icordebugprocess5-enumerateheap-method.md)メソッドを呼び出すことによって設定された、表示されていない[heapenum](icordebugheapenum-interface.md)インターフェイスオブジェクトを列挙することによって取得できます。  
  
 `COR_HEAPOBJECT` インスタンスは、マネージヒープ上のライブオブジェクトに関する情報、またはオブジェクトではなく、ガベージコレクターによってまだ収集されていないオブジェクトに関する情報を提供します。  
  
 パフォーマンスを向上させるために、`COR_HEAPOBJECT.address` フィールドは、デバッグ API の多くで使用される ICorDebugValue インターフェイス値ではなく `CORDB_ADDRESS` 値です。 指定されたオブジェクトアドレスの ICorDebugValue オブジェクトを取得するには、`CORDB_ADDRESS` 値を[ICorDebugProcess5:: GetObject](icordebugprocess5-getobject-method.md)メソッドに渡すことができます。  
  
 パフォーマンスを向上させるために、`COR_HEAPOBJECT.type` フィールドは、デバッグ API の多くで使用されているのではなく、`COR_TYPEID` 値です。 指定された型 ID の ICorDebugProcess5 型オブジェクトを取得するには、その値を[:: GetTypeForTypeID](icordebugprocess5-gettypefortypeid-method.md)メソッドに `COR_TYPEID` 渡すことができます。  
  
 `COR_HEAPOBJECT` 構造体には、参照カウントの COM インターフェイスが含まれています。 [次](icordebugheapenum-next-method.md)のメソッドを呼び出して列挙子から `COR_HEAPOBJECT` インスタンスを取得する場合は、その後で参照を解放する必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
