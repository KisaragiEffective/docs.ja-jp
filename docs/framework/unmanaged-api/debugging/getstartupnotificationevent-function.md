---
title: GetStartupNotificationEvent 関数
ms.date: 03/30/2017
api_name:
- GetStartupNotificationEvent
api_location:
- dbgshim.dll
api_type:
- COM
f1_keywords:
- GetStartupNotificationEvent
helpviewer_keywords:
- GetStartupNotificationEvent function
- debugging API [Silverlight]
- Silverlight, debugging
ms.assetid: c94b1b61-045a-4695-bacd-0f18c5acc246
topic_type:
- apiref
ms.openlocfilehash: fb158b35165fb229fc78169e2508679b6749752e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122950"
---
# <a name="getstartupnotificationevent-function"></a>GetStartupNotificationEvent 関数
指定された対象プロセスに読み込まれている任意の共通言語ランタイム (CLR: Common Language Runtime) によって通知されるイベント ハンドルを作成または開きます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetStartupNotificationEvent  
    (  
    [in]  DWORD     debuggeePID,  
    [out]  HANDLE*  phStartupEvent  
    );  
```  
  
## <a name="parameters"></a>パラメーター  
 `debuggeePID`  
 [in] 受信する CLR スタートアップ通知の送信元である対象プロセスのプロセス識別子。  
  
 `phStartupEvent`  
 [out] スタートアップ時に CLR によって通知されるハンドルへのポインター。  
  
## <a name="return-value"></a>戻り値  
 S_OK  
 スタートアップ通知イベントに対するハンドルを正常に取得しました。  
  
 E_INVALIDARG  
 `phStartupEvent` が nullであるか、または `debuggeePID` が現在実行されているプロセスを参照していません。  
  
 E_FAIL (またはその他の E_ リターン コード)  
 スタートアップ通知イベントに対するハンドルを取得できません。  
  
## <a name="remarks"></a>Remarks  
 Windows オペレーティング システムでは、`debuggeePID` がプロセス識別子に対応づけられます。  
  
 イベントは、通知元の CLR によってマネージド コードが実行される前に通知されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** dbgshim. h  
  
 **ライブラリ:** dbgshim .dll  
  
 **.NET Framework のバージョン:** 3.5 SP1
