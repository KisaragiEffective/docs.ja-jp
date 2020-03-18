---
title: スレッドの協調的な取り消し
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- threads, cancellation
ms.assetid: d2d6d5fd-e263-4fa0-847b-2fc3e0d82337
ms.openlocfilehash: 1d1433ecf39974bf9e68fe07b9d0818ac16fb544
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138122"
---
# <a name="canceling-threads-cooperatively"></a>スレッドの協調的な取り消し

.NET Framework 4 より前のバージョンの .NET Framework には、開始されたスレッドを協調的に取り消す手段は組み込まれていませんでした。 ただし、.NET Framework 4 以降、<xref:System.Threading.CancellationToken?displayProperty=nameWithType> オブジェクトや PLINQ クエリを取り消す場合と同様に、<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> を使用してスレッドを取り消すことができます。 <xref:System.Threading.Thread?displayProperty=nameWithType> クラスにはキャンセル トークンのサポートは組み込まれていませんが、<xref:System.Threading.Thread> デリゲートを受け取る <xref:System.Threading.ParameterizedThreadStart> コンストラクターを使用して、トークンをスレッド プロシージャに渡すことができます。 この方法を次の例に示します。  
  
 [!code-csharp[Cancellation#14](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/CooperativeThreads.cs#14)]
 [!code-vb[Cancellation#14](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/CooperativeThreads.vb#14)]  
  
## <a name="see-also"></a>参照

- [スレッドの使用とスレッド処理](using-threads-and-threading.md)
