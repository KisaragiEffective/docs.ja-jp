---
title: '方法: WebRequest でインターネットとの通信にプロキシを使用できるようにする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 63c0ef2c-44b5-4c54-9804-ba0b9b001ac7
ms.openlocfilehash: 8b38973e4cb2c83ce32b8a08e54d828a8eeef879
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73039539"
---
# <a name="how-to-enable-a-webrequest-to-use-a-proxy-to-communicate-with-the-internet"></a>方法: WebRequest でインターネットとの通信にプロキシを使用できるようにする

この例では、<xref:System.Net.WebRequest> でインターネットとの通信にプロキシを使用できるようにするグローバル プロキシ インスタンスを作成します。 ここでは、プロキシ サーバーが `webproxy` という名前で、ポート 80 (標準 HTTP ポート) で通信を行うことを想定します。

## <a name="example"></a>例

```csharp
var proxyObject = new WebProxy("http://webproxy:80/");
GlobalProxySelection.Select = proxyObject;
```

```vb
Dim proxyObject As New WebProxy("http://webproxy:80/")
GlobalProxySelection.Select = proxyObject
```

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- [System.Net`using` 名前空間の C# ](../../csharp/language-reference/keywords/using-directive.md)**ディレクティブ**。
- [System.Net`Imports` 名前空間の Visual Basic ](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)**ステートメント**。

## <a name="see-also"></a>参照

- [アプリケーション プロトコルの使用](using-application-protocols.md)
- [プロキシを介したインターネットへのアクセス](accessing-the-internet-through-a-proxy.md)
