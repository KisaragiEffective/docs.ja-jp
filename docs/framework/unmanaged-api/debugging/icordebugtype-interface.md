---
title: ICorDebugType インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType
helpviewer_keywords:
- ICorDebugType interface [.NET Framework debugging]
ms.assetid: 94e02e31-67ea-4b00-8148-a46740a4571d
topic_type:
- apiref
ms.openlocfilehash: 8210e67f4bdd615ff65d9bd3474043fc45fd8883
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791258"
---
# <a name="icordebugtype-interface"></a>ICorDebugType インターフェイス
基本または複合 (つまり、ユーザー定義) のいずれかの型を表します。 型がジェネリックの場合、`ICorDebugType` はインスタンス化されたジェネリック型を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnumerateTypeParameters メソッド](icordebugtype-enumeratetypeparameters-method.md)|この `ICorDebugType`によって参照されるクラスのジェネリック <xref:System.Type> パラメーターを参照する、ツールのインターフェイスポインターを取得します。|  
|[GetBase メソッド](icordebugtype-getbase-method.md)|この `ICorDebugType`によって参照されるクラスの基底クラスが存在する場合は、その基底クラスを参照する `ICorDebugType` へのインターフェイスポインターを取得します。|  
|[GetClass メソッド](icordebugtype-getclass-method.md)|この `ICorDebugType`の型指定されたコンストラクターを参照する、のクラスへのインターフェイスポインターを取得します。|  
|[GetFirstTypeParameter メソッド](icordebugtype-getfirsttypeparameter-method.md)|この `ICorDebugType`によって参照されるクラスのコンストラクターの最初のジェネリック <xref:System.Type> パラメーターを参照する `ICorDebugType` へのインターフェイスポインターを取得します。|  
|[GetRank メソッド](icordebugtype-getrank-method.md)|配列型の次元数を取得します。|  
|[GetStaticFieldValue メソッド](icordebugtype-getstaticfieldvalue-method.md)|指定したスタックフレーム内の指定したフィールドトークンによって参照される静的フィールドの値を格納する、ICorDebugValue へのインターフェイスポインターを取得します。|  
|[GetType メソッド](icordebugtype-gettype-method.md)|この `ICorDebugType`によって参照される共通言語ランタイム <xref:System.Type> のネイティブ型を記述する CorElementType 値を取得します。|  
  
## <a name="remarks"></a>コメント  
 型がジェネリックの場合、`ICorDebugClass` はインスタンス型を表します。 `ICorDebugType` インターフェイスは、インスタンス化されたジェネリック型を表します。 たとえば、Hashtable\<K、V > は `ICorDebugClass`によって表されますが、Hashtable\<Int32、String > は `ICorDebugType`によって表されます。  
  
 非ジェネリック型は、`ICorDebugClass` と `ICorDebugType`の両方で表されます。 後者のインターフェイスは、型のインスタンス化を処理するために .NET Framework バージョン2.0 で導入されました。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
