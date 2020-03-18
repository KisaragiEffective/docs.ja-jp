---
title: GetRequestedRuntimeVersionForCLSID 関数
ms.date: 03/30/2017
api_name:
- GetRequestedRuntimeVersionForCLSID
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- GetRequestedRuntimeVersionForCLSID
helpviewer_keywords:
- GetRequestedRuntimeVersionForCLSID function [.NET Framework hosting]
ms.assetid: 5bb12f9a-0612-434b-b4ed-2db636a20bec
topic_type:
- apiref
ms.openlocfilehash: ce0c6307defd93dcf63ac4e9051fc798041475f3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127053"
---
# <a name="getrequestedruntimeversionforclsid-function"></a>GetRequestedRuntimeVersionForCLSID 関数
指定した `CLSID`を使用して、クラスの適切な共通言語ランタイム (CLR) のバージョン情報を取得します。  
  
 この関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetRequestedRuntimeVersionForCLSID (  
    [in]  REFCLSID   rclsid,   
    [out]  LPWSTR     pVersion,   
    [in]  DWORD      cchBuffer,   
    [out] DWORD*     dwLength,   
    [in]  CLSID_RESOLUTION_FLAGS dwResolutionFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `rclsid`  
 から コンポーネントの `CLSID`。  
  
 `pVersion`  
 入出力 正常に完了したときのバージョン番号の文字列を格納するバッファー。  
  
 `cchBuffer`  
 から `pVersion` バッファーのサイズ (ワイド文字単位)。  
  
 `dwLength`  
 入出力返されたバッファーの長さ (バイト単位)。  
  
 `dwResolutionFlags`  
 から CLSID_RESOLUTION_FLAGS 値の1つ。 次の値がサポートされています。  
  
- CLSID_RESOLUTION_DEFAULT: (0x0) 既定の相互運用動作を使用することを指定します。  
  
- CLSID_RESOLUTION_REGISTERED: (0x1) レジストリを検索し、shim ポリシーを適用する必要があることを指定します。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|関数が正常に返されました。|  
|E_INVALIDARG|パラメーターの1つに無効な型または形式が指定されています。|  
|ERROR_INSUFFICIENT_BUFFER|`pVersion` バッファーが、バージョン文字列全体を格納するのに十分な大きさではありません。|  
|REGDB_E_CLASSNOTREG|指定された `CLSID`に登録されているクラスがありません。|  
|E_POINTER|`dwLength` が null であるか、または `cchBuffer` がバージョン文字列を保持するのに十分な大きさですが、`pVersion` が null です。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [非推奨の CLR ホスト関数](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
