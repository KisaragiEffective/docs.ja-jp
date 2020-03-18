---
title: CorPinvokeMap 列挙型
ms.date: 03/30/2017
api_name:
- CorPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPinvokeMap
helpviewer_keywords:
- CorPinvokeMap enumeration [.NET Framework metadata]
ms.assetid: f14f986e-f6ce-42bc-aa23-18150c46d28c
topic_type:
- apiref
ms.openlocfilehash: 17b7af7016cf88fd3ae263dd952502d515b0c833
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441561"
---
# <a name="corpinvokemap-enumeration"></a>CorPinvokeMap 列挙型
PInvoke 呼び出しのオプションを指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum  CorPinvokeMap {  
  
    pmNoMangle          = 0x0001,  
  
    pmCharSetMask       = 0x0006,  
    pmCharSetNotSpec    = 0x0000,  
    pmCharSetAnsi       = 0x0002,  
    pmCharSetUnicode    = 0x0004,  
    pmCharSetAuto       = 0x0006,  
  
    pmBestFitUseAssem   = 0x0000,  
    pmBestFitEnabled    = 0x0010,  
    pmBestFitDisabled   = 0x0020,  
    pmBestFitMask       = 0x0030,  
  
    pmThrowOnUnmappableCharUseAssem   = 0x0000,  
    pmThrowOnUnmappableCharEnabled    = 0x1000,  
    pmThrowOnUnmappableCharDisabled   = 0x2000,  
    pmThrowOnUnmappableCharMask       = 0x3000,  
  
    pmSupportsLastError = 0x0040,   
  
    pmCallConvMask      = 0x0700,  
    pmCallConvWinapi    = 0x0100,  
    pmCallConvCdecl     = 0x0200,  
    pmCallConvStdcall   = 0x0300,  
    pmCallConvThiscall  = 0x0400,  
    pmCallConvFastcall  = 0x0500,  
  
    pmMaxValue          = 0xFFFF  
  
} CorPinvokeMap;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`pmNoMangle`|各メンバー名は、指定されたとおりに使用します。|  
|`pmCharSetMask`|予約済み。|  
|`pmCharSetNotSpec`|予約済み。|  
|`pmCharSetAnsi`|文字列を複数バイト文字列としてマーシャリングします。|  
|`pmCharSetUnicode`|文字列を Unicode 2 バイト文字としてマーシャリングします。|  
|`pmCharSetAuto`|対象のオペレーティングシステムに合わせて自動的に文字列をマーシャリングします。 既定値は、Windows NT、Windows 2000、Windows XP、および Windows Server 2003 ファミリの Unicode です。Windows 98 および Windows Me では、既定値は ANSI です。|  
|`pmBestFitUseAssem`|予約済み。|  
|`pmBestFitEnabled`|ANSI 文字セットと完全に一致しない Unicode 文字の最適マッピングを実行します。|  
|`pmBestFitDisabled`|Unicode 文字の最適マッピングを実行しないでください。 この場合、マップされていないすべての文字は、'? ' に置き換えられます。|  
|`pmBestFitMask`|予約済み。|  
|`pmThrowOnUnmappableCharUseAssem`|予約済み。|  
|`pmThrowOnUnmappableCharEnabled`|相互運用マーシャラーがマップされていない文字を検出したときに、例外をスローします。|  
|`pmThrowOnUnmappableCharDisabled`|相互運用マーシャラーがマップ不可能な文字を検出した場合は、例外をスローしないでください。|  
|`pmThrowOnUnmappableCharMask`|予約済み|  
|`pmSupportsLastError`|呼び出し先が、属性付きメソッドから戻る前に Win32 `SetLastError` 関数を呼び出すことができるようにします。|  
|`pmCallConvMask`|予約済み|  
|`pmCallConvWinapi`|既定のプラットフォーム呼び出し規約を使用します。 たとえば、Windows では、既定値は `StdCall` であり Windows CE .NET では `Cdecl`です。|  
|`pmCallConvCdecl`|`Cdecl` の呼び出し規約を使用します。 この場合、呼び出し元はスタックを消去します。 これにより、`varargs` (つまり、可変個のパラメーターを受け取る関数) を使用して関数を呼び出すことができます。|  
|`pmCallConvStdcall`|`StdCall` の呼び出し規約を使用します。 この場合、呼び出し先がスタックを消去します。 これは、プラットフォーム呼び出しでアンマネージ関数を呼び出す場合の既定の規則です。|  
|`pmCallConvThiscall`|`ThisCall` の呼び出し規約を使用します。 この場合、最初のパラメーターは `this` ポインターであり、レジスタ ECX に格納されます。 その他のパラメーターはスタックにプッシュされます。 `ThisCall` 呼び出し規約は、アンマネージ DLL からエクスポートされたクラスのメソッドを呼び出すために使用されます。|  
|`pmCallConvFastcall`|予約済み。|  
|`pmMaxValue`|予約済み。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
