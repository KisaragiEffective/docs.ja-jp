---
title: IXCLRDataMethodDefinition インターフェイス
ms.date: 01/16/2019
api.name:
- IXCLRDataMethodDefinition Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodDefinition Interface
helpviewer.keywords:
- IXCLRDataMethodDefinition Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 708c681a98113a406249a360c2fc81087e5b97f8
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790419"
---
# <a name="ixclrdatamethoddefinition-interface"></a>IXCLRDataMethodDefinition インターフェイス

メソッド定義に関する情報を照会するためのメソッドを提供します。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>メソッド

次のメソッドは、インターフェイスで使用できるメソッドの一部です。

| メソッド                                                                                                                          | 説明                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| [StartEnumInstances](ixclrdatamethoddefinition-startenuminstances-method.md) | 指定された `IXCLRDataAppDomain`のメソッドインスタンスの列挙体のハンドルを提供します。 |
| [EnumInstance](ixclrdatamethoddefinition-enuminstance-method.md)             | このメソッド定義のインスタンスを列挙します。                                         |
| [EndEnumInstances](ixclrdatamethoddefinition-endenuminstances-method.md)     | インスタンスの列挙中に使用される内部反復子によって使用されるリソースを解放します。         |

## <a name="remarks"></a>コメント

このインターフェイスはランタイム内に存在し、ヘッダーまたはライブラリファイルを介して公開されることはありません。 ただし、これは、通常の COM 機構を通じて取得できる GUID `AAF60008-FB2C-420b-8FB1-42D244A54A97` を `IUnknown` から派生する COM インターフェイスです。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
**ヘッダー:** 存在  
**ライブラリ:** 存在  
**.NET Framework のバージョン:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>関連項目

- [デバッグ](index.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
