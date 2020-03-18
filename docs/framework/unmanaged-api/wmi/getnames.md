---
title: GetNames 関数 (アンマネージ API リファレンス)
description: GetNames 関数は、オブジェクトのプロパティの名前を取得します。
ms.date: 11/06/2017
api_name:
- GetNames
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetNames
helpviewer_keywords:
- GetNames function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 5b03ed6a68fbe288e93dedb4f425f1511563dfeb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73102524"
---
# <a name="getnames-function"></a>GetNames 関数
オブジェクトのプロパティの名前の一部またはすべてが取得されます。 

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
    
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetNames (
   [in] int                 vFunc, 
   [in] IWbemClassObject*   ptr, 
   [in] LPCWSTR             wszQualifierName,
   [in] LONG                lFlags,
   [in] VARIANT*            pQualifierValue,
   [out] SAFEARRAY (BSTR)** pstrNames
); 
```  

## <a name="parameters"></a>パラメーター

`vFunc`  
からこのパラメーターは使用されていません。

`ptr`  
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

`wszQualifierName`  
からフィルターの一部として動作する修飾子名を指定する有効な `LPCWSTR` へのポインター。 詳細については、「[解説](#remarks)」を参照してください。 このパラメーターは、`null` に設定できます。 

`lFlags`  
からビットフィールドの組み合わせ。 詳細については、「[解説](#remarks)」を参照してください。

`pQualifierValue`   
からフィルター値に初期化された有効な `VARIANT` 構造体へのポインター。 このパラメーターは、`null` に設定できます。 

`pstrNames`  
入出力プロパティ名を格納している `SAFEARRAY` 構造体。 エントリでは、このパラメーターは常に `null`へのポインターである必要があります。 詳細については、「[解説](#remarks)」を参照してください。 

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 1つ以上のパラメーターが無効であるか、またはフラグとパラメーターの組み合わせが正しくありません。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |
  
## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: GetNames](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getnames)メソッドの呼び出しをラップします。

返される名前付きのは、フラグとパラメーターの組み合わせによって制御されます。 たとえば、関数は、すべてのプロパティの名前、またはキープロパティの名前のみを返すことができます。  プライマリフィルターは `lFlags` パラメーターで指定され、その他のパラメーターはそれに応じて異なります。

`lFlags` のフラグ値はビットフィールドです。

`lEnumFlags` 引数として渡すことができるフラグは、 *WbemCli*ヘッダーファイルで定義されているビットフィールドです。また、コード内で定数として定義することもできます。  各グループのフラグは、他のグループのフラグと組み合わせることができます。 ただし、同じグループのフラグは相互に排他的です。 

| グループ1フラグ |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_ALWAYS` | 0 | すべてのプロパティ名を返します。 `strQualifierName` と `pQualifierVal` は使用されていません。 |
| `WBEM_FLAG_ONLY_IF_TRUE` | 1 | `strQualifierName` パラメーターによって指定された名前の修飾子を持つプロパティのみを返します。 このフラグを使用する場合は、`strQualifierName`を指定する必要があります。 |
|`WBEM_FLAG_ONLY_IF_FALSE` | 2 |  `strQualifierName` パラメーターによって指定された名前の修飾子を持たないプロパティだけを返します。 このフラグを使用する場合は、`strQualifierName`を指定する必要があります。 |
|`WBEM_FLAG_ONLY_IF_IDENTICAL` | 3 | `wszQualifierName` パラメーターによって指定された名前の修飾子を持ち、`pQualifierVal` 構造体によって指定された値と同じ値を持つプロパティのみを返します。 このフラグを使用する場合は、`wszQualifierName` と `pQualifierValue`の両方を指定する必要があります。 |

| グループ2のフラグ |[値]  |説明  |
|---------|---------|---------|
|`WBEM_FLAG_KEYS_ONLY` | 0x4 | キーを定義するプロパティの名前のみを返します。 |
|`WBEM_FLAG_REFS_ONLY` | 0x8 | オブジェクト参照であるプロパティ名だけを返します。 |

| グループ3のフラグ |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 最派生クラスに属しているプロパティ名だけを返します。 親クラスからプロパティを除外します。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 親クラスに属するプロパティ名だけを返します。 |
|`WBEM_FLAG_SYSTEM_ONLY` | 0x30 | システムプロパティの名前のみを返します。 |
|`WBEM_FLAG_NONSYSTEM_ONLY` | 0x40 | システム以外のプロパティの名前のみを返します。 |

関数は `WBEM_S_NO_ERROR`を返す場合は常に新しい `SAFEARRAY` を割り当て、`pstrNames` は常にそれを指すように設定します。 指定したフィルターに一致するプロパティがない場合、返される配列には0個の要素を含めることができます。 関数が `WBM_S_NO_ERROR`以外の値を返す場合、新しい `SAFEARRAY` 構造体は返されません。
 
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** WMINet_Utils  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
