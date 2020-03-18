---
title: IMetaDataTables インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataTables
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables
helpviewer_keywords:
- IMetaDataTables interface [.NET Framework metadata]
ms.assetid: 31272cce-506a-4f18-bcbf-01ee45e36356
topic_type:
- apiref
ms.openlocfilehash: 17305f2c088dd6f479da4c823d3db0fd50c0b3d7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443227"
---
# <a name="imetadatatables-interface"></a>IMetaDataTables インターフェイス
テーブル内のメタデータ情報の格納と取得のためのメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetBlob メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getblob-method.md)|指定した列インデックスにあるバイナリラージオブジェクト (BLOB) へのポインターを取得します。|  
|[GetBlobHeapSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getblobheapsize-method.md)|BLOB ヒープのサイズ (バイト単位) を取得します。|  
|[GetCodedTokenInfo メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcodedtokeninfo-method.md)|指定した行インデックスに関連付けられているトークンの配列へのポインターを取得します。|  
|[GetColumn メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcolumn-method.md)|指定したテーブルインデックスにあるテーブル内の、指定した列インデックスにある列に格納されている値へのポインターを取得します。|  
|[GetColumnInfo メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcolumninfo-method.md)|指定されたテーブル内の指定された列に関するデータを取得します。|  
|[GetGuid メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getguid-method.md)|指定したインデックス位置にある行から GUID を取得します。|  
|[GetGuidHeapSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getguidheapsize-method.md)|GUID ヒープのサイズ (バイト単位) を取得します。|  
|[GetNextBlob メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextblob-method.md)|テーブル内の次の BLOB のインデックスを取得します。|  
|[GetNextGuid メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextguid-method.md)|現在のテーブル列の次の GUID 値のインデックスを取得します。|  
|[GetNextString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextstring-method.md)|現在のテーブル列の次の文字列のインデックスを取得します。|  
|[GetNextUserString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextuserstring-method.md)|現在のテーブル列の次のハードコーディングされた文字列を含む行のインデックスを取得します。|  
|[GetNumTables メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnumtables-method.md)|現在の `IMetaDataTables` インスタンスのスコープ内にあるテーブルの数を取得します。|  
|[GetRow メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getrow-method.md)|指定したテーブルインデックスにあるテーブル内の指定した行インデックスにある行を取得します。|  
|[GetString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getstring-method.md)|現在の参照スコープのテーブル列から、指定したインデックス位置にある文字列を取得します。|  
|[GetStringHeapSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getstringheapsize-method.md)|文字列ヒープのサイズ (バイト単位) を取得します。|  
|[GetTableIndex メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-gettableindex-method.md)|指定したトークンによって参照されるテーブルのインデックスを取得します。|  
|[GetTableInfo メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-gettableinfo-method.md)|指定されたテーブルインデックスにあるテーブルの名前、行のサイズ、行数、列の数、およびキー列のインデックスを取得します。|  
|[GetUserString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getuserstring-method.md)|現在のスコープ内の文字列列にある、指定したインデックス位置にあるハードコーディングされた文字列を取得します。|  
|[GetUserStringHeapSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getuserstringheapsize-method.md)|ユーザー文字列ヒープのサイズ (バイト単位) を取得します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataTables2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatatables2-interface.md)
