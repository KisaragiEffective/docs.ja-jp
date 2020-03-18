---
title: GetALinkMessageDll 関数
ms.date: 03/30/2017
api_name:
- GetALinkMessageDll
api_location:
- alink.dll
api_type:
- DLLExport
f1_keywords:
- GetALinkMessageDll
helpviewer_keywords:
- Alink API, GetALinkMessageDll function
- GetALinkMessageDll function
ms.assetid: 67985a22-88a2-4c54-8d99-4bcde9d6213e
topic_type:
- apiref
ms.openlocfilehash: 63719d0c6e13768e9dc7ed80e52e2a293e32a8a1
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74449348"
---
# <a name="getalinkmessagedll-function"></a>GetALinkMessageDll 関数
メッセージ DLL を検索して読み込みます。 メッセージ DLL が見つからないか、または読み込むことができなかった場合は0を返します。 メッセージ DLL は、名前が言語 ID または現在のディレクトリ内のサブディレクトリにある必要があります。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HINSTANCE WINAPI GetALinkMessageDll();  
```  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** alink  
  
 **ライブラリ**: alink  
  
## <a name="see-also"></a>参照

- [Al.exe (アセンブリ リンカー)](../../tools/al-exe-assembly-linker.md)
