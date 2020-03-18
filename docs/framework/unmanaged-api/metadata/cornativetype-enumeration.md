---
title: CorNativeType 列挙型
ms.date: 03/30/2017
api_name:
- CorNativeType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorNativeType
helpviewer_keywords:
- CorNativeType enumeration [.NET Framework metadata]
ms.assetid: e47a72f1-9609-48ed-bb34-97170d7f6890
topic_type:
- apiref
ms.openlocfilehash: ef4788891e91608a394482319a89b8b0d258449f
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436516"
---
# <a name="cornativetype-enumeration"></a>CorNativeType 列挙型
ネイティブのアンマネージ型を記述する値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorNativeType {  
  
    NATIVE_TYPE_END                  = 0x0,  
    NATIVE_TYPE_VOID                 = 0x1,  
    NATIVE_TYPE_BOOLEAN              = 0x2,  
    NATIVE_TYPE_I1                   = 0x3,  
    NATIVE_TYPE_U1                   = 0x4,  
    NATIVE_TYPE_I2                   = 0x5,  
    NATIVE_TYPE_U2                   = 0x6,  
    NATIVE_TYPE_I4                   = 0x7,  
    NATIVE_TYPE_U4                   = 0x8,  
    NATIVE_TYPE_I8                   = 0x9,  
    NATIVE_TYPE_U8                   = 0xa,  
    NATIVE_TYPE_R4                   = 0xb,  
    NATIVE_TYPE_R8                   = 0xc,  
    NATIVE_TYPE_SYSCHAR              = 0xd,  
    NATIVE_TYPE_VARIANT              = 0xe,  
    NATIVE_TYPE_CURRENCY             = 0xf,  
    NATIVE_TYPE_PTR                  = 0x10,  
  
    NATIVE_TYPE_DECIMAL              = 0x11,  
    NATIVE_TYPE_DATE                 = 0x12,  
    NATIVE_TYPE_BSTR                 = 0x13,  
    NATIVE_TYPE_LPSTR                = 0x14,  
    NATIVE_TYPE_LPWSTR               = 0x15,  
    NATIVE_TYPE_LPTSTR               = 0x16,  
    NATIVE_TYPE_FIXEDSYSSTRING       = 0x17,  
    NATIVE_TYPE_OBJECTREF            = 0x18,  
    NATIVE_TYPE_IUNKNOWN             = 0x19,  
    NATIVE_TYPE_IDISPATCH            = 0x1a,  
    NATIVE_TYPE_STRUCT               = 0x1b,  
    NATIVE_TYPE_INTF                 = 0x1c,  
    NATIVE_TYPE_SAFEARRAY            = 0x1d,  
    NATIVE_TYPE_FIXEDARRAY           = 0x1e,  
    NATIVE_TYPE_INT                  = 0x1f,  
    NATIVE_TYPE_UINT                 = 0x20,  
  
    NATIVE_TYPE_NESTEDSTRUCT         = 0x21,  
    NATIVE_TYPE_BYVALSTR             = 0x22,  
    NATIVE_TYPE_ANSIBSTR             = 0x23,  
    NATIVE_TYPE_TBSTR                = 0x24,  
    NATIVE_TYPE_VARIANTBOOL          = 0x25,  
    NATIVE_TYPE_FUNC                 = 0x26,  
  
    NATIVE_TYPE_ASANY                = 0x28,  
    NATIVE_TYPE_ARRAY                = 0x2a,  
    NATIVE_TYPE_LPSTRUCT             = 0x2b,  
    NATIVE_TYPE_CUSTOMMARSHALER      = 0x2c,  
    NATIVE_TYPE_IINSPECTABLE         = 0x2e,  
    NATIVE_TYPE_HSTRING              = 0x2f,  
  
    NATIVE_TYPE_ERROR                = 0x2d,   
  
    NATIVE_TYPE_MAX                  = 0x50  
  
} CorNativeType;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`NATIVE_TYPE_END`|互換性のために残されています。|  
|`NATIVE_TYPE_VOID`|互換性のために残されています。|  
|`NATIVE_TYPE_BOOLEAN`|4バイトのブール値。 TRUE は0以外で、FALSE は0です。|  
|`NATIVE_TYPE_I1`|符号付き8ビット整数値。|  
|`NATIVE_TYPE_U1`|8ビットの符号なし整数値。|  
|`NATIVE_TYPE_I2`|符号付き16ビット整数値。|  
|`NATIVE_TYPE_U2`|符号なし16ビット整数値です。|  
|`NATIVE_TYPE_I4`|符号付き 32 ビット整数値。|  
|`NATIVE_TYPE_U4`|32 ビットの符号なし整数値。|  
|`NATIVE_TYPE_I8`|64ビットの符号付き整数値。|  
|`NATIVE_TYPE_U8`|64ビットの符号なし整数値。|  
|`NATIVE_TYPE_R4`|4バイト浮動小数点数値。|  
|`NATIVE_TYPE_R8`|8バイト浮動小数点数値。|  
|`NATIVE_TYPE_SYSCHAR`|互換性のために残されています。|  
|`NATIVE_TYPE_VARIANT`|互換性のために残されています。|  
|`NATIVE_TYPE_CURRENCY`|マネージ <xref:System.Decimal> 型に対応する数値 COM 型。|  
|`NATIVE_TYPE_PTR`|互換性のために残されています。|  
|`NATIVE_TYPE_DECIMAL`|互換性のために残されています。|  
|`NATIVE_TYPE_DATE`|互換性のために残されています。|  
|`NATIVE_TYPE_BSTR`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_LPSTR`|LPSTR 文字列値。|  
|`NATIVE_TYPE_LPWSTR`|LPWSTR 文字列値。|  
|`NATIVE_TYPE_LPTSTR`|LPTSTR 文字列値。|  
|`NATIVE_TYPE_FIXEDSYSSTRING`|システム定義の固定文字列値。|  
|`NATIVE_TYPE_OBJECTREF`|互換性のために残されています。|  
|`NATIVE_TYPE_IUNKNOWN`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_IDISPATCH`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_STRUCT`|ネイティブ構造体の値。|  
|`NATIVE_TYPE_INTF`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_SAFEARRAY`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_FIXEDARRAY`|固定長配列値。|  
|`NATIVE_TYPE_INT`|ネイティブ16ビット符号付き整数値。|  
|`NATIVE_TYPE_UINT`|ネイティブ16ビット符号なし整数値。|  
|`NATIVE_TYPE_NESTEDSTRUCT`|互換性のために残されています。<br /><br /> NATIVE_TYPE_STRUCT を使用します。|  
|`NATIVE_TYPE_BYVALSTR`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_ANSIBSTR`|「COM 相互運用」を参照してください。|  
|`NATIVE_TYPE_TBSTR`|「COM 相互運用」を参照してください。<br /><br /> プラットフォームに応じて、BSTR または ANSIBSTR を選択します。|  
|`NATIVE_TYPE_VARIANTBOOL`|2バイトのブール値。 TRUE は-1、FALSE は0です。|  
|`NATIVE_TYPE_FUNC`|関数ポインター。|  
|`NATIVE_TYPE_ASANY`|任意のネイティブ型への参照。|  
|`NATIVE_TYPE_ARRAY`|指定されていない型のメンバーを持つ配列への参照。|  
|`NATIVE_TYPE_LPSTRUCT`|構造体への32ビット整数ポインター。|  
|`NATIVE_TYPE_CUSTOMMARSHALER`|カスタムマーシャラーネイティブ型。<br /><br /> この後には、"ネイティブ型名/0Custom marshaler 型名/0Custom cookie/0" または "{ネイティブ型 GUID}/0Custom marshaler 型名/0Custom cookie/0" という形式の文字列を指定する必要があります。|  
|`NATIVE_TYPE_ERROR`|「COM 相互運用」を参照してください。<br /><br /> ELEMENT_TYPE_I4 この型は VT_HRESULT にマップされます。|  
|`NATIVE_TYPE_IINSPECTABLE`|ネイティブ `IInspectable` 型。|  
|`NATIVE_TYPE_HSTRING`|ネイティブ `HString`。|  
|`NATIVE_TYPE_MAX`|無効な値です。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.UnmanagedType>
- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
