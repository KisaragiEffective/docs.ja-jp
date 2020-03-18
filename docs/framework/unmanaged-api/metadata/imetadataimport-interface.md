---
title: IMetaDataImport インターフェイス
ms.date: 03/30/2017
api_name:
- IMetaDataImport
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport
helpviewer_keywords:
- IMetaDataImport interface [.NET Framework metadata]
ms.assetid: 0adbbd35-5e8d-4fec-8268-dc70a07c5975
topic_type:
- apiref
ms.openlocfilehash: 2d45a369def193b9c8d8f9a3aa954ede600a87dd
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74434738"
---
# <a name="imetadataimport-interface"></a>IMetaDataImport インターフェイス
ポータブル実行可能 (PE) ファイルまたはその他のソース (タイプ ライブラリ、スタンドアロンのランタイム メタデータ バイナリなど) から既存のメタデータをインポートおよび操作するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CloseEnum メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-closeenum-method.md)|指定したハンドルを持つ列挙子を閉じます。|  
|[CountEnum メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-countenum-method.md)|指定されたハンドルを持つ列挙子内の要素の数を取得します。|  
|[EnumCustomAttributes メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumcustomattributes-method.md)|指定した型またはメンバーに関連付けられているカスタム属性定義トークンのリストを列挙します。|  
|[EnumEvents メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumevents-method.md)|指定した TypeDef トークンのイベント定義トークンを列挙します。|  
|[EnumFields メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumfields-method.md)|指定した TypeDef トークンによって参照される型の FieldDef トークンを列挙します。|  
|[EnumFieldsWithName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumfieldswithname-method.md)|指定した名前を持つ指定した型の FieldDef トークンを列挙します。|  
|[EnumInterfaceImpls メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enuminterfaceimpls-method.md)|インターフェイス実装を表す MethodDef トークンを列挙します。|  
|[EnumMemberRefs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummemberrefs-method.md)|指定した型のメンバーを表す MemberRef トークンを列挙します。|  
|[EnumMembers メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummembers-method.md)|指定した型のメンバーを表す MemberDef トークンを列挙します。|  
|[EnumMembersWithName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummemberswithname-method.md)|指定した名前を持つ指定した型のメンバーを表す MemberDef トークンを列挙します。|  
|[EnumMethodImpls メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummethodimpls-method.md)|指定した型のメソッドを表す MethodBody トークンと MethodDeclaration トークンを列挙します。|  
|[EnumMethods メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummethods-method.md)|指定した型のメソッドを表す MethodDef トークンを列挙します。|  
|[EnumMethodSemantics メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummethodsemantics-method.md)|指定したメソッドが関連付けられているプロパティおよびプロパティ変更イベントを列挙します。|  
|[EnumMethodsWithName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummethodswithname-method.md)|指定された名前を持ち、指定された TypeDef トークンによって参照される型で定義されているメソッドを列挙します。|  
|[EnumModuleRefs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enummodulerefs-method.md)|インポートされたモジュールを表す ModuleRef トークンを列挙します。|  
|[EnumParams メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumparams-method.md)|指定した MethodDef トークンによって参照されるメソッドのパラメーターを表す ParamDef トークンを列挙します。|  
|[EnumPermissionSets メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumpermissionsets-method.md)|指定したメタデータ スコープ内のオブジェクトのアクセス許可を列挙します。|  
|[EnumProperties メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumproperties-method.md)|指定した TypeDef トークンによって参照される型のプロパティを表す PropertyDef トークンを列挙します。|  
|[EnumSignatures メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumsignatures-method.md)|現在のスコープ内のスタンドアロン シグネチャを表す Signature トークンを列挙します。|  
|[EnumTypeDefs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumtypedefs-method.md)|現在のスコープ内のすべての型を表す TypeDef トークンを列挙します。|  
|[EnumTypeRefs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumtyperefs-method.md)|現在のメタデータ スコープに定義されている TypeRef トークンを列挙します。|  
|[EnumTypeSpecs メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumtypespecs-method.md)|現在のメタデータ スコープに定義されている TypeSpec トークンを列挙します。|  
|[EnumUnresolvedMethods メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumunresolvedmethods-method.md)|現在のメタデータ スコープ内の未解決のメソッドを表す MemberDef トークンを列挙します。|  
|[EnumUserStrings メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-enumuserstrings-method.md)|現在のメタデータ スコープ内にあるハードコーディングされた文字列を表す String トークンを列挙します。|  
|[FindField メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findfield-method.md)|指定した型のメンバーであり、さらに指定した名前とメタデータ シグネチャを持つフィールドの FieldDef トークンを取得します。|  
|[FindMember メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findmember-method.md)|指定した名前とメタデータ シグネチャを持ち、指定した型で定義されるメンバーの MemberDef トークンへのポインターを取得します。|  
|[FindMemberRef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findmemberref-method.md)|指定した名前とメタデータ シグネチャを持ち、指定した型で定義されるメンバーの MemberRef トークンへのポインターを取得します。|  
|[FindMethod メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findmethod-method.md)|指定した名前とメタデータ シグネチャを持ち、指定した型で定義されるメソッドの MethodDef トークンへのポインターを取得します。|  
|[FindTypeDefByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findtypedefbyname-method.md)|指定した名前の型の TypeDef メタデータ トークンへのポインターを取得します。|  
|[FindTypeRef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findtyperef-method.md)|指定した検索スコープ内にある、指定した名前の型を参照する TypeRef メタデータ トークンへのポインターを取得します。|  
|[GetClassLayout メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getclasslayout-method.md)|指定した TypeDef トークンによって参照されるクラスのレイアウト情報を取得します。|  
|[GetCustomAttributeByName メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getcustomattributebyname-method.md)|指定した名前のカスタム属性の値を取得します。|  
|[GetCustomAttributeProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getcustomattributeprops-method.md)|指定したメタデータ トークンのカスタム属性の値を取得します。|  
|[GetEventProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-geteventprops-method.md)|指定したイベント トークンによって表されるイベントのメタデータ情報を取得します。この情報には、宣言型、デリゲートの add メソッドおよび remove メソッド、任意のフラグとその他の関連付けられているデータなどがあります。|  
|[GetFieldMarshal メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getfieldmarshal-method.md)|指定した Field メタデータ トークンによって表されるフィールドのネイティブなアンマネージ型へのポインターを取得します。|  
|[GetFieldProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getfieldprops-method.md)|指定した FieldDef トークンによって参照されるフィールドに関連付けられているメタデータを取得します。|  
|[GetInterfaceImplProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getinterfaceimplprops-method.md)|指定したメソッドを実装する型、およびそのメソッドを宣言するインターフェイスのメタデータ トークンへのポインターを取得します。|  
|[GetMemberProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmemberprops-method.md)|指定したメタデータ トークンによって参照される型メンバーのメタデータ情報 (名前、バイナリ シグネチャ、相対仮想アドレスなど) を取得します。|  
|[GetMemberRefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmemberrefprops-method.md)|指定したトークンによって参照されるメンバーに関連付けられているメタデータを取得します。|  
|[GetMethodProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmethodprops-method.md)|指定した MethodDef トークンによって参照されるメソッドに関連付けられているメタデータを取得します。|  
|[GetMethodSemantics メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmethodsemantics-method.md)|指定した MethodDef トークンによって参照されるメソッドと、指定した EventProp トークンによって参照されるプロパティとイベントのペアとの間の関係へのポインターを取得します。|  
|[GetModuleFromScope メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmodulefromscope-method.md)|現在のメタデータ スコープ内で参照されるモジュールのメタデータ トークンへのポインターを取得します。|  
|[GetModuleRefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getmodulerefprops-method.md)|指定したメタデータ トークンによって参照されるモジュールの名前を取得します。|  
|[GetNameFromToken メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getnamefromtoken-method.md)|指定したメタデータ トークンによって参照されるオブジェクトの UTF-8 名を取得します。|  
|[GetNativeCallConvFromSig メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getnativecallconvfromsig-method.md)|指定したシグネチャ ポインターで表されるメソッドのネイティブな呼び出し規約を取得します。|  
|[GetNestedClassProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getnestedclassprops-method.md)|入れ子にされた型を指定して、それを囲んでいる親の型の TypeDef トークンを取得します。|  
|[GetParamForMethodIndex メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getparamformethodindex-method.md)|指定した MethodDef トークンが表すメソッドの一連のメソッド パラメーターにおいて、指定した序数位置にあるパラメーターを表すトークンへのポインターを取得します。|  
|[GetParamProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getparamprops-method.md)|指定した ParamDef トークンによって参照されるパラメーターのメタデータ値を取得します。|  
|[GetPermissionSetProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getpermissionsetprops-method.md)|指定した Permission トークンが表す System.Security.PermissionSet に関連付けられているメタデータを取得します。|  
|[GetPinvokeMap](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getpinvokemap-method.md)|PInvoke 呼び出しの対象アセンブリを表す ModuleRef トークンを取得します。|  
|[GetPropertyProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getpropertyprops-method.md)|指定したトークンが表すプロパティに関連付けられているメタデータを取得します。|  
|[GetRVA メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getrva-method.md)|指定したトークンが表すコード オブジェクトの相対仮想アドレスのオフセットを取得します。|  
|[GetScopeProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getscopeprops-method.md)|現在のメタデータ スコープにあるアセンブリまたはモジュールの名前、およびオプションでバージョン ID を取得します。|  
|[GetSigFromToken メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getsigfromtoken-method.md)|指定したトークンに関連付けられているバイナリ メタデータ シグネチャを取得します。|  
|[GetTypeDefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-gettypedefprops-method.md)|指定した TypeDef トークンによって表される型のメタデータ情報を返します。|  
|[GetTypeRefProps メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-gettyperefprops-method.md)|指定した TypeRef トークンによって参照される型に関連付けられているメタデータを取得します。|  
|[GetTypeSpecFromToken メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-gettypespecfromtoken-method.md)|指定したトークンが表すタイプ仕様のバイナリ メタデータ シグネチャを取得します。|  
|[GetUserString メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getuserstring-method.md)|指定したメタデータ トークンで表されるリテラル文字列を取得します。|  
|[IsGlobal メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-isglobal-method.md)|指定したメタデータ トークンによって表されるフィールド、メソッド、または型がグローバル スコープを保持しているかどうかを示す値を取得します。|  
|[IsValidToken メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-isvalidtoken-method.md)|指定したトークンが、コード オブジェクトへの有効な参照を保持しているかどうかを示す値を取得します。|  
|[ResetEnum メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-resetenum-method.md)|指定した列挙子を指定した位置にリセットします。|  
|[ResolveTypeRef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-resolvetyperef-method.md)|指定した TypeRef トークンによって参照される型の型情報を取得します。|  
  
## <a name="remarks"></a>コメント  
 `IMetaDataImport` インターフェイスは、型情報のインポート (開発ツールなど)、または配置されたコンポーネントの管理 (解決サービス、アクティブ化サービスなど) を行うツールとサービスで使用することを主な目的としてデザインされています。 `IMetaDataImport` のメソッドは、次のタスク カテゴリに分類されます。  
  
- メタデータ スコープ内の項目のコレクションの列挙。  
  
- 特定の特性セットを持つ項目の検索。  
  
- 指定した項目のプロパティの取得。  
  
- Get メソッドは、メタデータ項目の単一値のプロパティを返すように特別にデザインされています。 プロパティが別の項目への参照である場合、その項目のトークンが返されます。 特定の値が要求されていないことを示すために、ポインター入力型を NULL に設定できます。 基本的にコレクション オブジェクトであるプロパティ (クラスが実装するインターフェイスのコレクションなど) を取得するには、列挙メソッドを使用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
