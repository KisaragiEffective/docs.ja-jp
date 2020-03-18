---
title: _CorExeMain 関数
ms.date: 03/30/2017
api_name:
- _CorExeMain
api_location:
- mscoree.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- _CorExeMain
helpviewer_keywords:
- _CorExeMain function [.NET Framework hosting]
ms.assetid: 898f76e2-16f4-4a63-b7d9-dad2d3824d8a
topic_type:
- apiref
ms.openlocfilehash: 8541e7761e2f8e1839d028fdaea3eb71307ba615
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131200"
---
# <a name="_corexemain-function"></a>_CorExeMain 関数
共通言語ランタイム (CLR) を初期化し、実行可能アセンブリの CLR ヘッダーでマネージエントリポイントを検索して、実行を開始します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
__int32 STDMETHODCALLTYPE _CorExeMain ();  
```  
  
## <a name="remarks"></a>Remarks  
 この関数は、マネージ実行可能アセンブリから作成されたプロセスでローダーによって呼び出されます。 DLL アセンブリの場合、ローダーは代わりに[Cordllmain](../../../../docs/framework/unmanaged-api/hosting/cordllmain-function.md)関数を呼び出します。  
  
 オペレーティングシステムローダーは、イメージファイルで指定されたエントリポイントに関係なく、このメソッドを呼び出します。  
  
 Windows 98、Windows ME、Windows NT、および Windows 2000 では、`_CorExeMain` 関数は、オペレーティングシステムローダーの修正によって間接的に呼び出されます。 それ以外のすべてのバージョンの Windows では、オペレーティングシステムローダーによって直接呼び出されます。  
  
 詳細につい[ては、このトピックの](../../../../docs/framework/unmanaged-api/hosting/corvalidateimage-function.md)「解説」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ グローバル静的関数](../../../../docs/framework/unmanaged-api/metadata/metadata-global-static-functions.md)
