---
title: CoEEShutDownCOM 関数
ms.date: 03/30/2017
api_name:
- CoEEShutDownCOM
api_location:
- mscoree.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
- mscorsvr.dll
api_type:
- DLLExport
f1_keywords:
- CoEEShutDownCOM
helpviewer_keywords:
- CoEEShutDownCOM function [.NET Framework hosting]
ms.assetid: b634cae2-632f-4737-9be4-92d0652844d7
topic_type:
- apiref
ms.openlocfilehash: 4e85a9a98bf0550fa906f8b905c73890948f4ac1
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124938"
---
# <a name="coeeshutdowncom-function"></a>CoEEShutDownCOM 関数
共通言語ランタイム (CLR) に対して、ランタイム呼び出し可能ラッパー (RCW) 内に保持されているすべてのインターフェイスポインターを強制的に解放します。 これにより、すべての RCW キャッシュが解放されます。 このグローバル関数は .NET Framework 4 では非推奨とされます。 代わりに、特定のランタイムのエントリポイントを使用します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
void CoEEShutDownCOM ();  
```  
  
## <a name="remarks"></a>Remarks  
 `CoEEShutDownCOM` 関数は、まずすべてのコンテキストのすべての Rcw とすべてのキャッシュを解放し、次にセットアップに存在するすべての破棄通知を削除します。 DLL のアンロードは行われません。  
  
> [!CAUTION]
> この関数は、プロセスに読み込まれるすべてのランタイムに影響します。  
  
 .NET Framework 4 から、影響を与える特定のランタイムに対して、この関数のエントリポイントを呼び出します。 エントリポイントを取得するには、 [ICLRRuntimeInfo:: GetProcAddress](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-getprocaddress-method.md)メソッドを呼び出し、"Coees Tdowncom" を指定します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ グローバル静的関数](../../../../docs/framework/unmanaged-api/metadata/metadata-global-static-functions.md)
