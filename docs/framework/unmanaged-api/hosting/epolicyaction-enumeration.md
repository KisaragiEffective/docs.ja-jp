---
title: EPolicyAction 列挙型
ms.date: 03/30/2017
api_name:
- EPolicyAction
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EPolicyAction
helpviewer_keywords:
- EPolicyAction enumeration [.NET Framework hosting]
ms.assetid: 72dd76ba-239e-45ac-9ded-318fb07d6c6d
topic_type:
- apiref
ms.openlocfilehash: eaba6b2166a82cfe825ffb98db515e24d4656462
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73138233"
---
# <a name="epolicyaction-enumeration"></a>EPolicyAction 列挙型
[EClrOperation](../../../../docs/framework/unmanaged-api/hosting/eclroperation-enumeration.md)によって記述された操作や[eclrfailure](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)によって記述されたエラーについて、ホストが設定できるポリシーアクションについて説明します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    eNoAction,  
    eThrowException,  
    eAbortThread,  
    eRudeAbortThread,  
    eUnloadAppDomain,  
    eRudeUnloadAppDomain,  
    eExitProcess,  
    eFastExitProcess,  
    eRudeExitProcess,  
    eDisableRuntime  
} EPolicyAction;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`eAbortThread`|共通言語ランタイム (CLR) がスレッドを正常に中止する必要があることを指定します。 正常な中止には、すべての `finally` ブロック、スレッドの中止に関連するすべての `catch` ブロック、およびファイナライザーを実行する試行が含まれます。|  
|`eDisableRuntime`|CLR が無効化された状態になるように指定します。 影響を受けるプロセスでは、それ以上のマネージコードを実行できず、スレッドは CLR への入力がブロックされます。|  
|`eExitProcess`|CLR がプロセスを正常に終了する必要があることを指定します。これには、ファイナライザーの実行やクリーンアップおよびログ操作の実行が含まれます。|  
|`eFastExitProcess`|CLR がファイナライザーを実行したりクリーンアップ操作やログ操作を実行したりせずに、プロセスをすぐに終了するように指定します。 ただし、通知はデバッガーに送信されます。|  
|`eNoAction`|アクションを実行しないことを指定します。|  
|`eRudeAbortThread`|CLR がルースレッド中止を実行することを指定します。 <xref:System.EnterpriseServices.MustRunInClientContextAttribute> でマークされた `catch` および `finally` ブロックだけが実行されます。|  
|`eRudeExitProcess`|CLR がファイナライザーまたはログ操作を実行せずにプロセスを終了する必要があることを指定します。|  
|`eRudeUnloadAppDomain`|CLR が <xref:System.AppDomain>のルードアンロードを実行する必要があることを指定します。 <xref:System.EnterpriseServices.MustRunInClientContextAttribute> でマークされたファイナライザーだけが実行されます。 同様に、スタック内のこの <xref:System.AppDomain> を持つすべてのスレッドは `ThreadAbortException`を受け取りますが、<xref:System.EnterpriseServices.MustRunInClientContextAttribute> でマークされた `catch` および `finally` ブロックだけが実行されます。|  
|`eThrowException`|メモリ不足、バッファーオーバーフローなどの条件に適した例外をスローする必要があることを指定します。|  
|`eUnloadAppDomain`|<xref:System.AppDomain> をアンロードする必要があることを指定します。 CLR はファイナライザーの実行を試みます。|  
  
## <a name="remarks"></a>Remarks  
 ホストは、 [ICLRPolicyManager](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)インターフェイスのメソッドを呼び出すことによって、ポリシーアクションを設定します。 ルードと正常な中止の詳細については、 [EClrOperation](../../../../docs/framework/unmanaged-api/hosting/eclroperation-enumeration.md)列挙体を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [EClrFailure 列挙型](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)
- [ICLRPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)
- [IHostPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostpolicymanager-interface.md)
- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
