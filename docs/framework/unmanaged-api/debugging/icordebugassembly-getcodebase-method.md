---
description: '詳細情報: ICorDebugAssembly::GetCodeBase メソッド'
title: ICorDebugAssembly::GetCodeBase メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugAssembly.GetCodeBase
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAssembly::GetCodeBase
helpviewer_keywords:
- GetCodeBase method [.NET Framework debugging]
- ICorDebugAssembly::GetCodeBase method [.NET Framework debugging]
ms.assetid: 48adc154-9058-4fef-9c43-e9aad80e4dbf
topic_type:
- apiref
ms.openlocfilehash: 9774ffe69089305909aab68f2164bfd9e59b3e94
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99711961"
---
# <a name="icordebugassemblygetcodebase-method"></a>ICorDebugAssembly::GetCodeBase メソッド

このメソッドは .NET Framework の現在のバージョンでは実装されていません。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetCodeBase (  
    [in] ULONG32  cchName,  
    [out] ULONG32 *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]
        WCHAR szName[]  
);  
```
