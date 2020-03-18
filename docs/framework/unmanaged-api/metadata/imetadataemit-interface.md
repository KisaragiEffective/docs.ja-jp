---
title: IMetaDataEmit インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataEmit
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit
helpviewer_keywords:
- IMetaDataEmit interface [.NET Framework metadata]
ms.assetid: 3b48fd47-7397-4e2c-8bec-8157aa08978c
topic_type:
- apiref
ms.openlocfilehash: b4ae599a0e5cdb604fd9a610728671b39c67af31
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74440901"
---
# <a name="imetadataemit-interface"></a>IMetaDataEmit インターフェイス
現在定義されているスコープ内のアセンブリに関するメタデータを作成、変更、および保存するためのメソッドを提供します。 メタデータは、メモリに格納することも、ディスクに保存することもできます。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ApplyEditAndContinue メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-applyeditandcontinue-method.md)|指定した `pImport`に加えられた変更を使用して、現在のアセンブリスコープを更新します。|  
|[DefineCustomAttribute メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definecustomattribute-method.md)|指定したメタデータシグネチャを使用して、指定したオブジェクトにアタッチするカスタム属性の定義を作成し、そのカスタム属性定義へのトークンを取得します。|  
|[DefineEvent メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineevent-method.md)|指定したメタデータシグネチャを持つイベントの定義を作成し、そのイベント定義へのトークンを取得します。|  
|[DefineField メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definefield-method.md)|指定したメタデータシグネチャを持つフィールドの定義を作成し、そのフィールド定義へのトークンを取得します。|  
|[DefineImportMember メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineimportmember-method.md)|現在のスコープ外のモジュールで定義されている型のメンバーの定義を作成し、その参照定義のトークンを取得します。|  
|[DefineImportType メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineimporttype-method.md)|現在のスコープ外のモジュールで定義されている型への参照の定義を作成し、その参照定義へのトークンを取得します。|  
|[DefineMemberRef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definememberref-method.md)|現在のスコープ外のモジュールのメンバーへの参照の定義を作成し、その参照定義へのトークンを取得します。|  
|[DefineMethod メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemethod-method.md)|指定したシグネチャを使用してメソッドの定義を作成し、そのメソッド定義に対するトークンを返します。|  
|[DefineMethodImpl メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemethodimpl-method.md)|インターフェイスから継承されたメソッドの実装の定義を作成し、そのメソッド実装定義にトークンを返します。|  
|[DefineModuleRef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemoduleref-method.md)|指定された名前を持つモジュールのメタデータシグネチャを作成します。|  
|[DefineNestedType メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definenestedtype-method.md)|型定義のメタデータシグネチャを作成し、その型の `mdTypeDef` トークンを返します。さらに、定義された型が `tdEncloser`によって参照される型のメンバーであることを指定します。|  
|[DefineParam メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineparam-method.md)|指定したトークンによって参照されるメソッドに対して、指定したシグネチャを持つパラメーター定義を作成し、そのパラメーター定義のトークンを取得します。|  
|[DefinePermissionSet メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definepermissionset-method.md)|指定したメタデータシグネチャを持つアクセス許可セットの定義を作成し、そのアクセス許可セットの定義に対するトークンを取得します。|  
|[DefinePinvokeMap メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definepinvokemap-method.md)|指定したトークンによって参照されるメソッドの PInvoke 署名の特徴を設定します。|  
|[DefineProperty メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineproperty-method.md)|指定した `get` および `set` メソッドアクセサーを使用して、指定した型のプロパティ定義を作成し、そのプロパティ定義へのトークンを取得します。|  
|[DefineSecurityAttributeSet メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definesecurityattributeset-method.md)|指定したトークンによって参照されるオブジェクトにアタッチするセキュリティアクセス許可のセットを作成します。|  
|[DefineTypeDef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definetypedef-method.md)|共通言語ランタイム型の型定義を作成し、その型定義へのメタデータトークンを取得します。|  
|[DefineTypeRefByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definetyperefbyname-method.md)|現在のスコープ外の別のモジュールで定義されている型のメタデータトークンを取得します。|  
|[DefineUserString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineuserstring-method.md)|指定されたリテラル文字列のメタデータトークンを取得します。|  
|[DeleteClassLayout メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-deleteclasslayout-method.md)|指定したトークンによって参照される型のクラスレイアウトメタデータシグネチャを破棄します。|  
|[DeleteFieldMarshal メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-deletefieldmarshal-method.md)|指定したトークンによって参照されるオブジェクトの PInvoke マーシャリングメタデータ署名を破棄します。|  
|[DeletePinvokeMap メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-deletepinvokemap-method.md)|指定したトークンによって参照されるオブジェクトの PInvoke マッピングメタデータを破棄します。|  
|[DeleteToken メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-deletetoken-method.md)|現在のメタデータスコープから指定されたトークンを削除します。|  
|[GetSaveSize メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-getsavesize-method.md)|現在のスコープ内のアセンブリの推定バイナリサイズを取得します。|  
|[GetTokenFromSig メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-gettokenfromsig-method.md)|指定したメタデータシグネチャのトークンを取得します。|  
|[GetTokenFromTypeSpec メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-gettokenfromtypespec-method.md)|指定したメタデータシグネチャを持つ型のメタデータトークンを取得します。|  
|[Merge メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-merge-method.md)|マージするスコープの一覧に、指定したインポートされたスコープを追加します。|  
|[MergeEnd メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-mergeend-method.md)|`IMetaDataEmit::Merge`の1つ以上の前の呼び出しで指定されたすべてのメタデータスコープを現在のスコープにマージします。|  
|[Save メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-save-method.md)|現在のスコープ内のすべてのメタデータを、指定したアドレスにあるファイルに保存します。|  
|[SaveToMemory メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-savetomemory-method.md)|現在のスコープ内のすべてのメタデータを、指定したメモリ領域に保存します。|  
|[SaveToStream メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-savetostream-method.md)|現在のスコープ内のすべてのメタデータを、指定した `IStream`に保存します。|  
|[SetClassLayout メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setclasslayout-method.md)|`IMetaDataEmit::DefineTypeDef`の前の呼び出しで定義された型のクラスレイアウト署名を設定または更新します。|  
|[SetCustomAttributeValue メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setcustomattributevalue-method.md)|`IMetaDataEmit::DefineCustomAttribute`の前の呼び出しで定義されたカスタム属性の値を設定または更新します。|  
|[SetEventProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-seteventprops-method.md)|`IMetaDataEmit::DefineEvent`の前の呼び出しで定義されたイベントの指定された機能を設定または更新します。|  
|[SetFieldMarshal メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setfieldmarshal-method.md)|指定したトークンによって参照されるフィールド、メソッドの戻り値、またはメソッドパラメーターの PInvoke マーシャリング情報を設定します。|  
|[SetFieldProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setfieldprops-method.md)|指定したフィールドトークンによって参照されるフィールドの既定値を設定または更新します。|  
|[SetFieldRVA メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setfieldrva-method.md)|指定したトークンによって参照されるフィールドの相対仮想アドレスのグローバル変数値を設定します。|  
|[SetHandler メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-sethandler-method.md)|指定した `IUnknown` ポインターによって参照されるメソッドを、トークンリマップの通知コールバックとして設定します。|  
|[SetMethodImplFlags メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setmethodimplflags-method.md)|指定したトークンによって参照される、継承されたメソッドの実装のメタデータシグネチャを設定または更新します。|  
|[SetMethodProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setmethodprops-method.md)|`IMetaDataEmit::DefineMethod`の前の呼び出しで定義されたメソッドの、指定した相対仮想アドレスに格納されている機能を設定または更新します。|  
|[SetModuleProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setmoduleprops-method.md)|`IMetaDataEmit::DefineModuleRef`の前の呼び出しで定義されたモジュールへの参照を更新します。|  
|[SetParamProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setparamprops-method.md)|`IMetaDataEmit::DefineParam`の前の呼び出しで定義されたメソッドパラメーターの機能を設定または変更します。|  
|[SetParent メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setparent-method.md)|前の `IMetaDataEmit::DefineMemberRef`の呼び出しで定義されている指定されたメンバーが、`IMetaDataEmit::DefineTypeDef`の前の呼び出しで定義されている、指定された型のメンバーであることを確立します。|  
|[SetPermissionSetProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setpermissionsetprops-method.md)|`IMetaDataEmit::DefinePermissionSet`の前の呼び出しで定義されたアクセス許可セットのメタデータ署名の機能を設定または更新します。|  
|[SetPinvokeMap メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setpinvokemap-method.md)|`IMetaDataEmit::DefinePinvokeMap`の前の呼び出しで定義されているように、メソッドの PInvoke 署名の機能を設定または変更します。|  
|[SetPropertyProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setpropertyprops-method.md)|以前の `IMetaDataEmit::DefineProperty`の呼び出しで定義されたプロパティのメタデータに格納されている機能を設定します。|  
|[SetRVA メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-setrva-method.md)|指定したメソッドの相対仮想アドレスを設定します。|  
|[SetTypeDefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-settypedefprops-method.md)|以前の `IMetaDataEmit::DefineTypeDef`の呼び出しで定義された型の機能を設定します。|  
|[TranslateSigWithScope メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-translatesigwithscope-method.md)|現在のスコープにアセンブリをインポートし、マージされたスコープの新しいメタデータシグネチャを取得します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
