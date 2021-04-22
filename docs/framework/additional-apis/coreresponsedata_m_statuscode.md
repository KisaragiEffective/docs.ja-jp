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
ms.openlocfilehash: 05950290bde96511432941ce679e663126878663
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989777"
---
# <a name="coreresponsedatam_statuscode-field"></a>CoreResponseData.m\_StatusCode フィールド

`CoreResponseData.m_StatusCode` 応答の状態が格納されている <xref:System.Net.HttpStatusCode> です。

## <a name="syntax"></a>構文
  
```csharp
public HttpStatusCode m_StatusCode
```

> [!WARNING]
> この API は、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
