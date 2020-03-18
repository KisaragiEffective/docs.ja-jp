---
title: '方法: プロバイダーを実装する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observer design pattern [.NET Framework], implementing providers
- providers [.NET Framework], in observer design pattern
- observables [.NET Framework], in observer design pattern
ms.assetid: 790b5d8b-d546-40a6-beeb-151b574e5ee5
ms.openlocfilehash: f5bb3cda0caa39ba3fd094b80e0b769a4bfc1f85
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141557"
---
# <a name="how-to-implement-a-provider"></a>方法: プロバイダーを実装する
オブザーバー デザイン パターンでは、データを監視して通知を送信するプロバイダーと、プロバイダーから通知 (コールバック) を受信する 1 つまたは複数のオブザーバーを分ける必要があります。 このトピックでは、プロバイダーを作成する方法について説明します。 関連トピックの「[方法: オブザーバーを実装する](../../../docs/standard/events/how-to-implement-an-observer.md)」でオブザーバーの作成方法について説明します。  
  
### <a name="to-create-a-provider"></a>プロバイダーを作成するには  
  
1. プロバイダーがオブザーバーに送信するデータを定義します。 プロバイダーとそれがオブザーバーに送信するデータは 1 つの型にすることができますが、一般的には、異なる型で表されます。 たとえば、温度を監視するアプリケーションでは、`Temperature` 構造によって、プロバイダー (次の手順で定義する `TemperatureMonitor` クラスで表されます) が監視し、オブザーバーがサブスクライブするデータが定義されます。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/data.cs#1)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/data.vb#1)]  
  
2. データ プロバイダーを定義します。これは <xref:System.IObservable%601?displayProperty=nameWithType> インターフェイスを実装する型です。 プロバイダーのジェネリック型引数は、プロバイダーがオブザーバーに送信する型です。 次の例では、`TemperatureMonitor` クラスが定義されています。これは構築 <xref:System.IObservable%601?displayProperty=nameWithType> 実装であり、ジェネリック型引数の `Temperature` が指定されています。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#2)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#2)]  
  
3. 各オブザーバーに適時通知できるようにプロバイダーがオブザーバーの参照を格納する方法を定義します。 最も一般的には、ジェネリック <xref:System.Collections.Generic.List%601> オブジェクトのようなコレクション オブジェクトがこの目的で使用されます。 次の例では、<xref:System.Collections.Generic.List%601> クラス コンストラクターでインスタンス化されるプライベート `TemperatureMonitor` オブジェクトが定義されます。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#3)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#3)]  
  
4. 通知の受信をいつでも停止できるように、プロバイダーがサブスクライバーに返す <xref:System.IDisposable> 実装を定義します。 次の例では、入れ子になった `Unsubscriber` クラスが定義されています。インスタンス化されるとき、サブスクライバー コレクションとサブスクライバーの参照がこのクラスに渡されます。 このコードによって、サブスクライバーはオブジェクトの <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 実装を呼び出し、それ自体をサブスクライバー コレクションから削除できます。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#4)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#4)]  
  
5. <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> メソッドを実装します。 このメソッドには <xref:System.IObserver%601?displayProperty=nameWithType> インターフェイスの参照が渡されます。手順 3 で、その目的のために作られたオブジェクトに格納されます。 このメソッドはその後、手順 4 で作られる <xref:System.IDisposable> 実装を返します。 次の例は、<xref:System.IObservable%601.Subscribe%2A> クラスの `TemperatureMonitor` メソッドの実装です。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#5)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#5)]  
  
6. <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>、<xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType>、<xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> の実装を呼び出し、オブザーバーに適宜通知します。 エラーが発生し、プロバイダーが <xref:System.IObserver%601.OnError%2A> メソッドを呼び出さない場合もあります。 たとえば、次の `GetTemperature` メソッドは 5 秒おきに温度データを読み取るモニターをシミュレートし、温度が前の計測から 1 度以上変化するとオブザーバーに通知します。 デバイスが温度を報告しないと (つまり、その値が null)、プロバイダーは転送が完了していることをオブザーバーに通知します。 各オブザーバーの <xref:System.IObserver%601.OnCompleted%2A> メソッドを呼び出すだけではなく、`GetTemperature` メソッドは <xref:System.Collections.Generic.List%601> コレクションを消去することに注意してください。 この場合、プロバイダーはそのオブザーバーの <xref:System.IObserver%601.OnError%2A> メソッドを呼び出しません。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#6)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#6)]  
  
## <a name="example"></a>例  
 次の例では、温度を監視するアプリケーションの <xref:System.IObservable%601> 実装を定義するソース コードをまるごと確認できます。 オブザーバーに送信されるデータである `Temperature` 構造と `TemperatureMonitor` 実装である <xref:System.IObservable%601> クラスが含まれています。  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#7)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#7)]  
  
## <a name="see-also"></a>参照

- <xref:System.IObservable%601>
- [オブサーバー デザイン パターン](../../../docs/standard/events/observer-design-pattern.md)
- [方法: オブザーバーを実装する](../../../docs/standard/events/how-to-implement-an-observer.md)
- [オブザーバー デザイン パターンのベスト プラクティス](../../../docs/standard/events/observer-design-pattern-best-practices.md)
