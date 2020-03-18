---
title: ICorPublishProcess インターフェイス
ms.date: 03/30/2017
api_name:
- ICorPublishProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishProcess
helpviewer_keywords:
- ICorPublishProcess interface [.NET Framework debugging]
ms.assetid: 6d1dc41b-8aa2-4889-bb00-1cbccc00c123
topic_type:
- apiref
ms.openlocfilehash: 3ae48df9e66890161c1aef944d37b0a279939d56
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790539"
---
# <a name="icorpublishprocess-interface"></a>ICorPublishProcess インターフェイス
プロセスについて表示される情報にアクセスするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnumAppDomains メソッド](icorpublishprocess-enumappdomains-method.md)|この `ICorPublishProcess`によって参照されるプロセス内のアプリケーションドメインを含む[ICorPublishAppDomainEnum](icorpublishappdomainenum-interface.md)インスタンスを取得します。|  
|[GetDisplayName メソッド](icorpublishprocess-getdisplayname-method.md)|この `ICorPublishProcess`によって参照されるプロセスの実行可能ファイルの完全パスを取得します。|  
|[GetProcessID メソッド](icorpublishprocess-getprocessid-method.md)|この `ICorPublishProcess`によって参照されるプロセスのオペレーティングシステム識別子を取得します。|  
|[IsManaged メソッド](icorpublishprocess-ismanaged-method.md)|この `ICorPublishProcess` によって参照されるプロセスがマネージコードを実行していることがわかっているかどうかを示す値を取得します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorPub .idl、CorPub .h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [CorpubPublish コクラス](corpubpublish-coclass.md)
