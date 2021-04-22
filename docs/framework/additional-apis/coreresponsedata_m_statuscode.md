---
title: CoreResponseData.m_StatusCode フィールド
description: .NET の CoreResponseData.m_StatusCode フィールドについて説明します。 このフィールドは、HTTP 応答の状態が格納されている HttpStatusCode 型です。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_StatusCode
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 29e46f71fd6e819dd311b214733fe56ff0548d74
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875903"
---
# <a name="coreresponsedatam_statuscode-field"></a>CoreResponseData.m\_StatusCode フィールド

`CoreResponseData.m_StatusCode` 応答の状態が格納されている <xref:System.Net.HttpStatusCode> です。

## <a name="syntax"></a>構文
  
```csharp
public HttpStatusCode m_StatusCode
```

> [!WARNING]
> この API は、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
