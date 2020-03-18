---
title: ConnectionGroup. m_ConnectionList フィールド
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
ms.openlocfilehash: d53eeb54d212adb011dae138e103ea5b30f7fb99
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215531"
---
# <a name="connectiongroupm_connectionlist-field"></a>ConnectionGroup. m\_Connectiongroup フィールド

`ConnectionGroup.m_ConnectionList` は、同じ URI を提供し、有効期限や認証などの他のいくつかのプロパティで同じ値を共有する接続オブジェクトの <xref:System.Collections.ArrayList> です。

## <a name="syntax"></a>構文
  
```csharp  
private ArrayList m_ConnectionList
```

> [!WARNING]
> `ConnectionGroup.m_ConnectionList` フィールドはプライベートであり、コードで直接使用するためのものではありません。
> 
> Microsoft では、どのような状況でも、実稼働アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Net>

**アセンブリ:** システム (.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
