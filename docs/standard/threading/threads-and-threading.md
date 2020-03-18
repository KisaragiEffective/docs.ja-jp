---
title: スレッドおよびスレッド処理
ms.date: 11/08/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- multiple threads
- threading [.NET]
- threading [.NET], multiple threads
ms.assetid: 5baac3aa-e603-4fa6-9f89-0f2c1084e6b1
ms.openlocfilehash: ad36789579b95e0129e402765194b9f5e45a4cc1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127512"
---
# <a name="threads-and-threading"></a>スレッドおよびスレッド処理

マルチスレッドによりアプリケーションの応答性が向上し、さらにアプリケーションがマルチプロセッサやマルチコア システム上で実行されている場合にはそのスループットも向上します。

## <a name="processes-and-threads"></a>プロセスおよびスレッド

*プロセス*は、実行中のプログラムです。 オペレーティング システムはプロセスを使用して実行中のアプリケーションを区別します。 *スレッド*は、オペレーティング システムがプロセッサ時間を割り当てる基本単位です。 各スレッドには[スケジューリングの優先順位](scheduling-threads.md)があり、スレッドの実行が一時停止されるときにシステムがスレッド コンテキストを保存するために使用する構造体のセットが保持されています。 スレッド コンテキストには、スレッドがシームレスに実行を再開するために必要なすべての情報 (スレッドの CPU レジスタとスタックのセットなど) が含まれています。 複数のスレッドは、プロセスのコンテキストで実行できます。 プロセスのすべてのスレッドで、仮想アドレス空間が共有されます。 スレッドは、別のスレッドによって実行されている部分を含む、プログラム コードのすべての部分を実行できます。

> [!NOTE]
> .NET Framework では、*アプリケーション ドメイン*の使用によりプロセス内でアプリケーションを分離する方法が提供されます。 (アプリケーション ドメインは .NET Core で使用できません。)詳細については、「[アプリケーション ドメイン](../../framework/app-domains/application-domains.md)」の記事の「[アプリケーション ドメインとスレッド](../../framework/app-domains/application-domains.md#application-domains-and-threads)」のセクションを参照してください。

既定では、.NET プログラムは、1 つのスレッド (よく*プライマリ* スレッドと呼ばれます) で開始します。 ただし、コードを並列実行したり、プライマリ スレッドと同時に実行したりするための追加のスレッドを作成することができます。 このようなスレッドは、よく*ワーカー* スレッドと呼ばれます。

## <a name="when-to-use-multiple-threads"></a>複数のスレッドを使用する場合

複数のスレッドを使用すると、アプリケーションの応答性が向上し、さらにマルチプロセッサやマルチコア システムの利点を活用してアプリケーションのそのスループットも向上します。

プライマリ スレッドがユーザー インターフェイス要素を担い、ユーザーの操作に応答するデスクトップ アプリケーションを考えてください。 ワーカー スレッドは、時間のかかる操作を実行する場合に使用します。そうしないと、プライマリ スレッドが一杯になり、ユーザー インターフェイスが応答しなくなります。 ネットワークやデバイスの通信専用のスレッドを使用して、受信したメッセージやイベントへの応答性を高めることもできます。

並列で実行できる操作をプログラムが実行する場合、そのような操作を別のスレッドで実行し、プログラムをマルチプロセッサやマルチコア システムで実行することで、合計実行時間を短縮することができます。 このようなシステムでは、マルチスレッドを使用すると、応答性とともにスループットも向上する場合があります。

## <a name="how-to-use-multithreading-in-net"></a>.NET でのマルチスレッドの使用方法

.NET Framework 4 より、マルチスレッドを使用するために推奨される方法が、[タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md) と [Parallel LINQ (PLINQ)](../parallel-programming/parallel-linq-plinq.md) の使用です。 詳細については、[並列プログラミング](../parallel-programming/index.md)に関するページを参照してください。

TPL も PLINQ も、<xref:System.Threading.ThreadPool> スレッドに依存しています。 <xref:System.Threading.ThreadPool?displayProperty=nameWithType> クラスから .NET アプリケーションにワーカー スレッドのプールが提供されます。 スレッド プールのスレッドを使用することもできます。 詳しくは、「[マネージド スレッド プール](the-managed-thread-pool.md)」を参照してください。

最後に、マネージド スレッドを表す <xref:System.Threading.Thread?displayProperty=nameWithType> クラスを使用することができます。 詳細については、「[スレッドの使用とスレッド処理](using-threads-and-threading.md)」を参照してください。

複数のスレッドで、共有リソースにアクセスできる必要があります。 リソースを壊れていない状態のまま保持し、競合状態を避けるには、スレッド アクセス権を同期する必要があります。 複数のスレッドの相互作用の調整が必要になる場合もあります。 .NET では、共有リソースへのアクセスを同期する場合や、スレッド相互作用を調整する場合に使用できるさまざまな型が提供されます。 詳細については、「[同期プリミティブの概要](overview-of-synchronization-primitives.md)」を参照してください。

スレッドで例外を処理します。 スレッドで未処理の例外が発生すると、通常はプロセスが終了します。 詳細については、「[マネージド スレッドの例外](exceptions-in-managed-threads.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [スレッド処理オブジェクトと機能](threading-objects-and-features.md)
- [マネージド スレッド処理の実施](managed-threading-best-practices.md)
- [.NET での並列処理、コンカレンシー、および非同期プログラミング](../parallel-processing-and-concurrency.md)
- [プロセスとスレッドについて](/windows/desktop/procthread/about-processes-and-threads)
