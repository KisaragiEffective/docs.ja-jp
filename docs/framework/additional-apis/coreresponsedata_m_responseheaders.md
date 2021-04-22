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
ms.openlocfilehash: 6e0203094376de6ec2870649dd3c025e88639bb8
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875929"
---
# <a name="coreresponsedatam_responseheaders-field"></a>CoreResponseData.m\_ResponseHeaders フィールド

`CoreResponseData.m_ResponseHeaders` は、サーバー応答に関連付けられたヘッダーの <xref:System.Net.WebHeaderCollection> です。

## <a name="syntax"></a>構文
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> この API は、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
