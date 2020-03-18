---
title: IAppIdAuthority インターフェイス
ms.date: 03/30/2017
api_name:
- IAppIdAuthority
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAppIdAuthority
helpviewer_keywords:
- IAppIdAuthority interface [.NET Framework fusion]
ms.assetid: ec354fa1-1efd-41b0-bc43-b90597b6e253
topic_type:
- apiref
ms.openlocfilehash: 004e4f70e3385e7a71c356cce38e64d0253dcfa4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127130"
---
# <a name="iappidauthority-interface"></a>IAppIdAuthority インターフェイス
アプリケーション id と参照のキーを生成して比較するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IAppIdAuthority::AreDefinitionsEqual`|指定した2つの[IDefinitionAppId](idefinitionappid-interface.md)インスタンスが等しいかどうかを示す値を取得します。 フラグ値 IAPPIDAUTHORITY_ARE_DEFINITIONS_EQUAL_FLAG_IGNORE_VERSION を渡して、それぞれのバージョン情報を無視することができます。|  
|`IAppIdAuthority::AreReferencesEqual`|指定した2つの[IReferenceAppId](ireferenceappid-interface.md)インスタンスが等しいかどうかを示す値を取得します。 フラグ値 IAPPIDAUTHORITY_ARE_REFERENCES_EQUAL_FLAG_IGNORE_VERSION を渡して、それぞれのバージョン情報を無視することができます。|  
|`IAppIdAuthority::AreTextualDefinitionsEqual`|指定した2つの文字列定義が等しいかどうかを示す値を取得します。 フラグ値 IAPPIDAUTHORITY_ARE_DEFINITIONS_EQUAL_FLAG_IGNORE_VERSION を渡して、それぞれのバージョン情報を無視することができます。|  
|`IAppIdAuthority::AreTextualReferencesEqual`|指定した2つの文字列参照が等しいかどうかを示す値を取得します。 フラグ値 IAPPIDAUTHORITY_ARE_REFERENCES_EQUAL_FLAG_IGNORE_VERSION を渡して、それぞれのバージョン情報を無視することができます。|  
|`IAppIdAuthority::CreateDefinition`|現在のスコープ内のアセンブリを表す、新しく生成された `IDefinitionAppId` インスタンスへのインターフェイスポインターを取得します。|  
|`IAppIdAuthority::CreateReference`|現在のスコープ内のアセンブリを表す、新しく作成された `IReferenceAppId` へのインターフェイスポインターを取得します。|  
|`IAppIdAuthority::DefinitionToText`|指定したフラグ値を使用して、指定した `IDefinitionAppId`の文字列バージョンを取得します。|  
|`IAppIdAuthority::DoesDefinitionMatchReference`|指定した `IDefinitionAppId` と `IReferenceAppId` が同じアセンブリを表しているかどうかを示す値を取得します。|  
|`IAppIdAuthority::DoesTextualDefinitionMatchTextualReference`|指定した定義文字列と参照文字列が同じアセンブリを表しているかどうかを示す値を取得します。|  
|`IAppIdAuthority::GenerateDefinitionKey`|指定された `IDefinitionAppId` インスタンスを表す文字列キーを取得します。|  
|`IAppIdAuthority::GenerateReferenceKey`|指定された `IReferenceAppId` インスタンスを表す文字列キーを取得します。|  
|`IAppIdAuthority::HashDefinition`|指定された `IDefinitionAppId` インスタンスのハッシュキーを取得します。|  
|`IAppIdAuthority::HashReference`|指定された `IReferenceAppId` インスタンスのハッシュキーを取得します。|  
|`IAppIdAuthority::ReferenceToText`|指定したフラグ値を使用して、指定した `IReferenceAppId`の文字列バージョンを取得します。|  
|`IAppIdAuthority::TextToDefinition`|指定した文字列キーによって参照されるアセンブリを表す `IDefinitionAppId` インスタンスへのインターフェイスポインターを取得します。|  
|`IAppIdAuthority::TextToReference`|指定した文字列キーによって参照されるアセンブリを表す `IReferenceAppId` インスタンスへのインターフェイスポインターを取得します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
