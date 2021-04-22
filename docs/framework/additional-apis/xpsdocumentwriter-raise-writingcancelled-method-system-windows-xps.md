---
title: XpsDocumentWriter.raise__WritingCancelled メソッド (System.Windows.Xps)
description: .NET で XML Paper Specification (XPS) ドキュメントの WritingCancelled イベントを発生させる XpsDocumentWriter.raise__WritingCancelled メソッドについて説明します。
ms.date: 12/12/2007
api_location:
- system.printing.dll
api_name:
- System.Windows.Xps.XpsDocumentWriter.raise__WritingCancelled
api_type:
- Assembly
topic_type:
- apiref
ms.openlocfilehash: 5436be347792209780c4b3b617f26f731d98ac90
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105246"
---
# <a name="xpsdocumentwriterraise__writingcancelled-method"></a>XpsDocumentWriter.raise\_\_WritingCancelled メソッド

<xref:System.Windows.Xps.XpsDocumentWriter.WritingCancelled> イベントを発生させます。

## <a name="syntax"></a>構文

```csharp
public void raise__WritingCancelled (object value0,
  System.Windows.Documents.Serialization.WritingCancelledEventArgs value1);
```

## <a name="parameters"></a>パラメーター

- `value0` <xref:System.Object>  
  イベントのソース。

- `value1` <xref:System.Windows.Documents.Serialization.WritingCancelledEventArgs>  
  イベントのデータ。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Windows.Xps>

**アセンブリ:** System.Printing (system.printing.dll 内)

**.NET Framework のバージョン:** 3.0
