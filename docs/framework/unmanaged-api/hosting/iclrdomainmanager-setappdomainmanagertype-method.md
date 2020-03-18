---
title: ICLRDomainManager::SetAppDomainManagerType メソッド
ms.date: 03/30/2017
api_name:
- ICLRDomainManager.SetAppDomainManagerType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDomainManager::SetAppDomainManagerType
helpviewer_keywords:
- SetAppDomainManagerType method, ICLRDomainManager interface [.NET Framework hosting]
- ICLRDomainManager::SetAppDomainManagerType method [.NET Framework hosting]
ms.assetid: ee91abb0-cb74-41dd-927b-e117fb8ffdf4
ms.openlocfilehash: 5c61e2e1208cec0bda1492964a8d02bd71f5a1c6
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129332"
---
# <a name="iclrdomainmanagersetappdomainmanagertype-method"></a>ICLRDomainManager::SetAppDomainManagerType メソッド
既定のアプリケーションドメインを初期化するために使用されるアプリケーションドメインマネージャーの <xref:System.AppDomainManager?displayProperty=nameWithType> クラスから派生した型を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetAppDomainManagerType(  
    [in] LPCWSTR wszAppDomainManagerAssembly,  
    [in] LPCWSTR wszAppDomainManagerType,  
    [in] EInitializeNewDomainFlags dwInitializeDomainFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `wszAppDomainManagerAssembly`  
 からアプリケーションドメインマネージャーの種類を含むアセンブリの表示名です。たとえば、"使用しているバージョン = 1.0.0.0, Culture = ニュートラル, PublicKeyToken = 6856bccf150f00b3" のようになります。  
  
 `wszAppDomainManagerType`  
 から名前空間を含む、アプリケーションドメインマネージャーの型名。  
  
 `dwInitializeDomainFlags`  
 からアプリケーションドメインマネージャーに関する情報を提供する[EInitializeNewDomainFlags](../../../../docs/framework/unmanaged-api/hosting/einitializenewdomainflags-enumeration.md)列挙値の組み合わせ。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|メソッドは正常に完了しました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
  
## <a name="remarks"></a>Remarks  
 現時点では、`dwInitializeDomainFlags` に対して定義されている唯一の値は `eInitializeNewDomainFlags_NoSecurityChanges`であり、<xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> メソッドの実行中にアプリケーションドメインマネージャーがセキュリティ設定を変更しないことを共通言語ランタイム (CLR) に通知します。 これにより、CLR は条件付き <xref:System.Security.AllowPartiallyTrustedCallersAttribute> (APTCA) 属性を持つアセンブリの読み込みを最適化できます。 これにより、このアセンブリセットの推移的なクロージャが大きい場合に、起動時間が大幅に向上する可能性があります。  
  
> [!IMPORTANT]
> ホストでアプリケーションドメインマネージャーの `eInitializeNewDomainFlags_NoSecurityChanges` が指定されている場合、アプリケーションドメインのセキュリティを変更しようとすると、<xref:System.InvalidOperationException> がスローされます。  
  
 [ICLRControl:: SetAppDomainManagerType](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-setappdomainmanagertype-method.md)メソッドを呼び出すことは、`eInitializeNewDomainFlags_None`で `ICLRDomainManager::SetAppDomainManagerType` を呼び出すことと同じです。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)
- [ICLRDomainManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrdomainmanager-interface.md)
- [EInitializeNewDomainFlags 列挙型](../../../../docs/framework/unmanaged-api/hosting/einitializenewdomainflags-enumeration.md)
