---
title: ISymUnmanagedMethod インターフェイス
ms.date: 03/30/2017
api_name:
- ISymUnmanagedMethod
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedMethod
helpviewer_keywords:
- ISymUnmanagedMethod interface [.NET Framework debugging]
ms.assetid: f204d74c-cc79-4092-83bb-60654be95649
topic_type:
- apiref
ms.openlocfilehash: 1d3ccb2265f056d5776199d997dc067c8d5513e5
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448782"
---
# <a name="isymunmanagedmethod-interface"></a>ISymUnmanagedMethod インターフェイス
シンボルストア内のメソッドを表します。 このインターフェイスは、型関連の属性ではなく、メソッドのシンボル関連の属性にのみアクセスできるようにします。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetNamespace メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getnamespace-method.md)|このメソッドが定義されている名前空間を取得します。|  
|[GetOffset メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getoffset-method.md)|ドキュメント内の指定された位置に対応する、このメソッド内のオフセットを返します。|  
|[GetParameters メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getparameters-method.md)|このメソッドのパラメーターを取得します。|  
|[GetRanges メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getranges-method.md)|ドキュメント内の位置が指定されている場合、は、位置がこのメソッド内でカバーする Microsoft 中間言語 (MSIL) の範囲に対応する開始オフセットと終了オフセットのペアの配列を返します。|  
|[GetRootScope メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getrootscope-method.md)|このメソッド内のルート構文のスコープを取得します。 このスコープはメソッド全体を囲みます。|  
|[GetScopeFromOffSet メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getscopefromoffset-method.md)|指定されたオフセットを囲む、このメソッド内で最も外側にある構文のスコープを取得します。|  
|[GetSequencePointCount メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getsequencepointcount-method.md)|このメソッド内のシーケンスポイントの数を取得します。|  
|[GetSequencePoints メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getsequencepoints-method.md)|このメソッド内のすべてのシーケンスポイントを取得します。|  
|[GetSourceStartEnd メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-getsourcestartend-method.md)|このメソッドのソースのドキュメントの開始位置と終了位置を取得します。|  
|[GetToken メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-gettoken-method.md)|このメソッドのメタデータトークンを返します。|  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [シンボル ストア診断インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-interfaces.md)
