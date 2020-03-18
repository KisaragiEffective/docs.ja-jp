---
title: ICorProfilerInfo::GetILFunctionBody メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetILFunctionBody
helpviewer_keywords:
- GetILFunctionBody method [.NET Framework profiling]
- ICorProfilerInfo::GetILFunctionBody method [.NET Framework profiling]
ms.assetid: e29b46bc-5fdc-4894-b0c2-619df4b65ded
topic_type:
- apiref
ms.openlocfilehash: 8160bb5b9ca5e0a4e22a1a831e978eaf125e7605
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870493"
---
# <a name="icorprofilerinfogetilfunctionbody-method"></a>ICorProfilerInfo::GetILFunctionBody メソッド
Microsoft 中間言語 (MSIL) コード内のメソッドの本文へのポインターを、ヘッダーを開始位置として取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetILFunctionBody(  
    [in]  ModuleID    moduleId,  
    [in]  mdMethodDef methodId,  
    [out] LPCBYTE     *ppMethodHeader,  
    [out] ULONG       *pcbMethodSize);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleId`  
 から関数が存在するモジュールの ID。  
  
 `methodId`  
 からメソッドのメタデータトークン。  
  
 `ppMethodHeader`  
 入出力メソッドのヘッダーへのポインター。  
  
 `pcbMethodSize`  
 入出力メソッドのサイズを指定する整数。  
  
## <a name="remarks"></a>コメント  
 メソッドは、それが存在するモジュールによってスコープが設定されます。 `GetILFunctionBody` メソッドは、MSIL コードが共通言語ランタイム (CLR) によって読み込まれる前に、ツールにアクセスできるように設計されているため、メソッドのメタデータトークンを使用して目的のインスタンスを検索します。  
  
 `methodId` が MSIL コード (抽象メソッドや platform invoke (PInvoke) メソッドなど) を含まないメソッドを指している場合、`GetILFunctionBody` は CORPROF_E_FUNCTION_NOT_IL HRESULT を返すことができます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
