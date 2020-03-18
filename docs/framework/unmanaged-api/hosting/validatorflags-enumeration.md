---
title: ValidatorFlags 列挙型
ms.date: 03/30/2017
api_name:
- ValidatorFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ValidatorFlags
helpviewer_keywords:
- ValidatorFlags enumeration [.NET Framework hosting]
ms.assetid: a3f5c266-3fcc-4ad1-aaf5-4cdbe26304ad
topic_type:
- apiref
ms.openlocfilehash: 61aafb8dc99bb908fc603945ff6ea74054f812c4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141417"
---
# <a name="validatorflags-enumeration"></a>ValidatorFlags 列挙型
[ICLRValidator:: Validate](../../../../docs/framework/unmanaged-api/hosting/iclrvalidator-validate-method.md)メソッドの呼び出しで実行する必要がある検証の種類を示す値を格納します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
enum ValidatorFlags {  
    VALIDATOR_EXTRA_VERBOSE =       0x00000001,  
    VALIDATOR_SHOW_SOURCE_LINES =   0x00000002,  
    VALIDATOR_CHECK_ILONLY =        0x00000004,  
    VALIDATOR_CHECK_PEFORMAT_ONLY = 0x00000008,  
    VALIDATOR_NOCHECK_PEFORMAT =    0x00000010,  
};  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`VALIDATOR_CHECK_ILONLY`|実行可能ファイル内の Microsoft 中間言語 (MSIL) のみを検証するように指定します。|  
|`VALIDATOR_CHECK_PEFORMAT_ONLY`|実行可能ファイルの形式だけを検証することを指定します。|  
|`VALIDATOR_EXTRA_VERBOSE`|すべての種類の検証を実行してレポートすることを指定します。|  
|`VALIDATOR_NOCHECK_PEFORMAT`|実行可能ファイルの形式を検証しないことを指定します。|  
|`VALIDATOR_SHOW_SOURCE_LINES`|検証エラーメッセージに検証エラーを発生させるソースコード行を含めるように指定します。 このフィールド値は .NET Framework バージョン2.0 では無効です。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** IValidator、IValidator  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRErrorReportingManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrerrorreportingmanager-interface.md)
- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
