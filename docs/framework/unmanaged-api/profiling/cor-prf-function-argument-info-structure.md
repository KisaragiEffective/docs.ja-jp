---
title: COR_PRF_FUNCTION_ARGUMENT_INFO 構造体
ms.date: 03/30/2017
api_name:
- COR_PRF_FUNCTION_ARGUMENT_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO
helpviewer_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO structure [.NET Framework profiling]
ms.assetid: 07cf3bab-e193-4991-8205-3f41cf2d67b3
topic_type:
- apiref
ms.openlocfilehash: c92ee580caed9f1fb87a31b676747769ad31a0e2
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867253"
---
# <a name="cor_prf_function_argument_info-structure"></a>COR_PRF_FUNCTION_ARGUMENT_INFO 構造体
関数の引数を左から右方向で表します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _COR_PRF_FUNCTION_ARGUMENT_INFO {  
    ULONG numRanges;  
    ULONG totalArgumentSize;  
    COR_PRF_FUNCTION_ARGUMENT_RANGE ranges[1];  
} COR_PRF_FUNCTION_ARGUMENT_INFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`numRanges`|引数のブロックの数。 つまり、この値は、`ranges` 配列内の[COR_PRF_FUNCTION_ARGUMENT_RANGE](cor-prf-function-argument-range-structure.md)構造体の数です。|  
|`totalArgumentSize`|すべての引数の合計サイズ。 言い換えると、この値は引数の長さの合計になります。|  
|`ranges`|`COR_PRF_FUNCTION_ARGUMENT_RANGE` 構造体の配列。それぞれが関数の引数の1つのブロックを表します。|  
  
## <a name="remarks"></a>コメント  
 関数には、多くの引数を含めることができます。 これらの引数は、連続してメモリに格納されていない可能性があります。 1つの場所に3つの引数のブロック、別の場所に2つの引数のブロック、および別の場所にある1つの引数の最後のブロックがある場合があります。 これらの引数はすべて同じ関数に対して使用されます。これらは、さまざまな場所に格納されています。  
  
 `COR_PRF_FUNCTION_ARGUMENT_INFO` 構造体は、1つの関数のすべての引数を表します。 配列を使用して、関数の引数のすべてのブロックを参照します。 そのため、1つの関数に対して、1つの `COR_PRF_FUNCTION_ARGUMENT_INFO` 構造体を使用して、それぞれが1つ以上の関数引数を指し示す複数の `COR_PRF_FUNCTION_ARGUMENT_RANGE` 構造体を参照します。  
  
 レジスタに格納されている引数は、構造体を構築するためにメモリに書き込まれます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [構造体のプロファイリング](profiling-structures.md)
