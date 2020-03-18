---
title: IValidator::Validate メソッド
ms.date: 03/30/2017
api_name:
- IValidator.Validate
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- Validate
helpviewer_keywords:
- IValidator::Validate method [.NET Framework hosting]
- Validate method, IValidator interface [.NET Framework hosting]
ms.assetid: 7d68666a-fb73-4455-bebd-908d49a16abc
topic_type:
- apiref
ms.openlocfilehash: 8ae47eac713fbee30ea543538957b12460b8e1fc
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123280"
---
# <a name="ivalidatorvalidate-method"></a>IValidator::Validate メソッド
指定された移植可能な実行可能 (PE) ファイルまたは MSIL (Microsoft 中間言語) ファイルを検証します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Validate (  
    [in] IVEHandler            *veh,  
    [in] IUnknown              *pAppDomain,  
    [in] unsigned long          ulFlags,  
    [in] unsigned long          ulMaxError,  
    [in] unsigned long          token,  
    [in] LPWSTR                 fileName,  
    [in, size_is(ulSize)] BYTE *pe,  
    [in] unsigned long          ulSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `veh`  
 から検証エラーを処理する `IVEHandler` インスタンスへのポインター。  
  
 `pAppDomain`  
 からファイルが読み込まれるアプリケーションドメインへのポインター。  
  
 `ulFlags`  
 から実行する必要のある検証を示す、 [Validatorflags](../../../../docs/framework/unmanaged-api/hosting/validatorflags-enumeration.md)値のビットごとの組み合わせ。  
  
 `ulMaxError`  
 から検証を終了するまでに許容されるエラーの最大数。  
  
 `token`  
 から使用しません。  
  
 `fileName`  
 から検証するファイルの名前を指定する文字列。  
  
 `pe`  
 からファイルが格納されているメモリバッファーへのポインター。  
  
 `ulSize`  
 から検証するファイルのサイズ (バイト単位)。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** IValidator、IValidator  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
