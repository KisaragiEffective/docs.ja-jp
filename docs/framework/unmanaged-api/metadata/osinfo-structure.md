---
title: OSINFO 構造体
ms.date: 03/30/2017
api_name:
- OSINFO
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- OSINFO
helpviewer_keywords:
- OSINFO structure [.NET Framework metadata]
ms.assetid: fac7b480-7adb-4450-a5e9-690fed81ffae
topic_type:
- apiref
ms.openlocfilehash: d66e9bc3a027610d917e15dc9769b92ea1c5fb71
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345603"
---
# <a name="osinfo-structure"></a>OSINFO 構造体
アセンブリまたはモジュールのオペレーティングシステムの詳細が含まれています。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct {  
    DWORD   dwOSPlatformId;  
    DWORD   dwOSMajorVersion;   
    DWORD   dwOSMinorVersion;   
} OSINFO;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`dwOSPlatformId`|Microsoft Windows プラットフォーム関数によって定義された識別子の値の1つ `GetVersionEx`。 次の値がサポートされています。<br /><br /> -VER_PLATFORM_WIN32s、または0x0000。 Microsoft Windows 3.1 を指定します。<br />-VER_PLATFORM_WIN32_WINDOWS、または0x0001 を指定して、Windows 95、Windows 98、またはそれらから派生したオペレーティングシステムを指定します。<br />-VER_PLATFORM_WIN32_NT、または0x0002 を指定して、Windows NT またはそれからのオペレーティングシステムを指定します。|  
|`dwOSMajorVersion`|オペレーティングシステムのメジャーバージョン。任意のバージョンを示す NULL 値。|  
|`dwOSMinorVersion`|オペレーティングシステムのマイナーバージョン。任意のバージョンを示す NULL 値。|  
  
## <a name="remarks"></a>コメント  
 `OSINFO` は、Microsoft Windows プラットフォーム関数 `GetVersionEx`の呼び出しで使用される `OSVERSIONINFOEX` の構造に基づいています。 この構造体は、そのオペレーティングシステムのサポートを示すために ASSEMBLYMETADATA 構造体によって使用されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ構造体](../../../../docs/framework/unmanaged-api/metadata/metadata-structures.md)
- [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
