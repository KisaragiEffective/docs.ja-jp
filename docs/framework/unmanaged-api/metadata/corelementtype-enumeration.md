---
title: CorElementType 列挙型
ms.date: 03/30/2017
api_name:
- CorElementType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorElementType
helpviewer_keywords:
- CorElementType enumeration [.NET Framework metadata]
ms.assetid: c3809c8f-1737-4f0f-9442-0c01ee689871
topic_type:
- apiref
ms.openlocfilehash: a4e9268d292004f447b30c82f1db4d0fe58404fe
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937945"
---
# <a name="corelementtype-enumeration"></a>CorElementType 列挙型

共通言語ランタイム <xref:System.Type>、型修飾子、またはメタデータ型シグネチャの型に関する情報を指定します。

## <a name="syntax"></a>構文

```cpp
typedef enum CorElementType {
    ELEMENT_TYPE_END            = 0x0,
    ELEMENT_TYPE_VOID           = 0x1,
    ELEMENT_TYPE_BOOLEAN        = 0x2,
    ELEMENT_TYPE_CHAR           = 0x3,
    ELEMENT_TYPE_I1             = 0x4,
    ELEMENT_TYPE_U1             = 0x5,
    ELEMENT_TYPE_I2             = 0x6,
    ELEMENT_TYPE_U2             = 0x7,
    ELEMENT_TYPE_I4             = 0x8,
    ELEMENT_TYPE_U4             = 0x9,
    ELEMENT_TYPE_I8             = 0xa,
    ELEMENT_TYPE_U8             = 0xb,
    ELEMENT_TYPE_R4             = 0xc,
    ELEMENT_TYPE_R8             = 0xd,
    ELEMENT_TYPE_STRING         = 0xe,

    ELEMENT_TYPE_PTR            = 0xf,
    ELEMENT_TYPE_BYREF          = 0x10,

    ELEMENT_TYPE_VALUETYPE      = 0x11,
    ELEMENT_TYPE_CLASS          = 0x12,
    ELEMENT_TYPE_VAR            = 0x13,
    ELEMENT_TYPE_ARRAY          = 0x14,
    ELEMENT_TYPE_GENERICINST    = 0x15,
    ELEMENT_TYPE_TYPEDBYREF     = 0x16,

    ELEMENT_TYPE_I              = 0x18,
    ELEMENT_TYPE_U              = 0x19,
    ELEMENT_TYPE_FNPTR          = 0x1B,
    ELEMENT_TYPE_OBJECT         = 0x1C,
    ELEMENT_TYPE_SZARRAY        = 0x1D,
    ELEMENT_TYPE_MVAR           = 0x1e,

    ELEMENT_TYPE_CMOD_REQD      = 0x1F,
    ELEMENT_TYPE_CMOD_OPT       = 0x20,

    ELEMENT_TYPE_INTERNAL       = 0x21,
    ELEMENT_TYPE_MAX            = 0x22,

    ELEMENT_TYPE_MODIFIER       = 0x40,
    ELEMENT_TYPE_SENTINEL       = 0x01 | ELEMENT_TYPE_MODIFIER,
    ELEMENT_TYPE_PINNED         = 0x05 | ELEMENT_TYPE_MODIFIER

} CorElementType;
```

## <a name="members"></a>メンバー

