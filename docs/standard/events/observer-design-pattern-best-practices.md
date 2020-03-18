---
title: オブサーバー デザイン パターンのベスト プラクティス
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- observer design pattern [.NET Framework], best practices
- best practices [.NET Framework], observer design pattern
ms.assetid: c834760f-ddd4-417f-abb7-a059679d5b8c
ms.openlocfilehash: 2da29e0baf429142707d0ddd39b1a11c13a17a90
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141536"
---
# <a name="observer-design-pattern-best-practices"></a>オブサーバー デザイン パターンのベスト プラクティス
.NET Framework では、オブザーバー デザイン パターンは、一連のインターフェイスとして実装されます。 <xref:System.IObservable%601?displayProperty=nameWithType> インターフェイスはデータ プロバイダーを表し、データ プロバイダーはオブザーバーで通知のサブスクリプションを解除できるようにする <xref:System.IDisposable> 実装も提供します。 <xref:System.IObserver%601?displayProperty=nameWithType> インターフェイスはオブザーバーを表します。 このトピックでは、これらのインターフェイスを使用してオブザーバー デザイン パターンを実装するときに、開発者が適用することが望ましいベスト プラクティスについて説明します。  
  
## <a name="threading"></a>スレッド  
 通常、プロバイダーは、何らかのコレクション オブジェクトで表されるサブスクライバー リストに特定のオブザーバーを追加することで、<xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> メソッドを実装し、サブスクライバー リストから特定のオブザーバーを削除することで、<xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> メソッドを実装します。 オブザーバーは、これらのメソッドをいつでも呼び出すことができます。 また、プロバイダー/オブザーバーのコントラクトでは、<xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> コールバック メソッドの後にだれがサブスクリプションを解除するかが指定されていないため、プロバイダーとオブザーバーの両方で同じメンバーをリストから削除しようとする可能性があります。 このような可能性があるため、<xref:System.IObservable%601.Subscribe%2A> メソッドと <xref:System.IDisposable.Dispose%2A> メソッドはどちらもスレッド セーフである必要があります。 通常、これには、[同時実行コレクション](../../../docs/standard/parallel-programming/data-structures-for-parallel-programming.md)またはロックの使用が必要です。 非スレッド セーフの実装では、スレッド セーフではないことが明示的に記載されている必要があります。  
  
 その他の保証は、プロバイダー/オブザーバーのコントラクトの最上部のレイヤーで指定されている必要があります。 実装側で要件を追加する場合には、ユーザーがオブザーバー コントラクトについて混乱しないように明確に示す必要があります。  
  
## <a name="handling-exceptions"></a>例外処理  
 データ プロバイダーとオブザーバーの結合は疎であるため、オブザーバー デザイン パターンの例外は情報提供を目的としています。 このことは、プロバイダーとオブザーバーがオブザーバー デザイン パターンの例外を処理する方法に影響します。  
  
### <a name="the-provider----calling-the-onerror-method"></a>プロバイダー -- OnError メソッドの呼び出し  
 <xref:System.IObserver%601.OnError%2A> メソッドは、<xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType> メソッドと同様に、オブザーバーへの情報メッセージとして使用されます。 ただし、<xref:System.IObserver%601.OnNext%2A> メソッドが、現在のデータまたは更新されたデータをオブザーバーに提供するように設計されているのに対して、<xref:System.IObserver%601.OnError%2A> メソッドは、プロバイダーが有効なデータを提供できないことを示すように設計されています。  
  
 例外を処理して <xref:System.IObserver%601.OnError%2A> メソッドを呼び出す場合、プロバイダーが次のベスト プラクティスに従うことをお勧めします。  
  
- プロバイダーに固有の要件がある場合、プロバイダーは独自の例外を処理する必要があります。  
  
- プロバイダーは、オブザーバーが特定の方法で例外を処理することを期待または要求しないようにします。  
  
- 更新を提供する機能を損なうような例外を処理する場合、プロバイダーは <xref:System.IObserver%601.OnError%2A> メソッドを呼び出す必要があります。 このような例外の情報はオブザーバーに渡すことができます。 それ以外の場合は、オブザーバーに例外を通知する必要はありません。  
  
 プロバイダーが <xref:System.IObserver%601.OnError%2A> メソッドまたは <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> メソッドを呼び出すと、それ以上の通知は行われなくなるため、プロバイダーはそのオブザーバーのサブスクリプションを解除することができます。 ただし、オブザーバーも、<xref:System.IObserver%601.OnError%2A> 通知または <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 通知を受信する前と後の両方を含め、いつでも自分自身のサブスクリプションを解除できます。 オブザーバー デザイン パターンでは、プロバイダーとオブザーバーのどちらがサブスクリプションの解除を行うかは指定しません。そのため、両方でサブスクリプションの解除を試みる可能性があります。 通常、オブザーバーは、サブスクリプションを解除すると、サブスクライバーのコレクションから削除されます。 シングルスレッド アプリケーションでは、削除を試みる前に、オブジェクト参照が有効であること、およびオブジェクトがサブスクライバーのコレクションのメンバーであることを、<xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の実装で確認する必要があります。 マルチスレッド アプリケーションでは、<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> オブジェクトなどのスレッド セーフなコレクション オブジェクトを使用する必要があります。  
  
### <a name="the-observer----implementing-the-onerror-method"></a>オブザーバー -- OnError メソッドの実装  
 プロバイダーからエラー通知を受信した場合、オブザーバーは例外を情報として処理する必要がありますが、特定のアクションの実行は要求されません。  
  
 プロバイダーからの <xref:System.IObserver%601.OnError%2A> メソッド呼び出しに応答する場合、オブザーバーが次のベスト プラクティスに従うことをお勧めします。  
  
- <xref:System.IObserver%601.OnNext%2A> または <xref:System.IObserver%601.OnError%2A> などのインターフェイス実装からオブザーバーが例外をスローすることを回避するようにします。 オブザーバーが例外をスローする場合は、これらの例外が未処理になることを想定する必要があります。  
  
- 呼び出し履歴を保持するために、<xref:System.Exception> メソッドに渡された <xref:System.IObserver%601.OnError%2A> オブジェクトをスローするオブザーバーは、オブジェクトをスローする前に例外をラップする必要があります。 このためには、標準の例外オブジェクトを使用する必要があります。  
  
## <a name="additional-best-practices"></a>その他のベスト プラクティス  
 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> メソッドで登録を解除しようとすると、null 参照になる場合があります。 そのため、この方法は避けることをお勧めします。  
  
 1 つのオブザーバーを複数のプロバイダーにアタッチすることは可能ですが、推奨パターンは、<xref:System.IObserver%601> インスタンスを 1 つの <xref:System.IObservable%601> インスタンスにのみアタッチすることです。  
  
## <a name="see-also"></a>参照

- [オブサーバー デザイン パターン](../../../docs/standard/events/observer-design-pattern.md)
- [方法: オブザーバーを実装する](../../../docs/standard/events/how-to-implement-an-observer.md)
- [方法: プロバイダーを実装する](../../../docs/standard/events/how-to-implement-a-provider.md)
