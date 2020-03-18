---
title: CorTypeAttr 列挙型
ms.date: 03/30/2017
api_name:
- CorTypeAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorTypeAttr
helpviewer_keywords:
- CorTypeAttr enumeration [.NET Framework metadata]
ms.assetid: 9bede0ec-5fdf-42a2-b5b7-bee64056acb6
topic_type:
- apiref
ms.openlocfilehash: b1586184c91619994ba0dfc9d5dcc277c10f99cf
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436453"
---
# <a name="cortypeattr-enumeration"></a>CorTypeAttr 列挙型
メタデータ型を示す値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorTypeAttr {  
  
    tdVisibilityMask        =   0x00000007,  
    tdNotPublic             =   0x00000000,  
    tdPublic                =   0x00000001,  
    tdNestedPublic          =   0x00000002,  
    tdNestedPrivate         =   0x00000003,  
    tdNestedFamily          =   0x00000004,  
    tdNestedAssembly        =   0x00000005,  
    tdNestedFamANDAssem     =   0x00000006,  
    tdNestedFamORAssem      =   0x00000007,  
  
    tdLayoutMask            =   0x00000018,  
    tdAutoLayout            =   0x00000000,  
    tdSequentialLayout      =   0x00000008,  
    tdExplicitLayout        =   0x00000010,  
  
    tdClassSemanticsMask    =   0x00000020,  
    tdClass                 =   0x00000000,  
    tdInterface             =   0x00000020,  
  
    tdAbstract              =   0x00000080,  
    tdSealed                =   0x00000100,  
    tdSpecialName           =   0x00000400,  
  
    tdImport                =   0x00001000,  
    tdSerializable          =   0x00002000,  
    tdWindowsRuntime        =   0x00004000,  
  
    tdStringFormatMask      =   0x00030000,  
    tdAnsiClass             =   0x00000000,  
    tdUnicodeClass          =   0x00010000,  
    tdAutoClass             =   0x00020000,  
    tdCustomFormatClass     =   0x00030000,  
    tdCustomFormatMask      =   0x00C00000,  
  
    tdBeforeFieldInit       =   0x00100000,  
    tdForwarder             =   0x00200000,  
  
    tdReservedMask          =   0x00040800,  
    tdRTSpecialName         =   0x00000800,  
    tdHasSecurity           =   0x00040000,  
  
} CorTypeAttr;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`tdVisibilityMask`|型の可視性情報に使用されます。|  
|`tdNotPublic`|型がパブリックスコープ内にないことを指定します。|  
|`tdPublic`|型がパブリックスコープ内にあることを指定します。|  
|`tdNestedPublic`|型がパブリックの可視性で入れ子になっていることを指定します。|  
|`tdNestedPrivate`|型がプライベート可視性で入れ子になっていることを指定します。|  
|`tdNestedFamily`|型がファミリの可視性で入れ子になっていることを指定します。|  
|`tdNestedAssembly`|型がアセンブリの可視性で入れ子になっていることを指定します。|  
|`tdNestedFamANDAssem`|型がファミリおよびアセンブリの可視性で入れ子になっていることを指定します。|  
|`tdNestedFamORAssem`|型が、ファミリまたはアセンブリの参照可能範囲に入れ子になっていることを指定します。|  
|`tdLayoutMask`|型のレイアウト情報を取得します。|  
|`tdAutoLayout`|この型のフィールドが自動的にレイアウトされることを指定します。|  
|`tdSequentialLayout`|この型のフィールドを順番に配置することを指定します。|  
|`tdExplicitLayout`|フィールドレイアウトが明示的に指定されていることを指定します。|  
|`tdClassSemanticsMask`|型に関するセマンティック情報を取得します。|  
|`tdClass`|型がクラスであることを示します。|  
|`tdInterface`|型がインターフェイスであることを示します。|  
|`tdAbstract`|型が抽象的であることを示します。|  
|`tdSealed`|型を拡張できないことを指定します。|  
|`tdSpecialName`|クラス名が特別であることを指定します。 その名前は、方法を説明します。|  
|`tdImport`|型がインポートされることを指定します。|  
|`tdSerializable`|型がシリアル化可能であることを指定します。|  
|`tdWindowsRuntime`|この型が Windows ランタイム型であることを指定します。|  
|`tdStringFormatMask`|文字列をエンコードおよび書式設定する方法に関する情報を取得します。|  
|`tdAnsiClass`|この型が LPTSTR を ANSI と解釈することを指定します。|  
|`tdUnicodeClass`|この型が LPTSTR を Unicode として解釈することを指定します。|  
|`tdAutoClass`|この型が LPTSTR を自動的に解釈することを指定します。|  
|`tdCustomFormatClass`|`CustomFormatMask`によって指定された標準以外のエンコーディングを型が持つことを指定します。|  
|`tdCustomFormatMask`|ネイティブ相互運用機能の非標準のエンコード情報を取得するには、このマスクを使用します。 これら2つのビットの値の意味は、指定されていません。|  
|`tdBeforeFieldInit`|静的フィールドに最初にアクセスしようとする前に型を初期化する必要があることを指定します。|  
|`tdForwarder`|型がエクスポートされ、型フォワーダーが指定されていることを示します。|  
|`tdReservedMask`|このフラグと以下のフラグは、共通言語ランタイムによって内部的に使用されます。|  
|`tdRTSpecialName`|共通言語ランタイムが名前のエンコーディングを確認する必要があることを指定します。|  
|`tdHasSecurity`|型にセキュリティが関連付けられていることを指定します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
