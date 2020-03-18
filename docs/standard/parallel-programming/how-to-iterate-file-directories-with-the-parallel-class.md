---
title: '方法: Parallel クラスを使用してファイル ディレクトリを反復処理する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel loops, how to iterate directories
ms.assetid: 555e9f48-f53d-4774-9bcf-3e965c732ec5
ms.openlocfilehash: fda8443666d1c90b31cf02c2f925d1c89243a8e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73091314"
---
# <a name="how-to-iterate-file-directories-with-the-parallel-class"></a>方法: Parallel クラスを使用してファイル ディレクトリを反復処理する
多くの場合、ファイル反復処理は簡単に並列化できる操作です。 「[方法: PLINQ を使用してファイル ディレクトリを反復処理する](../../../docs/standard/parallel-programming/how-to-iterate-file-directories-with-plinq.md)」のトピックは、多くのシナリオでこのタスクを実行するための簡単な方法を示しています。 ただし、ファイル システムへのアクセス時に発生する可能性のある多くの種類の例外をコードで処理する必要がある場合は、複雑さが生じることがあります。 次の例は、この問題への対処方法の 1 つを示しています。 この例では、スタック ベースの反復処理を使用して、指定されたディレクトリにあるすべてのファイルとフォルダーを走査し、コードで各種例外をキャッチして処理できるようにしています。 例外を処理する方法は開発者に委ねられています。  
  
## <a name="example"></a>例  
 次の例では、ディレクトリの反復処理は順次実行されますが、ファイルの処理は並列で実行されます。 これは、ディレクトリのファイル占有率が大きい場合に最適な方法と考えられます。 また、ディレクトリの反復処理を並列化し、各ファイルに順次アクセスすることもできます。 多数のプロセッサを搭載したコンピューターを明確に対象としている場合を除き、両方のループを並列化するのは効率的とは言えません。 ただし、どの場合もアプリケーションを徹底的にテストして、最適な方法を決定する必要があります。  
  
 [!code-csharp[TPL_Parallel#08](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallel_file.cs#08)]
 [!code-vb[TPL_Parallel#08](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/fileiteration08.vb#08)]  
  
 この例では、ファイル I/O を同期的に実行します。 大きなファイルを処理する場合やネットワーク接続が低速の場合は、ファイルに非同期にアクセスするよりも望ましいと考えられます。 非同期 I/O の手法を並列反復処理と組み合わせることができます。 詳細については、「[TPL と従来の .NET Framework 非同期プログラミング](../../../docs/standard/parallel-programming/tpl-and-traditional-async-programming.md)」を参照してください。  
  
 この例では、ローカル変数 `fileCount` を使用して、処理済みファイルの合計数を示すカウントを管理します。 この変数は複数のタスクから同時にアクセスされる可能性があるため、この変数へのアクセスは <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> メソッドの呼び出しによって同期されています。  
  
 メイン スレッドで例外がスローされた場合、<xref:System.Threading.Tasks.Parallel.ForEach%2A> メソッドによって開始されたスレッドが引き続き実行されることがあります。 これらのスレッドを停止するには、例外ハンドラーで Boolean 変数を設定し、並列ループを反復処理するたびに値をチェックします。 例外がスローされたことを値が示している場合は、<xref:System.Threading.Tasks.ParallelLoopState> 変数を使用してループを停止または中断します。 詳細については、「[方法: Parallel.For ループを停止または中断する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd460721(v=vs.100))」を参照してください。  
  
## <a name="see-also"></a>参照

- [データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)
