---
title: ISymUnmanagedReader::ReplaceSymbolStore メソッド
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.ReplaceSymbolStore
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::ReplaceSymbolStore
helpviewer_keywords:
- ReplaceSymbolStore method [.NET Framework debugging]
- ISymUnmanagedReader::ReplaceSymbolStore method [.NET Framework debugging]
ms.assetid: 43257761-8cb1-4eaf-8fb5-1f3980cb66cd
topic_type:
- apiref
ms.openlocfilehash: 60c3537a80c39f758f46e6f2f0a5f2bcd27350b5
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74445736"
---
# <a name="isymunmanagedreaderreplacesymbolstore-method"></a>ISymUnmanagedReader::ReplaceSymbolStore メソッド
既存のシンボル ストアをデルタ シンボル ストアで置き換えます。 このメソッドは、指定されたデルタが更新ではなく完全な置換として機能する点を除いて、"更新プログラム"[ストア](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-updatesymbolstore-method.md)メソッドに似ています。  
  
> [!NOTE]
> 両方ではなく、`filename` または `pIStream` のパラメーターのいずれか1つだけを指定する必要があります。 `filename` が指定されている場合、シンボルストアはそのファイル内のシンボルで更新されます。 `pIStream` が指定されている場合、ストアは <xref:System.Runtime.InteropServices.ComTypes.IStream>からのデータで更新されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ReplaceSymbolStore (  
    [in] const WCHAR *filename,  
    [in] IStream *pIStream);  
```  
  
## <a name="parameters"></a>パラメーター  
 `filename`  
 からシンボルストアを格納しているファイルの名前。  
  
 `pIStream`  
 から`filename` パラメーターの代わりに使用されるファイルストリーム。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK。それ以外の場合は、E_FAIL またはその他のエラーコードを指定します。  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [ISymUnmanagedReader インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-interface.md)
