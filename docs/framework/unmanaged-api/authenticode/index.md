---
title: Authenticode (アンマネージ API リファレンス)
ms.date: 03/30/2017
ms.assetid: 7e8cc303-6e77-4116-aa8b-7ea297a3a467
ms.openlocfilehash: 1b8b2222950c75f7f9d2ec2704f722087645cd7e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73132465"
---
# <a name="authenticode-unmanaged-api-reference"></a>Authenticode (アンマネージ API リファレンス)
Authenticode XrML ライセンスの作成および検証モジュールをサポートします。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [_AxlGetIssuerPublicKeyHash 関数](axlgetissuerpublickeyhash-function.md)  
 指定された証明書の署名に使用する秘密キーに関連付けられている公開キーの SHA-1 ハッシュを取得します。  
  
 [_AxlPublicKeyBlobToPublicKeyToken 関数](axlpublickeyblobtopublickeytoken-function.md)  
 CSP PUBLICKEYBLOB 形式から厳密な名前の公開キー トークンを算出します。  
  
 [_AxlRSAKeyValueToPublicKeyToken 関数](axlrsakeyvaluetopublickeytoken-function.md)  
 Modulus および Exponent を、厳密な名前の公開キー トークンに変換します。  
  
 [CertFreeAuthenticodeSignerInfo 関数](certfreeauthenticodesignerinfo-function.md)  
 AXL_AUTHENTICODE_SIGNER_INFO 構造に割り当てられているリソースを解放します。  
  
 [CertFreeAuthenticodeTimestamperInfo 関数](certfreeauthenticodetimestamperinfo-function.md)  
 AXL_AUTHENTICODE_TIMESTAMPER_INFO 構造に割り当てられているリソースを開放します。  
  
 [CertTimestampAuthenticodeLicense 関数](certtimestampauthenticodelicense-function.md)  
 CertCreateAuthenticodeLicense で作成された Authenticode XrML ライセンスにタイム スタンプを付けます。  
  
 [CertVerifyAuthenticodeLicense 関数](certverifyauthenticodelicense-function.md)  
 Authenticode XrML ライセンスの有効性を検証します。  
  
 [AXL_AUTHENTICODE_SIGNER_INFO 構造体](axl-authenticode-signer-info-structure.md)  
 Authenticode の署名者情報を定義します。  
  
 [AXL_AUTHENTICODE_TIMESTAMPER_INFO 構造体](axl-authenticode-timestamper-info-structure.md)  
 Authenticode のタイム スタンパー情報を定義します。  
  
## <a name="see-also"></a>参照

- [アンマネージ API リファレンス](../index.md)
