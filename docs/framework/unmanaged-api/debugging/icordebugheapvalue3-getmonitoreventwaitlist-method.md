---
title: ICorDebugHeapValue3::GetMonitorEventWaitList メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue3.GetMonitorEventWaitList
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue3::GetMonitorEventWaitList
helpviewer_keywords:
- ICorDebugHeapValue3::GetMonitorEventWaitList method [.NET Framework debugging]
- GetMonitorEventWaitList method [.NET Framework debugging]
ms.assetid: 035a9035-ac66-4953-b48a-99652b42b7fe
topic_type:
- apiref
ms.openlocfilehash: 15900fab59d172ada67d8aefeab698e1b44f808e
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794389"
---
# <a name="icordebugheapvalue3getmonitoreventwaitlist-method"></a>ICorDebugHeapValue3::GetMonitorEventWaitList メソッド
モニターロックに関連付けられているイベントでキューに登録されているスレッドの順序付きリストを提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetMonitorEventWaitList (  
    [out] ICorDebugThreadEnum **ppThreadEnum  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppThreadEnum`  
 入出力順序付けされたスレッドのリストを提供する、の型の定数列挙子。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|The list is not empty.|  
|S_FALSE|リストが空です。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>コメント  
 リスト内の最初のスレッドは、次に <xref:System.Threading.Monitor.Pulse%28System.Object%29?displayProperty=nameWithType>を呼び出すことによって解放される最初のスレッドです。 リスト内の次のスレッドは、次の呼び出しで解放されます。  
  
 リストが空でない場合、このメソッドは S_OK を返します。 リストが空の場合、メソッドは S_FALSE を返します。この場合、列挙体は空ですが、有効です。  
  
 どちらの場合も、列挙インターフェイスは、現在の同期された状態の間のみ使用できます。 ただし、スレッドのインターフェイスディスペンサーは、スレッドが終了するまで有効です。  
  
 `ppThreadEnum` が有効なポインターでない場合、結果は未定義になります。  
  
 どのスレッドがモニターを待機しているかを特定できないようなエラーが発生した場合、メソッドはエラーを示す HRESULT を返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
