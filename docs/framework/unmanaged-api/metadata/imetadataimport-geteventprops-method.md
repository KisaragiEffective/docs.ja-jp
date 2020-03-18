---
title: IMetaDataImport::GetEventProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetEventProps
helpviewer_keywords:
- IMetaDataImport::GetEventProps method [.NET Framework metadata]
- GetEventProps method [.NET Framework metadata]
ms.assetid: 5eaf3b4a-92b7-4d5b-97e0-1e83721e0052
topic_type:
- apiref
ms.openlocfilehash: 18fe0c834506d0ac4cd15fd7af4c4f15904b0f81
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437576"
---
# <a name="imetadataimportgeteventprops-method"></a>IMetaDataImport::GetEventProps メソッド
宣言する型、デリゲートの add メソッドおよび remove メソッド、すべてのフラグおよびその他の関連データを含む、指定したイベントトークンによって表されるイベントのメタデータ情報を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetEventProps (  
   [in]  mdEvent       ev,  
   [out] mdTypeDef     *pClass,   
   [out] LPCWSTR       szEvent,   
   [in]  ULONG         cchEvent,   
   [out] ULONG         *pchEvent,   
   [out] DWORD         *pdwEventFlags,  
   [out] mdToken       *ptkEventType,  
   [out] mdMethodDef   *pmdAddOn,   
   [out] mdMethodDef   *pmdRemoveOn,   
   [out] mdMethodDef   *pmdFire,   
   [out] mdMethodDef   rmdOtherMethod[],   
   [in]  ULONG         cMax,  
   [out] ULONG         *pcOtherMethod  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ev`  
 からメタデータを取得するイベントを表すイベントメタデータトークン。  
  
 `pClass`  
 入出力イベントを宣言するクラスを表す TypeDef トークンへのポインター。  
  
 `szEvent`  
 入出力`ev`によって参照されるイベントの名前。  
  
 `pchEvent`  
 から`szEvent`の、要求された長さをワイド文字数で指定します。  
  
 `pdwEventFlags`  
 入出力`szEvent`のワイド文字数で返された長さ。  
  
 `ptkEventType`  
 入出力イベントの <xref:System.Delegate> 型を表す TypeRef または TypeDef メタデータトークンへのポインター。  
  
 `pmdAddOn`  
 入出力イベントのハンドラーを追加するメソッドを表すメタデータトークンへのポインター。  
  
 `pmdRemoveOn`  
 入出力イベントのハンドラーを削除するメソッドを表すメタデータトークンへのポインター。  
  
 `pmdFire`  
 入出力イベントを発生させるメソッドを表すメタデータトークンへのポインター。  
  
 `rmdOtherMethod`  
 入出力イベントに関連付けられている他のメソッドへのトークンポインターの配列。  
  
 `cMax`  
 [in] `rmdOtherMethod` 配列の最大サイズ。  
  
 `pcOtherMethod`  
 入出力`rmdOtherMethod`で返されたトークンの数。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
