---
title: COR_PRF_ASSEMBLY_REFERENCE_INFO 構造体
ms.date: 03/30/2017
dev_langs:
- cpp
ms.assetid: c8c1d916-8d1a-4f82-8128-9fd3732383fc
ms.openlocfilehash: 4fdc8e1074bf45de3a8ab85500a85b124ce34fa1
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867335"
---
# <a name="cor_prf_assembly_reference_info-structure"></a>COR_PRF_ASSEMBLY_REFERENCE_INFO 構造体
[.NET Framework 4.5.2 以降のバージョンでのみでサポート]  
  
 アセンブリ参照クロージャのウォークを実行するときに考慮する必要があるアセンブリ参照の情報を、共通言語ランタイムに提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _COR_PRF_ASSEMBLY_REFERENCE_INFO {  
    void* pbPublicKeyOrToken;  
    ULONG cbPublicKeyOrToken;  
    LPCWSTR szName;  
    ASSEMBLYMETADATA* pMetaData;  
    void* pbHashValue;  
    ULONG cbHashValue;  
    DWORD dwAssemblyRefFlags;  
} COR_PRF_EX_CLAUSE_INFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`pbPublicKeyOrToken`|公開キー、またはアセンブリのトークンへのポインター。|  
|`cbPublicKeyOrToken`|公開キーまたはトークンのバイト数。|  
|`szName`|参照されるアセンブリの名前。|  
|`pMetaData`|アセンブリのメタデータへのポインター。|  
|`pbHashValue`|ハッシュ バイナリ ラージ オブジェクト (BLOB) へのポインター。|  
|`cbHashValue`|ハッシュ BLOB のバイト数。|  
|`dwAssemblyRefFlags`|アセンブリのフラグ。|  
  
## <a name="remarks"></a>コメント  
 `COR_PRF_EX_CLAUSE_INFO` 構造は、追加のアセンブリ参照を宣言するときに、プロファイラーによって値が設定されます。ここでの追加のアセンブリ参照は、アセンブリ参照クロージャのウォークを実行するときに共通言語ランタイムが考慮するものです。  
  
 プロファイラーが[ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) callback メソッドを登録すると、ランタイムは、読み込まれるアセンブリのパスと名前、およびそのメソッドへの[ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)インターフェイスオブジェクトへのポインターを渡します。 プロファイラーは、 [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)コールバックで指定されたアセンブリから参照する各ターゲットアセンブリの `COR_PRF_ASSEMBLY_REFERENCE_INFO` オブジェクトを使用して、 [ICorProfilerAssemblyReferenceProvider:: addassemblyreference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)メソッドを呼び出すことができます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [構造体のプロファイリング](profiling-structures.md)
- [GetAssemblyReferences メソッド](icorprofilercallback6-getassemblyreferences-method.md)
- [AddAssemblyReference メソッド](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)
