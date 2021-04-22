---
title: HttpWebRequest._HttpResponse フィールド
description: .NET の HttpWebRequest._HttpResponse フィールドについて説明します。 このフィールドは、HTTP 要求からの HTTP 応答の詳細が格納されている HttpWebResponse 型です。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.HttpWebRequest._HttpResponse
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: eab9b789-beb4-4c28-b2d8-78debc7ba129
ms.openlocfilehash: 70058e1183abf5b6bfd172497f65a3ceb2344060
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989958"
---
# <a name="httpwebrequest_httpresponse-field"></a>HttpWebRequest.\_HttpResponse フィールド

`HttpWebRequest._HttpResponse` は、HTTP 要求からの HTTP 応答の詳細が格納されている <xref:System.Net.HttpWebResponse> です。 HTTP 応答を受信するまで、`null` になっている場合があります。

## <a name="syntax"></a>構文
  
```csharp  
internal HttpWebResponse _HttpResponse
```

> [!WARNING]
> `HttpWebRequest._HttpResponse` は内部フィールドであり、コードで直接使用されるものではありません。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
