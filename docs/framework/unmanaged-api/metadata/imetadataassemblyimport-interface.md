---
title: IMetaDataAssemblyImport インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport
helpviewer_keywords:
- IMetaDataAssemblyImport interface [.NET Framework metadata]
ms.assetid: 29c6fba5-4cea-417d-b484-7ed22ebff1c9
topic_type:
- apiref
ms.openlocfilehash: 289e26868ff2eb9e1d97cf084e9a888815062ea4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436306"
---
# <a name="imetadataassemblyimport-interface"></a>IMetaDataAssemblyImport インターフェイス
アセンブリ マニフェストの内容にアクセスして確認するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CloseEnum メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-closeenum-method.md)|指定した列挙子へのハンドルを解放します。|  
|[EnumAssemblyRefs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-enumassemblyrefs-method.md)|現在のメタデータスコープ内のアセンブリによって参照されるアセンブリの `mdAssemblyRef` トークンを格納している列挙子へのインターフェイスポインターを取得します。|  
|[EnumExportedTypes メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-enumexportedtypes-method.md)|現在のメタデータスコープ内のアセンブリによって参照される COM 型の `mdExportedType` トークンを格納している列挙子へのインターフェイスポインターを取得します。|  
|[EnumFiles メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-enumfiles-method.md)|現在のメタデータスコープ内のアセンブリによって参照されるファイルの `mdFile` トークンを格納している列挙子へのインターフェイスポインターを取得します。|  
|[EnumManifestResources メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-enummanifestresources-method.md)|現在のメタデータスコープ内のアセンブリによって参照されるリソースの `mdManifestResource` トークンを格納している列挙子へのインターフェイスポインターを取得します。|  
|[FindAssembliesByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-findassembliesbyname-method.md)|指定した名前のアセンブリの `mdAssemblyRef` トークンの配列を取得します。|  
|[FindExportedTypeByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-findexportedtypebyname-method.md)|指定した名前を持つ COM 型の `mdExportedType` トークンを取得します。|  
|[FindManifestResourceByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-findmanifestresourcebyname-method.md)|指定した名前のリソースの `mdManifestResource` トークンを取得します。|  
|[GetAssemblyFromScope メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getassemblyfromscope-method.md)|現在のメタデータスコープ内のアセンブリのトークンを取得します。|  
|[GetAssemblyProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getassemblyprops-method.md)|指定したアセンブリのプロパティ設定を取得します。|  
|[GetAssemblyRefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getassemblyrefprops-method.md)|指定された `mdAssemblyRef` トークンのプロパティ設定を取得します。|  
|[GetExportedTypeProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getexportedtypeprops-method.md)|指定した COM 型のプロパティ設定を取得します。|  
|[GetFileProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getfileprops-method.md)|指定されたファイルのプロパティ設定を取得します。|  
|[GetManifestResourceProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-getmanifestresourceprops-method.md)|指定されたマニフェストリソースのプロパティ設定を取得します。|  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
