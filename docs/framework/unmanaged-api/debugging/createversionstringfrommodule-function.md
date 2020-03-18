---
title: CreateVersionStringFromModule 関数
ms.date: 03/30/2017
api_name:
- CreateVersionStringFromModule
api_location:
- dbgshim.dll
api_type:
- COM
f1_keywords:
- CreateVersionStringFromModule
helpviewer_keywords:
- debugging API [Silverlight]
- Silverlight, debugging
- CreateVersionStringFromModule function
ms.assetid: 3d2fe9bd-75ef-4364-84a6-da1e1994ac1a
topic_type:
- apiref
ms.openlocfilehash: 609d6e47c951aa104cb23084b65e98827a6851f1
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789177"
---
# <a name="createversionstringfrommodule-function"></a>CreateVersionStringFromModule 関数
対象プロセス内の共通言語ランタイム (CLR: Common Language Runtime) パスからバージョン文字列を作成します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateVersionStringFromModule (  
    [in]  DWORD      pidDebuggee,  
    [in]  LPCWSTR    szModuleName,  
    [out, size_is(cchBuffer),  
          length_is(*pdwLength)] LPWSTR Buffer,  
    [in]  DWORD      cchBuffer,  
    [out] DWORD*     pdwLength  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pidDebuggee`  
 [in] 対象 CLR が読み込まれているプロセスの識別子。  
  
 `szModuleName`  
 [in] プロセスに読み込まれている対象 CLR の完全パスまたは相対パス。  
  
 `pBuffer`  
 [out] 対象 CLR のバージョン文字列を格納する戻りバッファー。  
  
 `cchBuffer`  
 [in] `pBuffer` のサイズ。  
  
 `pdwLength`  
 [out] `pBuffer` によって返されるバージョン文字列の長さ。  
  
## <a name="return-value"></a>戻り値  
 S_OK  
 対象 CLR のバージョン文字列が `pBuffer` に正常に返されました。  
  
 E_INVALIDARG  
 `szModuleName` が null であるか、`pBuffer` または `cchBuffer` が null です。 `pBuffer` と `cchBuffer` は、両方が null であるか、両方が null 以外である必要があります。  
  
 HRESULT_FROM_WIN32(ERROR_INSUFFICIENT_BUFFER)  
 `pdwLength` が `cchBuffer` より大きくなっています。 `pBuffer` と `cchBuffer` の両方に null を渡して、`pdwLength` を使用して必要なバッファー サイズを照会した場合に、このような結果になることが予期されます。  
  
 HRESULT_FROM_WIN32(ERROR_MOD_NOT_FOUND)  
 `szModuleName` に、対象プロセスの有効な CLR のパスが含まれていません。  
  
 E_FAIL (またはその他の E_ リターン コード)  
 `pidDebuggee` が有効なプロセスを参照していません。または、その他のエラーが発生しました。  
  
## <a name="remarks"></a>コメント  
 この関数は、`pidDebuggee` が識別した CLR プロセスと、`szModuleName` で指定された文字列パスを受け取ります。 バージョン文字列は、`pBuffer` が指すバッファーに返されます。 この文字列は関数のユーザーには不透明です。つまり、バージョン文字列自体に特別な意味はありません。 この関数と[CreateDebuggingInterfaceFromVersion 関数](createdebugginginterfacefromversion-function-for-silverlight.md)のコンテキストでのみ使用されます。  
  
 この関数は、2 回呼び出す必要があります。 1 回目の呼び出しでは、`pBuffer` と `cchBuffer` の両方に null を渡します。 これにより、`pBuffer` に必要なバッファーのサイズが `pdwLength` に返されます。 その後、2 回目の関数呼び出しを実行し、`pBuffer` にはバッファーを、`cchBuffer` にはバッファーのサイズを渡すことができます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** dbgshim. h  
  
 **ライブラリ:** dbgshim .dll  
  
 **.NET Framework のバージョン:** 3.5 SP1
