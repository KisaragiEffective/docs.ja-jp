---
title: EmitManifest メソッド
ms.date: 03/30/2017
api_name:
- EmitManifest
- IALink.EmitManifest
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EmitManifest
helpviewer_keywords:
- EmitManifest method
ms.assetid: fdebc1f3-b62e-4d9e-b775-8ccaa8ecb250
topic_type:
- apiref
ms.openlocfilehash: f3bb978b8358992fd9aa7da922e28efc1ed1a951
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446483"
---
# <a name="emitmanifest-method"></a>EmitManifest メソッド
最終的なマニフェストを出力します。 他のすべてのファイルをインポートし、すべてのオプションを設定した後に、このメソッドを呼び出します。 バインドされていないモジュールに対しては、このメソッドを呼び出さないでください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EmitManifest(  
    mdAssembly   AssemblyID,  
    DWORD*       pdwReserveSize,  
    mdAssembly*  ptkManifest  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 アセンブリの ID。  
  
 `pdwReserveSize`  
 [StrongNameSignatureSize 関数](../strong-naming/strongnamesignaturesize-function.md)から取得した、アセンブリファイルで予約するサイズを受け取ります。  
  
 `ptkManifest`  
 必要に応じて、アセンブリマニフェストトークンを受け取ります。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>関連項目

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
