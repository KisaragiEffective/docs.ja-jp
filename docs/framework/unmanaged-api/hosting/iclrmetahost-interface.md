---
title: ICLRMetaHost インターフェイス
ms.date: 03/30/2017
api_name:
- ICLRMetaHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost
helpviewer_keywords:
- ICLRMetaHost interface [.NET Framework hosting]
ms.assetid: c627fcdd-fc4f-4b1c-8e91-df8536f627d8
topic_type:
- apiref
ms.openlocfilehash: bb760f4923cc3530a28bc68180db743ee468b51d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140911"
---
# <a name="iclrmetahost-interface"></a>ICLRMetaHost インターフェイス
バージョン番号に基づいて特定のバージョンの共通言語ランタイム (CLR) を返すメソッド、すべてのインストールされた CLRs の一覧表示、指定されたプロセスで読み込まれたすべてのランタイムの一覧表示、アセンブリのコンパイルに使用される CLR バージョンの検出、プロセスの終了を行うメソッドを提供します。クリーンランタイムをシャットダウンし、従来の API バインディングにクエリを実行します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnumerateInstalledRuntimes メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-enumerateinstalledruntimes-method.md)|コンピューターにインストールされている各 CLR バージョンの有効な[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスポインターを含む列挙を返します。|  
|[EnumerateLoadedRuntimes メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-enumerateloadedruntimes-method.md)|指定されたプロセスに読み込まれる各 CLR の有効な[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスポインターを含む列挙体を返します。 このメソッドは、 [Getversionfromprocess](../../../../docs/framework/unmanaged-api/hosting/getversionfromprocess-function.md)よりも優先します。|  
|[ExitProcess メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-exitprocess-method.md)|読み込まれたすべてのランタイムを正常にシャットダウンしてから、プロセスを終了しようとします。 [CorExitProcess](../../../../docs/framework/unmanaged-api/hosting/corexitprocess-function.md)関数よりも優先されます。|  
|[GetRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-getruntime-method.md)|特定の CLR バージョンに対応する[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスを取得します。 このメソッドは、 [STARTUP_LOADER_SAFEMODE](../../../../docs/framework/unmanaged-api/hosting/startup-flags-enumeration.md)フラグと共に使用される[Corbindtoruntimeex](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimeex-function.md)関数よりも優先されます。|  
|[GetVersionFromFile メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-getversionfromfile-method.md)|ファイルパスを指定して、アセンブリの元の .NET Framework コンパイルバージョン (メタデータに格納されている) を取得します。 このメソッドは、 [GetFileVersion](../../../../docs/framework/unmanaged-api/hosting/getfileversion-function.md)よりも優先します。|  
|[QueryLegacyV2RuntimeBinding メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-querylegacyv2runtimebinding-method.md)|レガシアクティブ化ポリシーがバインドされているランタイムを表すインターフェイスを返します。たとえば、 [\<スタートアップ > 要素](../../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)構成ファイルエントリの `useLegacyV2RuntimeActivationPolicy` 属性を使用したり、レガシアクティベーション api を直接使用したり、レガシアクティブ化 api を直接使用したり、[ICLRRuntimeInfo:: BindAsLegacyV2Runtime](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-bindaslegacyv2runtime-method.md)メソッドを呼び出しています。|  
|[RequestRuntimeLoadedNotification メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-requestruntimeloadednotification-method.md)|CLR バージョンが最初に読み込まれたが、まだ開始されていない場合は、指定された関数ポインターへのコールバックを保証します。 このメソッドは、 [Lockclrversion](../../../../docs/framework/unmanaged-api/hosting/lockclrversion-function.md)を置き換えます。|  
  
## <a name="remarks"></a>Remarks  
 このインターフェイスのインスタンスを取得する唯一の方法は、 [Clrcreateinstance](../../../../docs/framework/unmanaged-api/hosting/clrcreateinstance-function.md)関数を次のように呼び出します。  
  
```cpp  
ICLRMetaHost *pMetaHost = NULL;  
HRESULT hr = CLRCreateInstance(CLSID_CLRMetaHost,  
                   IID_ICLRMetaHost, (LPVOID*)&pMetaHost);  
```  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)
