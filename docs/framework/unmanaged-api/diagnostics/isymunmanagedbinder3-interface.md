---
description: 詳細については、「ISymUnmanagedBinder3 インターフェイス」を参照してください。
title: ISymUnmanagedBinder3 インターフェイス
ms.date: 03/30/2017
api_name:
- ISymUnmanagedBinder3
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedBinder3 Interface
helpviewer_keywords:
- ISymUnmanagedBinder3 interface [.NET Framework debugging]
ms.assetid: 37527a84-4b03-4f08-8135-94d898599089
topic_type:
- apiref
ms.openlocfilehash: 6d2172fb119076cea6d0f912fb3f403d36856599
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99689834"
---
# <a name="isymunmanagedbinder3-interface"></a>ISymUnmanagedBinder3 インターフェイス

シンボルバインダーインターフェイスを拡張します。 このインターフェイスを取得するには `QueryInterface` 、インターフェイスを実装するオブジェクトに対してを呼び出し `ISymUnmanagedBinder` ます。  
  
> [!IMPORTANT]
> 信頼されていないソースからプログラムデータベース (PDB) ファイルを開くと、セキュリティ上の危険があります。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetReaderFromCallback メソッド](isymunmanagedbinder3-getreaderfromcallback-method.md)|`IID_IDiaReadExeAtRVACallback` `IID_IDiaReadExeAtOffsetCallback` メモリからデバッグディレクトリ情報を取得するために、またはのいずれかを使用して、ユーザーがを実装または提供できるようにします。|  
  
## <a name="requirements"></a>要件  

 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>関連項目

- [シンボル ストア診断インターフェイス](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedBinder インターフェイス](isymunmanagedbinder-interface.md)
- [ISymUnmanagedBinder2 インターフェイス](isymunmanagedbinder2-interface.md)
