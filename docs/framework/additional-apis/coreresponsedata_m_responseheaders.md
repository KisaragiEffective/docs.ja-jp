---
title: M_ResponseHeaders CoreResponseData フィールド
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_ResponseHeaders
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: df0b592a5f85d4c99dee4ecb60963f4abb560a13
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741009"
---
# <a name="coreresponsedatam_responseheaders-field"></a>CoreResponseData\_ResponseHeaders フィールド

`CoreResponseData.m_ResponseHeaders` は、サーバーの応答に関連付けられているヘッダーの <xref:System.Net.WebHeaderCollection> です。

## <a name="syntax"></a>構文
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> この API は、コード内で直接使用するためのものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワークコードをフックする必要があります。 「 [DiagnosticSource User'S Guide](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
> 
> Microsoft では、どのような状況でも、実稼働アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Net>

**アセンブリ:** システム (.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
