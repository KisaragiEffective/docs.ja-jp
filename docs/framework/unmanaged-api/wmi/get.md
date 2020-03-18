---
title: Get 関数 (アンマネージ API リファレンス)
description: Get 関数は、指定されたプロパティ値を取得します。
ms.date: 11/06/2017
api_name:
- Get
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Get
helpviewer_keywords:
- Get function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 60f29b91000fd3c07efea88dcc319eb283a4af78
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120319"
---
# <a name="get-function"></a>Get 関数

指定されたプロパティ値が存在する場合は、その値を取得します。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT Get (
   [in] int               vFunc, 
   [in] IWbemClassObject* ptr, 
   [in] LPCWSTR           wszName,
   [in] LONG              lFlags,
   [out] VARIANT*         pVal,
   [out] CIMTYPE*         pvtType,
   [out] LONG*            plFlavor
); 
```

## <a name="parameters"></a>パラメーター

`vFunc`\
からこのパラメーターは使用されていません。

`ptr`\
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

`wszName`\
からプロパティの名前。

`lFlags`\
[in] 予約されています。 このパラメーターには0を指定する必要があります。

`pVal`\
入出力関数が正常に返された場合は、`wszName` プロパティの値を格納します。 `pval` 引数には、修飾子の正しい型と値が割り当てられます。

`pvtType`\
入出力関数が正常に返された場合、はプロパティの型を示す[CIM 型の定数](/windows/win32/api/wbemcli/ne-wbemcli-cimtype_enumeration)を格納します。 この値は、`null`することもできます。 

`plFlavor`\
入出力関数が正常に返された場合、はプロパティの配信元に関する情報を受け取ります。 この値には `null`、または*WbemCli*ヘッダーファイルで定義されている次の WBEM_FLAVOR_TYPE 定数のいずれかを指定できます。 

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAVOR_ORIGIN_SYSTEM` | 0x40 | プロパティは、標準のシステムプロパティです。 |
| `WBEM_FLAVOR_ORIGIN_PROPAGATED` | 0x20 | クラスの場合: プロパティは親クラスから継承されます。 <br> インスタンスの場合: 親クラスから継承されたプロパティは、インスタンスによって変更されていません。  |
| `WBEM_FLAVOR_ORIGIN_LOCAL` | 0 | クラスの場合: プロパティは派生クラスに属します。 <br> インスタンスの場合: プロパティは、インスタンスによって変更されます。つまり、値が指定されたか、または修飾子が追加または変更されたことを示します。 |

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 1つ以上のパラメーターが無効です。 |
|`WBEM_E_NOT_FOUND` | 0x80041002 | 指定されたプロパティが見つかりませんでした。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: Get](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-get)メソッドの呼び出しをラップします。

`Get` 関数は、システムプロパティも返すことができます。

`pVal` 引数には、修飾子と COM [VariantInit](https://docs.microsoft.com/previous-versions/windows/desktop/api/oleauto/nf-oleauto-variantinit)関数の正しい型と値が割り当てられます。

## <a name="requirements"></a>［要件］

 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

 **ヘッダー:** WMINet_Utils

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
