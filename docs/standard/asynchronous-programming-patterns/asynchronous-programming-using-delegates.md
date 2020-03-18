---
title: デリゲートを使用した非同期プログラミング
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- BeginInvoke method
- asynchronous programming, delegates
- asynchronous delegates
- calling synchronous methods in asynchronous manner
- EndInvoke method
- calling asynchronous methods
- delegates [.NET Framework], asynchronous
- synchronous calling in asynchronous manner
ms.assetid: 38a345ca-6963-4436-9608-5c9defef9c64
ms.openlocfilehash: 4e17e6a96a12b705cf455d70add7e12a30f5fa90
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73121748"
---
# <a name="asynchronous-programming-using-delegates"></a>デリゲートを使用した非同期プログラミング
デリゲートを使用すると、同期メソッドを非同期的に呼び出すことができます。 デリゲートを同期的に呼び出すと、`Invoke` メソッドによって対象メソッドが現在のスレッドで直接呼び出されます。 `BeginInvoke` メソッドが呼び出されると、共通言語ランタイム (CLR) は要求をキューに置き、すぐに呼び出し元に戻ります。 対象メソッドは、スレッド プールのスレッドで非同期的に呼び出されます。 要求を送信した元のスレッドは、対象メソッドと並行して継続実行できます。 `BeginInvoke` メソッドの呼び出しにコールバック メソッドを指定した場合、対象メソッドの終了時に、そのコールバック メソッドが呼び出されます。 コールバック メソッドでは、`EndInvoke` メソッドを使用して、戻り値と、入出力パラメーターまたは出力専用パラメーターを取得します。 `BeginInvoke` の呼び出しにコールバック メソッドを指定しなかった場合は、`EndInvoke` を呼び出したスレッドから `BeginInvoke` を呼び出すことができます。  
  
> [!IMPORTANT]
> コンパイラは、ユーザーが指定したデリゲート シグネチャを使用して、`Invoke`、`BeginInvoke`、`EndInvoke` の各メソッドを持つデリゲート クラスを出力します。 `BeginInvoke` メソッドと `EndInvoke` メソッドは、ネイティブとして修飾されます。 これら 2 つのメソッドはネイティブとしてマークされるため、クラスの読み込み時には、CLR によって自動的にメソッドが実装されます。 ローダーは、これらのメソッドがオーバーライドされないことを保証します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [同期メソッドの非同期呼び出し](../../../docs/standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)  
 デリゲートを使用した通常のメソッドの非同期呼び出しについて説明すると共に、簡単なコード例を示して、非同期呼び出しが戻るのを待機する 4 つの方法を紹介します。  
  
## <a name="related-sections"></a>関連項目  
 [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)  
 .NET Framework での非同期プログラミングについて説明します。  
  
## <a name="see-also"></a>参照

- <xref:System.Delegate>
