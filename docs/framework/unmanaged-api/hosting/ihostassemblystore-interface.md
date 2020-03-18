---
title: IHostAssemblyStore インターフェイス
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore
helpviewer_keywords:
- IHostAssemblyStore interface [.NET Framework hosting]
ms.assetid: cccb650f-abe0-41e2-9fd1-b383788eb1f6
topic_type:
- apiref
ms.openlocfilehash: d7475e2423d4dc6f57e8928514d7991169eef232
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124502"
---
# <a name="ihostassemblystore-interface"></a>IHostAssemblyStore インターフェイス
共通言語ランタイム (CLR) とは独立して、ホストがアセンブリとモジュールを読み込むことができるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ProvideAssembly メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-provideassembly-method.md)|[IHostAssemblyManager:: GetNonHostStoreAssemblies](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-getnonhoststoreassemblies-method.md)の呼び出しから返された[ICLRAssemblyReferenceList](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)によって参照されていないアセンブリへの参照を取得します。|  
|[ProvideModule メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-providemodule-method.md)|アセンブリ内のモジュール、またはリンクされた (埋め込まれていない) リソースファイル内のモジュールを解決します。|  
  
## <a name="remarks"></a>Remarks  
 `IHostAssemblyStore` を使用すると、ホストはアセンブリ id に基づいてアセンブリを効率的に読み込むことができます。 ホストは、バイトを直接指す `IStream` インスタンスを返すことによって、アセンブリを読み込みます。  
  
 CLR は、初期化時に `IHostAssemblyManager::GetNonHostAssemblyStores` を呼び出すことによって、ホストが `IHostAssemblyStore` 実装されているかどうかを判断します。 これにより、ホストは、たとえば、ユーザーアセンブリへのバインドを制御できますが、ランタイムに依存して .NET Framework アセンブリにバインドすることができます。  
  
> [!NOTE]
> `IHostAssemblyStore`の実装を提供する場合、ホストは `IHostAssemblyManager::GetNonHostStoreAssemblies`から返された `ICLRAssemblyReferenceList` によって参照されていないすべてのアセンブリを解決するための目的を指定します。  
  
> [!NOTE]
> .NET Framework バージョン2.0 では、[ネイティブイメージジェネレーター (ngen.exe)](../../../../docs/framework/tools/ngen-exe-native-image-generator.md)ユーティリティによって提供されるアセンブリのネイティブイメージをホストが読み込む方法は提供されていません。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRAssemblyReferenceList インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
