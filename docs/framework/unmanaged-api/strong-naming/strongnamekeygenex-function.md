---
title: StrongNameKeyGenEx 関数
ms.date: 03/30/2017
api_name:
- StrongNameKeyGenEx
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyGenEx
helpviewer_keywords:
- StrongNameKeyGenEx function [.NET Framework strong naming]
ms.assetid: 36bd10b9-9857-45f3-8d3b-0da091d6169e
topic_type:
- apiref
ms.openlocfilehash: 63ddd90f3a8090853d10f03052915d10e1503ea6
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125210"
---
# <a name="strongnamekeygenex-function"></a>StrongNameKeyGenEx 関数
厳密な名前の使用のために、指定されたキーサイズを持つ新しい公開/秘密キーのペアを生成します。  
  
 この関数は非推奨とされます。 代わりに[ICLRStrongName:: StrongNameKeyGenEx](../hosting/iclrstrongname-strongnamekeygenex-method.md)メソッドを使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
BOOLEAN StrongNameKeyGenEx (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
    [in]  DWORD     dwKeySize,  
    [out] BYTE      **ppbKeyBlob,  
    [out] ULONG     *pcbKeyBlob  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `wszKeyContainer`  
 から要求されたキーコンテナー名。 `wszKeyContainer` は、空でない文字列であるか、または一時名を生成するために null である必要があります。  
  
 `dwFlags`  
 からキーを登録したままにするかどうかを指定します。 次の値がサポートされています。  
  
- 0x00000000-一時キーコンテナー名を生成するために `wszKeyContainer` が null の場合に使用します。  
  
- 0x00000001 (`SN_LEAVE_KEY`)-キーを登録したままにすることを指定します。  
  
 `dwKeySize`  
 から要求されたキーのサイズ (ビット単位)。  
  
 `ppbKeyBlob`  
 入出力返された公開/秘密キーのペア。  
  
 `pcbKeyBlob`  
 入出力`ppbKeyBlob`のサイズ (バイト単位)。  
  
## <a name="return-value"></a>戻り値  
 正常に完了した場合は `true`。それ以外の場合は、`false`ます。  
  
## <a name="remarks"></a>Remarks  
 .NET Framework バージョン1.0 および1.1 では、厳密な名前でアセンブリに署名するには、1024ビットの `dwKeySize` が必要です。バージョン2.0 では、2048ビットキーのサポートが追加されています。  
  
 キーが取得されたら、 [StrongNameFreeBuffer](strongnamefreebuffer-function.md)関数を呼び出して、割り当てられたメモリを解放する必要があります。  
  
 `StrongNameKeyGenEx` 関数が正常に完了しない場合は、 [StrongNameErrorInfo](strongnameerrorinfo-function.md)関数を呼び出して、最後に生成されたエラーを取得します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** StrongName  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [StrongNameKeyGenEx メソッド](../hosting/iclrstrongname-strongnamekeygenex-method.md)
- [StrongNameKeyGen メソッド](../hosting/iclrstrongname-strongnamekeygen-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
