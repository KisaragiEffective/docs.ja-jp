---
title: '方法: リフレクションを使用して型およびメンバーの情報を取得する'
ms.date: 09/03/2019
helpviewer_keywords:
- reflection, obtaining member information
- types [.NET Framework], obtaining member information from
ms.assetid: 348ae651-ccda-4f13-8eda-19e8337e9438
dev_langs:
- cpp
- csharp
- vb
ms.openlocfilehash: 9ffc173bbd0ed12eedea0c191f6d39baf181793a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130216"
---
# <a name="how-to-get-type-and-member-information-by-using-reflection"></a>方法: リフレクションを使用して型およびメンバーの情報を取得する
<xref:System.Reflection> 名前空間には、型とそのメンバーに関する情報を取得するための多くのメソッドがあります。 この記事では、これらのメソッドの 1 つである <xref:System.Type.GetMembers%2A?displayProperty=nameWithType> を例として示します。 詳細については、「[リフレクションの概要](reflection.md)」を参照してください。
  
## <a name="example"></a>例

リフレクションを使用して型とメンバーの情報を取得する例を次に示します。

[!code-cpp[Get type members](../../../samples/snippets/standard/reflection/memberinfo/gettypemembers.cpp)]
[!code-csharp[Get type members](../../../samples/snippets/standard/reflection/memberinfo/gettypemembers.cs)]
[!code-vb[Get type members](../../../samples/snippets/standard/reflection/memberinfo/gettypemembers.vb)]

## <a name="see-also"></a>関連項目

- [アプリケーション ドメインを使用したプログラミング](../app-domains/application-domains.md#programming-with-application-domains)
- [リフレクション](reflection.md)
- [アプリケーション ドメインを使用する](../app-domains/use.md)
