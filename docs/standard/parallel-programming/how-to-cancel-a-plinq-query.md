---
title: '方法: PLINQ クエリを取り消す'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to cancel
- cancellation, PLINQ
ms.assetid: 80b14640-edfa-4153-be1b-3e003d3e9c1a
ms.openlocfilehash: 272f25d62cb63c60209be3bc54dc5e76fb30df54
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73134225"
---
# <a name="how-to-cancel-a-plinq-query"></a>方法: PLINQ クエリを取り消す
次の例は、PLINQ クエリを取り消す 2 つの方法を示しています。 最初の例は、主にデータ トラバーサルで構成されるクエリを取り消す方法を示しています。 2 つ目の例は、負荷の大きいユーザー関数を含むクエリを取り消す方法を示しています。

> [!NOTE]
> [マイ コードのみ] が有効になっている場合、Visual Studio では、例外をスローする行で処理が中断され、"ユーザー コードで処理されない例外" に関するエラー メッセージが表示されます。 このエラーは問題にはなりません。 F5 キーを押して、処理が中断された箇所から続行し、以下の例に示す例外処理動作を確認できます。 Visual Studio による処理が最初のエラーで中断しないようにするには、 **[ツール] メニューの [オプション]、[デバッグ] 、[全般]** の順にクリックし、[マイ コードのみ] チェック ボックスをオフにします。
>
> この例は、使用方法を示すことを意図したものであるため、同等の順次的な LINQ to Objects クエリほど高速ではない可能性があります。 高速化の詳細については、「[PLINQ での高速化について](../../../docs/standard/parallel-programming/understanding-speedup-in-plinq.md)」を参照してください。

## <a name="example"></a>例

[!code-csharp[PLINQ#16](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#16)]
[!code-vb[PLINQ#16](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#16)]

PLINQ フレームワークでは単一の <xref:System.OperationCanceledException> が <xref:System.AggregateException?displayProperty=nameWithType> にローリングされません。<xref:System.OperationCanceledException> は別個のキャッチ ブロックで処理する必要があります。 1 つ以上のユーザー デリゲートが (外部の <xref:System.Threading.CancellationToken?displayProperty=nameWithType> を使用して) OperationCanceledException(externalCT) をスローし、他の例外はスローしない場合で、クエリが `AsParallel().WithCancellation(externalCT)` として定義されている場合は、PLINQ は <xref:System.OperationCanceledException> ではなく、単一の <xref:System.AggregateException?displayProperty=nameWithType> (externalCT) を発行します。 ただし、1 つのユーザー デリゲートが <xref:System.OperationCanceledException> をスローし、別のデリゲートが別の種類の例外をスローした場合、両方の例外が <xref:System.AggregateException> にローリングされます。

取り消しに関する一般的なガイダンスは次のとおりです。

1. ユーザー デリゲートを取り消す場合、外部の <xref:System.Threading.CancellationToken> について PLINQ に通知し、<xref:System.OperationCanceledException>(externalCT) をスローする必要があります。

2. 取り消しが発生し、その他の例外がスローされない場合は、<xref:System.OperationCanceledException> ではなく <xref:System.AggregateException> を処理する必要があります。

## <a name="example"></a>例

次の例は、ユーザー コードで負荷が大きい関数を使用する場合の取り消しの処理方法を示しています。

[!code-csharp[PLINQ#17](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#17)]
[!code-vb[PLINQ#17](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#17)]

ユーザー コードで取り消しを処理する場合、クエリ定義で <xref:System.Linq.ParallelEnumerable.WithCancellation%2A> を使用する必要はありません。 ただし、<xref:System.Linq.ParallelEnumerable.WithCancellation%2A> がクエリのパフォーマンスに影響を与えることはなく、クエリ演算子とユーザー コードで取り消しを処理できるため、このようにすることをお勧めします。

システムの応答性を確保できるように、ミリ秒単位で取り消しを確認することをお勧めします。ただし、許容可能と見なされるのは 10 ミリ秒までです。 この頻度であれば、コードのパフォーマンスに悪影響を与えることはありません。

列挙子が破棄された場合、たとえば、クエリ結果を反復処理している foreach (Visual Basic では For Each) ループからコードが抜け出た場合、クエリは取り消されますが、例外はスローされません。

## <a name="see-also"></a>参照

- <xref:System.Linq.ParallelEnumerable>
- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
- [マネージド スレッドのキャンセル](../../../docs/standard/threading/cancellation-in-managed-threads.md)
