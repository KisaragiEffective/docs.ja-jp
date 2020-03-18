---
title: CorLaunchApplication 関数
ms.date: 03/30/2017
api_name:
- CorLaunchApplication
api_location:
- mscoree.dll
- clr.dll
api_type:
- COM
f1_keywords:
- CorLaunchApplication
helpviewer_keywords:
- CorLaunchApplication function [.NET Framework hosting]
ms.assetid: 71f362a9-8fe2-47ce-9302-05a645cf3d7d
topic_type:
- apiref
ms.openlocfilehash: e01698d2d8491b2496bb664c13dca97964cd1481
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136946"
---
# <a name="corlaunchapplication-function"></a>CorLaunchApplication 関数
指定したネットワーク パスのアプリケーションを、指定したマニフェストとその他のアプリケーション データを使用して起動します。  
  
 この関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CorLaunchApplication (  
    [in]  HOST_TYPE                dwClickOnceHost,  
    [in]  LPCWSTR                  pwzAppFullName,  
    [in]  DWORD                    dwManifestPaths,  
    [in]  LPCWSTR                 *ppwzManifestPaths,  
    [in]  DWORD                    dwActivationData,  
    [in]  LPCWSTR                 *ppwzActivationData,  
    [out] LPPROCESS_INFORMATION    lpProcessInformation  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `dwClickOnceHost`  
 からアプリケーションを起動しているホストの種類を指定する[HOST_TYPE](../../../../docs/framework/unmanaged-api/hosting/host-type-enumeration.md)列挙体の値。  
  
 `pwzAppFullName`  
 から起動されるアプリケーションの完全な名前。  
  
 `dwManifestPaths`  
 からアプリケーションのマニフェストパスの数。  
  
 `ppwzManifestPaths`  
 から文字列の配列。それぞれが、起動されるアプリケーションのマニフェストへのパスを指定します。  
  
 `dwActivationData`  
 から起動されるアプリケーションのアクティブ化データ項目の数。  
  
 `ppwzActivationData`  
 から文字列の配列。各文字列は、起動されるアプリケーションのアクティベーションデータ項目です。  
  
 `lpProcessInformation`  
 入出力アプリケーションが読み込まれたプロセスに関する情報へのポインター。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [非推奨の CLR ホスト関数](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
