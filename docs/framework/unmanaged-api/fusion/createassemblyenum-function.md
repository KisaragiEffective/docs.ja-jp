---
title: CreateAssemblyEnum 関数
ms.date: 03/30/2017
api_name:
- CreateAssemblyEnum
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- DLLExport
f1_keywords:
- CreateAssemblyEnum
helpviewer_keywords:
- CreateAssemblyEnum function [.NET Framework fusion]
ms.assetid: 3506df38-6cea-42f6-946e-4287863bcfb3
topic_type:
- apiref
ms.openlocfilehash: 0e54027806cef07fad4740c3bf5226fd26c72570
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73108774"
---
# <a name="createassemblyenum-function"></a>CreateAssemblyEnum 関数
指定された[IAssemblyName](iassemblyname-interface.md)を持つアセンブリ内のオブジェクトを列挙できる[iassemblyenum](iassemblyenum-interface.md)インスタンスへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateAssemblyEnum (  
    [out] IAssemblyEnum  **pEnum,  
    [in]  IUnknown       *pUnkReserved,  
    [in]  IAssemblyName  *pName,  
    [in]  DWORD          dwFlags,  
    [in]  LPVOID         pvReserved  
 );  
```  
  
## <a name="parameters"></a>パラメーター  
 `pEnum`  
 入出力要求された `IAssemblyEnum` ポインターを格納しているメモリ位置へのポインター。  
  
 `pUnkReserved`  
 [入力] 将来の機能拡張に備えて予約されています。 `pUnkReserved` は null 参照である必要があります。  
  
 `pName`  
 から要求されたアセンブリの `IAssemblyName`。 この名前は、列挙をフィルター処理するために使用されます。 グローバルアセンブリキャッシュ内のすべてのアセンブリを列挙するには null を指定できます。  
  
 `dwFlags`  
 から列挙子の動作を変更するためのフラグ。 このパラメーターには、 [ASM_CACHE_FLAGS](asm-cache-flags-enumeration.md)列挙体とのビットが1つだけ含まれます。  
  
 `pvReserved`  
 [入力] 将来の機能拡張に備えて予約されています。 `pvReserved` は null 参照である必要があります。  
  
## <a name="remarks"></a>Remarks  
 `dwFlags` パラメーターには、`ASM_CACHE_FLAGS` 列挙体とは1ビットだけが含まれます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IAssemblyEnum インターフェイス](iassemblyenum-interface.md)
- [IAssemblyName インターフェイス](iassemblyname-interface.md)
- [Fusion グローバル静的関数](fusion-global-static-functions.md)
