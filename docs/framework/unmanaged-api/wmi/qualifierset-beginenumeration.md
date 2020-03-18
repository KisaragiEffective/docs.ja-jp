---
title: QualifierSet_BeginEnumeration 関数 (アンマネージ API リファレンス)
description: QualifierSet_BeginEnumeration 関数は、オブジェクトの修飾子の列挙子をリセットします。
ms.date: 11/06/2017
api_name:
- QualifierSet_BeginEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_BeginEnumeration
helpviewer_keywords:
- QualifierSet_BeginEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 79edbd876fc9992f088b9adb159e005c735a72cb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127323"
---
# <a name="qualifierset_beginenumeration-function"></a>QualifierSet_BeginEnumeration 関数

オブジェクトの修飾子の列挙子が列挙型の先頭にリセットされます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT QualifierSet_BeginEnumeration (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LONG                 lFlags
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
からこのパラメーターは使用されていません。

`ptr`\
から[IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)インスタンスへのポインター。

`lFlags`\
から列挙体に含める修飾子を指定する、「[解説](#remarks)」で説明されているフラグまたは値のビットごとの組み合わせ。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `lFlags` パラメーターが正しくありません。 |
|`WBEM_E_UNEXPECTED` | 0x8004101d | `QualifierSet_BeginEnumeration` の2回目の呼び出しが、 [`QualifierSet_EndEnumeration`](qualifierset-endenumeration.md)の介在する呼び出しなしで行われました。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 新しい列挙を開始するために必要なメモリが不足しています。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemQualifierSet:: BeginEnumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-beginenumeration)メソッドの呼び出しをラップします。

オブジェクトのすべての修飾子を列挙するには、 [QualifierSet_Next](qualifierset-next.md)の最初の呼び出しの前にこのメソッドを呼び出す必要があります。 修飾子が列挙される順序は、特定の列挙体に対して不変であることが保証されます。

`lEnumFlags` 引数として渡すことができるフラグは、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|  | 0 | すべての修飾子の名前を返します。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 現在のプロパティまたはオブジェクトに固有の修飾子の名前のみを返します。 <br/> プロパティの場合: プロパティに固有の修飾子 (オーバーライドを含む) だけを返します。クラス定義から伝達された修飾子は返しません。 <br/> インスタンスの場合: インスタンス固有の修飾子名だけを返します。 <br/> クラスの場合: 派生するクラスに固有の修飾子だけを返します。
|`WBEM_FLAG_PROPAGATED_ONLY` | 0x20 | 別のオブジェクトから伝達された修飾子の名前のみを返します。 <br/> プロパティの場合: プロパティ自体からではなく、クラス定義からこのプロパティに反映された修飾子だけを返します。 <br/> インスタンスの場合: クラス定義から伝達された修飾子だけを返します。 <br/> クラスの場合: 親クラスから継承された修飾子名だけを返します。 |

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
