---
title: _EFN_StackTrace 関数
ms.date: 03/30/2017
api_name:
- _EFN_StackTrace
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- _EFN_StackTrace
helpviewer_keywords:
- _EFN_StackTrace function [.NET Framework debugging]
ms.assetid: caea7754-867c-4360-a65c-5ced4408fd9d
topic_type:
- apiref
ms.openlocfilehash: cc5093a5ba0afcccaf960e9b8776f93a061cc2f5
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76785680"
---
# <a name="_efn_stacktrace-function"></a>\_EFN\_StackTrace 関数
マネージド スタック トレースのテキスト表現および `CONTEXT` レコードの配列 (アンマネージド コードとマネージド コードの間の各移行につき 1 つ) を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CALLBACK _EFN_StackTrace(  
    [in]  PDEBUG_CLIENT  Client,  
    [out] WCHAR          wszTextOut[],  
    [out] size_t         *puiTextLength,  
    [out] LPVOID         pTransitionContexts,  
    [out] size_t         *puiTransitionContextCount,  
    [in]  size_t         uiSizeOfContext,  
    [in]  DWORD          Flags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `Client`  
 からデバッグ中のクライアント。  
  
 `wszTextOut`  
 入出力スタックトレースのテキスト表現。  
  
 `puiTextLength`  
 入出力`wszTextOut`内の文字数へのポインター。  
  
 `pTransitionContexts`  
 入出力遷移コンテキストの配列。  
  
 `puiTransitionContextCount`  
 入出力配列内の遷移コンテキストの数へのポインター。  
  
 `uiSizeOfContext`  
 からコンテキスト構造のサイズ。  
  
 `Flags`  
 から各 `module!functionname` 行の前に EBP レジスタと enter stack ポインター (ESP) を表示するには、0または SOS_STACKTRACE_SHOWADDRESSES (0x01) のいずれかに設定します。  
  
## <a name="remarks"></a>コメント  
 `_EFN_StackTrace` 構造体は、WinDbg プログラムインターフェイスから呼び出すことができます。 パラメーターは次のように使用されます。  
  
- `wszTextOut` が null で `puiTextLength` が null でない場合、関数は `puiTextLength`に文字列の長さを返します。  
  
- `wszTextOut` が null でない場合、関数は `puiTextLength`によって示される場所まで `wszTextOut` にテキストを格納します。 バッファーに十分な空き領域がある場合は、正常に返されます。バッファーの長さが十分でない場合は E_OUTOFMEMORY を返します。  
  
- `pTransitionContexts` と `puiTransitionContextCount` が両方とも null の場合、関数の移行部分は無視されます。 この場合、関数は呼び出し元に関数名のみのテキスト出力を提供します。  
  
- `pTransitionContexts` が null で `puiTransitionContextCount` が null でない場合、関数は `puiTransitionContextCount`に必要なコンテキストエントリの数を返します。  
  
- `pTransitionContexts` が null でない場合、関数はそれを `puiTransitionContextCount`長さの構造体の配列として扱います。 構造体のサイズは `uiSizeOfContext`によって指定され、 [Simplecontext](stacktrace-simplecontext-structure.md)のサイズまたはアーキテクチャの `CONTEXT` である必要があります。  
  
- `wszTextOut` は次の形式で記述されます。  
  
    ```output  
    "<ModuleName>!<Function Name>[+<offset in hex>]  
    ...  
    (TRANSITION)  
    ..."  
    ```  
  
- 16進数のオフセットが0x0 の場合、オフセットは書き込まれません。  
  
- 現在コンテキスト内にあるスレッドにマネージコードがない場合、関数は SOS_E_NOMANAGEDCODE を返します。  
  
- `Flags` パラメーターは0または SOS_STACKTRACE_SHOWADDRESSES のいずれかになり、各 `module!functionname` 行の前に EBP と ESP が表示されます。 既定値は0です。  
  
    ```cpp  
    #define SOS_STACKTRACE_SHOWADDRESSES   0x00000001  
    ```  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** SOS_Stacktrace  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ グローバル静的関数](debugging-global-static-functions.md)
