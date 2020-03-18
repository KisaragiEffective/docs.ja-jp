---
title: _HttpResponse HttpWebRequest フィールド
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
ms.openlocfilehash: 236298921ecd286ddba4e74dbce1b63e96055412
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215101"
---
# <a name="httpwebrequest_httpresponse-field"></a>HttpWebRequest.\_Httpresponse.cache フィールド

`HttpWebRequest._HttpResponse` は、http 要求からの HTTP 応答の詳細を含む <xref:System.Net.HttpWebResponse> です。 HTTP 応答を受信するまで `null` できます。

## <a name="syntax"></a>構文
  
```csharp  
internal HttpWebResponse _HttpResponse
```

> [!WARNING]
> `HttpWebRequest._HttpResponse` フィールドは内部であり、コード内で直接使用するためのものではありません。
> 
> Microsoft では、どのような状況でも、実稼働アプリケーションでのこのフィールドの使用はサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Net>

**アセンブリ:** システム (.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
