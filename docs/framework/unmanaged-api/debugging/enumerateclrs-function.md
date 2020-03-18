---
title: EnumerateCLRs 関数
ms.date: 03/30/2017
api_name:
- EnumerateCLRs
api_location:
- dbgshim.dll
api_type:
- COM
f1_keywords:
- EnumerateCLRs
helpviewer_keywords:
- debugging API [Silverlight]
- Silverlight, debugging
- EnumerateCLRs function
ms.assetid: f8d50cb3-ec4f-4529-8fe3-bd61fd28e13c
topic_type:
- apiref
ms.openlocfilehash: cdf88ef193df71a638fff43add1a9648d8631731
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789122"
---
# <a name="enumerateclrs-function"></a>EnumerateCLRs 関数
プロセスで CLR を列挙するメカニズムを提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumerateCLRs (  
    [in]  DWORD      debuggeePID,  
    [out] HANDLE**   ppHandleArrayOut,  
    [out] LPWSTR**   ppStringArrayOut,  
    [out] DWORD*     pdwArrayLengthOut  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `debuggeePID`  
 [in] 読み込まれている CLR を列挙するプロセスのプロセス識別子。  
  
 `ppHandleArrayOut`  
 [out] CLR スタートアップの継続に使用されるイベント ハンドルを含む配列へのポインター。 配列内の各ハンドルが有効であることは保証されません。 ハンドルは、`ppStringArrayOut` の同じインデックスにある対応するランタイムの継続スタートアップ イベントとして使用されます。  
  
 `ppStringArrayOut`  
 [out] プロセスに読み込まれている CLR の完全パスを指定する文字列の配列へのポインター。  
  
 `pdwArrayLengthOut`  
 [out] 同じサイズの `ppHandleArrayOut` および `pdwArrayLengthOut` の長さを含む DWORD へのポインター。  
  
## <a name="return-value"></a>戻り値  
 S_OK  
 プロセス内の CLR 数が正常に判別され、対応するハンドルとパスの配列が正しく入力されました。  
  
 E_INVALIDARG  
 `ppHandleArrayOut` または `ppStringArrayOut` のいずれかが null であるか、または `pdwArrayLengthOut` が null です。  
  
 E_OUTOFMEMORY  
 ハンドルおよびパス配列に十分なメモリを割り当てることができません。  
  
 E_FAIL (またはその他の E_ リターン コード)  
 読み込まれている CLR を列挙できません。  
  
## <a name="remarks"></a>コメント  
 この関数は、`debuggeePID` で指定された対象プロセスについて、プロセスに読み込まれている CLR のパスの配列 (`ppStringArrayOut`)、同じインデックスにある CLR の継続スタートアップ イベントを含むイベント ハンドルの配列 (`ppHandleArrayOut`)、および読み込まれている CLR の数を示す配列のサイズ (`pdwArrayLengthOut`) を返します。  
  
 Windows オペレーティング システムでは、`debuggeePID` がプロセス識別子に対応づけられます。  
  
 `ppHandleArrayOut` および `ppStringArrayOut` のメモリはこの関数によって割り当てられます。 割り当てられたメモリを解放するには、 [CloseCLREnumeration 関数](closeclrenumeration-function.md)を呼び出す必要があります。  
  
 両方の配列パラメーターを null に設定してこの関数を呼び出すことで、対象プロセス内の CLR の数を取得できます。 この数から、呼び出し元は、作成されるバッファーのサイズを推論できます (`(sizeof(HANDLE) * count) + (sizeof(LPWSTR) * count) + (sizeof(WCHAR*) * count * MAX_PATH)`)。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** dbgshim. h  
  
 **ライブラリ:** dbgshim .dll  
  
 **.NET Framework のバージョン:** 3.5 SP1
