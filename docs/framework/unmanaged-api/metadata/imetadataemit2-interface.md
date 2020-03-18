---
title: IMetaDataEmit2 インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2
helpviewer_keywords:
- IMetaDataEmit2 interface [.NET Framework metadata]
ms.assetid: 866dc96b-bbfc-4c0f-80c2-38ce93072106
topic_type:
- apiref
ms.openlocfilehash: 7ceae6f7713ab0eb1feff550838325df0ea52de2
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447918"
---
# <a name="imetadataemit2-interface"></a>IMetaDataEmit2 インターフェイス
[IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)インターフェイスを拡張して、主にジェネリック型を操作できるようにします。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[DefineGenericParam メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-definegenericparam-method.md)|ジェネリック型パラメーターの定義を作成し、そのジェネリック型パラメーターへのトークンを取得します。|  
|[DefineMethodSpec メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-definemethodspec-method.md)|メソッドのジェネリックインスタンスを作成し、その定義へのトークンを取得します。|  
|[GetDeltaSaveSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-getdeltasavesize-method.md)|現在のエディットコンティニュセッションの変更を表すために必要なデータのサイズの差を示す値を取得します。|  
|[ResetENCLog メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-resetenclog-method.md)|エディットコンティニュログをリセットし、新しいセッションを開始します。|  
|[SaveDelta メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-savedelta-method.md)|現在のエディットコンティニュセッションから、指定したファイルへの変更を保存します。|  
|[SaveDeltaToMemory メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-savedeltatomemory-method.md)|現在のエディットコンティニュセッションの変更をメモリに保存します。|  
|[SaveDeltaToStream メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-savedeltatostream-method.md)|現在のエディットコンティニュセッションから、指定されたストリームに変更を保存します。|  
|[SetGenericParamProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-setgenericparamprops-method.md)|指定したトークンによって参照されるジェネリックパラメーター定義のプロパティ値を設定します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
