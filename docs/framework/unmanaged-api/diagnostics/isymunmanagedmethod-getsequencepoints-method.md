---
title: ISymUnmanagedMethod::GetSequencePoints メソッド
ms.date: 03/30/2017
api_name:
- ISymUnmanagedMethod.GetSequencePoints
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedMethod::GetSequencePoints
helpviewer_keywords:
- ISymUnmanagedMethod::GetSequencePoints method [.NET Framework debugging]
- GetSequencePoints method [.NET Framework debugging]
ms.assetid: f909ac48-3d8f-49fb-a369-e3d9959151cd
topic_type:
- apiref
ms.openlocfilehash: 75d477af7395a9b7d3328b2a5787f810733f3749
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448878"
---
# <a name="isymunmanagedmethodgetsequencepoints-method"></a>ISymUnmanagedMethod::GetSequencePoints メソッド
このメソッド内のすべてのシーケンスポイントを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetSequencePoints(  
    [in]  ULONG32  cPoints,  
    [out] ULONG32  *pcPoints,  
    [in, size_is(cPoints)] ULONG32  offsets[],  
    [in, size_is(cPoints)] ISymUnmanagedDocument* documents[],  
    [in, size_is(cPoints)] ULONG32  lines[],  
    [in, size_is(cPoints)] ULONG32  columns[],  
    [in, size_is(cPoints)] ULONG32  endLines[],  
    [in, size_is(cPoints)] ULONG32  endColumns[]);  
```  
  
## <a name="parameters"></a>パラメーター  
 `cPoints`  
 から`offsets`、`documents`、`lines`、`columns`、`endLines`、および `endColumns` の各配列のサイズを受け取る `ULONG32`。  
  
 `pcPoints`  
 入出力シーケンスポイントを格納するために必要なバッファーの長さを受け取る `ULONG32` へのポインター。  
  
 `offsets`  
 からシーケンスポイントのメソッドの先頭からの MSIL (Microsoft 中間言語) オフセットを格納する配列。  
  
 `documents`  
 からシーケンスポイントが配置されているドキュメントを格納する配列。  
  
 `lines`  
 からシーケンスポイントが配置されているドキュメント内の行を格納する配列。  
  
 `columns`  
 からシーケンスポイントが配置されているドキュメント内の列を格納する配列。  
  
 `endLines`  
 からシーケンスポイントが終了するドキュメント内の行の配列。  
  
 `endColumns`  
 からシーケンスポイントが終了するドキュメント内の列の配列。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK。それ以外の場合は、E_FAIL またはその他のエラーコードを指定します。  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [ISymUnmanagedMethod インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-interface.md)
