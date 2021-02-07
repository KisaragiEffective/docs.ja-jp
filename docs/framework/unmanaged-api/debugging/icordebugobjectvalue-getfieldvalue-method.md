---
description: '詳細については、次を参照してください: GetFieldValue メソッド'
title: ICorDebugObjectValue::GetFieldValue メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugObjectValue.GetFieldValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugObjectValue::GetFieldValue
helpviewer_keywords:
- ICorDebugObjectValue::GetFieldValue method [.NET Framework debugging]
- GetFieldValue method [.NET Framework debugging]
ms.assetid: c96770b0-3e09-47bb-bd29-20353b043459
topic_type:
- apiref
ms.openlocfilehash: 38dac36747b286ab16ae3310b6b59695480a6ff1
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99722192"
---
# <a name="icordebugobjectvaluegetfieldvalue-method"></a>ICorDebugObjectValue::GetFieldValue メソッド

このオブジェクト値について、指定したクラスの指定したフィールドの値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetFieldValue (  
    [in]  ICorDebugClass     *pClass,  
    [in]  mdFieldDef         fieldDef,  
    [out] ICorDebugValue     **ppValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `pClass`  
 からフィールド値を取得する対象のクラスを表す "表示クラス" オブジェクトへのポインター。  
  
 `fieldDef`  
 から `mdFieldDef` フィールドを記述するメタデータを参照するトークン。  
  
 `ppValue`  
 入出力指定したフィールドの値を表す "ICorDebugValue" オブジェクトへのポインター。  
  
## <a name="remarks"></a>解説  

 パラメーターで指定されたクラスは、 `pClass` オブジェクト値のクラスの階層内に存在する必要があり、フィールドはそのクラスのフィールドである必要があります。  
  
 `GetFieldValue`ジェネリックオブジェクトとジェネリッククラスでは、メソッドは引き続き成功します。 たとえば、MyDictionary が \<V> ディクショナリから継承 \<string,V> し、オブジェクト値が mydictionary 型の場合、 \<int32> ディクショナリのオブジェクトを渡すと `ICorDebugClass` \<K,V> ディクショナリのフィールドが正常に取得され \<string,int32> ます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目
