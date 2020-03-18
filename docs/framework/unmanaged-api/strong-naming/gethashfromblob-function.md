---
title: GetHashFromBlob 関数
ms.date: 03/30/2017
api_name:
- GetHashFromBlob
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- GetHashFromBlob
helpviewer_keywords:
- GetHashFromBlob function [.NET Framework strong naming]
ms.assetid: b712d862-f2d0-4b55-87d4-65bbeadef982
topic_type:
- apiref
ms.openlocfilehash: d1027aea1d800bda1654b223fec992aa70efd4b7
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140714"
---
# <a name="gethashfromblob-function"></a>GetHashFromBlob 関数

指定したハッシュ アルゴリズムを使用して、指定したメモリ アドレスにあるアセンブリのハッシュが取得されます。

この関数は非推奨とされます。 代わりに[ICLRStrongName:: GetHashFromBlob](../hosting/iclrstrongname-gethashfromblob-method.md)メソッドを使用してください。

## <a name="syntax"></a>構文

```cpp
HRESULT GetHashFromBlob (
    [in]  BYTE    *pbBlob,
    [in]  DWORD   cchBlob,
    [in, out] unsigned int   *piHashAlg,
    [out] BYTE    *pbHash,
    [in]  DWORD   cchHash,
    [out] DWORD   *pchHash
);
```

## <a name="parameters"></a>パラメーター

`pbBlob`\
からハッシュされるメモリブロックのアドレスへのポインター。

`cchBlob`\
からメモリブロックの長さ (バイト単位)。

`piHashAlg`\
[入力、出力]ハッシュアルゴリズムを指定する定数。 既定のアルゴリズムには0を使用します。

`pbHash`\
入出力返されたハッシュバッファー。

`cchHash`\
から要求された `pbHash`の最大サイズ。

`pchHash`\
入出力返された `pbHash`のサイズ (バイト単位)。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** StrongName

**ライブラリ:** Mscoree.dll にリソースとして含まれています

**.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>関連項目

- [GetHashFromBlob メソッド](../hosting/iclrstrongname-gethashfromblob-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
