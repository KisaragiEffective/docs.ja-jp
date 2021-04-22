---
title: ConnectionGroup.m_ConnectionList フィールド
description: .NET の ConnectionGroup.m_ConnectionList フィールドについて説明します。これには、同じ URI を提供し、他のプロパティの値を共有する接続オブジェクトが格納されています。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ConnectionGroup.m_ConnectionList
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 186083cf-8dff-4600-a2ab-6fed4b4de6af
ms.openlocfilehash: 478b2441c062e8df6f4e718bd66d7af329f20f12
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989725"
---
# <a name="connectiongroupm_connectionlist-field"></a>ConnectionGroup.m\_ConnectionList フィールド

`ConnectionGroup.m_ConnectionList` は、同じ URI を提供し、有効期限や認証などの他のいくつかのプロパティについて同じ値を共有する、接続オブジェクトの <xref:System.Collections.ArrayList> です。

## <a name="syntax"></a>構文
  
```csharp  
private ArrayList m_ConnectionList
```

> [!WARNING]
> `ConnectionGroup.m_ConnectionList` フィールドはプライベートであり、コードで直接使用されるものではありません。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
