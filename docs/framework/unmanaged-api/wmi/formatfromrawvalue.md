---
title: FormatFromRawValue 関数 (アンマネージ API リファレンス)
description: FormatFromRawValue 関数は、生のパフォーマンスデータを指定された形式に変換します。
ms.date: 11/21/2017
api_name:
- FormatFromRawValue
api_location:
- PerfCounter.dll
api_type:
- DLLExport
f1_keywords:
- FormatFromRawValue
helpviewer_keywords:
- FormatFromRawValue function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 5097cfe43ae785461a1e2af1217bcbd5e8c4b79c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120287"
---
# <a name="formatfromrawvalue-function"></a>FormatFromRawValue 関数
1 つの生のパフォーマンス データ値が指定した形式に変換されます。この形式変換が時間ベースである場合は、2 つの生のパフォーマンス データ値が変換されます。 

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
int FormatFromRawValue (
   [in] uint                    dwCounterType, 
   [in] uint                    dwFormat, 
   [in] long*                   pTimeBase,
   [in] PDH_RAW_COUNTER*        pRawValue1,
   [in] PDH_RAW_COUNTER*        pRawValue2,
   [out] PDH_FMT_COUNTERVALUE*  pFmtValue
); 
```

## <a name="parameters"></a>パラメーター

`dwCounterType`\
からカウンターの種類。 カウンターの種類の一覧については、「 [WMI パフォーマンスカウンターの種類](/windows/desktop/WmiSdk/wmi-performance-counter-types)」を参照してください。 `dwCounterType` には、`PERF_LARGE_RAW_FRACTION` と `PERF_LARGE_RAW_BASE`を除く任意の種類のカウンターを指定できます。 

`dwFormat`\
から生のパフォーマンスデータを変換する形式。 次のいずれかの値を指定できます。

|定数  |[値]  |説明 |
|---------|---------|---------|
| `PDH_FMT_DOUBLE` |0x00000200 | 計算された値を倍精度浮動小数点値として返します。 | 
| `PDH_FMT_LARGE` | 0x00000400 | 計算された値を64ビット整数として返します。 |
| `PDH_FMT_LONG` | 0x00000100 | 計算された値を32ビット整数として返します。 |

前の値の1つは、次のいずれかのスケーリングフラグと共に使用できます。

|定数  |[値]  |説明 |
|---------|---------|---------|
| `PDH_FMT_NOSCALE` | 0x00001000 | カウンターのスケールファクターは適用しないでください。 |
| `PDH_FMT_1000` | 0x00002000 | 最終的な値を1000で乗算します。 | 

`pTimeBase`\
から形式変換に必要な場合は、タイムベースへのポインター。 形式変換に時間ベース情報が不要な場合、このパラメーターの値は無視されます。

`pRawValue1`\ [in] は、生のパフォーマンス値を表す[`PDH_RAW_COUNTER`](/windows/win32/api/pdh/ns-pdh-pdh_raw_counter)構造体へのポインターです。

`pRawValue2`\
から2番目の生のパフォーマンス値を表す[`PDH_RAW_COUNTER`](/windows/win32/api/pdh/ns-pdh-pdh_raw_counter)構造体へのポインター。 2番目の生のパフォーマンス値が不要な場合は、このパラメーターを `null`する必要があります。

`pFmtValue`\
入出力書式設定されたパフォーマンス値を受け取る[`PDH_FMT_COUNTERVALUE`](/windows/win32/api/pdh/ns-pdh-pdh_fmt_countervalue)構造体へのポインター。

## <a name="return-value"></a>戻り値

この関数によって返される値は次のとおりです。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `ERROR_SUCCESS` | 0 | 関数の呼び出しは成功しました。 |
| `PDH_INVALID_ARGUMENT` | 0xC0000BBD | 必須の引数が指定されていないか、正しくありません。 | 
| `PDH_INVALID_HANDLE` | 0xC0000BBC | ハンドルは、有効な PDH オブジェクトではありません。 |

## <a name="remarks"></a>Remarks

この関数は、 [FormatFromRawValue](https://docs.microsoft.com/previous-versions/ms231047(v=vs.85))関数の呼び出しをラップします。

## <a name="requirements"></a>［要件］

 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

 **ライブラリ:** PerfCounter .dll

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
