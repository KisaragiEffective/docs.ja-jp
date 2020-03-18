---
title: CorPEKind 列挙型
ms.date: 03/30/2017
api_name:
- CorPEKind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPEKind
helpviewer_keywords:
- CorPEKind enumeration [.NET Framework metadata]
ms.assetid: 22dc6dea-b1b9-4982-a730-a022d586b117
topic_type:
- apiref
ms.openlocfilehash: 74670a1477546066145bd4bbf2f123a252e10b55
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436476"
---
# <a name="corpekind-enumeration"></a>CorPEKind 列挙型
[IMetaDataImport2:: GetPEKind](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-getpekind-method.md)の呼び出しから返される、ポータブル実行可能 (PE) ファイルを記述する値を格納します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorPEKind {  
  
    peNot           = 0x00000000,  
    peILonly        = 0x00000001,  
    pe32BitRequired = 0x00000002,  
    pe32Plus        = 0x00000004,  
    pe32Unmanaged   = 0x00000008,  
    pe32BitPreferred= 0x00000010  
  
} CorPEKind;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`peNot`|これが PE ファイルではないことを示します。|  
|`peILOnly`|この PE ファイルにマネージコードのみが含まれていることを示します。|  
|`pe32BitRequired`|この PE ファイルが Win32 呼び出しを行うことを示します。|  
|`pe32Plus`|この PE ファイルが64ビットプラットフォームで実行されることを示します。|  
|`pe32Unmanaged`|この PE ファイルがネイティブコードであることを示します。|  
|pe32BitPreferred|この PE ファイルがプラットフォームに依存せず、32ビット環境で読み込まれることを示すことを示します。|  
  
## <a name="remarks"></a>コメント  
 これらの値は、ビットごとの組み合わせで使用できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
