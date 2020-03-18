---
title: ICorDebugClass インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugClass
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass
helpviewer_keywords:
- ICorDebugClass interface [.NET Framework debugging]
ms.assetid: 03a6facb-f12f-49be-9839-e73b9c791cd5
topic_type:
- apiref
ms.openlocfilehash: 7ac588591222a1abbc7b99ec7e973284c055f95e
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76784166"
---
# <a name="icordebugclass-interface"></a>ICorDebugClass インターフェイス

基本型または複合型 (つまり、ユーザー定義) のいずれかの型を表します。 型がジェネリックの場合、`ICorDebugClass` はインスタンス化されないジェネリック型を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetModule メソッド](icordebugclass-getmodule-method.md)|このクラスを定義するモジュールを取得します。|  
|[GetStaticFieldValue メソッド](icordebugclass-getstaticfieldvalue-method.md)|指定された静的フィールドの値を取得します。|  
|[GetToken メソッド](icordebugclass-gettoken-method.md)|このクラスの `TypeDef` メタデータトークンを取得します。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugClass` インターフェイスは、インスタンスジェネリック型を表します。 は、インスタンス化されたジェネリック型を表します。 たとえば、`Hashtable<K, V>` は `ICorDebugClass`によって表されるのに対し、`Hashtable<Int32, String>` は `ICorDebugType`で表されます。  
  
 非ジェネリック型は、`ICorDebugClass` と `ICorDebugType`の両方で表されます。 後者のインターフェイスは、型のインスタンス化を処理するために .NET Framework バージョン2.0 で導入されました。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
