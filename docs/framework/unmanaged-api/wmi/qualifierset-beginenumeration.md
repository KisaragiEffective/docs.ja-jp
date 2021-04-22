---
title: QualifierSet_BeginEnumeration 関数 (アンマネージド API リファレンス)
description: QualifierSet_BeginEnumeration 関数を使用すると、オブジェクトの修飾子の列挙子がリセットされます。
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
ms.translationtype: HT
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
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) インスタンスへのポインター。

`lFlags`\
[in] 列挙に含める修飾子を指定する、「[解説](#remarks)」セクションで説明されているフラグまたは値のビットごとの組み合わせ。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `lFlags` パラメーターが有効ではありません。 |
|`WBEM_E_UNEXPECTED` | 0x8004101d | `QualifierSet_BeginEnumeration` への 2 回目の呼び出しは、[`QualifierSet_EndEnumeration`](qualifierset-endenumeration.md) への中間呼び出しなしで行われました。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 新しい列挙を開始するのに十分なメモリがありません。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemQualifierSet::BeginEnumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-beginenumeration) メソッドの呼び出しがラップされます。

オブジェクトのすべての修飾子を列挙するには、[QualifierSet_Next](qualifierset-next.md) を初めて呼び出す前に、このメソッドを呼び出す必要があります。 修飾子が列挙される順序は、特定の列挙型に対して不変であることが保証されます。

`lEnumFlags` 引数として渡すことができるフラグは、*WbemCli.h* ヘッダー ファイル内で定義されているか、コード内で定数として定義することができます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|  | 0 | すべての修飾子の名前を返します。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 現在のプロパティまたはオブジェクトに固有の修飾子の名前のみを返します。 <br/> プロパティの場合: プロパティに固有の修飾子だけを返します (オーバーライドを含む)。クラス定義から反映された修飾子は返しません。 <br/> インスタンスの場合: インスタンス固有の修飾子名だけを返します。 <br/> クラスの場合: 派生するクラスに固有の修飾子だけを返します。
|`WBEM_FLAG_PROPAGATED_ONLY` | 0x20 | 別のオブジェクトから反映された修飾子の名前だけを返します。 <br/> プロパティの場合: クラス定義からこのプロパティに反映された修飾子だけを返します。プロパティ自体からは返しません。 <br/> インスタンスの場合: クラス定義から反映された修飾子だけを返します。 <br/> クラスの場合: 親クラスから継承された修飾子名だけを返します。 |

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
