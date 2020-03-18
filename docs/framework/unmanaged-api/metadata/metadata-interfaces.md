---
title: メタデータ インターフェイス
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], metadata
- metadata interfaces [.NET Framework]
- interfaces (.NET Framework metadata]
ms.assetid: f5cdac93-a28c-48ef-8a19-5773376e9e7c
ms.openlocfilehash: 4672cb813cec4a127f7888a2273eb26c3f34c3d9
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431590"
---
# <a name="metadata-interfaces"></a>メタデータ インターフェイス
このセクションでは、.NET Framework の型、メソッド、フィールドなどによって公開されるメタデータにアクセスできるようにするアンマネージ インターフェイスについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [ICeeGen インターフェイス](../../../../docs/framework/unmanaged-api/metadata/iceegen-interface.md)  
 動的なコード コンパイルのためのメソッドを提供します。  
  
 [IHostFilter インターフェイス](../../../../docs/framework/unmanaged-api/metadata/ihostfilter-interface.md)  
 ランタイム ホストがメタデータ トークンに処理済みのマークを付けるためのメソッドを提供します。  
  
 [IMapToken インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md)  
 インポートされたメタデータ署名と生成されたメタデータ署名との間の割り当て機能を提供します。  
  
 [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)  
 共通言語ランタイム (CLR) がリソースの解決および消費に使用する自己記述モデルをサポートするメソッドを提供します。  
  
 [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)  
 アセンブリ マニフェストの内容にアクセスして確認するメソッドを提供します。  
  
 [IMetaDataConverter インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataconverter-interface.md)  
 タイプ ライブラリをそれぞれのメタデータ署名にマップして、一方から他方に変換するメソッドを提供します。  
  
 [IMetaDataDispenser インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-interface.md)  
 `IMetaDataDispenser` は互換性のために残されています。 代わりに、 `IMetaDataDispenserEx` を使用してください。  
  
 [IMetaDataDispenserEx インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-interface.md)  
 メタデータを作成または変更するためのメモリ領域を割り当てるメソッドを提供します。  
  
 [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)  
 現在定義されているスコープ内のアセンブリについてのメタデータを作成、変更、格納するメソッドを提供します。  
  
 [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)  
 <xref:System.Type?displayProperty=nameWithType> 型のパラメーターを持つメソッドおよびコンストラクターのメタデータ署名を定義および変更するメソッドを提供します。  
  
 [IMetaDataError インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataerror-interface.md)  
 アセンブリのメタデータ署名の解決時のエラーを報告するためのコールバック機構を提供します。  
  
 [IMetaDataFilter インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatafilter-interface.md)  
 メタデータ トークンにマークを付け、フィルター処理をして、既に実行されたアクションが繰り返し行われないようにするメソッドを提供します。  
  
 [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)  
 他のアセンブリの型をインポートおよび操作するメソッドを提供します。  
  
 [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)  
 `IMetaDataImport` を拡張して、ジェネリック型を使用できるようにします。  
  
 [IMetaDataInfo インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatainfo-interface.md)  
 ディスク上のファイルからメモリへのメタデータのマッピングに関する情報を取得するメソッドを提供します。  
  
 [IMetaDataTables インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-interface.md)  
 テーブル内のメタデータ情報の格納と取得のためのメソッドを提供します。  
  
 [IMetaDataTables2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatatables2-interface.md)  
 `IMetaDataTables` を拡張して、メタデータ ストリームを使用するためのメソッドを含めます。  
  
 [IMetaDataValidate インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatavalidate-interface.md)  
 メタデータ署名の検証で使用するメソッドを提供します。  
  
## <a name="related-sections"></a>関連セクション  
 [メタデータ グローバル静的関数](../../../../docs/framework/unmanaged-api/metadata/metadata-global-static-functions.md)  
  
 [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)  
  
 [メタデータ構造体](../../../../docs/framework/unmanaged-api/metadata/metadata-structures.md)  
  
 [メタデータ共用体](../../../../docs/framework/unmanaged-api/metadata/metadata-unions.md)
