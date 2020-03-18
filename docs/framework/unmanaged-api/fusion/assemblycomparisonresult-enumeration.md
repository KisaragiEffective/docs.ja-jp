---
title: AssemblyComparisonResult 列挙型
ms.date: 03/30/2017
api_name:
- AssemblyComparisonResult
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- AssemblyComparisonResult
helpviewer_keywords:
- AssemblyComparisonResult enumeration [.NET Framework fusion]
ms.assetid: bd042f89-10b1-40ca-946e-46da082f5263
topic_type:
- apiref
ms.openlocfilehash: 3d3fd88a2c1ac90f823b23d8d2bcb5b177a625c3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73109011"
---
# <a name="assemblycomparisonresult-enumeration"></a>AssemblyComparisonResult 列挙型
[CompareAssemblyIdentity](compareassemblyidentity-function.md)関数によって決定される2つのアセンブリ id の等価性を示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum _tagAssemblyComparisonResult {  
    ACR_Unknown,   
    ACR_EquivalentFullMatch,  
    ACR_EquivalentWeakNamed,  
    ACR_EquivalentFXUnified,  
    ACR_EquivalentUnified,    
    ACR_NonEquivalentVersion,  
    ACR_NonEquivalent,      
    ACR_EquivalentPartialMatch,  
    ACR_EquivalentPartialWeakNamed,    
    ACR_EquivalentPartialUnified,  
    ACR_EquivalentPartialFXUnified,  
    ACR_NonEquivalentPartialVersion    
} AssemblyComparisonResult;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー名|説明|  
|-----------------|-----------------|  
|`ACR_EquivalentFullMatch`|比較対象のすべてのアセンブリフィールドが一致することを示します。|  
|`ACR_EquivalentFXUnified`|アセンブリが同等と見なされることを示します。これは、.NET Framework バージョン2.0 でのアセンブリバージョン番号の共通言語ランタイムバージョン (CLR) の統合に基づいています。|  
|`ACR_EquivalentPartialFXUnified`|.NET Framework 2.0 のアセンブリバージョン番号と CLR の統合に基づいて、アセンブリの部分的な一致を示します。|  
|`ACR_EquivalentPartialMatch`|アセンブリの部分的な一致を示します。|  
|`ACR_EquivalentPartialUnified`|バージョン番号の従来の統合に基づいて、アセンブリの部分的な一致を示します。|  
|`ACR_EquivalentPartialWeakNamed`|単純に名前付きアセンブリの部分一致を示します。|  
|`ACR_EquivalentUnified`|.NET Framework のレガシバージョンでの CLR のバージョン番号の統合に基づいて、アセンブリが同等と見なされることを示します。|  
|`ACR_EquivalentWeakNamed`|バージョン番号が無視された、単純に名前が付けられた2つのアセンブリ間の一致を示します。|  
|`ACR_NonEquivalent`|2つのアセンブリ間で一致するものがないことを示します。|  
|`ACR_NonEquivalentPartialVersion`|2つのアセンブリが、部分的にのみ一致するバージョン番号を除き、一致することを示します。|  
|`ACR_NonEquivalentVersion`|2つのアセンブリが一致しないバージョン番号を除き、一致することを示します。|  
|`ACR_Unknown`|非等価性の理由が不明であることを示します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [CompareAssemblyIdentity 関数](compareassemblyidentity-function.md)
- [Fusion 列挙型](fusion-enumerations.md)
