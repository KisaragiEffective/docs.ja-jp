---
title: DacpMethodDescData 構造体
ms.date: 02/01/2019
api.name:
- DacpMethodDescData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpMethodDescData Structure
helpviewer.keywords:
- DacpMethodDescData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: cc54664ea8ad61005de3f3fae7407946d1c861b2
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793842"
---
# <a name="dacpmethoddescdata-structure"></a>DacpMethodDescData 構造体

メソッドのランタイム情報のトランスポートバッファーを定義します。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>構文

```cpp
struct DacpMethodDescData
{
    int             bHasNativeCode;
    int             bIsDynamic;
    unsigned short  wSlotNumber;
    CLRDATA_ADDRESS NativeCodeAddr;
    CLRDATA_ADDRESS data;
    CLRDATA_ADDRESS MethodDescPtr;
    CLRDATA_ADDRESS nativeCodeInfo;
    CLRDATA_ADDRESS moduleInfo;
    mdToken         MDToken;
    CLRDATA_ADDRESS payloadGC;
    CLRDATA_ADDRESS payloadGC2;
    CLRDATA_ADDRESS managedDynamicMethodObject;
    CLRDATA_ADDRESS requestedIP;
    DacpReJitData   rejitDataCurrent;
    DacpReJitData   rejitDataRequested;
    unsigned long   cJittedRejitVersions;
};
```

## <a name="members"></a>メンバー

| メンバー                       | 説明                                                                                     |
| ---------------------------- | ----------------------------------------------------------------------------------------------- |
| `bHasNativeCode`             | メソッドの特定のインスタンス化に対してランタイムにネイティブコードがあるかどうかを示します。 |
| `bIsDynamic`                 | 軽量コード生成によってメソッドが動的に生成されるかどうかを示します。           |
| `wSlotNumber`                | メソッドテーブル内のメソッドのスロット番号。                                                   |
| `NativeCodeAddr`             | メソッドの初期ネイティブアドレス。                                                            |
| `data`                       | ランタイムによって内部的に使用されるバッファーへのポインター。                                             |
| `MethodDescPtr`              | ランタイム内の `MethodDesc` へのポインター。                                                     |
| `nativeCodeInfo`             | メソッドを追跡するためにランタイムによって内部的に使用されるバッファーへのポインター。                            |
| `moduleInfo`                 | モジュール情報のランタイムによって内部的に使用されるバッファーへのポインター。                      |
| `MDToken`                    | 指定したメソッドに関連付けられているトークン。                                                         |
| `payloadGC`                  | ランタイムによって内部的に使用されるガベージコレクションバッファーへのポインター。                          |
| `payloadGC2`                 | ランタイムによって内部的に使用されるガベージコレクションバッファーへのポインター。                          |
| `managedDynamicMethodObject` | メソッドが動的である場合、ランタイムは情報追跡のためにこのバッファーを内部的に使用します。     |
| `requestedIP`                | ネイティブコードアドレスが指定された場合に、要求ごとに構造体を設定するために使用されます。                    |
| `rejitDataCurrent`           | メソッドのインストルメント化された最新バージョンに関する情報。                                   |
| `rejitDataRequested`         | 要求されたネイティブアドレスの rejit 情報。                                             |
| `cJittedRejitVersions`       | メソッドがインストルメンテーションによって rejitted された回数。                           |

## <a name="remarks"></a>コメント

この構造体はランタイム内に存在し、ヘッダーまたはライブラリファイルを介して公開されることはありません。 これを使用するには、前に示したように構造体を定義します。

## <a name="requirements"></a>要件
**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
**ヘッダー:** 存在  
**ライブラリ:** 存在  
**.NET Framework のバージョン:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>関連項目

- [デバッグ](index.md)
- [デバッグ構造体](debugging-structures.md)
- [共有のデータ型](../../../../docs/framework/unmanaged-api/common-data-types-unmanaged-api-reference.md)
