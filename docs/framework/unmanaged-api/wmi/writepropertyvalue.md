---
title: WritePropertyValue 関数 (アンマネージ API リファレンス)
description: WritePropertyValue 関数は、バイトをプロパティに書き込みます。
ms.date: 11/06/2017
api_name:
- WritePropertyValue
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- WritePropertyValue
helpviewer_keywords:
- WritePropertyValue function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: f02fb3877d55e9f47384b281573202712c29c606
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107287"
---
# <a name="writepropertyvalue-function"></a>WritePropertyValue 関数
指定したバイト数が、プロパティ ハンドルによって識別されるプロパティに書き込まれます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
    
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT WritePropertyValue (
   [in] int                  vFunc, 
   [in] IWbemObjectAccess*   ptr, 
   [in] long                 lHandle,
   [in] long                 lNumBytes,
   [in] byte*                aData
); 
```  

## <a name="parameters"></a>パラメーター

`vFunc`  
からこのパラメーターは使用されていません。

`ptr`  
から[IWbemObjectAccess](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemobjectaccess)インスタンスへのポインター。

`lHandle`  
からこのプロパティを識別するハンドルを格納している整数。 ハンドルは、 [Getpropertyhandle](getpropertyhandle.md)関数を呼び出すことによって取得できます。   

`lNumBytes`  
からプロパティに書き込むバイト数。 詳細については、「[解説](#remarks)」を参照してください。

`pHandle`   
入出力データを格納しているバイト配列へのポインター。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが有効ではありません。 |
|`WBEM_E_TYPE_MISMATCH` | 0x80041005 | 型の不一致が発生しました。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |
  
## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: WritePropertyValue](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemobjectaccess-writepropertyvalue)メソッドの呼び出しをラップします。

この関数を使用すると、文字列およびその他のすべての非`DWORD` データまたは非`QWORD` データを設定できます。

文字列以外のプロパティ値の場合、`lNumBytes` は、指定されたプロパティ型の適切なデータサイズである必要があります。 文字列プロパティ値の場合、`lNumBytes` は指定した文字列の長さをバイト単位で指定する必要があり、文字列自体はバイト単位の長さで、その後に null 終端文字が続く必要があります。

## <a name="requirements"></a>［要件］  
**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** WMINet_Utils  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
