---
title: '方法: データフロー ブロックで並列処理の範囲を指定する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dataflow block, specifying parallelism in TPL
- Task Parallel Library, dataflows
- TPL dataflow library, specifying parallelism
ms.assetid: e4088541-ee05-40db-95f5-147cfe62fde7
ms.openlocfilehash: 50399d6cd32fe310089395ac8c660b08151ba808
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141656"
---
# <a name="how-to-specify-the-degree-of-parallelism-in-a-dataflow-block"></a>方法: データフロー ブロックで並列処理の範囲を指定する
このドキュメントでは、<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A?displayProperty=nameWithType> プロパティを設定して、実行データフロー ブロックが一度に複数のメッセージを処理できるようにする方法について説明します。 実行時間の長い計算を実行し、並行してメッセージの処理からメリットを得るデータ フロー ブロックがある場合は、これを行うと役立ちます。 この例では、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601?displayProperty=nameWithType> クラスを使用して、同時に複数のデータフローの操作を実行しますが、TPL データフロー ライブラリが提供する定義済みの実行ブロック <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602?displayProperty=nameWithType>、および <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602?displayProperty=nameWithType> のいずれかで並列処理の最大範囲を指定することができます。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>例  
 次の例では、2 つのデータ フローの計算を実行し、それぞれの計算に必要な経過時間を出力します。 最初の計算では、既定である、並列処理の最大範囲 1 を指定します。 並列処理の最大範囲 1 を指定すると、データフロー ブロックが順番にメッセージを処理します。 2 番目の計算は、使用可能なプロセッサの数に相当する並列処理の最大範囲を指定する点以外は、最初の計算と似ています。 これにより、データフフロー ブロックは複数の操作を並行して実行できます。  
  
 [!code-csharp[TPLDataflow_DegreeOfParallelism#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_degreeofparallelism/cs/dataflowdegreeofparallelism.cs#1)]
 [!code-vb[TPLDataflow_DegreeOfParallelism#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_degreeofparallelism/vb/dataflowdegreeofparallelism.vb#1)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 既定では、定義済みの各データフロー ブロックは、メッセージが受信される順序でメッセージを処理します。  並列処理の最大範囲に 1 を超える数を指定すると、複数のメッセージが同時に処理されますが、メッセージはそれでも受信した順序で処理されます。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> プロパティは並列処理の最大範囲を表すため、データ フロー ブロックは、指定より低い並列化の度合いで実行される場合があります。 機能要件を満たすため、または使用可能なシステム リソースの不足が原因で、データフロー ブロックは、より小さい並列処理の範囲を使う場合があります。 データフロー ブロックが、指定より大きな並列処理の範囲を選択することはありません。  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
