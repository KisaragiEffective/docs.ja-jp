---
title: IAssemblyName インターフェイス
ms.date: 03/30/2017
api_name:
- IAssemblyName
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName
helpviewer_keywords:
- IAssemblyName interface [.NET Framework fusion]
ms.assetid: f7f8e605-6b67-4151-936f-f04ecd671d90
topic_type:
- apiref
ms.openlocfilehash: de49d66667033dfc6918b139f90cd5523661597f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73134317"
---
# <a name="iassemblyname-interface"></a>IAssemblyName インターフェイス
アセンブリの一意の id を記述および操作するためのメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Clone メソッド](iassemblyname-clone-method.md)|この `IAssemblyName` オブジェクトの簡易コピーを作成します。|  
|[Finalize メソッド](iassemblyname-finalize-method.md)|デストラクターが呼び出される前に、この `IAssemblyName` オブジェクトがリソースを解放し、その他のクリーンアップ操作を実行できるようにします。|  
|[GetDisplayName メソッド](iassemblyname-getdisplayname-method.md)|この `IAssemblyName` オブジェクトによって参照されるアセンブリの、人間が判読できる名前を取得します。|  
|[GetName メソッド](iassemblyname-getname-method.md)|この `IAssemblyName` オブジェクトによって参照されるアセンブリの単純な、暗号化されていない名前を取得します。|  
|[GetProperty メソッド](iassemblyname-getproperty-method.md)|指定した `PropertyId`によって参照されるプロパティへのポインターを取得します。|  
|[GetVersion メソッド](iassemblyname-getversion-method.md)|この `IAssemblyName` オブジェクトによって参照されるアセンブリのバージョン情報を取得します。|  
|[IsEqual メソッド](iassemblyname-isequal-method.md)|指定した比較フラグに基づいて、指定した `IAssemblyName` オブジェクトがこの `IAssemblyName`と等しいかどうかを判断します。|  
|[SetProperty メソッド](iassemblyname-setproperty-method.md)|指定した `PropertyId`によって参照されるプロパティの値を設定します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
- [IAssemblyEnum インターフェイス](iassemblyenum-interface.md)
