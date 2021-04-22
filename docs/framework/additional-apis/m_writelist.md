---
title: Connection.m_WriteList フィールド
description: .NET の Connection.m_WriteList フィールドについて説明します。 この ArrayList フィールドには、HTTP 経由で送信されるためにキューに登録されている HttpWebRequest オブジェクトがあります。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.Connection.m_WriteList
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 235503c1-1d01-4f59-895f-ae2cf15b3345
ms.openlocfilehash: a627cb062036e3ab098c2d6e97f9a77ebfa75a33
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989592"
---
# <a name="connectionm_writelist-field"></a>Connection.m\_WriteList フィールド

`Connection.m_WriteList` は、HTTP 経由で送信されるためにキューに登録されている <xref:System.Net.HttpWebRequest> オブジェクトの <xref:System.Collections.ArrayList> です。

## <a name="syntax"></a>構文
  
```csharp  
private ArrayList m_WriteList
```

> [!WARNING]
> `Connection.m_WriteList` フィールドはプライベートであり、コードで直接使用されるものではありません。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
