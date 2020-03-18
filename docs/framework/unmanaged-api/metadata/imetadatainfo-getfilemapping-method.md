---
title: IMetaDataInfo::GetFileMapping メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataInfo.GetFileMapping
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataInfo::GetFileMapping
helpviewer_keywords:
- IMetaDataInfo::GetFileMapping method [.NET Framework metadata]
- GetFileMapping method [.NET Framework metadata]
ms.assetid: 2868dfec-c992-4606-88bb-a8e0b6b18271
topic_type:
- apiref
ms.openlocfilehash: 0cd2071d4410615a08e774ba30e0e8fe8d1fa7c7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436178"
---
# <a name="imetadatainfogetfilemapping-method"></a>IMetaDataInfo::GetFileMapping メソッド
マップされたファイルのメモリ領域とマッピングの種類を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetFileMapping (  
    [out] const void           **ppvData,   
    [out] ULONGLONG            *pcbData,   
    [out] DWORD                *pdwMappingType  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppvData`  
 入出力マップされたファイルの先頭へのポインター。  
  
 `pcbData`  
 入出力マップされた領域のサイズ。 `pdwMappingType` が `fmFlat`場合は、ファイルのサイズです。  
  
 `pdwMappingType`  
 入出力マッピングの種類を示す[CorFileMapping](../../../../docs/framework/unmanaged-api/metadata/corfilemapping-enumeration.md)値です。 共通言語ランタイム (CLR) の現在の実装では、常に `fmFlat`を返します。 他の値は将来使用するために予約されています。 ただし、その他の値は将来のバージョンまたはサービスリリースで有効になる可能性があるため、常に戻り値を確認する必要があります。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|`S_OK`|すべての出力が入力されます。|  
|`E_INVALIDARG`|引数の値として NULL が渡されました。|  
|`COR_E_NOTSUPPORTED`|CLR 実装では、メモリ領域に関する情報を提供できません。 これは、次の理由で発生することがあります。<br /><br /> -メタデータスコープが、`ofWrite` または `ofCopyMemory` フラグで開かれました。<br />-`ofReadOnly` フラグを指定せずにメタデータスコープが開かれました。<br />- [IMetaDataDispenser:: OpenScopeOnMemory](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-openscopeonmemory-method.md)メソッドは、ファイルのメタデータ部分のみを開くために使用されました。<br />-ファイルは、ポータブル実行可能 (PE) ファイルではありません。 **注:** これらの条件は CLR の実装によって異なり、CLR の将来のバージョンでは緩和される可能性があります。|  
  
## <a name="remarks"></a>コメント  
 `ppvData` が指すメモリは、基になるメタデータスコープが開いている間のみ有効です。  
  
 このメソッドを機能させるには、 [IMetaDataDispenser:: OpenScope](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-openscope-method.md)メソッドを呼び出して、ディスク上のファイルのメタデータをメモリにマップするときに、`ofReadOnly` フラグを指定し、`ofWrite` または `ofCopyMemory` フラグを指定しないようにする必要があります。  
  
 各スコープでのファイルマッピングの種類の選択は、CLR の特定の実装に固有です。 ユーザーが設定することはできません。 CLR の現在の実装では、常に `pdwMappingType`で `fmFlat` が返されますが、これは CLR の将来のバージョンまたは特定のバージョンの今後のサービスリリースで変更される可能性があります。 型によってレイアウトとオフセットが異なるため、必ず `pdwMappingType`で戻り値を確認してください。  
  
 3つのパラメーターのいずれかに対して NULL を渡すことはサポートされていません。 メソッドは `E_INVALIDARG`を返し、出力はすべて入力されません。 マッピングの種類または領域のサイズを無視すると、プログラムが異常終了する可能性があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataInfo インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadatainfo-interface.md)
- [CorFileMapping 列挙型](../../../../docs/framework/unmanaged-api/metadata/corfilemapping-enumeration.md)
