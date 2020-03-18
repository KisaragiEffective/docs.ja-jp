---
title: ICorDebugCode::GetCode メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugCode.GetCode
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode::GetCode
helpviewer_keywords:
- ICorDebugCode::GetCode method [.NET Framework debugging]
- GetCode method, ICorDebugCode interface [.NET Framework debugging]
ms.assetid: 7137e3d1-1dad-48d8-8c37-16ac816534d3
topic_type:
- apiref
ms.openlocfilehash: 14a72e4622aac09840e43f8bcdcf8a8c8d6e6892
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76777911"
---
# <a name="icordebugcodegetcode-method"></a>ICorDebugCode::GetCode メソッド
指定した関数のすべてのコードを取得し、逆アセンブリ用に書式設定します。 このメソッドは .NET Framework バージョン2.0 では非推奨とされました。 代わりに[ICorDebugCode2:: GetCodeChunks](icordebugcode2-getcodechunks-method.md)を使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetCode (  
    [in] ULONG32     startOffset,   
    [in] ULONG32     endOffset,  
    [in] ULONG32     cBufferAlloc,  
    [out, size_is(cBufferAlloc),  
        length_is(*pcBufferSize)] BYTE buffer[],  
    [out] ULONG32    *pcBufferSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `startOffset`  
 から関数の開始位置のオフセット。  
  
 `endOffset`  
 から関数の終了位置のオフセット。  
  
 `cBufferAlloc`  
 からコードが返される `buffer` 配列のサイズ。  
  
 `buffer`  
 入出力コードが返される配列。  
  
 `pcBufferSize`  
 入出力返されたバイト数。  
  
## <a name="remarks"></a>コメント  
 関数のコードが複数のチャンクに分割されている場合は、ネイティブオフセットが増加する順序で連結されます。 命令の境界はチェックされません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 1.1、1.0  
  
## <a name="see-also"></a>関連項目

- [GetCodeChunks メソッド](icordebugcode2-getcodechunks-method.md)
