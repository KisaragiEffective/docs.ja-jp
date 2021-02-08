---
description: 詳細については、「Certverify認証 Odelicense 関数」を参照してください。
title: CertVerifyAuthenticodeLicense 関数
ms.date: 03/30/2017
api_name:
- CertVerifyAuthenticodeLicense
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: 00118de7-33c6-41c4-8e1f-5d5e35e0da83
topic_type:
- apiref
ms.openlocfilehash: 0174223a4c1b984bf2d5d957219a85230fef8d0e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99772849"
---
# <a name="certverifyauthenticodelicense-function"></a>CertVerifyAuthenticodeLicense 関数

Authenticode XrML ライセンスの有効性を検証します。

## <a name="syntax"></a>構文

```cpp
HRESULT CertVerifyAuthenticodeLicense (
    [in]   PCRYPT_DATA_BLOB                   pLicenseBlob,
    [in]   OPTIONAL DWORD                     dwFlags,
    [out]  PAXL_AUTHENTICODE_SIGNER_INFO      pSignerInfo,
    [out]  PAXL_AUTHENTICODE_TIMESTAMPER_INFO pTimestamperInfo
);
```

## <a name="parameters"></a>パラメーター

 `pLicenseBlob`\
 [in] 検証する Authenticode XrML ライセンス。

 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob)構造を参照してください。

 `dwFlags`\
 [in] オプション。 以下の値の組み合わせ。

- AXL_REVOCATION_NO_CHECK

- AXL_REVOCATION_CHECK_END_CERT_ONLY

- AXL_REVOCATION_CHECK_ENTIRE_CHAIN

- AXL_URL_CACHE_ONLY_RETRIEVAL

- AXL_LIFETIME_SIGNING

- AXL_TRUST_MICROSOFT_ROOT_ONLY

 `pSignerInfo`\
 [out] 署名者の情報を受け取るため。 ライセンスに署名がない場合、`dwError` は TRUST_E_NOSIGNATURE に設定されます。 使用後に、 [Certfree認証 Oデザイナ情報](certfreeauthenticodesignerinfo-function.md) 関数を使用してリソースを解放するのは、呼び出し元の責任です。

 「 [AXL_AUTHENTICODE_SIGNER_INFO 構造](axl-authenticode-signer-info-structure.md)」を参照してください。

 `pTimestamperInfo`\
 [out] 可能な場合は、タイム スタンプの情報を受け取るため。 ライセンスにタイム スタンプがない場合、`dwError` は TRUST_E_NOSIGNATURE に設定されます。 使用後に [CertFreeAuthenticodeTimestamperInfo](certfreeauthenticodetimestamperinfo-function.md) 関数を使用してリソースを解放するのは、呼び出し元の責任です。

 「 [AXL_AUTHENTICODE_TIMESTAMPER_INFO 構造](axl-authenticode-timestamper-info-structure.md)」を参照してください。

## <a name="return-value"></a>戻り値

 正常に終了した場合は `S_OK` を返します。 それ以外の場合はエラー コードを返します。

## <a name="requirements"></a>要件

**アセンブリ**: clr.dll

## <a name="see-also"></a>関連項目

- [Authenticode](index.md)
- [GetHashFromHandle メソッド](../hosting/iclrstrongname-gethashfromhandle-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
