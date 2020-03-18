---
title: CreateALink 関数
ms.date: 03/30/2017
api_name:
- CreateALink
api_location:
- alink.dll
api_type:
- DLLExport
f1_keywords:
- CreateALink
helpviewer_keywords:
- CreateALink function
- Alink API, CreateALink function
ms.assetid: fc73bcb9-6af6-44d8-bc39-2f4400325dae
topic_type:
- apiref
ms.openlocfilehash: 9165a4db7e65fb0f409a902b06d32e9c2988aa69
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446548"
---
# <a name="createalink-function"></a>CreateALink 関数
アセンブリリンカーのインスタンスを作成し、指定したインターフェイスへのポインターを設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateALink (  
   REFIID riid,  
   IUnknown **ppInterface  
);  
```  
  
## <a name="parameters"></a>パラメーター  
  
|パラメーター|説明|  
|---------------|-----------------|  
|`riid`|アセンブリリンカーインターフェイスの1つの物理名。|  
|`ppInterface`|正常に完了した場所には、`riid` インターフェイスへのポインターが含まれています。|  
  
## <a name="requirements"></a>要件  
 **ライブラリ**: alink  
  
## <a name="see-also"></a>参照

- [Al.exe (アセンブリ リンカー)](../../tools/al-exe-assembly-linker.md)
