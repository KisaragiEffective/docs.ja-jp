---
title: SetManifestFile メソッド
ms.date: 03/30/2017
api_name:
- IALink3.SetManifestFile
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- SetManifestFile
helpviewer_keywords:
- SetManifestFile method
ms.assetid: 1b33de4c-19cb-4a36-a93f-8675b2a36d58
topic_type:
- apiref
ms.openlocfilehash: df97f4c37d8f335ce183685debd7c0933be910ed
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74445564"
---
# <a name="setmanifestfile-method"></a>SetManifestFile メソッド
リンカーがアセンブリを作成するときに使用するマニフェストファイルを指定またはリセットできます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetManifestFile(  
    LPCWSTR pszFile  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `pszFile`  
  
 コンテンツが Win32 リソース blob に格納されるマニフェストファイルの名前。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="remarks"></a>コメント  
 Win32ResBlob を要求する前に、これを呼び出します。 `pszFile` パラメーターの値は、コンテンツが読み取られ、RT_MANIFEST の ID を持つ Win32 リソースに格納されるマニフェストファイルの名前です。 NULL のパラメーターを使用して呼び出されると、以前に読み取られたマニフェストはクリアされます。 これにより、1つのリンカーの状態を初期化時間にリセットできます。  
  
## <a name="requirements"></a>要件  
 ALink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink3 インターフェイス](ialink3-interface.md)
- [ALink API](index.md)
- [IALink インターフェイス](ialink-interface.md)
- [Al.exe (アセンブリ リンカー)](../../tools/al-exe-assembly-linker.md)
