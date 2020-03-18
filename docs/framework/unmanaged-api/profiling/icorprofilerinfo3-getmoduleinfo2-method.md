---
title: ICorProfilerInfo3::GetModuleInfo2 メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetModuleInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetModuleInfo2
helpviewer_keywords:
- ICorProfilerInfo3::GetModuleInfo2 method [.NET Framework profiling]
- GetModuleInfo2 method [.NET Framework profiling]
ms.assetid: f1f6b8f3-dcfc-49e8-be76-ea50ea90d5a7
topic_type:
- apiref
ms.openlocfilehash: 96cde35c7151bb7ce58715f2826feaa59b30efab
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76862309"
---
# <a name="icorprofilerinfo3getmoduleinfo2-method"></a>ICorProfilerInfo3::GetModuleInfo2 メソッド
モジュール ID を指定して、モジュールのファイル名、モジュールの親アセンブリの ID、およびモジュールのプロパティを示すビットマスクを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetModuleInfo2(  
    [in]  ModuleID   moduleId,  
    [out] LPCBYTE    *ppBaseLoadAddress,  
    [in]  ULONG      cchName,  
    [out] ULONG      *pcchName,  
    [out, annotation("__out_ecount_part(cchName, *pcchName)")]  
          WCHAR      szName[] ,  
    [out] AssemblyID *pAssemblyId);  
    [out] DWORD                 *pdwModuleFlags);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleId`  
 [in] 情報が取得されるモジュールの ID。  
  
 `ppBaseLoadAddress`  
 [out] モジュールが読み込まれるベース アドレス。  
  
 `cchName`  
 [in] `szName` 戻りバッファーの長さ (文字単位)。  
  
 `pcchName`  
 [out] 返されるモジュールのファイル名の文字列長の合計へのポインター。  
  
 `szName`  
 [out] 呼び出し元が提供したワイド文字バッファー。 メソッドから制御が戻るとき、このバッファーにモジュールのファイル名が格納されます。  
  
 `pAssemblyId`  
 [out] モジュールの親アセンブリ ID へのポインター。  
  
 `pdwModuleFlags`  
 入出力モジュールのプロパティを指定する[COR_PRF_MODULE_FLAGS](cor-prf-module-flags-enumeration.md)列挙体の値のビットマスク。  
  
## <a name="remarks"></a>コメント  
 動的モジュールの場合、`szName` パラメーターはモジュールのメタデータ名になり、ベース アドレスは 0 (ゼロ) になります。 メタデータ名は、メタデータ内の Module テーブルの Name 列の値です。 これは、<xref:System.Reflection.Module.ScopeName%2A?displayProperty=nameWithType> プロパティとしてマネージコードにも公開されます。また、 [IMetaDataImport:: GetScopeProps](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-getscopeprops-method.md)メソッドの `szName` パラメーターとして、アンマネージメタデータクライアントコードにも公開されます。  
  
 `GetModuleInfo2` メソッドは、モジュールの ID が存在するとすぐに呼び出される場合がありますが、プロファイラーが[ICorProfilerCallback:: ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md)コールバックを受け取るまで、親アセンブリの id は使用できません。  
  
 `GetModuleInfo2` から制御が戻ったら、`szName` バッファーのサイズが十分で、モジュールのファイル名全体を格納できたかどうかを確認する必要があります。 これを行うには、`pcchName` が指している値を `cchName` パラメーターの値と比較します。 `pcchName` が指している値が `cchName` の値より大きい場合は、`szName` バッファーの割り当てを増やし、`cchName` を新しい大きいサイズに更新して、`GetModuleInfo2` を再度呼び出します。  
  
 別の方法として、最初に `GetModuleInfo2` を長さゼロの `szName` バッファーで呼び出して、適切なバッファーのサイズを取得します。 その後、バッファーのサイズを `pcchName` で返された値に設定し、`GetModuleInfo2` を再度呼び出します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
