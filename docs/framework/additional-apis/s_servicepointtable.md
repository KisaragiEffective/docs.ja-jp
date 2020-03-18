---
title: ServicePointManager. s_ServicePointTable フィールド
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
ms.openlocfilehash: 272a0c113fd70d804c763ba0e7e6e9a4a4ee04ce
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214920"
---
# <a name="servicepointmanagers_servicepointtable-field"></a>ServicePointManager. s\_Servicepointmanager フィールド

`ServicePointManager.s_ServicePointTable` は、<xref:System.AppDomain>内のアクティブな HTTP 接続 (<xref:System.Net.ServicePoint>s) の一覧を含む <xref:System.Collections.Hashtable> です。

## <a name="syntax"></a>構文
  
```csharp  
private static Hashtable s_ServicePointTable
```

> [!WARNING]
> `ServicePointManager.s_ServicePointTable` フィールドはプライベートであり、コードで直接使用するためのものではありません。
> 
> Microsoft では、どのような状況でも、実稼働アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Net>

**アセンブリ:** システム (.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
