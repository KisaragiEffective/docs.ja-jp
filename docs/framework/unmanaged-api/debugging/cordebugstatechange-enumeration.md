---
title: CorDebugStateChange 列挙体
ms.date: 02/07/2019
api_name:
- CorDebugStateChange
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: 1d4424ab-5143-4e50-a84a-ceeb4ddf3bba
topic_type:
- apiref
ms.openlocfilehash: 239e3a82df0e6010278669f9f429bfad0d163319
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133717"
---
# <a name="cordebugstatechange-enumeration"></a>CorDebugStateChange 列挙体

プロセスへの変更に基づいて破棄が必要となった、キャッシュされたデータの量を示します。

## <a name="syntax"></a>構文

```cpp
typedef enum CorDebugStateChange
{
    PROCESS_RUNNING = 0x0000001,
    FLUSH_ALL       = 0x0000002,
} CorDebugStateChange;
```

## <a name="members"></a>メンバー

| メンバー            | 説明                                                              |
| ----------------- | ------------------------------------------------------------------------ |
| `PROCESS_RUNNING` | プロセスはフォワード実行によって新しいメモリ状態に達しています。            |
| `FLUSH_ALL`       | プロセスのメモリは、以前とは異なる状態になっている場合があります。 |

## <a name="remarks"></a>Remarks

 `CorDebugStateChange` 列挙体のメンバーは、デバッガーが[ICorDebugProcess4::P rocessstatechanged](icordebugprocess4-processstatechanged-method.md)または[ICorDebugProcess6::P rocessstatechanged](icordebugprocess6-processstatechanged-method.md)で `ProcessStateChanged` メソッドを呼び出すと、引数として指定されます。

## <a name="requirements"></a>［要件］

 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

 **ヘッダー:** CorDebug.idl、CorDebug.h

 **ライブラリ:** CorGuids.lib

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
- [デバッグ](index.md)
