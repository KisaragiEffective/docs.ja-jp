---
title: '方法: 単純な PLINQ クエリを作成して実行する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to create
ms.assetid: 983b4213-bddd-4a44-9262-cbe59186df4c
ms.openlocfilehash: 349cc8d78e9a080d720e09a7e3e5e314752605c7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73106956"
---
# <a name="how-to-create-and-execute-a-simple-plinq-query"></a>方法: 単純な PLINQ クエリを作成して実行する
次の例に、ソース シーケンスに対する <xref:System.Linq.ParallelEnumerable.AsParallel%2A> 拡張メソッドを使用して単純な並列 LINQ クエリを作成し、<xref:System.Linq.ParallelEnumerable.ForAll%2A> メソッドを使用して、そのクエリを実行する方法を示します。  
  
> [!NOTE]
> ここでは、ラムダ式を使用して PLINQ でデリゲートを定義します。 C# または Visual Basic のラムダ式についての情報が必要な場合は、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
## <a name="example"></a>例  
 [!code-csharp[PLINQ#11](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/create1.cs#11)]
 [!code-vb[PLINQ#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/create1.vb#11)]  
  
 この例で示しているのは、結果シーケンスの順序付けが重要ではない場合に、並列 LINQ クエリを作成して実行する基本的なパターンです。通常、順序なしのクエリは、順序ありのクエリより高速になります。 このクエリはソースを、複数のスレッドで非同期的に実行した複数のタスクにパーティション分割します。 各タスクが完了する順序は、その特定のパーティションに含まれる要素を処理するために必要な作業量だけでなく、オペレーティング システムが各スレッドをどのようにスケジュールするかといった外部要因にも依存します。 この例は、使用方法を示すことを意図したものであるため、同等の順次的な LINQ to Objects クエリほど高速ではない可能性があります。 高速化の詳細については、「[PLINQ での高速化について](../../../docs/standard/parallel-programming/understanding-speedup-in-plinq.md)」を参照してください。 クエリの要素の順序を保持する方法の詳細については、「[方法: PLINQ クエリの順序を制御する](../../../docs/standard/parallel-programming/how-to-control-ordering-in-a-plinq-query.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
