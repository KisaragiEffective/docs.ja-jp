---
description: '詳細情報: CoreClrDebugProcInfo 構造体'
title: CoreClrDebugProcInfo 構造体
ms.date: 03/30/2017
api_name:
- CoreClrDebugProcInfo
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- CoreClrDebugProcInfo
helpviewer_keywords:
- remote debugging API [Silverlight]
- CoreClrDebugProcInfo structure
- Silverlight, remote debugging
ms.assetid: 4ddc37da-5c94-4beb-b61c-b54071c0e749
topic_type:
- apiref
ms.openlocfilehash: 04befb057be689e68dd3571a13990da9af64551f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99801489"
---
# <a name="coreclrdebugprocinfo-structure"></a>CoreClrDebugProcInfo 構造体

リモート コンピューターで実行されているプロセスを表します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
struct  CoreClrDebugProcInfo {  
    DWORD m_dwPID;  
    DWORD m_dwInternalID;  
    WCHAR m_wszName[256];  
};  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`m_dwPID`|OS によって割り当てられたプロセス識別子。|  
|`m_dwInternalID`|対象のコンピューターで実行されているリモート デバッグ プロキシによって割り当てられたプロセス識別子。 この識別子は OS 識別子よりも少ない頻度で再利用されます。|  
|`m_wszName`|プロセスのコマンド ライン。 このメンバーは切り詰められる場合があります。|  
  
## <a name="requirements"></a>必要条件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CoreClrRemoteDebuggingInterfaces.h  
  
 **ライブラリ:** mscordbi_macx86.dll  
  
 **.NET Framework のバージョン:** 3.5 SP1
