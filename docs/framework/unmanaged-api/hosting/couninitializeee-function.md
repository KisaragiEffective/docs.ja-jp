---
title: CoUninitializeEE 関数
ms.date: 03/30/2017
api_name:
- CoUninitializeEE
api_location:
- mscoree.dll
- mscorsvr.dll
api_type:
- DLLExport
f1_keywords:
- CoUninitializeEE
helpviewer_keywords:
- CoUninitializeEE function [.NET Framework hosting]
ms.assetid: 5f5a311a-839a-465f-89d9-ff1c74da9736
topic_type:
- apiref
ms.openlocfilehash: 3531cfc0815c3f8a9479e35b2df60b2825801b39
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136851"
---
# <a name="couninitializeee-function"></a>CoUninitializeEE 関数
`CoUninitializeEE` は互換性のために残されていますが、機能はありません。  
  
## <a name="syntax"></a>構文  
  
```cpp  
void CoUninitializeEE (  
    BOOL fFlags  
);  
```  
  
## <a name="remarks"></a>Remarks  
 共通言語ランタイムの実行エンジンをプロセスからアンロードできません。 実行エンジンをシャットダウンするには、 [CorExitProcess](../../../../docs/framework/unmanaged-api/hosting/corexitprocess-function.md)を呼び出します。  
  
## <a name="see-also"></a>関連項目

- [CoInitializeEE 関数](../../../../docs/framework/unmanaged-api/hosting/coinitializeee-function.md)
- [メタデータ グローバル静的関数](../../../../docs/framework/unmanaged-api/metadata/metadata-global-static-functions.md)
