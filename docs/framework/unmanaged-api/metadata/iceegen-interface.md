---
title: ICeeGen インターフェイス
ms.date: 03/30/2017
api_name:
- ICeeGen
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICeeGen
helpviewer_keywords:
- ICeeGen interface [.NET Framework metadata]
ms.assetid: 383d20b0-efe9-4e71-8fb8-1186b2e7d0c0
topic_type:
- apiref
ms.openlocfilehash: ae3c372951de00b097d0a8437039a3a85bf3aabf
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74426154"
---
# <a name="iceegen-interface"></a>ICeeGen インターフェイス
動的なコード コンパイルのためのメソッドを提供します。  
  
 このインターフェイスは互換性のために残されています。使用しないでください。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[AddSectionReloc メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-addsectionreloc-method.md)|互換性のために残されています。 コードベースに reloc 命令を追加します。|  
|[AllocateMethodBuffer メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-allocatemethodbuffer-method.md)|互換性のために残されています。 メソッドに対して指定したサイズのバッファーを作成し、メソッドの相対仮想アドレスを取得します。|  
|[ComputePointer メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-computepointer-method.md)|互換性のために残されています。 指定されたコードセクションのバッファーを決定します。|  
|[EmitString メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-emitstring-method.md)|互換性のために残されています。 指定した文字列をコードベースに出力します。|  
|[GenerateCeeFile メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-generateceefile-method.md)|互換性のために残されています。 この `ICeeGen`に現在読み込まれているコードベースを含むコードベースファイルを生成します。|  
|[GenerateCeeMemoryImage メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-generateceememoryimage-method.md)|互換性のために残されています。 コードベースのイメージをメモリ内に生成します。|  
|[GetIlSection メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getilsection-method.md)|互換性のために残されています。 指定したハンドルによって参照される中間言語コードベースのセクションを取得します。|  
|[GetIMapTokenIface メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getimaptokeniface-method.md)|互換性のために残されています。 指定したトークンによって参照されるインターフェイスを取得します。|  
|[GetMethodBuffer メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getmethodbuffer-method.md)|互換性のために残されています。 指定した相対仮想アドレスで、メソッドの適切なサイズのバッファーを取得します。|  
|[GetSectionBlock メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getsectionblock-method.md)|互換性のために残されています。 コードベースのセクションブロックを取得します。|  
|[GetSectionCreate メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getsectioncreate-method.md)|互換性のために残されています。 指定された名前とフラグ値を使用して、コードセクションを生成して取得します。|  
|[GetSectionDataLen メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getsectiondatalen-method.md)|互換性のために残されています。 指定したセクションの長さを取得します。|  
|[GetString メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getstring-method.md)|互換性のために残されています。 指定した相対仮想アドレスに格納されている文字列を取得します。|  
|[GetStringSection メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-getstringsection-method.md)|互換性のために残されています。 指定したハンドルによって参照されるコードセクションの文字列表現を取得します。|  
|[TruncateSection メソッド](../../../../docs/framework/unmanaged-api/metadata/iceegen-truncatesection-method.md)|互換性のために残されています。 指定したコードセクションを指定した長さだけ切り捨てます。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ インターフェイス](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
