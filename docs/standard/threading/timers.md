---
title: タイマー
description: マルチスレッド環境で使用する .NET タイマーについて説明します。
ms.date: 07/03/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [.NET Framework], timers
- timers, about timers
ms.assetid: 7091500d-be18-499b-a942-95366ce185e5
author: pkulikov
ms.openlocfilehash: d7d1fa13b02fe7425fa9b4cb81ba20297a23fe4b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73128948"
---
# <a name="timers"></a>タイマー

.NET には、マルチスレッド環境で使用するタイマーが 2 つあります。

- <xref:System.Threading.Timer?displayProperty=nameWithType> は <xref:System.Threading.ThreadPool> スレッドで決まった間隔を空けながら呼び出しメソッドを 1 つ実行します。
- <xref:System.Timers.Timer?displayProperty=nameWithType> は既定では、<xref:System.Threading.ThreadPool> スレッドで決まった間隔を空けながらイベントを発生させます。

> [!NOTE]
> 一部の .NET 実装には追加タイマーが含まれています。
>
> - <xref:System.Windows.Forms.Timer?displayProperty=nameWithType>: 一定の間隔でイベントを発生させる Windows フォーム コンポーネント。 このコンポーネントにはユーザー インターフェイスがなく、シングルスレッド環境で使用するように設計されています。  
> - <xref:System.Web.UI.Timer?displayProperty=nameWithType>: 非同期または同期の Web ページのポストバックを一定の間隔で実行する ASP.NET コンポーネント。
> - <xref:System.Windows.Threading.DispatcherTimer?displayProperty=nameWithType>: 指定の間隔と指定の優先順位で処理される <xref:System.Windows.Threading.Dispatcher> キューに統合されるタイマー。

## <a name="the-systemthreadingtimer-class"></a>System.Threading.Timer クラス

<xref:System.Threading.Timer?displayProperty=nameWithType> クラスによって、指定の間隔でデリゲートを連続的に呼び出すことができます。 このクラスを使用し、指定の間隔でデリゲートの呼び出しを 1 つスケジュールすることもできます。 デリゲートは <xref:System.Threading.ThreadPool> スレッドで実行されます。

<xref:System.Threading.Timer?displayProperty=nameWithType> オブジェクトを作成するとき、呼び出しメソッドを定義する <xref:System.Threading.TimerCallback> デリゲート、呼び出しに渡される任意の状態オブジェクト、最初の呼び出しまで遅らせる時間、呼び出し間隔を指定します。 保留中のタイマーを取り消すには、<xref:System.Threading.Timer.Dispose%2A?displayProperty=nameWithType> メソッドを呼び出します。

次の例では、1 秒 (1000 ミリ秒) 後に最初の指定デリゲートを呼び出し、その後、2 秒おきに呼び出すタイマーが作成されます。 この例の状態オブジェクトは、デリゲートを呼び出す回数を数えるために使用されます。 デリゲートが少なくとも 10 回呼び出されると、タイマーが停止します。

[!code-cpp[System.Threading.Timer#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.Timer/CPP/source2.cpp#2)]
[!code-csharp[System.Threading.Timer#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.Timer/CS/source2.cs#2)]
[!code-vb[System.Threading.Timer#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.Timer/VB/source2.vb#2)]

使用例を含む詳細については、「<xref:System.Threading.Timer?displayProperty=nameWithType>」を参照してください。

## <a name="the-systemtimerstimer-class"></a>System.Timers.Timer クラス

マルチスレッド環境で使用できる別のタイマーが <xref:System.Timers.Timer?displayProperty=nameWithType> です。このタイマーは既定で、<xref:System.Threading.ThreadPool> スレッドでイベントを発生させます。

<xref:System.Timers.Timer?displayProperty=nameWithType> オブジェクトを作成するとき、<xref:System.Timers.Timer.Elapsed> イベントを発生させる間隔を指定できます。 <xref:System.Timers.Timer.Enabled%2A> プロパティを使用し、タイマーが <xref:System.Timers.Timer.Elapsed> イベントを発生させるかどうかを示します。 指定間隔の経過後、1 回だけ <xref:System.Timers.Timer.Elapsed> イベントを発生させる必要がある場合、<xref:System.Timers.Timer.AutoReset%2A> を `false` に設定します。 <xref:System.Timers.Timer.AutoReset%2A> プロパティの既定値は `true` です。<xref:System.Timers.Timer.Elapsed> プロパティで定義される間隔で <xref:System.Timers.Timer.Interval%2A> イベントが発生します。

使用例を含む詳細については、「<xref:System.Timers.Timer?displayProperty=nameWithType>」を参照してください。
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Timer?displayProperty=nameWithType>
- <xref:System.Timers.Timer?displayProperty=nameWithType>
- [スレッド処理オブジェクトと機能](threading-objects-and-features.md)
