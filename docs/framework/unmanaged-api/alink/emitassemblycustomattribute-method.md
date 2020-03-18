---
title: EmitAssemblyCustomAttribute メソッド
ms.date: 03/30/2017
api_name:
- IALink.EmitAssemblyCustomAttribute
- EmitAssemblyCustomAttribute
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EmitAssemblyCustomAttribute
helpviewer_keywords:
- EmitAssemblyCustomAttribute method
ms.assetid: b72f5409-79af-4fa7-90a7-7630eec170f1
topic_type:
- apiref
ms.openlocfilehash: ec0a86e3396ad42152bc0a244f74ad13deba16e4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446506"
---
# <a name="emitassemblycustomattribute-method"></a>EmitAssemblyCustomAttribute メソッド
を呼び出して、アセンブリレベルのカスタム属性を設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EmitAssemblyCustomAttribute(  
    mdAssembly   AssemblyID,  
    mdToken      FileToken,  
    mdToken      tkType,  
    void const*  pCustomValue,  
    DWORD        cbCustomValue,  
    BOOL         bSecurity,  
    BOOL         bAllowMulti  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 アセンブリの ID。  
  
 `FileToken`  
 属性をなするファイル。 `AssemblyID` がバインドされていない .netmodule を示していない場合は NULL を指定できます。  
  
 `tkType`  
 カスタム属性の型。  
  
 `pCustomValue`  
 カスタム値データ。  
  
 `cbCustomValue`  
 カスタム値データの長さ。  
  
 `bSecurity`  
 カスタム属性がアセンブリ署名に関連付けられている場合は TRUE。  
  
 `bAllowMulti`  
 複数の属性を出力する場合は TRUE。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>関連項目

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
