---
title: ICorDebugProcess5 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5
helpviewer_keywords:
- ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: 30a39d79-1f10-4328-9c5d-094ed824e2ba
topic_type:
- apiref
ms.openlocfilehash: 263124db75abdc058d26ffb606a13fc711aed8bf
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792300"
---
# <a name="icordebugprocess5-interface"></a>ICorDebugProcess5 インターフェイス
は、アプリケーションインターフェイスを拡張して、マネージヒープへのアクセスをサポートし、マネージオブジェクトのガベージコレクションに関する情報を提供し、デバッガーがアプリケーションのローカルネイティブイメージキャッシュからイメージを読み込むかどうかを判断します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnableNGENPolicy メソッド](icordebugprocess5-enablengenpolicy-method.md)|マネージデバッガーで実行中にアプリケーションがネイティブイメージを読み込む方法を決定する値を設定します。|  
|[EnumerateGCReferences メソッド](icordebugprocess5-enumerategcreferences-method.md)|プロセスでガベージコレクトされるすべてのオブジェクトの列挙子を取得します。|  
|[EnumerateHandles メソッド](icordebugprocess5-enumeratehandles-method.md)|プロセス内のオブジェクトハンドルの列挙子を取得します。|  
|[EnumerateHeap メソッド](icordebugprocess5-enumerateheap-method.md)|マネージヒープ上のオブジェクトの列挙子を取得します。|  
|[EnumerateHeapRegions メソッド](icordebugprocess5-enumerateheapregions-method.md)|マネージヒープの領域の列挙子を取得します。|  
|[GetArrayLayout メソッド](icordebugprocess5-getarraylayout-method.md)|メモリ内の配列のレイアウトに関する情報を取得します。|  
|[GetGCHeapInformation メソッド](icordebugprocess5-getgcheapinformation-method.md)|マネージヒープでガベージコレクトされるオブジェクトに関する情報を格納している[COR_HEAPINFO](cor-heapinfo-structure.md)構造体へのポインターを取得します。|  
|[GetObject メソッド](icordebugprocess5-getobject-method.md)|マネージヒープ上のオブジェクトへのポインターを取得します。|  
|[GetTypeFields メソッド](icordebugprocess5-gettypefields-method.md)|型の識別子に基づいて、型のフィールド情報を格納している配列へのポインターを取得します。|  
|[GetTypeForTypeID メソッド](icordebugprocess5-gettypefortypeid-method.md)|型識別子に基づいてオブジェクトに関する情報を提供する型オブジェクトを取得します。|  
|[GetTypeID メソッド](icordebugprocess5-gettypeid-method.md)|指定したアドレスにあるオブジェクトの型識別子を取得します。|  
|[GetTypeLayout メソッド](icordebugprocess5-gettypelayout-method.md)|型識別子に基づいて、メモリ内のオブジェクトのレイアウトに関する情報を取得します。|  
  
## <a name="remarks"></a>コメント  
 このインターフェイスは、ICorDebugProcess、ICorDebugProcess2、および [ICorDebugProcess3](icordebugprocess3-interface.md) の各インターフェイスを論理的に拡張します。  
  
> [!NOTE]
> このインターフェイスは、別のコンピューターまたは別のプロセスからのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
