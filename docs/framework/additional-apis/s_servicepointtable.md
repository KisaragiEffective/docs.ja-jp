---
title: ServicePointManager.s_ServicePointTable フィールド
description: .NET の ServicePointManager.s_ServicePointTable フィールドについて説明します。 このハッシュ テーブル フィールドには、AppDomain 内のアクティブな HTTP 接続 (ServicePoints) が格納されています。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ServicePointManager.s_ServicePointTable
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 24459679-291c-401a-9def-e42b29466fcf
ms.openlocfilehash: 9462ae10125dd37706f786a1f2cef78e62fbabcc
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989547"
---
# <a name="servicepointmanagers_servicepointtable-field"></a>ServicePointManager.s\_ServicePointTable フィールド

`ServicePointManager.s_ServicePointTable` は、<xref:System.AppDomain> 内のアクティブな HTTP 接続 (<xref:System.Net.ServicePoint>) の一覧が格納されている <xref:System.Collections.Hashtable> です。

## <a name="syntax"></a>構文
  
```csharp  
private static Hashtable s_ServicePointTable
```

> [!WARNING]
> `ServicePointManager.s_ServicePointTable` フィールドはプライベートであり、コードで直接使用されるものではありません。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
