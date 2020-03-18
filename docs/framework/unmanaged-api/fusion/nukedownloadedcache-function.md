---
title: NukeDownloadedCache 関数
ms.date: 03/30/2017
api_name:
- NukeDownloadedCache
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- DLLExport
f1_keywords:
- NukeDownloadedCache
helpviewer_keywords:
- NukeDownloadedCache function [.NET Framework fusion]
ms.assetid: fac2b1c6-6fa3-4818-805b-b63972024c86
topic_type:
- apiref
ms.openlocfilehash: 8f97614412eb2d49b202e86bdabc727159deb5d6
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131697"
---
# <a name="nukedownloadedcache-function"></a>NukeDownloadedCache 関数
共通言語ランタイム (CLR) のダウンロードキャッシュを削除します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT NukeDownloadedCache();  
```  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、Winerror.h で定義されているように、標準の COM エラーコードを返します。  
  
## <a name="remarks"></a>Remarks  
 CLR ダウンロードキャッシュは、URL からダウンロードされる厳密な名前付きアセンブリが再利用できるように格納される領域です。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **ライブラリ:** Fusion .dll と Mscorwks.dll。 Mscorwks.dll の代わりに Fusion を使用して、正しいバージョンの .NET Framework を対象としていることを確認してください。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [CreateHistoryReader 関数](createhistoryreader-function.md)
- [GetHistoryFileDirectory 関数](gethistoryfiledirectory-function.md)
- [Fusion グローバル静的関数](fusion-global-static-functions.md)
