---
title: STARTUP_FLAGS 列挙型
ms.date: 03/30/2017
api_name:
- STARTUP_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- STARTUP_FLAGS
helpviewer_keywords:
- STARTUP_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 4f043594-0c45-4bc6-988e-a6793f0d8d06
topic_type:
- apiref
ms.openlocfilehash: 1799e0af91fa6074f174120b29e2302a27230c62
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141455"
---
# <a name="startup_flags-enumeration"></a>STARTUP_FLAGS 列挙型
共通言語ランタイム (CLR: Common Language Runtime) の起動動作を示す値を含みます。 既定では、ガベージ コレクションは非同時実行で、基底クラス ライブラリだけがドメイン中立領域に読み込まれます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    STARTUP_CONCURRENT_GC                         = 0x1,  
    STARTUP_LOADER_OPTIMIZATION_MASK              = 0x3<<1,  
    STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN     = 0x1<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN      = 0x2<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST = 0x3<<1,  
  
    STARTUP_LOADER_SAFEMODE                       = 0x10,  
    STARTUP_LOADER_SETPREFERENCE                  = 0x100,  
  
    STARTUP_SERVER_GC                             = 0x1000,  
    STARTUP_HOARD_GC_VM                           = 0x2000,  
  
    STARTUP_SINGLE_VERSION_HOSTING_INTERFACE      = 0x4000,  
    STARTUP_LEGACY_IMPERSONATION                  = 0x10000,  
    STARTUP_DISABLE_COMMITTHREADSTACK             = 0x20000,  
    STARTUP_ALWAYSFLOW_IMPERSONATION              = 0x40000,  
    STARTUP_TRIM_GC_COMMIT                        = 0x80000,  
  
    STARTUP_ETW                                   = 0x100000,  
    STARTUP_ARM                                   = 0x400000  
} STARTUP_FLAGS;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`STARTUP_CONCURRENT_GC`|同時実行ガベージ コレクションを使用することを指定します。 呼び出し元がサーバー ビルドと同時実行ガベージ コレクションをシングル プロセッサ コンピューター上で要求した場合は、代わりにワークステーション ビルドと非同時実行ガベージ コレクションが実行されます。 **注:** 同時実行ガベージコレクションは、Intel Itanium アーキテクチャ (旧称 IA-64) を実装する64ビットシステムで WOW64 x86 エミュレーターを実行しているアプリケーションではサポートされていません。 64ビット Windows システムでの WOW64 の使用の詳細については、「 [32 ビットアプリケーションの実行](/windows/desktop/WinProg64/running-32-bit-applications)」を参照してください。|  
|`STARTUP_LOADER_OPTIMIZATION_MASK`|ローダーの最適化を行う必要があることを指定します。|  
|`STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN`|どのアセンブリもドメイン中立として読み込まないことを指定します。|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN`|すべてのアセンブリをドメイン中立として読み込むことを指定します。|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST`|厳密な名前を付けられたすべてのアセンブリをドメイン中立として読み込むことを指定します。|  
|`STARTUP_LOADER_SAFEMODE`|CLR バージョン ポリシーが、渡されたバージョンに適用されないことを指定します。 指定された CLR のバージョンが読み込まれます。 シムは、互換性のある最新バージョンを決めるためのポリシーの評価を実行しません。|  
|`STARTUP_LOADER_SETPREFERENCE`|推奨ランタイムを設定するように指定しますが、実際には起動されません。|  
|`STARTUP_SERVER_GC`|サーバー ガベージ コレクションを使用することを指定します。|  
|`STARTUP_HOARD_GC_VM`|ガベージ コレクションで、使用された仮想アドレスを保持することを指定します。|  
|`STARTUP_SINGLE_VERSION_HOSTING_INTERFACE`|ホスト インターフェイスの混合を許可しないことを指定します。|  
|`STARTUP_LEGACY_IMPERSONATION`|既定として偽装が非同期ポイント間をフローしないように指定します。|  
|`STARTUP_DISABLE_COMMITTHREADSTACK`|スレッドが実行を開始するときにスレッド スタック全体をコミットしないことを指定します。|  
|`STARTUP_ALWAYSFLOW_IMPERSONATION`|マネージド偽装およびプラットフォーム呼び出しによって実行された偽装が非同期ポイント間をフローするように指定します。 既定では、マネージド偽装だけが非同期ポイント間をフローします。|  
|`STARTUP_TRIM_GC_COMMIT`|システム メモリが少ないときに、ガベージ コレクションによるコミットされた領域の使用量を抑えることを指定します。 「[共有 Web ホスティングの最適化」の](../../../standard/garbage-collection/optimization-for-shared-web-hosting.md)`gcTrimCommitOnLowMemory` を参照してください。|  
|`STARTUP_ETW`|共通言語ランタイム イベントで Windows イベント トレーシング (ETW) が有効になっていることを指定します。 Windows Vista 以降では、イベントのトレースは常に有効になっているため、このフラグによる影響はありません。 「 [.NET Framework のログ記録の制御](../../../../docs/framework/performance/controlling-logging.md)」を参照してください。|  
|`STARTUP_ARM`|アプリケーション ドメインのリソース監視が有効になっていることを指定します。 <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> プロパティと[\<appDomainResourceMonitoring > 要素](../../../../docs/framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)を参照してください。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
