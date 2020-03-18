---
title: IsFrameworkAssembly 関数
ms.date: 03/30/2017
api_name:
- IsFrameworkAssembly
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IsFrameworkAssembly
helpviewer_keywords:
- IsFrameworkAssembly function [.NET Framework fusion]
ms.assetid: b0c6f19b-d4fd-4971-88f0-12ffb5793da3
topic_type:
- apiref
ms.openlocfilehash: e30b6f2d2254d2d107c4c82a2c5664850ce6ec23
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123068"
---
# <a name="isframeworkassembly-function"></a>IsFrameworkAssembly 関数
指定したアセンブリが管理されているかどうかを示す値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT IsFrameworkAssembly (  
    [in]  LPCWSTR pwzAssemblyReference,  
    [out] LPBOOL  pbIsFrameworkAssembly,  
    [in]  LPWSTR  pwzFrameworkAssemblyIdentity,  
    [in]  LPDWORD pccSize  
 );  
```  
  
## <a name="parameters"></a>パラメーター  
 `pwzAssemblyReference`  
 から確認するアセンブリの名前。  
  
 `pbIsFrameworkAssembly`  
 入出力アセンブリが管理されているかどうかを示すブール値。  
  
 `pwzFrameworkAssemblyIdentity`  
 からアセンブリの一意の id を含む、正規化されていない文字列。  
  
 `pccSize`  
 [入力] `pwzFrameworkAssemblyIdentity` のサイズ。  
  
## <a name="remarks"></a>Remarks  
 `pwzAssemblyReference` パラメーターは、アセンブリの名前を含む文字列へのポインターです。  
  
 このアセンブリが .NET Framework の一部である場合、`pbIsFrameworkAssembly` パラメーターには `true`のブール値が格納されます。  
  
 名前付きアセンブリが .NET Framework の一部でない場合、または `pwzAssemblyReference` パラメーターがアセンブリの名前を指定しない場合、`pbIsFrameworkAssembly` には `false`のブール値が格納されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [Fusion グローバル静的関数](fusion-global-static-functions.md)
