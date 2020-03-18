---
title: IMetaDataDispenserEx インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx
helpviewer_keywords:
- IMetaDataDispenserEx interface [.NET Framework metadata]
ms.assetid: 78b3629e-77a2-4406-89c3-56b5cc2c4594
topic_type:
- apiref
ms.openlocfilehash: 985cdea670714394119fb846e9e55a01713559a9
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431144"
---
# <a name="imetadatadispenserex-interface"></a>IMetaDataDispenserEx インターフェイス
[IMetaDataDispenser インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-interface.md)インターフェイスを拡張して、メタデータ api が現在のメタデータスコープに対してどのように動作するかを制御する機能を提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[FindAssembly メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-findassembly-method.md)|このメソッドは実装されていません。 呼び出された場合は E_NOTIMPL を返します。|  
|[FindAssemblyModule メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-findassemblymodule-method.md)|このメソッドは実装されていません。 呼び出された場合は E_NOTIMPL を返します。|  
|[GetCORSystemDirectory メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-getcorsystemdirectory-method.md)|現在の共通言語ランタイム (CLR) が格納されているディレクトリを取得します。 このメソッドは、アウトプロセスデバッガーでの使用に対してのみサポートされています。 別のコンポーネントから呼び出された場合は、E_NOTIMPL を返します。|  
|[GetOption メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-getoption-method.md)|現在のメタデータスコープの指定したオプションの値を取得します。 オプションは、現在のメタデータスコープへの呼び出しの処理方法を制御します。|  
|[OpenScopeOnITypeInfo メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-openscopeonitypeinfo-method.md)|このメソッドは実装されていません。 呼び出された場合は E_NOTIMPL を返します。|  
|[SetOption メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-setoption-method.md)|指定されたオプションを、現在のメタデータスコープの指定された値に設定します。 オプションは、現在のメタデータスコープへの呼び出しの処理方法を制御します。|  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataDispenser インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-interface.md)
- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
