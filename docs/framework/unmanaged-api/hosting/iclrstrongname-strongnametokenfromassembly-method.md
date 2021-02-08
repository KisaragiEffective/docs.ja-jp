---
description: '詳細について: ICLRStrongName:: StrongNameTokenFromAssembly メソッド'
title: ICLRStrongName::StrongNameTokenFromAssembly メソッド
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameTokenFromAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameTokenFromAssembly
helpviewer_keywords:
- ICLRStrongName::StrongNameTokenFromAssembly method [.NET Framework hosting]
- StrongNameTokenFromAssembly method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: fc725afb-b66b-4015-aa44-1c0d1304197f
topic_type:
- apiref
ms.openlocfilehash: 520d327767f91763f8f2b3efea098c7c2790939e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99781819"
---
# <a name="iclrstrongnamestrongnametokenfromassembly-method"></a>ICLRStrongName::StrongNameTokenFromAssembly メソッド

指定したアセンブリ ファイルから、厳密な名前トークンが作成されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT StrongNameTokenFromAssembly (  
    [in]  LPCWSTR   wszFilePath,  
    [out] BYTE      **ppbStrongNameToken,  
    [out] ULONG     *pcbStrongNameToken  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `wszFilePath`  
 からアセンブリのポータブル実行可能 (PE) ファイルへのパス。  
  
 `ppbStrongNameToken`  
 入出力返された厳密な名前トークン。  
  
 `pcbStrongNameToken`  
 入出力厳密な名前トークンのサイズ (バイト単位)。  
  
## <a name="return-value"></a>戻り値  

 `S_OK` メソッドが正常に完了した場合は。それ以外の場合は、失敗を示す HRESULT 値 (「リストの [一般的な Hresult 値](/windows/win32/seccrypto/common-hresult-values) 」を参照してください)。  
  
## <a name="remarks"></a>解説  

 厳密な名前トークンは、公開キーの短縮形です。 トークンは、アセンブリの署名に使用される公開キーから作成された64ビットのハッシュです。 トークンはアセンブリの厳密な名前の一部であり、アセンブリメタデータから読み取ることができます。  
  
 トークンが作成されたら、 [ICLRStrongName:: StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) メソッドを呼び出して、割り当てられたメモリを解放する必要があります。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .h  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [StrongNameTokenFromAssemblyEx メソッド](iclrstrongname-strongnametokenfromassemblyex-method.md)
- [ICLRStrongName インターフェイス](iclrstrongname-interface.md)
