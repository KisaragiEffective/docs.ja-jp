---
title: CompareAssemblyIdentity 関数
ms.date: 03/30/2017
api_name:
- CompareAssemblyIdentity
api_location:
- fusion.dll
- clr.dll
api_type:
- COM
f1_keywords:
- CompareAssemblyIdentity
helpviewer_keywords:
- CompareAssemblyIdentity function [.NET Framework fusion]
ms.assetid: 8b364ae1-8efa-4744-a7da-81fd093d84d6
topic_type:
- apiref
ms.openlocfilehash: f6711902fb9d5c5c16084057b731746843cf0230
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73108923"
---
# <a name="compareassemblyidentity-function"></a>CompareAssemblyIdentity 関数
2つのアセンブリ id を比較して、それらが等しいかどうかを判断します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
STDAPI CompareAssemblyIdentity (  
    [in]  LPCWSTR                  pwzAssemblyIdentity1,  
    [in]  BOOL                     fUnified1,  
    [in]  LPCWSTR                  pwzAssemblyIdentity2,  
    [in]  BOOL                     fUnified2,  
    [out] BOOL                     *pfEquivalent,  
    [out] AssemblyComparisonResult *pResult  
 );  
```  
  
## <a name="parameters"></a>パラメーター  
 `pwzAssemblyIdentity1`  
 から比較対象の最初のアセンブリのテキスト id。  
  
 `fUnified1`  
 から`pwzAssemblyIdentity1`のユーザー指定の統一を示すブール型のフラグ。  
  
 `pwzAssemblyIdentity2`  
 から比較対象の2番目のアセンブリのテキスト id。  
  
 `fUnified2`  
 から`pwzAssemblyIdentity2`のユーザー指定の統一を示すブール型のフラグ。  
  
 `pfEquivalent`  
 入出力2つのアセンブリが等しいかどうかを示すブール型のフラグ。  
  
 `pResult`  
 入出力比較に関する詳細情報を含む[AssemblyComparisonResult](assemblycomparisonresult-enumeration.md)列挙です。  
  
## <a name="return-value"></a>戻り値  
 `pfEquivalent` は、2つのアセンブリが等しいかどうかを示すブール値を返します。 `pResult` は、`AssemblyComparisonResult` の値の1つを返して、`pfEquivalent`の値のより詳細な理由を提供します。  
  
## <a name="remarks"></a>Remarks  
 `CompareAssemblyIdentity` `pwzAssemblyIdentity1` と `pwzAssemblyIdentity2` が等しいかどうかを確認します。 `pfEquivalent` は、次の条件の1つ以上で `true` に設定されます。  
  
- 2つのアセンブリ id は同等です。 厳密な名前が付けられたアセンブリの場合、等価性には、アセンブリ名、バージョン、公開キートークン、およびカルチャが同一である必要があります。 単純な名前付きアセンブリの場合、等価性にはアセンブリ名とカルチャの一致が必要です。  
  
- どちらのアセンブリ id も、.NET Framework で実行されるアセンブリを参照します。 この条件では、アセンブリのバージョン番号が一致しない場合でも `true` が返されます。  
  
- 2つのアセンブリはマネージアセンブリではありませんが、`fUnified1` または `fUnified2` が `true`に設定されています。  
  
 `fUnified` フラグは、厳密に名前が付けられたアセンブリのバージョン番号までのすべてのバージョン番号が、厳密な名前を持つアセンブリと同等であると見なされることを示します。 たとえば、`pwzAssemblyIndentity1` の値が "MyAssembly, version = 3.0.0.0, culture = ニュートラル, publicKeyToken =..." で、`fUnified1` の値が `true`の場合、バージョン0.0.0.0 から3.0.0.0 のすべてのバージョンの MyAssembly はとして処理する必要があることを示します。表現. このような場合、`pwzAssemblyIndentity2` は `pwzAssemblyIndentity1`と同じアセンブリを参照しますが、バージョン番号が低い点を除き、`pfEquivalent` は `true`に設定されます。 `pwzAssemblyIdentity2` がより新しいバージョン番号を参照している場合、`fUnified2` の値が `true`の場合にのみ、`pfEquivalent` は `true` に設定されます。  
  
 `pResult` パラメーターには、2つのアセンブリが同等であるかどうかについての特定の情報が含まれます。 詳細については、「 [AssemblyComparisonResult 列挙型](assemblycomparisonresult-enumeration.md)」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion グローバル静的関数](fusion-global-static-functions.md)
- [AssemblyComparisonResult 列挙型](assemblycomparisonresult-enumeration.md)
