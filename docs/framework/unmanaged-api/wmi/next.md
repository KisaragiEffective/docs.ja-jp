---
title: Next 関数 (アンマネージ API リファレンス)
description: 次の関数は、列挙体の次のプロパティを取得します。
ms.date: 11/06/2017
api_name:
- Next
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Next
helpviewer_keywords:
- Next function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 587e085f6fe9f6c19d3605c673cd3bd6f68162f1
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127377"
---
# <a name="next-function"></a>Next 関数
[Beginenumeration](beginenumeration.md)への呼び出しで始まる列挙体の次のプロパティを取得します。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT Next (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              lFlags,
   [out] BSTR*            pstrName,
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

`lFlags`\
[in] 予約されています。 このパラメーターには0を指定する必要があります。

`pstrName`\
入出力プロパティ名を含む新しい `BSTR`。 名前が不要な場合は、このパラメーターを `null` に設定できます。

`pVal`\
入出力プロパティの値が格納されている `VARIANT`。 値が不要な場合は、このパラメーターを `null` に設定できます。 関数がエラーコードを返すと、`pVal` に渡された `VARIANT` が変更されずに残ります。

`pvtType`\
入出力`CIMTYPE` 変数 (プロパティの型が配置される `LONG`) へのポインター。 このプロパティの値には `VT_NULL_VARIANT`を指定できます。この場合、プロパティの実際の型を決定する必要があります。 このパラメーターは、`null`することもできます。

`plFlavor`\
[out] `null`、またはプロパティの配信元に関する情報を受け取る値。 使用可能な値については、[解説] セクションを参照してください。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_UNEXPECTED` | 0x8004101d | [`BeginEnumeration`](beginenumeration.md)関数の呼び出しがありませんでした。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 新しい列挙を開始するために必要なメモリが不足しています。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと Windows の管理の間のリモートプロシージャコールが失敗しました。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |
| `WBEM_S_NO_MORE_DATA` | 0x40005 | 列挙体にはそれ以上のプロパティがありません。 |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: Next](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-next)メソッドの呼び出しをラップします。

このメソッドは、システムプロパティも返します。

プロパティの基になる型がオブジェクトパス、日付または時刻、または別の特殊な型である場合、返される型には十分な情報が含まれていません。 呼び出し元は、指定されたプロパティの `CIMTYPE` を調べて、プロパティがオブジェクト参照、日付または時刻、または別の特殊な型であるかどうかを判断する必要があります。

`plFlavor` が `null`されていない場合、`LONG` の値は、次のように、プロパティの配信元に関する情報を受け取ります。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAVOR_ORIGIN_SYSTEM` | 0x40 | プロパティは、標準のシステムプロパティです。 |
| `WBEM_FLAVOR_ORIGIN_PROPAGATED` | 0x20 | クラスの場合: プロパティは親クラスから継承されます。 <br> インスタンスの場合: 親クラスから継承されたプロパティは、インスタンスによって変更されていません。  |
| `WBEM_FLAVOR_ORIGIN_LOCAL` | 0 | クラスの場合: プロパティは派生クラスに属します。 <br> インスタンスの場合: プロパティは、インスタンスによって変更されます。つまり、値が指定されたか、または修飾子が追加または変更されたことを示します。 |

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
