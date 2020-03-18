---
title: IAppDomainSetup インターフェイス
ms.date: 03/30/2017
api_name:
- IAppDomainSetup
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IAppDomainSetup
helpviewer_keywords:
- IAppDomainSetup interface [.NET Framework hosting]
ms.assetid: 1844da85-c031-40bf-bea4-1a3d12a36c8c
topic_type:
- apiref
ms.openlocfilehash: 0fab64c31d4a73995c16d21767f4569f21c7df9a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126873"
---
# <a name="iappdomainsetup-interface"></a>IAppDomainSetup インターフェイス
[ICorRuntimeHost:: CreateDomainEx](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainex-method.md)メソッドを呼び出して作成する前に、ホストが <xref:System.AppDomain?displayProperty=nameWithType> の種類を構成できるようにするプロパティを提供します。  
  
## <a name="properties"></a>プロパティ  
  
|property|説明|  
|--------------|-----------------|  
|<xref:System.AppDomainSetup.ApplicationBase%2A>|アプリケーションが格納されているディレクトリの名前を取得または設定します。|  
|<xref:System.AppDomainSetup.ApplicationName%2A>|アプリケーションの名前を取得または設定します。|  
|<xref:System.AppDomainSetup.CachePath%2A>|ファイルがシャドウコピーされるアプリケーション固有の領域の名前を取得または設定します。|  
|<xref:System.AppDomainSetup.ConfigurationFile%2A>|アプリケーションの構成ファイルの名前を取得または設定します。|  
|<xref:System.AppDomainSetup.DynamicBase%2A>|動的に生成されたファイルが格納およびアクセスされるディレクトリの名前を取得または設定します。|  
|<xref:System.AppDomainSetup.LicenseFile%2A>|このドメインに関連付けられているライセンスファイルへのパスを取得または設定します。|  
|<xref:System.AppDomainSetup.PrivateBinPath%2A>|プライベートアセンブリをプローブするために <xref:System.AppDomainSetup.ApplicationBase%2A> ディレクトリと結合されたディレクトリの一覧を取得または設定します。|  
|<xref:System.AppDomainSetup.PrivateBinPathProbe%2A>|アプリケーションの検索パスから <xref:System.AppDomainSetup.ApplicationBase%2A> を含めたり、除外したりする文字列値を取得または設定します。|  
|<xref:System.AppDomainSetup.ShadowCopyDirectories%2A>|シャドウコピーされるアセンブリを含むディレクトリの名前を取得または設定します。|  
|<xref:System.AppDomainSetup.ShadowCopyFiles%2A>|シャドウコピーをオンまたはオフにするかどうかを示す文字列を取得または設定します。 有効な値は "true" または "false" です。|  
  
## <a name="remarks"></a>Remarks  
 `IAppDomainSetup` インターフェイスは、<xref:System.AppDomainSetup> 型が実装するマネージ <xref:System.IAppDomainSetup> インターフェイスに対応しています。 プロパティの詳細については、「<xref:System.IAppDomainSetup?displayProperty=nameWithType>」を参照してください。  
  
 `IAppDomainSetup` は、作成前に <xref:System.AppDomain> インスタンスに追加できるアセンブリバインディング情報を表します。 たとえば、ホストは、<xref:System.AppDomainSetup.ApplicationBase%2A> プロパティを設定して、マネージアセンブリの共通言語ランタイム (CLR) プローブがルートディレクトリを確立することができます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.AppDomain>
- <xref:System.AppDomainSetup>
- <xref:System.IAppDomainSetup>
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
