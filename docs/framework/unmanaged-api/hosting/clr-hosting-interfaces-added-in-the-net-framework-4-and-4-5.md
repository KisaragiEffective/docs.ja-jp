---
title: .NET Framework 4 および 4.5 で追加された CLR ホスト インターフェイス
ms.date: 03/30/2017
helpviewer_keywords:
- hosting interfaces [.NET Framework], version 4
- .NET Framework 4, hosting interfaces
- interfaces [.NET Framework hosting], version 4
ms.assetid: f6af6116-f5b0-4bda-a276-fffdba70893d
ms.openlocfilehash: 8484b47549f83795778420048d610e2d1626d87b
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75899719"
---
# <a name="clr-hosting-interfaces-added-in-the-net-framework-4-and-45"></a>.NET Framework 4 および 4.5 で追加された CLR ホスト インターフェイス
ここでは、アンマネージホストが .NET Framework 4、.NET Framework 4.5、およびそれ以降のバージョンの共通言語ランタイム (CLR) をアプリケーションに統合するために使用できるインターフェイスについて説明します。 これらのインターフェイスは、ホストがランタイムを構成してプロセスに読み込むためのメソッドを提供します。  
  
 .NET Framework 4 以降では、すべてのホストインターフェイスに次の特性があります。  
  
- 有効期間管理 (`AddRef` と `Release`)、カプセル化 (暗黙的なコンテキスト)、COM からの `QueryInterface` を使用します。  
  
- `BSTR`、`SAFEARRAY`、`VARIANT`などの COM 型は使用しません。  
  
- [CoCreateInstance 関数](/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstance)を使用するアパートメントモデル、集計、またはレジストリのアクティブ化はありません。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [ICLRAppDomainResourceMonitor インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md)  
 アプリケーションドメインのメモリおよび CPU 使用率を検査するメソッドを提供します。  
  
 [ICLRDomainManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrdomainmanager-interface.md)  
 既定のアプリケーションドメインを初期化し、初期化プロパティを指定するために使用されるアプリケーションドメインマネージャーをホストが指定できるようにします。  
  
 [ICLRGCManager2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrgcmanager2-interface.md)  
 [SetGCStartupLimitsEx](../../../../docs/framework/unmanaged-api/hosting/iclrgcmanager2-setgcstartuplimitsex-method.md)メソッドを提供します。これにより、ホストはガベージコレクションセグメントのサイズとガベージコレクションシステムのジェネレーション0の最大サイズを、`DWORD`より大きい値に設定できます。  
  
 [ICLRMetaHost インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-interface.md)  
 特定のバージョンの CLR を返し、インストールされているすべての CLRs の一覧を取得し、すべてのインプロセスランタイムを一覧表示し、アクティベーションインターフェイスを返し、アセンブリをコンパイルするために使用される CLR バージョンを検出するメソッドを提供します。  
  
 [ICLRMetaHostPolicy インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-interface.md)  
 ポリシー条件、マネージアセンブリ、バージョン、および構成ファイルに基づいて CLR インターフェイスを提供する[Getrequestedruntime](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md)メソッドを提供します。  
  
 [ICLRRuntimeInfo インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)  
 バージョン、ディレクトリ、読み込みステータスなど、特定のランタイムに関する情報を返すメソッドを提供します。  
  
 [ICLRStrongName インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname-interface.md)  
 厳密な名前でアセンブリに署名するための基本的なグローバル静的関数を提供します。 すべての[ICLRStrongName](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname-interface.md)メソッドは、標準 COM hresult を返します。  
  
 [ICLRStrongName2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname2-interface.md)  
 セキュリティで保護されたハッシュアルゴリズム (SHA-256、SHA-384、および SHA-512) の SHA-1 グループを使用して、厳密な名前を作成する機能を提供します。  
  
 [ICLRTask2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-interface.md)  
 には、 [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)のすべての機能が用意されています。また、には、現在のスレッドでスレッドの中止を遅延させることができるメソッドが用意されています。  
  
## <a name="related-sections"></a>関連セクション  
 [非推奨の CLR のホスト インターフェイスおよびコクラス](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-interfaces-and-coclasses.md)  
 .NET Framework バージョン1.0 および1.1 で提供されるホストインターフェイスについて説明します。  
  
 [CLR ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/clr-hosting-interfaces.md)  
 .NET Framework バージョン2.0、3.0、および3.5 で提供されるホストインターフェイスについて説明します。  
  
 [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)  
 .NET Framework でのホスティングについて説明します。
