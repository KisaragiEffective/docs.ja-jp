---
title: StackTrace_SimpleContext 構造体
ms.date: 03/30/2017
api_name:
- StackTrace_SimpleContext
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- SimpleContext
helpviewer_keywords:
- SimpleContext structure [.NET Framework debugging]
- StackTrace_SimpleContext structure [.NET Framework debugging]
ms.assetid: d4cef11f-a8ca-49bc-a1b8-6631f9e28f3e
topic_type:
- apiref
ms.openlocfilehash: c81a5787eb06971e3d52aff5d1c1154a72564daf
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790330"
---
# <a name="stacktrace_simplecontext-structure"></a>StackTrace_SimpleContext 構造体
完全な `CONTEXT` 構造の代わりに使用できる単純なコンテキストを提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
struct StackTrace_SimpleContext  
{  
    ULONG64 StackOffset;       // ESP on x86  
    ULONG64 FrameOffset;       // EBP on x86  
    ULONG64 InstructionOffset; // EIP on x86  
};  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`StackOffset`|スタックポインター、または x86 プラットフォームの enter スタックポインター (ESP)。|  
|`FrameOffset`|フレームオフセット、または x86 プラットフォームでの EBP レジスタ。|  
|`InstructionOffset`|命令ポインター、または x86 プラットフォームの enter 命令ポインター (EIP)。|  
  
## <a name="remarks"></a>コメント  
 通常、スタックトレース関数はアドレス、フレームオフセット、およびスタックアドレスだけを返す必要があるため、必要に応じて、大きな `CONTEXT` 構造の代わりに `SimpleContext` 構造を使用できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** SOS_Stacktrace  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ構造体](debugging-structures.md)
- [デバッグ](index.md)
