---
title: IMetaDataAssemblyImport::FindAssembliesByName メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.FindAssembliesByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::FindAssembliesByName
helpviewer_keywords:
- FindAssembliesByName method [.NET Framework metadata]
- IMetaDataAssemblyImport::FindAssembliesByName method [.NET Framework metadata]
ms.assetid: 4db97cf9-e4c1-4233-8efa-cbdc0e14a8e4
topic_type:
- apiref
ms.openlocfilehash: c12d3a7a7d1e52529435361aa12e22e4edeecf03
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448295"
---
# <a name="imetadataassemblyimportfindassembliesbyname-method"></a>IMetaDataAssemblyImport::FindAssembliesByName メソッド
参照を解決するために共通言語ランタイム (CLR) によって採用されている標準規則を使用して、指定した `szAssemblyName` パラメーターを持つアセンブリの配列を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT FindAssembliesByName (  
    [in]  LPCWSTR     szAppBase,   
    [in]  LPCWSTR     szPrivateBin,   
    [in]  LPCWSTR     szAssemblyName,   
    [out] IUnknown    *ppIUnk[],   
    [in]  ULONG       cMax,   
    [out] ULONG       *pcAssemblies  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `szAppBase`  
 から指定されたアセンブリを検索するルートディレクトリ。 この値が `null`に設定されている場合、`FindAssembliesByName` はアセンブリのグローバルアセンブリキャッシュだけを検索します。  
  
 `szPrivateBin`  
 からルートディレクトリの下のセミコロンで区切られたサブディレクトリ (たとえば、"bin; bin2") の一覧。このディレクトリで、アセンブリを検索します。 これらのディレクトリは、既定のプローブルールで指定されているものに加えてプローブされます。  
  
 `szAssemblyName`  
 から検索するアセンブリの名前。 この文字列の形式は、<xref:System.Reflection.AssemblyName>のクラス参照ページで定義されています。  
  
 `ppIUnk`  
 から`IMetadataAssemblyImport` インターフェイスポインターを格納する[IUnknown](/cpp/atl/iunknown)型の配列。  
  
 `cMax`  
 入出力`ppIUnk`に配置できるインターフェイスポインターの最大数。  
  
 `pcAssemblies`  
 入出力返されたインターフェイスポインターの数。 つまり、実際に `ppIUnk`に配置されたインターフェイスポインターの数。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|`S_OK`|`FindAssembliesByName` が正常に返されました。|  
|`S_FALSE`|アセンブリがありません。|  
  
## <a name="remarks"></a>コメント  
 アセンブリ名が指定されている場合、`FindAssembliesByName` メソッドは、アセンブリ参照を解決するための標準の規則に従って、アセンブリを検索します。 (詳細については、「[ランタイムがアセンブリを検索する方法](../../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md)」を参照してください)。`FindAssembliesByName` を使用すると、呼び出し元は、アプリケーションベースやプライベート検索パスなど、アセンブリリゾルバーコンテキストのさまざまな側面を構成できます。  
  
 `FindAssembliesByName` メソッドでは、アセンブリ解決ロジックを呼び出すために、プロセスで CLR を初期化する必要があります。 したがって、`FindAssembliesByName`を呼び出す前に[Coinitializeee](../../../../docs/framework/unmanaged-api/hosting/coinitializeee-function.md) (COINITEE_DEFAULT を渡す) を呼び出してから、 [CoUninitializeCor](../../../../docs/framework/unmanaged-api/hosting/couninitializecor-function.md)の呼び出しを実行する必要があります。  
  
 `FindAssembliesByName` は、渡されたアセンブリ名のアセンブリマニフェストを含むファイルへの[IMetaDataImport](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)ポインターを返します。 指定したアセンブリ名が完全に指定されていない場合 (たとえば、バージョンが含まれていない場合) は、複数のアセンブリが返されることがあります。  
  
 `FindAssembliesByName` は、コンパイル時に参照アセンブリを検索するコンパイラによって一般的に使用されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ランタイムがアセンブリを検索する方法](../../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md)
- [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
