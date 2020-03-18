---
title: タスク並列ライブラリ (TPL)
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- .NET, concurrency in
- .NET, parallel programming in
- Parallel Programming
ms.assetid: b8f99f43-9104-45fd-9bff-385a20488a23
ms.openlocfilehash: 487fe48462803ac19318f450f2576989f196a9be
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139959"
---
# <a name="task-parallel-library-tpl"></a>タスク並列ライブラリ (TPL)
タスク並列ライブラリ (TPL: Task Parallel Library) は、<xref:System.Threading?displayProperty=nameWithType> 名前空間および <xref:System.Threading.Tasks?displayProperty=nameWithType> 名前空間におけるパブリック型と API のセットです。 TPL の目的は、アプリケーションに並列処理とコンカレンシーを追加するプロセスを簡略化して、開発者の生産性を高めることです。 TPL は、使用可能なすべてのプロセッサを最も効率的に使用するように、コンカレンシーの程度を動的に拡大します。 さらに TPL は、作業のパーティション分割、<xref:System.Threading.ThreadPool> 上のスレッドのスケジュール、キャンセルのサポート、状態管理、および他の低水準の詳細を処理します。 TPL を使用すると、コードのパフォーマンスが大幅に向上し、目的を達成するためのプログラミング作業に集中できます。  
  
 .NET Framework 4 以降では、マルチスレッド コードおよび並列コードを作成する際に TPL を使用することをお勧めします。 ただし、すべてのコードが並列化に適している訳ではありません。ループの各反復処理で少量の作業のみを実行する場合、または多数の反復処理のために実行しない場合、並列化のオーバーヘッドによってコードの実行が遅くなる可能性があります。 さらに、マルチスレッド コードのような並列化によって、プログラムの実行がより複雑になります。 TPL はマルチスレッドのシナリオを簡略化しますが、たとえばロック、デッドロックおよび競合状態のスレッド処理の基本的な概念を理解し、TPL を効率的に使用することをお勧めします。  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-|-|  
|[データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)|並列の `for` ループおよび `foreach` ループ (Visual Basic では `For` および `For Each`) を作成する方法について説明します。|  
|[タスク ベースの非同期プログラミング](../../../docs/standard/parallel-programming/task-based-asynchronous-programming.md)|<xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> を使用して暗黙的にタスクを作成および実行する方法、または <xref:System.Threading.Tasks.Task> オブジェクトを直接使用して明示的にタスクを作成および実行する方法について説明します。|  
|[データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)|相互に非同期通信を行う必要がある複数の操作を処理する場合、またはデータが使用可能になったときにデータを処理する場合に、TPL データ フロー ライブラリのデータ フロー コンポーネントを使用する方法について説明します。|  
|[TPL と他の非同期パターンの使用](../../../docs/standard/parallel-programming/using-tpl-with-other-asynchronous-patterns.md)|TPL と他の非同期パターンを .NET で使用する方法について説明します。|  
|[データとタスクの並列化における注意点](../../../docs/standard/parallel-programming/potential-pitfalls-in-data-and-task-parallelism.md)|一般的な落とし穴とその回避方法について説明します。|  
|[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)|LINQ クエリでデータの並列化を達成する方法について説明します。|  
|[並列プログラミング](../../../docs/standard/parallel-programming/index.md)|.NET 並列プログラミングのトップ レベル ノード。|  
  
## <a name="see-also"></a>参照

- [.NET Framework による並列プログラミングのサンプル](https://code.msdn.microsoft.com/Samples-for-Parallel-b4b76364)
