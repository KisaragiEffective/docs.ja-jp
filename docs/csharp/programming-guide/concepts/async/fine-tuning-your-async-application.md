---
title: 非同期アプリケーションの微調整 (C#)
ms.date: 07/20/2015
ms.assetid: 97696eb9-81fc-4940-9655-84daa8eb4d5c
ms.openlocfilehash: cff50e62ff62b70e97e7ea6e03714326d774e407
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73970242"
---
# <a name="fine-tuning-your-async-application-c"></a>非同期アプリケーションの微調整 (C#)
<xref:System.Threading.Tasks.Task> 型を使用できるメソッドとプロパティを使用して、非同期アプリケーションに精度と柔軟性を追加できます。 このセクションのトピックでは <xref:System.Threading.CancellationToken> および `Task` などのような、重要な <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> メソッドおよび <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> の使用例を示します。  
  
 `WhenAny` と `WhenAll` を使用すると、複数のタスクをより簡単に開始し、単一のタスクを監視して、その完了を待機できます。  
  
- `WhenAny` は、コレクションのいずれかのタスクが完了すると完了するタスクを返します。  
  
     `WhenAny` を使う例については、「[完了後の残りの非同期タスクのキャンセル (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md)」および「[完了時での複数の同期タスクとプロセスの実行 (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md)」をご覧ください。  
  
- `WhenAll` は、コレクションのすべてのタスクが完了すると完了するタスクを返します。  
  
     `WhenAll` の使用に関する詳細と例については、「[Task.WhenAll を使用して async Walkthrough を拡張する方法 (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)」を参照してください。
  
 ここでは、次の例について説明します。  
  
- [非同期タスクまたはタスクの一覧のキャンセル (C#)](./cancel-an-async-task-or-a-list-of-tasks.md)  
  
- [指定した時間の経過後の非同期タスクのキャンセル (C#)](./cancel-async-tasks-after-a-period-of-time.md)  
  
- [完了後の残りの非同期タスクのキャンセル (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md)  
  
- [完了時での複数の同期タスクとプロセスの実行 (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
> この例を実行するには、Visual Studio 2012 以降および .NET Framework 4.5 以降が、コンピューターにインストールされている必要があります。  
  
 次の図が示すように、プロジェクトは、プロセスを開始するボタンとそれを取り消すボタンを含む UI を作成します。 ボタンの名前は `startButton` と `cancelButton` です。  
  
 ![[キャンセル] ボタンが表示された WPF ウィンドウ](./media/fine-tuning-your-async-application/cancellation-and-start-button.png "[開始] ボタンと [停止] ボタンがあるダイアログボックス")  
  
 完全な Windows Presentation Foundation (WPF) プロジェクトは、「[Async Sample: Fine Tuning Your Application (非同期のサンプル: アプリケーションの微調整)](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)」からダウンロードできます。  
  
## <a name="see-also"></a>参照

- [Async および Await を使用した非同期プログラミング (C#)](./index.md)
