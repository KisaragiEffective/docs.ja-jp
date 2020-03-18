---
title: EClrFailure 列挙型
ms.date: 03/30/2017
api_name:
- EClrFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EClrFailure
helpviewer_keywords:
- EClrFailure enumeration [.NET Framework hosting]
ms.assetid: 37b95cce-9bfb-4ecf-a00b-33dcba782c67
topic_type:
- apiref
ms.openlocfilehash: 7d935bff023d806cf8cfb6d87bde0f82666b51b5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131126"
---
# <a name="eclrfailure-enumeration"></a>EClrFailure 列挙型
ホストがポリシーアクションを設定できるエラーのセットについて説明します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    FAIL_NonCriticalResource,  
    FAIL_CriticalResource,  
    FAIL_FatalRuntime,  
    FAIL_OrphanedLock  
    FAIL_StackOverflow  
    FAIL_AccessViolation  
    FAIL_CodeContract  
} EClrFailure;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`FAIL_NonCriticalResource`|クリティカルでないコード領域に、リソース (スレッド、メモリブロック、ロックなど) を割り当てようとしたときにエラーが発生しました。|  
|`FAIL_CriticalResource`|コードの重要な領域に、リソース (スレッド、メモリブロック、ロックなど) を割り当てようとしたときにエラーが発生しました。|  
|`FAIL_FatalRuntime`|共通言語ランタイム (CLR) は、プロセスでマネージコードを実行できなくなりました。 その後、任意のホスト関数の呼び出しは、HOST_E_CLRNOTAVAILABLE の HRESULT 値を返します。|  
|`FAIL_OrphanedLock`|スレッドは、<xref:System.AppDomain> オブジェクトから戻るときにロックを解放できませんでした。 ホストは、このエラーを設定してスレッドを中止することはできません。|  
|`FAIL_StackOverflow`|スタックオーバーフローが発生しました。|  
|`FAIL_AccessViolation`|保護されたメモリの読み取りまたは書き込みが試行されました。 .NET Framework 4 ではサポートされていません。|  
|`FAIL_CodeContract`|コードコントラクトエラーが発生しました。 「[コードコントラクト](../../../../docs/framework/debug-trace-profile/code-contracts.md)」を参照してください。|  
  
## <a name="remarks"></a>Remarks  
 エラー条件のポリシーアクションを指定するためにホストで使用できる[Epolicyaction](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md)値の一覧については、 [ICLRPolicyManager:: SetActionOnFailure](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-setactiononfailure-method.md)メソッドを参照してください。 クリティカルな、またはクリティカルでないコード領域の詳細については、「 [EClrOperation](../../../../docs/framework/unmanaged-api/hosting/eclroperation-enumeration.md)」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)
- [SetActionOnFailure メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-setactiononfailure-method.md)
- [IHostPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostpolicymanager-interface.md)
- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
