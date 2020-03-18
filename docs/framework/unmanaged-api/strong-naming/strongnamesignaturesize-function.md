---
title: StrongNameSignatureSize 関数
ms.date: 03/30/2017
api_name:
- StrongNameSignatureSize
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameSignatureSize
helpviewer_keywords:
- StrongNameSignatureSize function [.NET Framework strong naming]
ms.assetid: 4fde4cd0-f53e-4411-a2fe-fc5c54472f95
topic_type:
- apiref
ms.openlocfilehash: a8856790b655f071df704879a247169f456ae2f5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130871"
---
# <a name="strongnamesignaturesize-function"></a>StrongNameSignatureSize 関数
厳密な名前の署名のサイズが返されます。 `StrongNameSignatureSize` は、通常、遅延署名されたアセンブリを作成するときに、ファイルに予約する容量を決定するためにコンパイラによって使用されます。  
  
 この関数は非推奨とされます。 代わりに[ICLRStrongName:: StrongNameSignatureSize](../hosting/iclrstrongname-strongnamesignaturesize-method.md)メソッドを使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
BOOLEAN StrongNameSignatureSize (   
    [in]  BYTE   *pbPublicKeyBlob,  
    [in]  ULONG  cbPublicKeyBlob,   
    [in]  DWORD  *pcbSize  
);   
```  
  
## <a name="parameters"></a>パラメーター  
 `pbPublicKeyBlob`  
 から厳密な名前の署名を生成するために使用されるキーペアの公開部分を格納する[Publickeyblob](publickeyblob-structure.md)型の構造体。  
  
 `cbPublicKeyBlob`  
 から`pbPublicKeyBlob`のサイズ (バイト単位)。  
  
 `pcbSize`  
 から厳密な名前の署名を格納するために必要なバイト数。  
  
## <a name="return-value"></a>戻り値  
 正常に完了した場合は `true`。それ以外の場合は、`false`ます。  
  
## <a name="remarks"></a>Remarks  
 `StrongNameSignatureSize` 関数が正常に完了しない場合は、 [StrongNameErrorInfo](strongnameerrorinfo-function.md)関数を呼び出して、最後に生成されたエラーを取得します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** StrongName  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [StrongNameSignatureSize メソッド](../hosting/iclrstrongname-strongnamesignaturesize-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
