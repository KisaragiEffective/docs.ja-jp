---
title: LoadLibraryShim 関数
ms.date: 03/30/2017
api_name:
- LoadLibraryShim
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- LoadLibraryShim
helpviewer_keywords:
- LoadLibraryShim function [.NET Framework hosting]
ms.assetid: 30931874-4d0e-4df1-b3d1-e425b50655d1
topic_type:
- apiref
ms.openlocfilehash: 11bb220068e978dc130701e3b28ab3f421be7337
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937649"
---
# <a name="loadlibraryshim-function"></a>LoadLibraryShim 関数
再頒布可能パッケージ .NET Framework に含まれている、指定したバージョンの DLL を読み込みます。  
  
 この関数は .NET Framework 4 で非推奨とされました。 代わりに[ICLRRuntimeInfo:: LoadLibrary](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-loadlibrary-method.md)メソッドを使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT LoadLibraryShim (  
    [in]  LPCWSTR  szDllName,  
    [in]  LPCWSTR  szVersion,  
          LPVOID   pvReserved,  
    [out] HMODULE *phModDll  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `szDllName`  
 から.NET Framework ライブラリから読み込む DLL の名前を表す、0で終わる文字列です。  
  
 `szVersion`  
 から読み込む DLL のバージョンを表す、0で終わる文字列。 `szVersion` が null の場合、読み込まれるように選択されたバージョンは、指定された DLL のバージョン4より小さい最新バージョンです。 つまり、バージョン4以降のすべてのバージョンは、`szVersion` が null の場合は無視され、バージョン4より前のバージョンがインストールされていない場合は、DLL の読み込みに失敗します。 これは、.NET Framework 4 のインストールが、既存のアプリケーションまたはコンポーネントに影響しないようにするためです。 CLR チームブログの「[インプロセス SxS And Migration クイックスタート](https://devblogs.microsoft.com/dotnet/in-proc-sxs-and-migration-quick-start/)」のエントリを参照してください。  
  
 `pvReserved`  
 将来使用するために予約されています。  
  
 `phModDll`  
 入出力モジュールのハンドルへのポインター。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の値に加えて、Winerror.h で定義されている標準のコンポーネントオブジェクトモデル (COM) エラーコードを返します。  
  
|リターン コード|説明|  
|-----------------|-----------------|  
|S_OK|メソッドは正常に終了しました。|  
|CLR_E_SHIM_RUNTIMELOAD|`szDllName` を読み込むには、共通言語ランタイム (CLR) を読み込む必要があり、必要なバージョンの CLR を読み込むことができません。|  
  
## <a name="remarks"></a>Remarks  
 この関数は、.NET Framework 再頒布可能パッケージに含まれている Dll を読み込むために使用されます。 ユーザーが生成した Dll は読み込まれません。  
  
> [!NOTE]
> .NET Framework バージョン2.0 以降では、Fusion .dll を読み込むと CLR が読み込まれます。 これは、Fusion の関数が、ランタイムによって実装されているラッパーであるためです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [非推奨の CLR ホスト関数](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
