---
title: ISymUnmanagedWriter::GetDebugInfo メソッド
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.GetDebugInfo
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::GetDebugInfo
helpviewer_keywords:
- ISymUnmanagedWriter::GetDebugInfo method [.NET Framework debugging]
- GetDebugInfo method [.NET Framework debugging]
ms.assetid: dd31c210-6829-45eb-927e-cc53932638b7
topic_type:
- apiref
ms.openlocfilehash: 2b901a3dac499f1ce3f843c59122dd8fd5022147
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74427965"
---
# <a name="isymunmanagedwritergetdebuginfo-method"></a>ISymUnmanagedWriter::GetDebugInfo メソッド
コンパイラがポータブル実行可能 (PE) ファイルヘッダーにデバッグディレクトリエントリを書き込むために必要な情報を返します。 シンボルライターは、`TimeDateStamp` と `PointerToRawData`を除くすべてのフィールドに入力します。 (コンパイラは、これらの2つのフィールドを適切に設定する必要があります)。  
  
 コンパイラはこのメソッドを呼び出し、PE ファイルにデータ blob を出力します。次に、IMAGE_DEBUG_DIRECTORY 内の `PointerToRawData` フィールドを、出力されたデータを指すように設定し、IMAGE_DEBUG_DIRECTORY を PE ファイルに書き込みます。 また、コンパイラは、生成される PE ファイルの `TimeDateStamp` と等しいように `TimeDateStamp` フィールドを設定する必要があります。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetDebugInfo(  
    [in, out] IMAGE_DEBUG_DIRECTORY *pIDD,  
    [in]  DWORD cData,  
    [out] DWORD *pcData,  
    [out, size_is(cData),  
        length_is(*pcData)] BYTE data[]);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pIDD`  
 [入力、出力]シンボルライターが入力する IMAGE_DEBUG_DIRECTORY へのポインター。  
  
 `cData`  
 からデバッグデータのサイズを格納している `DWORD`。  
  
 `pcData`  
 入出力デバッグデータを格納するために必要なバッファーのサイズを受け取る `DWORD` へのポインター。  
  
 `data`  
 入出力シンボルストアのデバッグデータを保持するのに十分な大きさのバッファーへのポインター。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK。それ以外の場合は、E_FAIL またはその他のエラーコードを指定します。  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [ISymUnmanagedWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)
