---
title: IIdentityAuthority インターフェイス
ms.date: 03/30/2017
api_name:
- IIdentityAuthority
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IIdentityAuthority
helpviewer_keywords:
- IIdentityAuthority interface [.NET Framework fusion]
ms.assetid: 6277f914-51a8-49be-bec6-52d6d648527d
topic_type:
- apiref
ms.openlocfilehash: 3e2d2335e37ced9139ea44092f10b19566894681
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127088"
---
# <a name="iidentityauthority-interface"></a>IIdentityAuthority インターフェイス

コードオブジェクトの id キーを管理します。

## <a name="methods"></a>メソッド

|メソッド|説明|
|------------|-----------------|
|`IIdentityAuthority::AreDefinitionsEqual`|指定した2つの[IDefinitionIdentity](idefinitionidentity-interface.md)インスタンスが等しいかどうかを示す値を取得します。|
|`IIdentityAuthority::AreReferencesEqual`|指定した2つの[IReferenceIdentity](ireferenceidentity-interface.md)インスタンスが等しいかどうかを示す値を取得します。|
|`IIdentityAuthority::AreTextualDefinitionsEqual`|指定した2つの文字列定義 id 表現が等しいかどうかを示す値を取得します。|
|`IIdentityAuthority::AreTextualReferencesEqual`|指定した2つの文字列参照 id 表現が等しいかどうかを示す値を取得します。|
|`IIdentityAuthority::CreateDefinition`|現在のスコープ内のコードオブジェクトを表す新しい `IDefinitionIdentity` インスタンスへのポインターを取得します。|
|`IIdentityAuthority::CreateReference`|現在のスコープ内のコードオブジェクトを表す新しい `IReferenceIdentity` インスタンスへのポインターを取得します。|
|`IIdentityAuthority::DefinitionToText`|指定した `IDefinitionIdentity`の書式設定された文字列バージョンを取得します。|
|`IIdentityAuthority::DefinitionToTextBuffer`|指定したワイド文字バッファーに、指定した `IDefinitionIdentity`の文字列バージョンを格納します。|
|`IIdentityAuthority::DoesDefinitionMatchReference`|指定した `IDefinitionIdentity` と `IReferenceIdentity` の各インスタンスが同じコードオブジェクトを参照しているかどうかを示す値を取得します。|
|`IIdentityAuthority::DoesTextualDefinitionMatchTextualReference`|指定した文字列が同じコードオブジェクトを参照しているかどうかを示す値を取得します。|
|`IIdentityAuthority::GenerateDefinitionKey`|指定した `IDefinitionIdentity`用に新しく作成された文字列キーへのポインターを取得します。|
|`IIdentityAuthority::GenerateReferenceKey`|指定した `IReferenceIdentity`用に新しく作成された文字列キーへのポインターを取得します。|
|`IIdentityAuthority::HashDefinition`|指定した `IDefinitionIdentity`のハッシュ値を取得します。|
|`IIdentityAuthority::HashReference`|指定した `IReferenceIdentity`のハッシュ値を取得します。|
|`IIdentityAuthority::ReferenceToText`|指定した `IReferenceIdentity`の書式設定された文字列バージョンを取得します。|
|`IIdentityAuthority::ReferenceToTextBuffer`|指定したワイド文字バッファーに、指定した `IReferenceIdentity`の文字列バージョンを格納します。|
|`IIdentityAuthority::TextToDefinition`|指定した書式設定された文字列から生成された `IDefinitionIdentity` インスタンスへのインターフェイスポインターを取得します。|
|`IIdentityAuthority::TextToReference`|指定した書式設定された文字列から生成された `IReferenceIdentity` インスタンスへのインターフェイスポインターを取得します。|

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** 分離 .h

**.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
