---
title: CeeSectionRelocType 列挙型
ms.date: 03/30/2017
api_name:
- CeeSectionRelocType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CeeSectionRelocType
helpviewer_keywords:
- CeeSectionRelocType enumeration [.NET Framework metadata]
ms.assetid: 124656f6-0dad-4ceb-9043-d3869ab65cde
topic_type:
- apiref
ms.openlocfilehash: efce0c13944b383c42cbff6a6af4795293ee2989
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74444159"
---
# <a name="ceesectionreloctype-enumeration"></a>CeeSectionRelocType 列挙型
[ICeeGen:: AddSectionReloc](../../../../docs/framework/unmanaged-api/metadata/iceegen-addsectionreloc-method.md)への呼び出しで生成される `reloc` 命令の型に影響を与える値を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum  {  
    srRelocAbsolute,  
    srRelocHighLow          = 3,  
    srRelocHighAdj,       
    srRelocMapToken,  
    srRelocRelative,  
    srRelocFilePos,  
    srRelocCodeRelative,  
    srRelocIA64Imm64,  
    srRelocDir64,  
    srRelocIA64PcRel25,  
    srRelocIA64PcRel64,    srRelocAbsoluteTagged,    srRelocSentinel,    srNoBaseReloc       = 0x4000,  
    srRelocPtr          = 0x8000,  
    srRelocAbsolutePtr      = srRelocPtr + srRelocAbsolute,  
    srRelocHighLowPtr       = srRelocPtr + srRelocHighLow,  
    srRelocRelativePtr      = srRelocPtr + srRelocRelative,  
    srRelocIA64Imm64Ptr     = srRelocPtr + srRelocIA64Imm64,  
    srRelocDir64Ptr         = srRelocPtr + srRelocDir64  
    } CeeSectionRelocType;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`srRelocAbsolute`|では、セクション相対 `reloc`が生成され、reloc セクションには何も送信されません。|  
|`srRelocHighLow`|ポインターサイズの位置の `reloc` を生成します。 これは、プラットフォームに応じて BASED_HIGHLOW または BASED_DIR64 に変換されます。|  
|`srRelocHighAdj`|では、32ビット数値の上位16ビットの `reloc` が生成されます。この場合、下位16ビットが、reloc テーブルの次の単語に含まれます。|  
|`srRelocMapToken`|トークンマップの再配置を生成し、reloc セクションに何も送信しません。|  
|`srRelocRelative`|値が相対アドレスの修正であることを示します。|  
|`srRelocFilePos`|では、セクション相対 `reloc`が生成され、reloc セクションには何も送信されません。 この `reloc` は、セクションの仮想アドレスではなく、セクションのファイルの位置を基準としています。|  
|`srRelocCodeRelative`|コード相対アドレスのフィックスアップを指定します。|  
|`srRelocIA64Imm64`|Ia64 `movl` 命令に64ビットアドレスの `reloc` を生成します。|  
|`srRelocDir64`|64ビットアドレスの `reloc` を生成します。|  
|`srRelocIA64PcRel25`|Ia64 `br.call` 命令で25ビット PC 相対アドレスの `reloc` を生成します。|  
|`srRelocIA64PcRel64`|Ia64 `brl.call` 命令で64ビット PC 相対アドレスの `reloc` を生成します。|  
|`srRelocAbsoluteTagged`|タグ付きポインター値に使用される、30ビットのセクション相対 `reloc`を生成します。|  
|`srRelocSentinel`|この列挙型への追加が内部 `reloc` 名配列に反映されるようにするための sentinel 値。|  
|`srNoBaseReloc`|基本 `reloc`を生成しないように指定します。|  
|`srRelocPtr`|メモリの事前修正の内容が、セクションオフセットではなくポインターであることを示す値。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
- [ICeeGen インターフェイス](../../../../docs/framework/unmanaged-api/metadata/iceegen-interface.md)
- [AddSectionReloc メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-addsectionreloc-method.md)
