---
title: METAHOST_POLICY_FLAGS 列挙体
ms.date: 03/30/2017
api_name:
- METAHOST_POLICY_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- METAHOST_POLICY_FLAGS
helpviewer_keywords:
- METAHOST_POLICY_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 3bb4b526-0118-42e2-ba59-c95648528ce9
topic_type:
- apiref
ms.openlocfilehash: 048286fb9e1af34cd964fb5b21892778f9575d2c
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75938202"
---
# <a name="metahost_policy_flags-enumeration"></a>METAHOST_POLICY_FLAGS 列挙体
ほとんどのランタイムホストに共通のバインドポリシーを提供します。 この列挙体は、 [ICLRMetaHostPolicy:: GetRequestedRuntime](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md)メソッドによって使用されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    METAHOST_POLICY_HIGHCOMPAT              = 0x00,  
    METAHOST_POLICY_APPLY_UPGRADE_POLICY    = 0x08,  
    METAHOST_POLICY_EMULATE_EXE_LAUNCH      = 0x10,  
    METAHOST_POLICY_SHOW_ERROR_DIALOG       = 0x20,  
    METAHOST_POLICY_USE_PROCESS_IMAGE_PATH  = 0x40,  
    METAHOST_POLICY_ENSURE_SKU_SUPPORTED    = 0x80,  
    METAHOST_POLICY_IGNORE_ERROR_MODE       = 0x1000  
  
} METAHOST_POLICY_FLAGS;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`METAHOST_POLICY_HIGHCOMPAT`|現在のプロセスに読み込まれる共通言語ランタイム (CLR: common language runtime) を考慮しない、互換性の高いポリシーを定義します。 代わりに、インストールされた CLRs と、コンポーネントの設定 (アセンブリファイル自体、宣言された組み込みバージョン、または構成ファイルから派生したもの) のみを考慮します。|  
|`METAHOST_POLICY_APPLY_UPGRADE_POLICY`|HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\の内容に基づいて完全一致が見つからなかった場合に、バージョンのバインド結果にアップグレードポリシーを適用します。NETFramework\Policy\Upgrades. これは[RUNTIME_INFO_UPGRADE_VERSION](../../../../docs/framework/unmanaged-api/hosting/runtime-info-flags-enumeration.md)と同じ効果があります。|  
|`METAHOST_POLICY_EMULATE_EXE_LAUNCH`|呼び出しに対して指定されたイメージが新しいプロセスで起動されたかのように、バインディングの結果が返されます。 現時点では、`GetRequestedRuntime` は、一連の読み込み可能なランタイムを無視し、インストールされている一連のランタイムに対してバインドします。 このフラグにより、ホストは、起動時に EXE がバインドされるランタイムを決定できます。|  
|`METAHOST_POLICY_SHOW_ERROR_DIALOG`|`GetRequestedRuntime` が入力パラメーターと互換性のあるランタイムを見つけられない場合、エラーダイアログボックスが表示されます。 .NET Framework 4.5 以降、このエラーダイアログボックスには、ユーザーが適切な機能を有効にするかどうかを確認する Windows の機能ダイアログボックスが表示されます。|  
|`METAHOST_POLICY_USE_PROCESS_IMAGE_PATH`|`GetRequestedRuntime` は、プロセスイメージ (およびそれに対応する構成ファイル) をバインドプロセスへの追加入力として使用します。 既定では、`GetRequestedRuntime` は、バインド先のランタイムを決定するときに、プロセスイメージのパス (通常はプロセスの起動に使用された EXE) にフォールバックしません。|  
|`METAHOST_POLICY_ENSURE_SKU_SUPPORTED`|構成ファイルに利用できる情報がない場合は、適切な SKU がインストールされているかどうかを確認 `GetRequestedRuntime` 必要があります。 これにより、構成ファイルを持たないアプリケーションは、.NET Framework の既定のインストールよりも小さい Sku では正常に機能しなくなります。 既定では、構成ファイル `<supportedRuntime />` 要素で SKU 属性が指定されていない限り、`GetRequestedRuntime` は適切な SKU がインストールされているかどうかを確認しません。|  
|`METAHOST_POLICY_IGNORE_ERROR_MODE`|`GetRequestedRuntime` は、( [SetErrorMode](/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode)関数を呼び出すことによって設定される) SEM_FAILCRITICALERRORS を無視し、エラーダイアログボックスを表示します。 既定では、SEM_FAILCRITICALERRORS はエラーダイアログボックスを表示しません。 このファイルは別のプロセスから継承されている可能性があり、シナリオによってはサイレントエラーが望ましくない場合があります。|  
  
## <a name="remarks"></a>Remarks  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
- [GetRequestedRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md)
