---
title: IMetaDataEmit::GetSaveSize メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.GetSaveSize
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::GetSaveSize
helpviewer_keywords:
- IMetaDataEmit::GetSaveSize method [.NET Framework metadata]
- GetSaveSize method [.NET Framework metadata]
ms.assetid: 8aea2e2c-23a3-4cda-9a06-e19f97383830
topic_type:
- apiref
ms.openlocfilehash: 125a63638a41707b8eed918253cb1f93abb907eb
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74434325"
---
# <a name="imetadataemitgetsavesize-method"></a>IMetaDataEmit::GetSaveSize メソッド
現在のスコープ内のアセンブリとそのメタデータの推定バイナリサイズを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetSaveSize (  
    [in]  CorSaveSize fSave,  
    [out] DWORD       *pdwSaveSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `fSave`  
 から正確またはおおよそのサイズを取得するかどうかを指定する[CorSaveSize](../../../../docs/framework/unmanaged-api/metadata/corsavesize-enumeration.md)列挙体の値。 有効な値は、cssAccurate、cssQuick、cssDiscardTransientCAs の3つだけです。  
  
- cssAccurate は正確な保存サイズを返しますが、計算にかかる時間は長くなります。  
  
- cssQuick は、サイズを返します。これは安全性のために埋め込まれていますが、計算にかかる時間は短くなります。  
  
- cssDiscardTransientCAs は、破棄可能なカスタム属性を破棄できることを `GetSaveSize` に伝えます。  
  
 `pdwSaveSize`  
 入出力ファイルを保存するために必要なサイズへのポインター。  
  
## <a name="remarks"></a>コメント  
 `GetSaveSize` は、現在のスコープ内のアセンブリとそのすべてのメタデータを保存するために必要な領域をバイト単位で計算します。 ( [IMetaDataEmit:: SaveToStream](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-savetostream-method.md)メソッドを呼び出すと、このバイト数が出力されます)。  
  
 呼び出し元が[IMapToken](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md)インターフェイス ( [IMetaDataEmit:: SetHandler](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-sethandler-method.md)または[IMetaDataEmit:: Merge](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-merge-method.md)) を実装している場合、`GetSaveSize` はメタデータに対して2つのパスを実行し、最適化と圧縮を行います。 それ以外の場合、最適化は実行されません。  
  
 最適化が実行された場合、最初のパスは単にメタデータ構造を並べ替えて、インポート時の検索のパフォーマンスを調整します。 この手順では、通常、後で参照するためにツールによって保持されているトークンが無効になるという副作用で、レコードを移動します。 ただし、メタデータは、2番目のパスの後に、これらのトークンの変更を呼び出し元に通知しません。 2番目のパスでは、現在のメタデータスコープ内で宣言されている型またはメンバーへの参照である場合に、メタ `mdMemberRef` `mdTypeRef` データの全体的なサイズを縮小することを目的としたさまざまな最適化を実行します このパスでは、別のトークンマッピングのラウンドが発生します。 このパスが経過すると、メタデータエンジンは、変更されたトークン値の `IMapToken` インターフェイスを介して呼び出し元に通知します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
