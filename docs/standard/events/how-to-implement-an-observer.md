---
title: '方法: オブザーバーを実装する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observers [.NET Framework], observer design pattern
- observer design pattern [.NET Framework], implementing observers
ms.assetid: 8ecfa9f5-b500-473d-bcf0-5652ffb1e53d
ms.openlocfilehash: e6aba4d85e502563291478640927bd0f234736a8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139305"
---
# <a name="how-to-implement-an-observer"></a>方法: オブザーバーを実装する
オブザーバー デザイン パターンでは、通知を登録するオブザーバーと、データを監視して 1 人以上のオブザーバーに通知を送信するプロバイダーを分ける必要があります。 このトピックでは、オブザーバーを作成する方法について説明します。 プロバイダーの作成方法については、関連トピックの「[方法: プロバイダーを実装する](../../../docs/standard/events/how-to-implement-a-provider.md)」を参照してください。  
  
### <a name="to-create-an-observer"></a>オブザーバーを作成するには  
  
1. オブザーバーを定義します。これは <xref:System.IObserver%601?displayProperty=nameWithType> インターフェイスを実装する型です。 たとえば、次のコードでは、`TemperatureReporter` という型が定義されています。これは、ジェネリック型引数の <xref:System.IObserver%601?displayProperty=nameWithType> を使用して構築された `Temperature` の実装です。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#8)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#8)]  
  
2. プロバイダーが <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> の実装を呼び出す前にオブザーバーが通知の受信を停止できる場合は、プロバイダーの <xref:System.IDisposable> メソッドから返される <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> の実装を保持するプライベート変数を定義します。 また、プロバイダーの <xref:System.IObservable%601.Subscribe%2A> メソッドを呼び出し、返された <xref:System.IDisposable> オブジェクトを格納するサブスクリプション メソッドも定義する必要があります。 たとえば、次のコードでは `unsubscriber` というプライベート変数を定義し、プロバイダーの `Subscribe` メソッドを呼び出し、返されたオブジェクトを <xref:System.IObservable%601.Subscribe%2A> 変数に割り当てる `unsubscriber` メソッドを定義します。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#9)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#9)]  
  
3. この機能が必要な場合は、プロバイダーが <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> の実装を呼び出す前に、オブザーバーが通知の受信を停止できるようにするメソッドを定義します。 次の例では、`Unsubscribe` メソッドを定義しています。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#10)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#10)]  
  
4. <xref:System.IObserver%601> インターフェイスに定義されている 3 つのメソッド <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>、<xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType>、および <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> の実装を用意します。 プロバイダーとアプリケーションのニーズに応じて、<xref:System.IObserver%601.OnError%2A> メソッドと <xref:System.IObserver%601.OnCompleted%2A> メソッドをスタブ実装にすることができます。 <xref:System.IObserver%601.OnError%2A> メソッドが渡された <xref:System.Exception> オブジェクトを例外として処理しない点と、<xref:System.IObserver%601.OnCompleted%2A> メソッドはプロバイダーの <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の実装を自由に呼び出すことができる点に注意してください。 次の例は、<xref:System.IObserver%601> クラスの `TemperatureReporter` の実装を示しています。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#11)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#11)]  
  
## <a name="example"></a>例  
 次の例には、`TemperatureReporter` クラスの完全なソース コードが含まれています。これは、温度監視アプリケーション向けに <xref:System.IObserver%601> の実装を提供するクラスです。  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#12)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#12)]  
  
## <a name="see-also"></a>参照

- <xref:System.IObserver%601>
- [オブサーバー デザイン パターン](../../../docs/standard/events/observer-design-pattern.md)
- [方法: プロバイダーを実装する](../../../docs/standard/events/how-to-implement-a-provider.md)
- [オブザーバー デザイン パターンのベスト プラクティス](../../../docs/standard/events/observer-design-pattern-best-practices.md)
