---
title: QualifierSet_GetNames 関数 (アンマネージド API リファレンス)
description: QualifierSet_GetNames 関数を使用すると、オブジェクトまたはプロパティから修飾子の名前が取得されます。
ms.date: 11/06/2017
api_name:
- QualifierSet_GetNames
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_GetNames
helpviewer_keywords:
- QualifierSet_GetNames function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: bd0a67987dd8ffa825114726d066249aed40cf05
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141694"
---
# <a name="qualifierset_getnames-function"></a>QualifierSet_GetNames 関数

現在のオブジェクトまたはプロパティから使用できるすべての修飾子または特定の修飾子の名前が取得されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT QualifierSet_GetNames (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LONG                 lFlags,
   [out] SAFEARRAY (BSTR)**  pstrNames
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) インスタンスへのポインター。

`lFlags`\
[in] 列挙に含める名前を指定する、次のいずれかのフラグまたは値。

|定数  |[値]  |説明  |
|---------|---------|---------|
|  | 0 | すべての修飾子の名前を返します。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 現在のプロパティまたはオブジェクトに固有の修飾子の名前のみを返します。 <br/> プロパティの場合: プロパティに固有の修飾子だけを返します (オーバーライドを含む)。クラス定義から反映された修飾子は返しません。 <br/> インスタンスの場合: インスタンス固有の修飾子名だけを返します。 <br/> クラスの場合: 派生するクラスに固有の修飾子だけを返します。
|`WBEM_FLAG_PROPAGATED_ONLY` | 0x20 | 別のオブジェクトから反映された修飾子の名前だけを返します。 <br/> プロパティの場合: クラス定義からこのプロパティに反映された修飾子だけを返します。プロパティ自体からは返しません。 <br/> インスタンスの場合: クラス定義から反映された修飾子だけを返します。 <br/> クラスの場合: 親クラスから継承された修飾子名だけを返します。 |

`pstrNames`\
[out] 要求した名前が格納されている新しい `SAFEARRAY`。 配列には要素が含まれていない場合があります。 エラーが発生した場合、新しい `SAFEARRAY` は返されません。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 新しい列挙を開始するのに十分なメモリがありません。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemQualifierSet::GetNames](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-getnames) メソッドの呼び出しがラップされます。

修飾子名を取得したら、[QualifierSet_Get](qualifierset-get.md) 関数を呼び出すことによって、名前で各修飾子にアクセスできます。

特定のオブジェクトに修飾子がなくてもエラーではないため、関数から `WBEM_S_NO_ERROR` が返された場合でも、戻った時点で `pstrNames` 内の文字列の数が 0 になることがあります。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
