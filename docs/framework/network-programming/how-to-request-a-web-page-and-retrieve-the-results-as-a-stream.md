---
title: '方法: Web ページを要求し、ストリームとして結果を取得する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d32b7f35-29d8-4fb7-ad71-d219edc5e359
ms.openlocfilehash: 65bda268cd77959dbcd786c365d0a30c324b89ce
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71393108"
---
# <a name="how-to-request-a-web-page-and-retrieve-the-results-as-a-stream"></a>方法: Web ページを要求し、ストリームとして結果を取得する

この例では、Web ページを要求し、ストリームとして結果を取得する方法を示します。
  
## <a name="example"></a>例

```csharp
var myClient = new WebClient();
Stream response = myClient.OpenRead("https://docs.microsoft.com/dotnet/");
// The stream data is used here.
response.Close();
```

```vb
Dim myClient As New WebClient()
Dim response As Stream = myClient.OpenRead("https://docs.microsoft.com/dotnet/")
' The stream data is used here.
response.Close()
```

## <a name="compiling-the-code"></a>コードのコンパイル

 この例で必要な要素は次のとおりです。

- <xref:System.IO> 名前空間と <xref:System.Net> 名前空間への参照。

## <a name="see-also"></a>参照

- [データの要求](requesting-data.md)
