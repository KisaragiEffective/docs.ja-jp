---
title: CorAssemblyFlags 列挙型
ms.date: 03/30/2017
api_name:
- CorAssemblyFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAssemblyFlags
helpviewer_keywords:
- CorAssemblyFlags enumeration [.NET Framework metadata]
ms.assetid: bb8db3b6-d81d-49fc-b74c-dbc908a9eab9
topic_type:
- apiref
ms.openlocfilehash: fda890cee5f513ea8cf7e82e710f5451a860c49f
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443912"
---
# <a name="corassemblyflags-enumeration"></a>CorAssemblyFlags 列挙型
アセンブリ コンパイルに適用されるメタデータを記述する値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorAssemblyFlags {  
  
    afPublicKey             =   0x0001,  
    afPA_None               =   0x0000,  
    afPA_MSIL               =   0x0010,  
    afPA_x86                =   0x0020,  
    afPA_IA64               =   0x0030,  
    afPA_AMD64              =   0x0040,  
    afPA_ARM                =   0x0050,  
    afPA_NoPlatform         =   0x0070,  
    afPA_Specified          =   0x0080,  
    afPA_Mask               =   0x0070,  
    afPA_FullMask           =   0x00F0,  
    afPA_Shift              =   0x0004,  
  
    afEnableJITcompileTracking  =   0x8000,  
    afDisableJITcompileOptimizer=   0x4000,  
  
    afRetargetable          =   0x0100,  
    afContentType_Default        =   0x0000,  
    afContentType_WindowsRuntime =   0x0200,  
    afContentType_Mask           =   0x0E00,  
  
} CorAssemblyFlags;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`afPublicKey`|アセンブリ参照が完全なハッシュされていない公開キーを保持していることを示します。|  
|`afPA_None`|プロセッサアーキテクチャが指定されていないことを示します。|  
|`afPA_MSIL`|プロセッサアーキテクチャがニュートラル (PE32) であることを示します。|  
|`afPA_x86`|プロセッサアーキテクチャが x86 (PE32) であることを示します。|  
|`afPA_IA64`|プロセッサアーキテクチャが Itanium (PE32 +) であることを示します。|  
|`afPA_AMD64`|プロセッサアーキテクチャが AMD X64 (PE32 +) であることを示します。|  
|`afPA_ARM`|プロセッサアーキテクチャが ARM (PE32) であることを示します。|  
|`afPA_NoPlatform`|アセンブリが参照アセンブリであることを示します。つまり、アーキテクチャには適用されますが、どのアーキテクチャでも実行することはできません。 したがって、フラグは `afPA_Mask`と同じです。|  
|`afPA_Specified`|プロセッサのアーキテクチャフラグを `AssemblyRef` レコードに反映する必要があることを示します。|  
|`afPA_Mask`|プロセッサアーキテクチャを記述するマスク。|  
|`afPA_FullMask`|プロセッサアーキテクチャの説明を含めることを指定します。|  
|`afPA_Shift`|プロセッサアーキテクチャフラグのインデックスとの間のシフト数を示します。|  
|`afEnableJITcompileTracking`|<xref:System.Diagnostics.DebuggableAttribute>の <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> に対応する値を示します。|  
|`afDisableJITcompileOptimizer`|<xref:System.Diagnostics.DebuggableAttribute>の <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> に対応する値を示します。|  
|`afRetargetable`|アセンブリを実行時に別のパブリッシャーからのアセンブリに再ターゲットできることを示します。|  
|`afContentType_Mask`|コンテンツの種類を説明するマスク。|  
|`afContentType_Default`|既定のコンテンツタイプを示します。|  
|`afContentType_WindowsRuntime`|Windows ランタイムコンテンツの種類を示します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
