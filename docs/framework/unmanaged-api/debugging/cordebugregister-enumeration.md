---
title: CorDebugRegister 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugRegister
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugRegister
helpviewer_keywords:
- CorDebugRegister enumeration [.NET Framework debugging]
ms.assetid: 003bb138-7960-4291-ac88-0d87e470ff70
topic_type:
- apiref
ms.openlocfilehash: 9af265144c9e38ffe132c16a318c374b08a920e3
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778247"
---
# <a name="cordebugregister-enumeration"></a>CorDebugRegister 列挙型
指定されたプロセッサ アーキテクチャに関連付けられたレジスタを指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugRegister {  
  
    REGISTER_INSTRUCTION_POINTER = 0,  
    REGISTER_STACK_POINTER,  
    REGISTER_FRAME_POINTER,  
  
    REGISTER_X86_EIP = 0,  
    REGISTER_X86_ESP,  
    REGISTER_X86_EBP,  
  
    REGISTER_X86_EAX,  
    REGISTER_X86_ECX,  
    REGISTER_X86_EDX,  
    REGISTER_X86_EBX,  
  
    REGISTER_X86_ESI,  
    REGISTER_X86_EDI,  
  
    REGISTER_X86_FPSTACK_0,  
    REGISTER_X86_FPSTACK_1,  
    REGISTER_X86_FPSTACK_2,  
    REGISTER_X86_FPSTACK_3,  
    REGISTER_X86_FPSTACK_4,  
    REGISTER_X86_FPSTACK_5,  
    REGISTER_X86_FPSTACK_6,  
    REGISTER_X86_FPSTACK_7,  
  
    REGISTER_AMD64_RIP = 0,  
    REGISTER_AMD64_RSP,  
    REGISTER_AMD64_RBP,  
    REGISTER_AMD64_RAX,  
    REGISTER_AMD64_RCX,  
    REGISTER_AMD64_RDX,  
    REGISTER_AMD64_RBX,  
    REGISTER_AMD64_RSI,  
    REGISTER_AMD64_RDI,  
    REGISTER_AMD64_R8,  
    REGISTER_AMD64_R9,  
    REGISTER_AMD64_R10,  
    REGISTER_AMD64_R11,  
    REGISTER_AMD64_R12,  
    REGISTER_AMD64_R13,  
    REGISTER_AMD64_R14,  
    REGISTER_AMD64_R15,  
  
    REGISTER_AMD64_XMM0,  
    REGISTER_AMD64_XMM1,  
    REGISTER_AMD64_XMM2,  
    REGISTER_AMD64_XMM3,  
    REGISTER_AMD64_XMM4,  
    REGISTER_AMD64_XMM5,  
    REGISTER_AMD64_XMM6,  
    REGISTER_AMD64_XMM7,  
    REGISTER_AMD64_XMM8,  
    REGISTER_AMD64_XMM9,  
    REGISTER_AMD64_XMM10,  
    REGISTER_AMD64_XMM11,  
    REGISTER_AMD64_XMM12,  
    REGISTER_AMD64_XMM13,  
    REGISTER_AMD64_XMM14,  
    REGISTER_AMD64_XMM15,  
  
    REGISTER_IA64_BSP = REGISTER_FRAME_POINTER,  
    REGISTER_IA64_R0  = REGISTER_IA64_BSP + 1,  
    REGISTER_IA64_F0  = REGISTER_IA64_R0  + 128,  
    REGISTER_ARM_PC = 0,  
    REGISTER_ARM_SP,  
    REGISTER_ARM_R0,  
    REGISTER_ARM_R1,  
    REGISTER_ARM_R2,  
    REGISTER_ARM_R3,  
    REGISTER_ARM_R4,  
    REGISTER_ARM_R5,  
    REGISTER_ARM_R6,  
    REGISTER_ARM_R7,  
    REGISTER_ARM_R8,  
    REGISTER_ARM_R9,  
    REGISTER_ARM_R10,  
    REGISTER_ARM_R11,  
    REGISTER_ARM_R12,  
    REGISTER_ARM_LR,  
  
} CorDebugRegister;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`REGISTER_INSTRUCTION_POINTER`|すべてのプロセッサ上の命令ポインター レジスタ。|  
|`REGISTER_STACK_POINTER`|すべてのプロセッサ上のスタック ポインター レジスタ。|  
|`REGISTER_FRAME_POINTER`|すべてのプロセッサ上のフレーム ポインター レジスタ。|  
|`REGISTER_X86_EIP`|x86 プロセッサ上の命令ポインター レジスタ。|  
|`REGISTER_X86_ESP`|x86 プロセッサ上のスタック ポインター レジスタ。|  
|`REGISTER_X86_EBP`|x86 プロセッサ上の基本ポインター レジスタ。|  
|`REGISTER_X86_EAX`|x86 プロセッサ上の A データ レジスタ。|  
|`REGISTER_X86_ECX`|x86 プロセッサ上の C データ レジスタ。|  
|`REGISTER_X86_EDX`|x86 プロセッサ上の D データ レジスタ。|  
|`REGISTER_X86_EBX`|x86 プロセッサ上の B データ レジスタ。|  
|`REGISTER_X86_ESI`|x86 プロセッサ上のソース インデックス レジスタ。|  
|`REGISTER_X86_EDI`|x86 プロセッサ上のターゲット インデックス レジスタ。|  
|`REGISTER_X86_FPSTACK_0`|x86 浮動小数点 (FP) プロセッサ上のスタック レジスタ 0。|  
|`REGISTER_X86_FPSTACK_1`|x86 FP プロセッサ上の #1 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_2`|x86 FP プロセッサ上の #2 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_3`|x86 FP プロセッサ上の #3 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_4`|x86 FP プロセッサ上の #4 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_5`|x86 FP プロセッサ上の #5 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_6`|x86 FP プロセッサ上の #6 スタック レジスタ。|  
|`REGISTER_X86_FPSTACK_7`|x86 FP プロセッサ上の #7 スタック レジスタ。|  
|`REGISTER_AMD64_RIP`|AMD64 プロセッサ上の命令ポインター レジスタ。|  
|`REGISTER_AMD64_RSP`|AMD64 プロセッサ上のスタック ポインター レジスタ。|  
|`REGISTER_AMD64_RBP`|AMD64 プロセッサ上の基本ポインター レジスタ。|  
|`REGISTER_AMD64_RAX`|AMD64 プロセッサ上の A データ レジスタ。|  
|`REGISTER_AMD64_RCX`|AMD64 プロセッサ上の C データ レジスタ。|  
|`REGISTER_AMD64_RDX`|AMD64 プロセッサ上の D データ レジスタ。|  
|`REGISTER_AMD64_RBX`|AMD64 プロセッサ上の B データ レジスタ。|  
|`REGISTER_AMD64_RSI`|AMD64 プロセッサ上のソース インデックス レジスタ。|  
|`REGISTER_AMD64_RDI`|AMD64 プロセッサ上のターゲット インデックス レジスタ。|  
|`REGISTER_AMD64_R8`|AMD64 プロセッサ上の #8 データ レジスタ。|  
|`REGISTER_AMD64_R9`|AMD64 プロセッサ上の #9 データ レジスタ。|  
|`REGISTER_AMD64_R10`|AMD64 プロセッサ上の #10 データ レジスタ。|  
|`REGISTER_AMD64_R11`|AMD64 プロセッサ上の #11 データ レジスタ。|  
|`REGISTER_AMD64_R12`|AMD64 プロセッサ上の #12 データ レジスタ。|  
|`REGISTER_AMD64_R13`|AMD64 プロセッサ上の #13 データ レジスタ。|  
|`REGISTER_AMD64_R14`|AMD64 プロセッサ上の #14 データ レジスタ。|  
|`REGISTER_AMD64_R15`|AMD64 プロセッサ上の #15 データ レジスタ。|  
|`REGISTER_AMD64_XMM0`|AMD64 プロセッサ上の #0 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM1`|AMD64 プロセッサ上の #1 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM2`|AMD64 プロセッサ上の #2 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM3`|AMD64 プロセッサ上の #3 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM4`|AMD64 プロセッサ上の #4 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM5`|AMD64 プロセッサ上の #5 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM6`|AMD64 プロセッサ上の #6 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM7`|AMD64 プロセッサ上の #7 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM8`|AMD64 プロセッサ上の #8 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM9`|AMD64 プロセッサ上の #9 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM10`|AMD64 プロセッサ上の #10 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM11`|AMD64 プロセッサ上の #11 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM12`|AMD64 プロセッサ上の #12 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM13`|AMD64 プロセッサ上の #13 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM14`|AMD64 プロセッサ上の #14 マルチメディア レジスタ。|  
|`REGISTER_AMD64_XMM15`|AMD64 プロセッサ上の #15 マルチメディア レジスタ。|  
|`REGISTER_IA64_BSP`|IA-64 プロセッサ上のスタック ポインター レジスタ。|  
|`REGISTER_IA64_R0`|IA-64 プロセッサ上の #0 データ レジスタ。|  
|`REGISTER_IA64_F0`|IA-64 プロセッサ上の #0 FP データ レジスタ。|  
|`REGISTER_ARM_PC`|ARM プロセッサ上のプログラム カウンター レジスタ (R15)。|  
|`REGISTER_ARM_SP`|ARM プロセッサ上のスタック ポインター レジスタ (R13)。|  
|`REGISTER_ARM_R0`|ARM プロセッサ上のデータ レジスタ R0。|  
|`REGISTER_ARM_R1`|ARM プロセッサ上のデータ レジスタ R1。|  
|`REGISTER_ARM_R2`|ARM プロセッサ上のデータ レジスタ R2。|  
|`REGISTER_ARM_R3`|ARM プロセッサ上のデータ レジスタ R3。|  
|`REGISTER_ARM_R4`|ARM プロセッサ上のレジスタ R4。|  
|`REGISTER_ARM_R5`|ARM プロセッサ上のレジスタ R5。|  
|`REGISTER_ARM_R6`|ARM プロセッサ上のレジスタ R6。|  
|`REGISTER_ARM_R7`|ARM プロセッサ上のレジスタ R7 (THUMB フレーム ポインター)。|  
|`REGISTER_ARM_R8`|ARM プロセッサ上のレジスタ R8。|  
|`REGISTER_ARM_R9`|ARM プロセッサ上のレジスタ R9。|  
|`REGISTER_ARM_R10`|ARM プロセッサ上のレジスタ R10。|  
|`REGISTER_ARM_R11`|ARM プロセッサ上のフレーム ポインター。|  
|`REGISTER_ARM_R12`|ARM プロセッサ上のレジスタ R12。|  
|`REGISTER_ARM_LR`|ARM プロセッサ上のリンク レジスタ (R14)。|  
  
## <a name="remarks"></a>コメント  
 IA-64 プロセッサには、128 個の汎用データ レジスタおよび 128 個の浮動小数点データ レジスタがありますが、提供されているのは `REGISTER_IA64_R0` および `REGISTER_IA64_F0` の値のみです。 その他の値は、次のように判断されます。  
  
- IA-64 プロセッサ上の #1 データ レジスタから #127 データ レジスタに対応する、`REGISTER_IA64_R0` から `REGISTER_IA64_R1` までの値の `REGISTER_IA64_R127` にレジスタ番号を追加します。  
  
- IA-64 プロセッサ上の #1 FP データ レジスタから #127 FP データ レジスタに対応する、`REGISTER_IA64_F0` から `REGISTER_IA64_F1` までの値の `REGISTER_IA64_F127` にレジスタ番号を追加します。  
  
 たとえば、IA-64 プロセッサ上で #83 データ レジスタを指定する必要がある場合、`REGISTER_IA64_R0` + 83 を使用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
