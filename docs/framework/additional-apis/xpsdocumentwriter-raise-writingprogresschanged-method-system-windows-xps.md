---
title: XpsDocumentWriter.raise__WritingProgressChanged メソッド (System.Windows.Xps)
description: .NET で XPS ドキュメントの WritingProgressChanged イベントを発生させる XpsDocumentWriter.raise__WritingProgressChanged メソッドについて説明します。
ms.date: 12/12/2007
api_location:
- system.printing.dll
api_name:
- System.Windows.Xps.XpsDocumentWriter.raise__WritingProgressChanged
api_type:
- Assembly
topic_type:
- apiref
ms.openlocfilehash: 1e012c0900a83e1adbf0ceaddeb91792598b4377
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105213"
---
# <a name="xpsdocumentwriterraise__writingprogresschanged-method"></a>XpsDocumentWriter.raise\_\_WritingProgressChanged メソッド

<xref:System.Windows.Xps.XpsDocumentWriter.WritingProgressChanged> イベントを発生させます。

## <a name="syntax"></a>構文

```csharp
public void raise__WritingProgressChanged (object value0,
  System.Windows.Documents.Serialization.WritingProgressChangedEventArgs value1);
```

## <a name="parameters"></a>パラメーター

- `value0` <xref:System.Object>  
  イベントのソース。

- `value1`  <xref:System.Windows.Documents.Serialization.WritingProgressChangedEventArgs>  
  イベントのデータ。
  
## <a name="requirements"></a>必要条件

**名前空間:** <xref:System.Windows.Xps>

**アセンブリ:** System.Printing (system.printing.dll 内)

**.NET Framework のバージョン:** 3.0
