---
title: '方法: PLINQ の実行モードを指定する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to use execution mode
ms.assetid: e52ff26c-c5d3-4fab-9fec-c937fb387963
ms.openlocfilehash: c602aba6e18f80b007b15cd61dfd2b48a36dd2c8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139242"
---
# <a name="how-to-specify-the-execution-mode-in-plinq"></a>方法: PLINQ の実行モードを指定する
この例では、PLINQ に既定のヒューリスティックをバイパスさせ、クエリのシェイプに関係なくクエリを並列化する方法を示します。  
  
> [!WARNING]
> この例は、使用方法を示すことを意図したものであるため、同等の順次的な LINQ to Objects クエリほど高速ではない可能性があります。 高速化の詳細については、「[PLINQ での高速化について](../../../docs/standard/parallel-programming/understanding-speedup-in-plinq.md)」を参照してください。  
  
## <a name="example"></a>例  
 [!code-csharp[PLINQ#22](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#22)]
 [!code-vb[PLINQ#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#22)]  
  
 PLINQ は、並列化を利用しやすくするために設計されています。 ただし、すべてのクエリが並列実行の利点を活用できるわけではありません。 たとえば、負荷が非常に小さい単一のユーザー デリゲートを含むクエリの場合、順次実行の方が速度が速くなります。 これは、実行を並列するために必要なオーバーヘッドが、並列化で高速にする場合の負荷より大きいためです。 このため、PLINQ はすべてのクエリを自動的に並列化するわけではありません。 最初に、クエリのシェイプとクエリを構成しているさまざまな演算子を調べます。 この分析に基づいて、既定の実行モードの PLINQ によって、クエリの一部またはすべてを順次実行するかどうかが決定されます。 ただし、PLINQ が分析から判断するよりも、ユーザーの方がクエリをより詳しく理解している場合があります。 たとえば、デリゲートの負荷が非常に大きいため、クエリで並列化を使用する方がよいとわかっているとします。 このような場合は、<xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A> メソッドを使用し、<xref:System.Linq.ParallelExecutionMode.ForceParallelism> 値を指定することで、クエリを常に並列実行するよう PLINQ に指示できます。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 このコードをコピーして [PLINQ Data Sample](../../../docs/standard/parallel-programming/plinq-data-sample.md) に貼り付けて、`Main` からメソッドを呼び出します。  
  
## <a name="see-also"></a>参照

- <xref:System.Linq.ParallelEnumerable.AsSequential%2A>
- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
