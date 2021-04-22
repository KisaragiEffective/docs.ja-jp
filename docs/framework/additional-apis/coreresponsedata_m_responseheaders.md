---
title: CoreResponseData.m_ResponseHeaders フィールド
description: .NET の CoreResponseData.m_ResponseHeaders フィールドについて説明します。 このフィールドは、サーバー応答に関連付けられたヘッダーが格納されている WebHeaderCollection 型です。
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
ms.openlocfilehash: 7c7b896193cb81e9fc9e3ec28110359003a36728
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989784"
---
# <a name="coreresponsedatam_responseheaders-field"></a>CoreResponseData.m\_ResponseHeaders フィールド

`CoreResponseData.m_ResponseHeaders` は、サーバー応答に関連付けられたヘッダーの <xref:System.Net.WebHeaderCollection> です。

## <a name="syntax"></a>構文
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> この API は、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
