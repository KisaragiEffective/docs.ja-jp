---
title: CoreResponseData クラス
description: HTTP ヘッダーと応答本文の解析を表す CoreResponseData クラスについて説明します。 これは、.NET の System.Net 名前空間にあります。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 8248cc20f6528a1c3bc64c9b9339a3a3000d03a0
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989805"
---
# <a name="coreresponsedata-class"></a>CoreResponseData クラス

`CoreResponseData` クラスは、HTTP ヘッダーと応答本文の解析を表します。

## <a name="syntax"></a>構文
  
```csharp
internal class CoreResponseData
```

> [!WARNING]
> これは内部 API であり、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
