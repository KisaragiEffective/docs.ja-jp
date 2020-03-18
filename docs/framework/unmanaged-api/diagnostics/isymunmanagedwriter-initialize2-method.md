---
title: ISymUnmanagedWriter::Initialize2 メソッド
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.Initialize2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::Initialize2
helpviewer_keywords:
- ISymUnmanagedWriter::Initialize2 method [.NET Framework debugging]
- Initialize2 method [.NET Framework debugging]
ms.assetid: 93de56b6-4ae8-4cca-acdc-25a434623509
topic_type:
- apiref
ms.openlocfilehash: 041df959139a0be77f40d6aa5655ff15f93fb26f
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74427949"
---
# <a name="isymunmanagedwriterinitialize2-method"></a>ISymUnmanagedWriter::Initialize2 メソッド
このライターが関連付けられるメタデータエミッタインターフェイスを設定し、デバッグシンボルの書き込み先となる出力ファイル名を設定します。 この方法では、プログラムデータベース (PDB) ファイルの最終的な場所を設定することもできます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Initialize2(  
    [in] IUnknown     *emitter,  
    [in] const WCHAR  *tempfilename,  
    [in] IStream      *pIStream,  
    [in] BOOL         fFullBuild,  
    [in] const WCHAR  *finalfilename);  
```  
  
## <a name="parameters"></a>パラメーター  
 `emitter`  
 からメタデータエミッタインターフェイスへのポインター。  
  
 `tempfilename`  
 からデバッグシンボルが書き込まれるファイル名を格納している `WCHAR` へのポインター。 ファイル名を使用しないライターに対してファイル名を指定した場合、このパラメーターは無視されます。  
  
 `pIStream`  
 から指定した場合、シンボルライターは、`filename` パラメーターで指定されたファイルではなく、指定された <xref:System.Runtime.InteropServices.ComTypes.IStream> にシンボルを出力します。 `pIStream` パラメーターは省略可能です。  
  
 `fFullBuild`  
 [in] フルリビルドの場合は `true`インクリメンタルコンパイルの場合は `false` します。  
  
 `finalfilename`  
 からPDB ファイルの最終的な場所へのパス文字列である `WCHAR` へのポインター。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK。それ以外の場合は、E_FAIL またはその他のエラーコードを指定します。  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>関連項目

- [ISymUnmanagedWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)
- [Initialize メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-initialize-method.md)
