---
title: BeginMethodEnumeration 関数 (アンマネージ API リファレンス)
description: BeginMethodEnumeration 関数は、オブジェクトのメソッドの列挙を開始します。
ms.date: 11/06/2017
api_name:
- BeginMethodEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- BeginMethodEnumeration
helpviewer_keywords:
- BeginMethodEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: be1e86e0b760ab403cf42ac19da03f84769a85cf
ms.sourcegitcommit: 68a4b28242da50e1d25aab597c632767713a6f81
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884424"
---
# <a name="beginmethodenumeration-function"></a>BeginMethodEnumeration 関数
オブジェクトで使用できるメソッドの列挙を開始します。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
    
## <a name="syntax"></a>構文  
  
```cpp 
HRESULT BeginMethodEnumeration (
   [in] int               vFunc, 
   [in] IWbemClassObject* ptr, 
   [in] LONG              lEnumFlags
); 
```  

## <a name="parameters"></a>パラメーター

`vFunc`  
からこのパラメーターは使用されていません。

`ptr`  
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

`lEnumFlags`  
からすべてのメソッドにゼロ (0)、または列挙体のスコープを指定するフラグ。 次のフラグは、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

定数  |Value  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 列挙体は、クラス自体で定義されているメソッドに限定します。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 列挙型を基底クラスから継承されたプロパティに限定します。 |

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |Value  |説明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `lEnnumFlags` が0以外で、が指定されたフラグの1つではありません。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |
  
## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: BeginMethodEnumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-beginmethodenumeration)メソッドの呼び出しをラップします。

このメソッド呼び出しは、現在のオブジェクトがクラス定義の場合にのみサポートされます。 インスタンスを指す[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)ポインターからは、メソッド操作を使用できません。 メソッドを列挙する順序は、 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)の特定のインスタンスに対して不変であることが保証されます。

## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** WMINet_Utils .idl  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>参照

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
