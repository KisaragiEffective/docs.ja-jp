---
title: ICorRuntimeHost インターフェイス
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost
helpviewer_keywords:
- ICorRuntimeHost interface [.NET Framework hosting]
ms.assetid: 4369533d-7834-4497-bc37-bfea0ad737b1
topic_type:
- apiref
ms.openlocfilehash: e66e1468a864ec85d88f759c481c7a9707d37f7e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73139549"
---
# <a name="icorruntimehost-interface"></a>ICorRuntimeHost インターフェイス
ホストが共通言語ランタイム (CLR) を明示的に開始および停止し、アプリケーションドメインの作成と構成、既定のドメインへのアクセス、およびプロセスで実行されているすべてのドメインの列挙を行うことができるようにするメソッドを提供します。  
  
 .NET Framework バージョン2.0 では、このインターフェイスは[ICLRRuntimeHost](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-interface.md)によって置き換えられます。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CloseEnum メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-closeenum-method.md)|ドメイン列挙子をドメインリストの先頭にリセットします。|  
|[CreateDomain メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomain-method.md)|アプリケーションドメインを作成します。 呼び出し元は、型 <xref:System._AppDomain> のインターフェイスポインターを <xref:System.AppDomain?displayProperty=nameWithType>型のインスタンスに受信します。|  
|[CreateDomainEx メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainex-method.md)|アプリケーションドメインを作成します。 このメソッドを使用すると、呼び出し元は IAppDomainSetup インスタンスを渡して、返された <xref:System._AppDomain> インスタンスの追加機能を構成できます。|  
|[CreateDomainSetup メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainsetup-method.md)|<xref:System.AppDomainSetup> インスタンスに `IAppDomainSetup` 型のインターフェイスポインターを取得します。 `IAppDomainSetup` には、アプリケーションドメインを作成する前に構成するためのメソッドが用意されています。|  
|[CreateEvidence メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createevidence-method.md)|<xref:System.Security.Principal.IIdentity>型のインターフェイスポインターを取得します。これにより、ホストは[Createdomain](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomain-method.md)または[createdomainex](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainex-method.md)に渡すセキュリティ証拠を作成できます。|  
|[CreateLogicalThreadState メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createlogicalthreadstate-method.md)|使用しないでください。|  
|[CurrentDomain メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-currentdomain-method.md)|現在のスレッドに読み込まれているドメインを表す <xref:System._AppDomain> 型のインターフェイスポインターを取得します。|  
|[DeleteLogicalThreadState メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-deletelogicalthreadstate-method.md)|使用しないでください。|  
|[EnumDomains メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-enumdomains-method.md)|現在のプロセス内のドメインの列挙子を取得します。|  
|[GetConfiguration メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-getconfiguration-method.md)|ホストが CLR のコールバック構成を指定できるようにするオブジェクトを取得します。|  
|[GetDefaultDomain メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-getdefaultdomain-method.md)|現在のプロセスの既定のドメインを表す <xref:System._AppDomain> 型のインターフェイスポインターを取得します。|  
|[LocksHeldByLogicalThread メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-locksheldbylogicalthread-method.md)|使用しないでください。|  
|[MapFile メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-mapfile-method.md)|指定されたファイルをメモリにマップします。 このメソッドは、互換性のために残されています。|  
|[NextDomain メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-nextdomain-method.md)|列挙体の次のドメインへのインターフェイスポインターを取得します。|  
|[Start メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-start-method.md)|CLR を開始します。|  
|[Stop メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-stop-method.md)|現在のプロセスのランタイムでコードの実行を停止します。|  
|[SwitchInLogicalThreadState メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-switchinlogicalthreadstate-method.md)|使用しないでください。|  
|[SwitchOutLogicalThreadState メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-switchoutlogicalthreadstate-method.md)|使用しないでください。|  
|[UnloadDomain メソッド](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-unloaddomain-method.md)|現在のプロセスから、指定されたアプリケーションドメインをアンロードします。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** 1.0、1.1  
  
## <a name="see-also"></a>関連項目

- <xref:System.AppDomain>
- [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)
- [ICLRRuntimeHost インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrruntimehost-interface.md)
- [ランタイム ホスト](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/a51xd4ze(v=vs.100))
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [CorRuntimeHost コクラス](../../../../docs/framework/unmanaged-api/hosting/corruntimehost-coclass.md)
