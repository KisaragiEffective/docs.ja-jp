---
title: ICorDebugArrayValue インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugArrayValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugArrayValue interface
helpviewer_keywords:
- ICorDebugArrayValue interface [.NET Framework debugging]
ms.assetid: dc437751-7093-44e2-bfdc-191d9ce3c192
topic_type:
- apiref
ms.openlocfilehash: 0944f3379c18cba56ab65fe40a5b94a64d8a8991
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778196"
---
# <a name="icordebugarrayvalue-interface"></a>ICorDebugArrayValue インターフェイス

1次元配列または多次元配列を表す、値のサブクラス。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetBaseIndicies メソッド](icordebugarrayvalue-getbaseindicies-method.md)|配列内の各次元のベースインデックスを取得します。|  
|[GetCount メソッド](icordebugarrayvalue-getcount-method.md)|配列内の要素の合計数を取得します。|  
|[GetDimensions メソッド](icordebugarrayvalue-getdimensions-method.md)|配列の次元を取得します。|  
|[GetElement メソッド](icordebugarrayvalue-getelement-method.md)|配列内の指定された要素を表す値を取得します。|  
|[GetElementAtPosition メソッド](icordebugarrayvalue-getelementatposition-method.md)|配列を0から始まる1次元配列として扱い、指定された位置にある要素を取得します。|  
|[GetElementType メソッド](icordebugarrayvalue-getelementtype-method.md)|配列内の要素の単純型を取得します。|  
|[GetRank メソッド](icordebugarrayvalue-getrank-method.md)|配列のディメンションの数を取得します。|  
|[HasBaseIndicies メソッド](icordebugarrayvalue-hasbaseindicies-method.md)|配列にベースインデックスがあるかどうかを判断します。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugArrayValue` は、1次元配列と多次元配列の両方をサポートします。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
