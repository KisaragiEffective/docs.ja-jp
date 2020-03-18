---
title: ICorDebugNativeFrame::SetIP メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame.SetIP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame::SetIP
helpviewer_keywords:
- ICorDebugNativeFrame::SetIP method [.NET Framework debugging]
- SetIP method, ICorDebugNativeFrame interface [.NET Framework debugging]
ms.assetid: 57784a51-c76d-48f8-9392-584d0e1946d9
topic_type:
- apiref
ms.openlocfilehash: bc33768e4155a0e272d3374d4c586c79ef2ff3fb
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792779"
---
# <a name="icordebugnativeframesetip-method"></a>ICorDebugNativeFrame::SetIP メソッド
命令ポインターをネイティブコード内の指定されたオフセット位置に設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetIP (  
    [in] ULONG32 nOffset  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `nOffset`  
 からネイティブコード内のオフセット位置。  
  
## <a name="remarks"></a>コメント  
 `SetIP` を呼び出すと、現在のスレッドのすべてのフレームとチェーンがすぐに無効になります。 `SetIP`の呼び出しの後にデバッガーがフレーム情報を必要とする場合は、新しいスタックトレースを実行する必要があります。  
  
 [ICorDebug](icordebug-interface.md)は、スタックフレームを有効な状態のままにします。 ただし、フレームが有効な状態の場合でも、ランタイムの場合と同様に、初期化されていないローカル変数などの問題がまだ発生している可能性があります。 呼び出し元は、実行中のプログラムの一貫性を保証する役割を担います。  
  
 64ビットプラットフォームでは、命令ポインターを `catch` または `finally` ブロックの外に移動することはできません。 64ビットプラットフォーム上でこのような移動を行うために `SetIP` を呼び出すと、失敗を示す HRESULT が返されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目
