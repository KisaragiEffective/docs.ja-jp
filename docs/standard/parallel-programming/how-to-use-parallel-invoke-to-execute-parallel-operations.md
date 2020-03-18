---
title: '方法: Parallel.Invoke を使用して並列操作を実行する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- task parallelism in .NET
- parallel programming, task parallelism
ms.assetid: 6b3ecd79-dec9-4ce1-abf4-62e5392a59c6
ms.openlocfilehash: 665490601cad9ccd7881042aed576b95bbc07115
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139727"
---
# <a name="how-to-use-parallelinvoke-to-execute-parallel-operations"></a>方法: Parallel.Invoke を使用して並列操作を実行する

この例では、タスク並列ライブラリの <xref:System.Threading.Tasks.Parallel.Invoke%2A> を使用して操作を並列化する方法を示します。 共有データ ソースで 3 つの操作が実行されます。 操作によってソースが変更されるわけではないため、簡単に並列実行できます。

> [!NOTE]
> ここでは、ラムダ式を使用して TPL でデリゲートを定義します。 C# または Visual Basic のラムダ式についての情報が必要な場合は、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[TPL_Parallel#06](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallelinvoke.cs#06)]
[!code-vb[TPL_Parallel#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/parallelinvoke.vb#06)]

なお、<xref:System.Threading.Tasks.Parallel.Invoke%2A> を使用すると、どの操作を同時に実行するかを示すだけで、ランタイムによりすべてのスレッドのスケジュール詳細が処理され、ホスト コンピューター上のコア数も自動的に調整されます。

この例では、データではなく操作を並列化します。 別の方法としては、PLINQ を使用して LINQ クエリを並列化し、クエリを順番に実行できます。 また、PLINQ を使用してデータを並列化することもできます。 もう 1 つのオプションとしては、クエリとタスクの両方を並列化する方法もあります。 プロセッサの数が比較的少ないホスト コンピューターでは、オーバーヘッドの発生によってパフォーマンスが低下しますが、プロセッサ数の多いコンピューターではパフォーマンスは適切に調整されます。

## <a name="compile-the-code"></a>コードのコンパイル

この例の全体をコピーして、Microsoft Visual Studio プロジェクトに貼り付けて **F5** キーを押します。

## <a name="see-also"></a>参照

- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [方法: タスクとその子を取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)
- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
