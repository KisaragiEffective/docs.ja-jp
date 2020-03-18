---
title: ICorDebugReferenceValue インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugReferenceValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugReferenceValue
helpviewer_keywords:
- ICorDebugReferenceValue interface [.NET Framework debugging]
ms.assetid: 2040e2be-119a-4cfb-ae52-b0b6f052665c
topic_type:
- apiref
ms.openlocfilehash: 2efba22b4ec372c5ddedd4982a29d66945d3511c
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792126"
---
# <a name="icordebugreferencevalue-interface"></a>ICorDebugReferenceValue インターフェイス
オブジェクトへの参照である値を管理するメソッドを提供します。 (つまり、このインターフェイスには、ポインターを管理するメソッドが用意されています)。このインターフェイスは、"ICorDebugValue" を実装します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Dereference メソッド](icordebugreferencevalue-dereference-method.md)|参照されているオブジェクトを取得します。|  
|[DereferenceStrong メソッド](icordebugreferencevalue-dereferencestrong-method.md)|実装されていません。 このメソッドを呼び出さないでください。|  
|[GetValue メソッド](icordebugreferencevalue-getvalue-method.md)|参照先のオブジェクトの現在のメモリアドレスを取得します。|  
|[IsNull メソッド](icordebugreferencevalue-isnull-method.md)|この `ICorDebugReferenceValue` が null 値であるかどうかを示す値を取得します。この場合、`ICorDebugReferenceValue` はオブジェクトを指していません。|  
|[SetValue メソッド](icordebugreferencevalue-setvalue-method.md)|現在のメモリアドレスを設定します。 つまり、このメソッドは、オブジェクトを指すようにこの `ICorDebugReferenceValue` を設定します。|  
  
## <a name="remarks"></a>コメント  
 共通言語ランタイム (CLR) は、デバッグされたプロセスが続行されると、オブジェクトのガベージコレクションを実行する場合があります。 ガベージコレクションでは、メモリ内でオブジェクトを移動できます。 `ICorDebugReferenceValue` はガベージコレクションと連携してガベージコレクションの後に情報が更新されるか、ガベージコレクションの前に暗黙的に無効になります。  
  
 デバッグされたプロセスが続行されると、`ICorDebugReferenceValue` オブジェクトが暗黙的に無効になる場合があります。 派生された "の値" は、明示的に解放または公開されるまで無効になりません。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
