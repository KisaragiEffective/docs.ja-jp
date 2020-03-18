---
title: IXCLRDataModule インターフェイス
ms.date: 01/16/2019
api.name:
- IXCLRDataModule Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataModule Interface
helpviewer.keywords:
- IXCLRDataModule Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 8757642db6c4375cf55d1f7288669c4c8a752a38
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790405"
---
# <a name="ixclrdatamodule-interface"></a>IXCLRDataModule インターフェイス

読み込まれたモジュールに関する情報を照会するためのメソッドを提供します。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>メソッド

| メソッド                                                                                                                                | 説明                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [GetMethodDefinitionByToken](ixclrdatamodule-getmethoddefinitionbytoken-method.md) | 指定されたメタデータトークンに対応するメソッド定義を取得します。 |
| [要求](ixclrdatamodule-request-method.md)                                       | モジュールのデータで指定されたバッファーへの読み込みを要求します。       |
| [GetVersionId](ixclrdatamodule-getversionid-method.md)                             | モジュールのバージョン ID を取得します。                                       |

## <a name="remarks"></a>コメント

このインターフェイスはランタイム内に存在し、ヘッダーまたはライブラリファイルを介して公開されることはありません。 ただし、これは、通常の COM 機構を通じて取得できる GUID `88E32849-0A0A-4cb0-9022-7CD2E9E139E2` を `IUnknown` から派生する COM インターフェイスです。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
**ヘッダー:** 存在  
**ライブラリ:** 存在  
**.NET Framework のバージョン:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>関連項目

- [デバッグ](index.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
