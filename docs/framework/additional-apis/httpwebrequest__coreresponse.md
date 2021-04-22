---
title: HttpWebRequest._CoreResponse フィールド
description: .NET の HttpWebRequest._CoreResponse フィールドについて説明します。 このフィールドは、HTTP 応答解析の結果が格納されている CoreResponseData または Exception オブジェクトです。
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
ms.openlocfilehash: 5093ec7ed2c3b94931dcd622ae9ccdb42feffa18
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989751"
---
# <a name="httpwebrequest_coreresponse-field"></a>HttpWebRequest.\_CoreResponse フィールド

`HttpWebRequest._CoreResponse` は、HTTP 応答解析の結果が格納されているオブジェクト ([CoreResponseData](coreresponsedata.md) または <xref:System.Exception>) です。

## <a name="syntax"></a>構文
  
```csharp
private object _CoreResponse
```

> [!WARNING]
> この API は、コードで直接使用されるものではありません。 代わりに、<xref:System.Diagnostics.DiagnosticSource> を使用してネットワーク コードをフックする必要があります。 「[DiagnosticSource User'S Guide (DiagnosticSource ユーザーズ ガイド)](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)」を参照してください。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのクラスの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Net>

**アセンブリ:** System (System.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用可能。
