---
description: '詳細情報: GetAssemblyIdentityFromFile 関数'
title: GetAssemblyIdentityFromFile 関数
ms.date: 03/30/2017
api_name:
- GetAssemblyIdentityFromFile
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- COM
f1_keywords:
- GetAssemblyIdentityFromFile
helpviewer_keywords:
- GetAssemblyIdentityFromFile function [.NET Framework fusion]
ms.assetid: 2c32da53-76c7-4048-84d0-d05207333004
topic_type:
- apiref
ms.openlocfilehash: 8832e03182a5652c938404c99115fa8059439d1d
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99761037"
---
# <a name="getassemblyidentityfromfile-function"></a>GetAssemblyIdentityFromFile 関数

指定した `IUnknown` `IID` ファイルパスのアセンブリ内で、指定したを持つオブジェクトへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetAssemblyIdentityFromFile (  
    [in]  LPCWSTR   pwzFilePath,  
    [in]  REFIID    riid,  
    [out] IUnknown  **ppIdentity  
 );  
```  
  
## <a name="parameters"></a>パラメーター  

 `pwzFilePath`  
 から要求されたアセンブリへの有効なパス。  
  
 `riid`  
 から `IID` 返されるインターフェイスの。  
  
 `ppIdentity`  
 入出力返されたインターフェイスポインター。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IUnknown](/cpp/atl/iunknown)
- [Fusion グローバル静的関数](fusion-global-static-functions.md)
