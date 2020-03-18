---
title: ICorDebugFunction インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugFunction
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFunction
helpviewer_keywords:
- ICorDebugFunction interface [.NET Framework debugging]
ms.assetid: 783faea9-8083-41c1-b04a-51a81ac4c8f3
topic_type:
- apiref
ms.openlocfilehash: ba0e0b1b2bac785e28f41e09dda74841121a748d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794510"
---
# <a name="icordebugfunction-interface"></a>ICorDebugFunction インターフェイス

マネージド関数またはマネージド メソッドを表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CreateBreakpoint メソッド](icordebugfunction-createbreakpoint-method.md)|この関数の先頭にブレークポイントを作成します。|  
|[GetClass メソッド](icordebugfunction-getclass-method.md)|この関数がメンバーとなっているクラスを表す、のオブジェクトを取得します。|  
|[GetCurrentVersionNumber メソッド](icordebugfunction-getcurrentversionnumber-method.md)|この関数に対して行われた最新の編集のバージョン番号を取得します。|  
|[GetILCode メソッド](icordebugfunction-getilcode-method.md)|この関数の MSIL (Microsoft 中間言語) コードを取得します。|  
|[GetLocalVarSigToken メソッド](icordebugfunction-getlocalvarsigtoken-method.md)|この `ICorDebugFunction` インスタンスによって表される関数のローカル変数シグネチャのメタデータトークンを取得します。|  
|[GetModule メソッド](icordebugfunction-getmodule-method.md)|この関数が定義されているモジュールを取得します。|  
|[GetNativeCode メソッド](icordebugfunction-getnativecode-method.md)|この関数のネイティブコードを取得します。|  
|[GetToken メソッド](icordebugfunction-gettoken-method.md)|この関数のメタデータトークンを取得します。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugFunction` インターフェイスは、ジェネリック型パラメーターを持つ関数を表していません。 たとえば、`ICorDebugFunction` インスタンスは `Func<T>` を表しますが `Func<string>`は表しません。 [ICorDebugILFrame2:: EnumerateTypeParameters](icordebugilframe2-enumeratetypeparameters-method.md)を呼び出して、ジェネリック型パラメーターを取得します。  
  
 メソッドのメタデータトークン、`mdMethodDef`、およびメソッドの `ICorDebugFunction` オブジェクト間のリレーションシップは、関数でエディットコンティニュが許可されているかどうかによって異なります。  
  
- 関数でエディットコンティニュが許可されていない場合、`ICorDebugFunction` オブジェクトと `mdMethodDef` トークンの間には一対一のリレーションシップが存在します。 つまり、関数は、1つの `ICorDebugFunction` オブジェクトと1つの `mdMethodDef` トークンを持ちます。  
  
- 関数でエディットコンティニュが許可されている場合、`ICorDebugFunction` オブジェクトと `mdMethodDef` トークンの間に多対一のリレーションシップが存在します。 つまり、関数は、関数の各バージョンに対して1つずつ `ICorDebugFunction`のインスタンスを多数持つことができますが、`mdMethodDef` トークンは1つだけです。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids .lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
