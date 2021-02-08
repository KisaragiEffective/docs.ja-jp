---
description: 詳細については、次のページを参照してください
title: ICorDebugILFrame::SetIP メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame.SetIP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame::SetIP
helpviewer_keywords:
- SetIP method, ICorDebugILFrame interface [.NET Framework debugging]
- ICorDebugILFrame::SetIP method [.NET Framework debugging]
ms.assetid: 23f38dc1-85e4-4263-9235-2d05bbb6a833
topic_type:
- apiref
ms.openlocfilehash: 75d2530a3d9151c64980316757074141776a78d3
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99791323"
---
# <a name="icordebugilframesetip-method"></a>ICorDebugILFrame::SetIP メソッド

命令ポインターを Microsoft 中間言語 (MSIL) コード内の指定されたオフセット位置に設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetIP (  
    [in] ULONG32 nOffset  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `nOffset`  
 MSIL コード内のオフセット位置。  
  
## <a name="remarks"></a>解説  

 を呼び出す `SetIP` と、現在のスレッドのすべてのフレームとチェーンがすぐに無効になります。 を呼び出した後にデバッガーがフレーム情報を必要とする場合は `SetIP` 、新しいスタックトレースを実行する必要があります。  
  
 [ICorDebug](icordebug-interface.md) は、スタックフレームを有効な状態のままにします。 ただし、フレームが有効な状態であっても、初期化されていないローカル変数などの問題が発生する可能性があります。 呼び出し元は、実行中のプログラムの一貫性を確保する役割を担います。  
  
 64ビットプラットフォームでは、命令ポインターをまたはブロックの外に移動することはできません `catch` `finally` 。 `SetIP`このような移動を64ビットプラットフォームで実行するためにを呼び出すと、エラーを示す HRESULT が返されます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
