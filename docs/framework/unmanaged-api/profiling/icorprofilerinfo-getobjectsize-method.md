---
title: ICorProfilerInfo::GetObjectSize メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetObjectSize
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetObjectSize
helpviewer_keywords:
- GetObjectSize method [.NET Framework profiling]
- ICorProfilerInfo::GetObjectSize method [.NET Framework profiling]
ms.assetid: 9f02e763-73f7-42cb-a41c-f78499d9482c
topic_type:
- apiref
ms.openlocfilehash: b860cf6eb07c3f063e3e51514f8492cf4af9e8ed
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869673"
---
# <a name="icorprofilerinfogetobjectsize-method"></a>ICorProfilerInfo::GetObjectSize メソッド
指定したオブジェクトのサイズを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetObjectSize(  
    [in]  ObjectID objectId,  
    [out] ULONG  *pcSize);  
```  
  
## <a name="parameters"></a>パラメーター  
 `objectId`  
 からオブジェクトの ID です。  
  
 `pcSize`  
 入出力オブジェクトのサイズへのポインター (バイト単位)。  
  
## <a name="remarks"></a>コメント  
  
> [!IMPORTANT]
> このメソッドは、互換性のために残されています。 64ビットプラットフォームで 4 GB を超えるオブジェクトの COR_E_OVERFLOW を返します。 代わりに[ICorProfilerInfo4:: GetObjectSize2](icorprofilerinfo4-getobjectsize2-method.md)メソッドを使用してください。  
  
 多くの場合、同じ種類の異なるオブジェクトのサイズは同じです。 ただし、配列や文字列など、一部の型では、オブジェクトごとにサイズが異なる場合があります。  
  
 `GetObjectSize` メソッドによって返されるサイズには、オブジェクトがガベージコレクションヒープに配置された後に表示されるアラインメントの埋め込みは含まれません。 `GetObjectSize` メソッドを使用してオブジェクトからガベージコレクションヒープ上のオブジェクトに進む場合は、必要に応じて、手動でアラインメントのパディングを追加します。  
  
- 32ビットの Windows では、COR_PRF_GC_GEN_0、COR_PRF_GC_GEN_1、および COR_PRF_GC_GEN_2 は4バイトの配置を使用し、COR_PRF_GC_LARGE_OBJECT_HEAP は8バイトの配置を使用します。  
  
- 64ビットの Windows では、アラインメントは常に8バイトです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
