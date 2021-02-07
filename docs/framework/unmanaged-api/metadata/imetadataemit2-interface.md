---
description: 詳細については、「IMetaDataEmit2 インターフェイス」を参照してください。
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
ms.openlocfilehash: db1880d64bf3b1e9084d6745251c174788a4afe7
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99745685"
---
# <a name="imetadataemit2-interface"></a>IMetaDataEmit2 インターフェイス

[IMetaDataEmit](imetadataemit-interface.md)インターフェイスを拡張して、主にジェネリック型を操作できるようにします。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[DefineGenericParam メソッド](imetadataemit2-definegenericparam-method.md)|ジェネリック型パラメーターの定義を作成し、そのジェネリック型パラメーターへのトークンを取得します。|  
|[DefineMethodSpec メソッド](imetadataemit2-definemethodspec-method.md)|メソッドのジェネリックインスタンスを作成し、その定義へのトークンを取得します。|  
|[GetDeltaSaveSize メソッド](imetadataemit2-getdeltasavesize-method.md)|現在のエディットコンティニュセッションの変更を表すために必要なデータのサイズの差を示す値を取得します。|  
|[ResetENCLog メソッド](imetadataemit2-resetenclog-method.md)|エディットコンティニュログをリセットし、新しいセッションを開始します。|  
|[SaveDelta メソッド](imetadataemit2-savedelta-method.md)|現在のエディットコンティニュセッションから、指定したファイルへの変更を保存します。|  
|[SaveDeltaToMemory メソッド](imetadataemit2-savedeltatomemory-method.md)|現在のエディットコンティニュセッションの変更をメモリに保存します。|  
|[SaveDeltaToStream メソッド](imetadataemit2-savedeltatostream-method.md)|現在のエディットコンティニュセッションから、指定されたストリームに変更を保存します。|  
|[SetGenericParamProps メソッド](imetadataemit2-setgenericparamprops-method.md)|指定したトークンによって参照されるジェネリックパラメーター定義のプロパティ値を設定します。|  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** MsCorEE.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ インターフェイス](metadata-interfaces.md)
- [IMetaDataEmit インターフェイス](imetadataemit-interface.md)
