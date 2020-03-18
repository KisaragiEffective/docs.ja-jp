---
title: IAssemblyCache インターフェイス
ms.date: 03/30/2017
api_name:
- IAssemblyCache
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCache
helpviewer_keywords:
- IAssemblyCache interface [.NET Framework fusion]
ms.assetid: 71ea170f-872d-4fc5-81b6-27da1dec9b19
topic_type:
- apiref
ms.openlocfilehash: 5ed0075da1429c70900750f3f28e8ce36a41fb28
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73134537"
---
# <a name="iassemblycache-interface"></a>IAssemblyCache インターフェイス
Fusion テクノロジによって使用されるグローバルアセンブリキャッシュを表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CreateAssemblyCacheItem メソッド](iassemblycache-createassemblycacheitem-method.md)|新しい[Iassemblycacheitem](iassemblycacheitem-interface.md)への参照を取得します。|  
|[CreateAssemblyScavenger メソッド](iassemblycache-createassemblyscavenger-method.md)|Fusion テクノロジによる内部使用のために予約されています。|  
|[InstallAssembly メソッド](iassemblycache-installassembly-method.md)|指定したアセンブリをグローバルアセンブリキャッシュにインストールします。|  
|[QueryAssemblyInfo メソッド](iassemblycache-queryassemblyinfo-method.md)|指定したアセンブリに関する要求されたデータを取得します。|  
|[UninstallAssembly メソッド](iassemblycache-uninstallassembly-method.md)|指定したアセンブリをグローバルアセンブリキャッシュからアンインストールします。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
- [グローバル アセンブリ キャッシュ](../../app-domains/gac.md)
