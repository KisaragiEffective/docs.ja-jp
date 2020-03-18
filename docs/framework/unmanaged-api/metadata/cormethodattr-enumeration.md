---
title: CorMethodAttr 列挙型
ms.date: 03/30/2017
api_name:
- CorMethodAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodAttr
helpviewer_keywords:
- CorMethodAttr enumeration [.NET Framework metadata]
ms.assetid: 4e0c3521-e54d-43c1-9857-cc76b49b8ffc
topic_type:
- apiref
ms.openlocfilehash: 74088d1cd018bb07406fc7d00ff83d783a98b663
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450231"
---
# <a name="cormethodattr-enumeration"></a>CorMethodAttr 列挙型
メソッドの機能を記述する値を格納します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorMethodAttr {  
  
    mdMemberAccessMask          =   0x0007,  
    mdPrivateScope              =   0x0000,  
    mdPrivate                   =   0x0001,  
    mdFamANDAssem               =   0x0002,  
    mdAssem                     =   0x0003,  
    mdFamily                    =   0x0004,  
    mdFamORAssem                =   0x0005,  
    mdPublic                    =   0x0006,  
  
    mdStatic                    =   0x0010,  
    mdFinal                     =   0x0020,  
    mdVirtual                   =   0x0040,  
    mdHideBySig                 =   0x0080,  
  
    mdVtableLayoutMask          =   0x0100,  
    mdReuseSlot                 =   0x0000,  
    mdNewSlot                   =   0x0100,  
  
    mdCheckAccessOnOverride     =   0x0200,  
    mdAbstract                  =   0x0400,  
    mdSpecialName               =   0x0800,  
  
    mdPinvokeImpl               =   0x2000,  
    mdUnmanagedExport           =   0x0008,  
  
    mdReservedMask              =   0xd000,  
    mdRTSpecialName             =   0x1000,  
    mdHasSecurity               =   0x4000,  
    mdRequireSecObject          =   0x8000,  
  
} CorMethodAttr;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`mdMemberAccessMask`|メンバーアクセスを指定します。|  
|`mdPrivateScope`|メンバーを参照できないことを指定します。|  
|`mdPrivate`|メンバーが親の型によってのみアクセス可能であることを指定します。|  
|`mdFamANDAssem`|このアセンブリ内のサブタイプだけがメンバーにアクセスできることを指定します。|  
|`mdAssem`|メンバーがアセンブリ内のすべてのユーザーによって accessibly されることを指定します。|  
|`mdFamily`|メンバーが型とサブタイプによってのみアクセス可能であることを指定します。|  
|`mdFamORAssem`|メンバーが、派生クラスおよびそのアセンブリ内の他の型によってアクセス可能であることを指定します。|  
|`mdPublic`|スコープへのアクセス権を持つすべての型からメンバーにアクセスできることを指定します。|  
|`mdStatic`|メンバーがインスタンスのメンバーとしてではなく、型の一部として定義されることを指定します。|  
|`mdFinal`|メソッドをオーバーライドできないことを指定します。|  
|`mdVirtual`|メソッドをオーバーライドできることを指定します。|  
|`mdHideBySig`|メソッドが名前だけではなく、名前とシグネチャで非表示になるように指定します。|  
|`mdVtableLayoutMask`|仮想テーブルのレイアウトを指定します。|  
|`mdReuseSlot`|仮想テーブルでこのメソッドに使用されるスロットを再利用することを指定します。 既定値です。|  
|`mdNewSlot`|メソッドが常に仮想テーブル内の新しいスロットを取得することを指定します。|  
|`mdCheckAccessOnOverride`|メソッドを、表示されているのと同じ型でオーバーライドできることを指定します。|  
|`mdAbstract`|メソッドが実装されていないことを指定します。|  
|`mdSpecialName`|メソッドが特別であり、その名前がどのように説明するかを指定します。|  
|`mdPinvokeImpl`|メソッドの実装が PInvoke を使用して転送されることを指定します。|  
|`mdUnmanagedExport`|メソッドがアンマネージコードにエクスポートされたマネージメソッドであることを指定します。|  
|`mdReservedMask`|共通言語ランタイムによる内部使用のために予約されています。|  
|`mdRTSpecialName`|共通言語ランタイムがメソッド名のエンコーディングを確認する必要があることを指定します。|  
|`mdHasSecurity`|メソッドにセキュリティが関連付けられていることを指定します。|  
|`mdRequireSecObject`|メソッドが、セキュリティコードを含む別のメソッドを呼び出すことを指定します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
