---
title: _CorDllMain 関数
ms.date: 03/30/2017
api_name:
- _CorDllMain
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- _CorDllMain
helpviewer_keywords:
- _CorDllMain function [.NET Framework hosting]
ms.assetid: bc7b51cf-39d3-48ec-a5cb-2f179fbefff8
topic_type:
- apiref
ms.openlocfilehash: f60f159ab4770023cee7123b39109040243e1ccd
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136972"
---
# <a name="_cordllmain-function"></a>\_CorDllMain 関数

共通言語ランタイム (CLR) を初期化し、DLL アセンブリの CLR ヘッダー内のマネージエントリポイントを検索して、実行を開始します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
BOOL STDMETHODCALLTYPE _CorDllMain (  
   [in] HINSTANCE hInst,  
   [in] DWORD     dwReason,  
   [in] LPVOID    lpReserved  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `hInst`  
 から読み込まれたモジュールのインスタンスハンドル。  
  
 `dwReason`  
 からDLL のエントリポイント関数が呼び出される理由を示します。 このパラメーターには、次のいずれかの値を指定できます: DLL\_PROCESS_ATTACH、DLL\_スレッド\_アタッチ、DLL\_スレッド\_アタッチ、または DLL\_プロセス\_デタッチします。 これらの値の詳細については、Platform SDK の `DllMain` のドキュメントを参照してください。  
  
 `lpReserved`  
 から未使用.  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、成功した場合は `true` を返し、エラーが発生した場合は `false` を返します。  
  
## <a name="remarks"></a>Remarks  
 この関数は、DLL アセンブリのオペレーティングシステムローダーによって呼び出されます。 実行可能アセンブリの場合、ローダーは代わりに[\_CorExeMain](../../../../docs/framework/unmanaged-api/hosting/corexemain-function.md)関数を呼び出します。  
  
 オペレーティングシステムローダーは、DLL ファイルで指定されたエントリポイントに関係なく、このメソッドを呼び出します。  
  
`_CorDllMain` 関数は、オペレーティングシステムローダーによって直接呼び出されます。
  
 詳細については、 [\_CorValidateImage](../../../../docs/framework/unmanaged-api/hosting/corvalidateimage-function.md)トピックの「解説」セクションを参照してください。  
  
## <a name="requirements"></a>［要件］  

 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ グローバル静的関数](../../../../docs/framework/unmanaged-api/metadata/metadata-global-static-functions.md)
