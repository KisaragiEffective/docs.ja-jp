---
title: IMetaDataDispenserEx::SetOption メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx.SetOption
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx::SetOption
helpviewer_keywords:
- IMetaDataDispenserEx::SetOption method [.NET Framework metadata]
- SetOption method [.NET Framework metadata]
ms.assetid: 9f1c7ccd-7fb2-41d8-aa00-24b823376527
topic_type:
- apiref
ms.openlocfilehash: bc30f9fb120db92ccfc1e7dd0560b3fe18c54b29
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432730"
---
# <a name="imetadatadispenserexsetoption-method"></a>IMetaDataDispenserEx::SetOption メソッド
指定されたオプションを、現在のメタデータスコープの指定された値に設定します。 オプションは、現在のメタデータスコープへの呼び出しの処理方法を制御します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetOption (  
    [in] REFGUID optionId,   
    [in] const VARIANT *pValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `optionId`  
 から設定するオプションを指定する GUID へのポインター。  
  
 `pValue`  
 からオプションを設定するために使用する値。 この値の型は、指定されたオプションの型のバリアントである必要があります。  
  
## <a name="remarks"></a>コメント  
 次の表に、`optionId` パラメーターが指すことのできる Guid と、`pValue` パラメーターに対応する有効な値を示します。  
  
|GUID|説明|`pValue` パラメーター|  
|----------|-----------------|------------------------|  
|MetaDataCheckDuplicatesFor|重複しているかどうかをチェックする項目を制御します。 新しい項目を作成する[IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)メソッドを呼び出すたびに、その項目が現在のスコープ内に既に存在するかどうかを確認するようにメソッドに要求できます。 たとえば、`mdMethodDef` の項目があるかどうかを確認できます。この場合、 [IMetaDataEmit::D efinemethod](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemethod-method.md)を呼び出すと、メソッドが現在のスコープに存在していないかどうかがチェックされます。 このチェックでは、指定されたメソッドを一意に識別するキー (親の型、名前、および署名) を使用します。|は UI4 型のバリアントである必要があり、 [CorCheckDuplicatesFor](../../../../docs/framework/unmanaged-api/metadata/corcheckduplicatesfor-enumeration.md)列挙体の値の組み合わせを含んでいる必要があります。|  
|MetaDataRefToDefCheck|定義に変換される参照項目を制御します。 既定では、参照される項目が現在のスコープで実際に定義されている場合は、参照される項目をその定義に変換することで、メタデータエンジンによってコードが最適化されます。|は UI4 型のバリアントである必要があり、 [Correftodefcheck](../../../../docs/framework/unmanaged-api/metadata/correftodefcheck-enumeration.md)列挙体の値の組み合わせを含んでいる必要があります。|  
|MetaDataNotificationForTokenMovement|メタデータのマージ中に発生するトークンのリマップを制御してコールバックを生成します。 [IMetaDataEmit:: SetHandler](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-sethandler-method.md)メソッドを使用して、 [IMapToken](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md)インターフェイスを確立します。|は UI4 型のバリアントである必要があり、 [Cornotificationfortokenmovement](../../../../docs/framework/unmanaged-api/metadata/cornotificationfortokenmovement-enumeration.md)列挙の値の組み合わせを含んでいる必要があります。|  
|MetaDataSetENC|エディットコンティニュ (ENC) の動作を制御します。 一度に設定できる動作のモードは1つだけです。|は UI4 型のバリアントである必要があり、 [Corsetenc](../../../../docs/framework/unmanaged-api/metadata/corsetenc-enumeration.md)列挙子の値を含んでいる必要があります。 値がビットマスクではありません。|  
|MetaDataErrorIfEmitOutOfOrder|発生した順不同エラーがコールバックを生成するように制御します。 順序のないメタデータの出力は致命的ではありません。ただし、メタデータエンジンで優先される順序でメタデータを出力する場合、メタデータはよりコンパクトであるため、より効率的に検索することができます。 `IMetaDataEmit::SetHandler` メソッドを使用して、 [IMetaDataError](../../../../docs/framework/unmanaged-api/metadata/imetadataerror-interface.md)インターフェイスを確立します。|は UI4 型のバリアントである必要があり、 [CorErrorIfEmitOutOfOrder](../../../../docs/framework/unmanaged-api/metadata/corerrorifemitoutoforder-enumeration.md)列挙体の値の組み合わせを含んでいる必要があります。|  
|MetaDataImportOption|は、ENC 中に削除された項目の種類を、列挙子によって取得します。|は UI4 型のバリアントである必要があり、 [CorImportOptions 列挙](../../../../docs/framework/unmanaged-api/metadata/corimportoptions-enumeration.md)型の値の組み合わせを含んでいる必要があります。|  
|Metadatathreadsaf Etyoptions|メタデータエンジンが読み取り/書き込みロックを取得するかどうかを制御し、それによってスレッドセーフを確保します。 既定では、エンジンは、アクセスが呼び出し元によってシングルスレッドであることを前提としているため、ロックは取得されません。 クライアントは、メタデータ API を使用するときに、適切なスレッド同期を維持する役割を担います。|は UI4 型のバリアントである必要があり、 [Corthreadsaf Etyoptions](../../../../docs/framework/unmanaged-api/metadata/corthreadsafetyoptions-enumeration.md)列挙体の値を含んでいる必要があります。 値がビットマスクではありません。|  
|MetaDataGenerateTCEAdapters|タイプライブラリインポーターが COM 接続ポイントコンテナー用の密結合イベント (TCE) アダプターを生成する必要があるかどうかを制御します。|BOOL 型の variant である必要があります。 `pValue` が `true`に設定されている場合は、タイプライブラリインポーターによって、TCE アダプターが生成されます。|  
|MetaDataTypeLibImportNamespace|インポートされるタイプライブラリの既定以外の名前空間を指定します。|Null 値または BSTR 型のバリアントのいずれかである必要があります。 `pValue` が null 値の場合、現在の名前空間は null に設定されます。それ以外の場合、現在の名前空間は、バリアントの BSTR 型に保持されている文字列に設定されます。|  
|MetaDataLinkerOptions|リンカーがアセンブリまたは .NET Framework モジュールファイルを生成するかどうかを制御します。|は UI4 型のバリアントである必要があり、 [CorLinkerOptions](../../../../docs/framework/unmanaged-api/metadata/corlinkeroptions-enumeration.md)列挙体の値の組み合わせを含んでいる必要があります。|  
|MetaDataRuntimeVersion|このイメージの作成に使用された共通言語ランタイムのバージョンを指定します。 バージョンは、"v v1.0.3705" などの文字列として格納されます。|は、null 値、VT_EMPTY 値、または BSTR 型のバリアントである必要があります。 `pValue` が null の場合、ランタイムのバージョンは null に設定されます。 `pValue` が VT_EMPTY 場合、バージョンは既定値に設定されます。これは、メタデータコードが実行されている Mscorwks.dll のバージョンから描画されます。 それ以外の場合、ランタイムのバージョンは、バリアントの BSTR 型に保持されている文字列に設定されます。|  
|MetaDataMergerOptions|メタデータをマージするためのオプションを指定します。|UI4 型のバリアントである必要があります。また、CorHdr .h ファイルに記述されている `MergeFlags` 列挙型の値の組み合わせが含まれている必要があります。|  
|MetaDataPreserveLocalRefs|定義へのローカル参照の最適化を無効にします。|[Corlocalrefpreservation](../../../../docs/framework/unmanaged-api/metadata/corlocalrefpreservation-enumeration.md)列挙の値の組み合わせが含まれている必要があります。|  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataDispenserEx インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-interface.md)
- [IMetaDataDispenser インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-interface.md)
