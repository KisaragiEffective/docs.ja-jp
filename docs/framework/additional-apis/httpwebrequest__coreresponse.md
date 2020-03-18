---
title: _CoreResponse HttpWebRequest フィールド
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.HttpWebRequest._CoreResponse
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: d16936f6984e73a886f5f48e05b53501ced63c1b
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75740450"
---
# <a name="httpwebrequest_coreresponse-field"></a>HttpWebRequest.\_CoreResponse フィールド

`HttpWebRequest._CoreResponse` は、HTTP 応答の解析結果を含むオブジェクト ( [CoreResponseData](coreresponsedata.md)または <xref:System.Exception>) です。

## <a name="syntax"></a>構文
  
```csharp
private object _CoreResponse
```

> [!WARNING]
> この API は、コード内で直接使用するためのものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワークコードをフックする必要があります。 「 [DiagnosticSource User'S Guide](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
> 
> Microsoft では、どのような状況でも、実稼働アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Net>

**アセンブリ:** システム (.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
