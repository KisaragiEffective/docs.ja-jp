---
title: PutMethod 関数 (アンマネージ API リファレンス)
description: PutMethod 関数は、メソッドを作成します。
ms.date: 11/06/2017
api_name:
- PutMethod
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- PutMethod
helpviewer_keywords:
- PutMethod function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 1d409507de593cf198fe87340eece6820eaefc63
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127344"
---
# <a name="putmethod-function"></a>PutMethod 関数
メソッドが作成されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
    
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT PutMethod (
   [in] int                vFunc, 
   [in] IWbemClassObject*  ptr, 
   [in] LPCWSTR            wszName,
   [in] LONG               lFlags,
   [in] IWbemClassObject*  pInSignature,
   [in] IWbemClassObject*  pOutSignature
); 
```  

## <a name="parameters"></a>パラメーター

`vFunc`  
からこのパラメーターは使用されていません。

`ptr`  
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

`wszName`  
から作成するメソッドの名前。 

`lFlags`  
[in] 予約されています。 このパラメーターには0を指定する必要があります。

`pSignatureIn`  
からメソッドの `in` パラメーターを格納している、 [__ parameters システムクラス](/windows/desktop/WmiSdk/--parameters)のコピーへのポインター。 `null`に設定した場合、このパラメーターは無視されます。  

`pSignatureOut`  
から メソッドの `out` パラメーターを格納している、 [__ parameters システムクラス](/windows/desktop/WmiSdk/--parameters)のコピーへのポインター。 `null`に設定した場合、このパラメーターは無視されます。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 1つ以上のパラメーターが無効です。 |
| `WBEM_E_INVALID_DUPLICATE_PARAMETER` | 0x80041043 | *Pinsignature*オブジェクトと*poutsignature*オブジェクトの両方で指定された `[in, out]` method パラメーターの修飾子が異なります。
| `WBEM_E_MISSING_PARAMETER_ID` | 0x80041036 | メソッドパラメーターに**ID**修飾子の指定がありません。 |
| `WBEM_E_NONCONSECUTIVE_PARAMETER_IDS` | 0x80041038 | メソッドのパラメーターに割り当てられている ID 系列が連続していないか、0から始まっていません。 |
| `WBEM_E_PARAMETER_ID_ON_RETVAL` | 0x80041039 | メソッドの戻り値には**ID**修飾子があります。 |
| `WBEM_E_PROPAGATED_METHOD` | 0x80041034 | 親クラスから既存のメソッド名を再利用しようとしましたが、シグネチャが一致しませんでした。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。 |
  
## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject::P utmethod](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-putmethod)メソッドの呼び出しをラップします。

このメソッド呼び出しは、`ptr` が CIM クラス定義である場合にのみサポートされます。 メソッド操作は、CIM インスタンスをポイントする[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)ポインターからは使用できません。

ユーザーは、アンダースコアで始まる名前または末尾を持つメソッドを作成することはできません。 これは、システムクラスおよびプロパティ用に予約されています。

メソッドの場合、`in` パラメーターと `out` パラメーターは[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)オブジェクトのプロパティとして記述されます。

`[in/out]` パラメーターを定義するには、`pInSignature` パラメーターと `pOutSignature` パラメーターが指す両方のオブジェクトに同じプロパティを追加します。 この場合、プロパティは同じ**ID**修飾子値を共有します。

`ReturnValue` 以外の[__ Parameters](/windows/desktop/WmiSdk/--parameters)クラスオブジェクトの各プロパティには、パラメーターが表示される順序を識別する**ID**修飾子 (0 から始まる) が必要です。 2つのパラメーターの**id**値を同じにすることはできません。また、 **id**値をスキップすることもできません。 どちらかの条件が発生した場合、`PutMethod` 関数は `WBEM_E_NONCONSECUTIVE_PARAMETER_IDS`を返します。

## <a name="example"></a>例

例については、 [IWbemClassObject::P utmethod](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-putmethod)メソッドを参照してください。

## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** WMINet_Utils  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
