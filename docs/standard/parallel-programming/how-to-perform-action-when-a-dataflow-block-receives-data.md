---
title: '方法: データフロー ブロックでデータを受信したときにアクションを実行する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, receiving data
ms.assetid: fc2585dc-965e-4632-ace7-73dd02684ed3
ms.openlocfilehash: 89ab2bb18e5fe00a4d1b79d911bb0f7524b83104
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73124208"
---
# <a name="how-to-perform-action-when-a-dataflow-block-receives-data"></a>方法: データフロー ブロックでデータを受信したときにアクションを実行する
"*実行データフロー ブロック*" の型は、データを受信したときに、ユーザーが指定したデリゲートを呼び出します。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602?displayProperty=nameWithType>、および <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602?displayProperty=nameWithType> クラスは、実行データフロー ブロックの種類です。 実行データフロー ブロックに処理関数を提供するときに、`delegate` キーワード (Visual Basic では `Sub`)、<xref:System.Action%601>、<xref:System.Func%602>、またはラムダ式を使用することができます。 このドキュメントでは、<xref:System.Func%602> とラムダ式を使用して、実行ブロックでアクションを実行する方法について説明します。  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>例  
 次の例では、データフローを使用してディスクからファイルを読み取り、ファイル内のバイト数がゼロに等しい件数を計算します。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> を使用してファイルを読み取り、ゼロ バイトの数を計算し、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601> を使用してゼロ バイト数をコンソールに出力します。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトは、ブロックがデータを受け取ったときに処理を実行する <xref:System.Func%602> オブジェクトを指定します。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトは、ラムダ式を使用して、読み取ったゼロ バイトの件数をコンソールに出力します。  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#1)]
 [!code-vb[TPLDataflow_ExecutionBlocks#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#1)]  
  
 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトにラムダ式を使用することはできますが、この例では <xref:System.Func%602> を使用して他のコードで `CountBytes` メソッドを使用できるようにしています。 実行される処理がこのタスク固有であり、他のコードでは役立ちそうにないため、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601> オブジェクトではラムダ式を使用しています。 タスク並列ライブラリでのラムダ式の動作の詳細については、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
 [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)に関するドキュメントの「デリゲート型の概要」セクションには、<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>、<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> オブジェクトに提供できるデリゲート型がまとめられています。 表では、デリゲート型が同期的または非同期的に動作するかどうかについても示しています。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 この例では、データフロー ブロックのタスクを同期的に実行するために、<xref:System.Func%602> オブジェクトに <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 型のデリゲートを提供しています。 データフロー ブロックが非同期的に動作できるようにするには、型 <xref:System.Func%601> のデリゲートをデータフロー ブロックに提供します。 データフロー ブロックが非同期的に動作している場合、返された <xref:System.Threading.Tasks.Task%601> オブジェクトが完了したときのみ、データフロー ブロックのタスクが完了します。 次の例では、`CountBytes` メソッドを変更して、[async](../../csharp/language-reference/keywords/async.md) 演算子と [await](../../csharp/language-reference/operators/await.md) 演算子 (Visual Basic では [Async](../../visual-basic/language-reference/modifiers/async.md) と [Await](../../visual-basic/language-reference/operators/await-operator.md)) を使用して、提供されたファイル内のゼロ バイトの件数の合計を非同期的に計算します。 <xref:System.IO.FileStream.ReadAsync%2A> メソッドは、ファイルの読み取り操作を非同期的に実行します。  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#2)]
 [!code-vb[TPLDataflow_ExecutionBlocks#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#2)]  
  
 実行データフロー ブロックでアクションを実行するために、非同期ラムダ式を使用することもできます。 次の例では、前の例で使用される <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> オブジェクトを変更して、ラムダ式を使用して処理を非同期的に実行できるようにしています。  
  
 [!code-csharp[TPLDataflow_ExecutionBlocks#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_executionblocks/cs/dataflowexecutionblocks.cs#3)]
 [!code-vb[TPLDataflow_ExecutionBlocks#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_executionblocks/vb/dataflowexecutionblocks.vb#3)]  
  
## <a name="see-also"></a>参照

- [データフロー](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