|メンバー|説明|
|------------|-----------------|
|`ELEMENT_TYPE_END`|内部使用。|
|`ELEMENT_TYPE_VOID`|Void 型。|
|`ELEMENT_TYPE_BOOLEAN`|ブール型|
|`ELEMENT_TYPE_CHAR`|文字型。|
|`ELEMENT_TYPE_I1`|符号付き1バイト整数。|
|`ELEMENT_TYPE_U1`|1 バイトの符号なし整数。|
|`ELEMENT_TYPE_I2`|符号付き2バイト整数。|
|`ELEMENT_TYPE_U2`|符号なし2バイト整数。|
|`ELEMENT_TYPE_I4`|4バイトの符号付き整数。|
|`ELEMENT_TYPE_U4`|4バイトの符号なし整数。|
|`ELEMENT_TYPE_I8`|8バイトの符号付き整数。|
|`ELEMENT_TYPE_U8`|8バイトの符号なし整数。|
|`ELEMENT_TYPE_R4`|4バイト浮動小数点。|
|`ELEMENT_TYPE_R8`|8バイト浮動小数点。|
|`ELEMENT_TYPE_STRING`|System.string 型。|
|`ELEMENT_TYPE_PTR`|ポインター型修飾子。|
|`ELEMENT_TYPE_BYREF`|参照型修飾子。|
|`ELEMENT_TYPE_VALUETYPE`|値型修飾子。|
|`ELEMENT_TYPE_CLASS`|クラス型修飾子。|
|`ELEMENT_TYPE_VAR`|クラス変数の型修飾子。|
|`ELEMENT_TYPE_ARRAY`|多次元配列型修飾子。|
|`ELEMENT_TYPE_GENERICINST`|ジェネリック型の型修飾子。|
|`ELEMENT_TYPE_TYPEDBYREF`|型指定された参照。|
|`ELEMENT_TYPE_I`|ネイティブ整数のサイズ。|
|`ELEMENT_TYPE_U`|符号なしネイティブ整数のサイズ。|
|`ELEMENT_TYPE_FNPTR`|関数へのポインター。|
|`ELEMENT_TYPE_OBJECT`|System.object 型。|
|`ELEMENT_TYPE_SZARRAY`|1次元の下限の配列型修飾子。|
|`ELEMENT_TYPE_MVAR`|メソッド変数の型修飾子。|
|`ELEMENT_TYPE_CMOD_REQD`|C 言語で必要な修飾子。|
|`ELEMENT_TYPE_CMOD_OPT`|C 言語の省略可能な修飾子。|
|`ELEMENT_TYPE_INTERNAL`|内部使用。|
|`ELEMENT_TYPE_MAX`|無効な型。|
|`ELEMENT_TYPE_MODIFIER`|内部使用。|
|`ELEMENT_TYPE_SENTINEL`|可変個のパラメーターのリストの sentinel である型修飾子。|
|`ELEMENT_TYPE_PINNED`|内部使用。|

## <a name="remarks"></a>Remarks

型修飾子は、より複雑な型を表すための基礎となります。 型シグネチャの直後に続く値に、`CorElementType` 型修飾子の値が適用されます。 `CorElementType` 型修飾子の値の後に続く値は、次の表に示すように `CorElementType` 単純型の値、メタデータトークン、またはその他の値にすることができます。

> [!NOTE]
> すべての数値 (*数値*、*引数の数*、*メタデータトークン*、*順位*、*カウント*、および*バインド*) は、圧縮された整数として格納されます。 詳細については、ECMA Web サイトの「 [STANDARD ECMA-335-共通言語基盤 (CLI)](http://www.ecma-international.org/publications/standards/Ecma-335.htm) 」を参照してください。

|型修飾子|形式|
|-------------------|------------|
|`ELEMENT_TYPE_PTR`|`CorElementType` 値 \<ELEMENT_TYPE_PTR >|
|`ELEMENT_TYPE_BYREF`|`CorElementType` 値 \<ELEMENT_TYPE_BYREF >|
|`ELEMENT_TYPE_VALUETYPE`|`mdTypeDef` メタデータトークンを \<ELEMENT_TYPE_VALUETYPE >|
|`ELEMENT_TYPE_CLASS`|`mdTypeDef` メタデータトークンを \<ELEMENT_TYPE_CLASS >|
|`ELEMENT_TYPE_VAR`|ELEMENT_TYPE_VAR \<数 >|
|`ELEMENT_TYPE_ARRAY`|ELEMENT_TYPE_ARRAY \<a `CorElementType` value> \<rank> \<count1> \<bound1> ... \<countN> \<boundN>|
|`ELEMENT_TYPE_GENERICINST`|`mdTypeDef` のメタデータ > トークンを \<ELEMENT_TYPE_GENERICINST \<の引数の数 > \<arg1 >... \<argN >|
|`ELEMENT_TYPE_FNPTR`|呼び出し規約を含め、関数の完全なシグネチャを ELEMENT_TYPE_FNPTR \<|
|`ELEMENT_TYPE_SZARRAY`|`CorElementType` 値 \<ELEMENT_TYPE_SZARRAY >|
|`ELEMENT_TYPE_MVAR`|ELEMENT_TYPE_MVAR \<数 >|
|`ELEMENT_TYPE_CMOD_REQD`|`mdTypeRef` または `mdTypeDef` メタデータトークンを\<ELEMENT_TYPE_ >|
|`ELEMENT_TYPE_CMOD_OPT`|`mdTypeRef` または `mdTypeDef` メタデータトークンを \<E_T_CMOD_OPT >|

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー:** CorHdr. h

**.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>関連項目

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
