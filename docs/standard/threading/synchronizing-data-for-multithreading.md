---
title: マルチスレッド処理のためのデータの同期
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- synchronization, threads
- threading [.NET], synchronizing threads
- managed threading
ms.assetid: b980eb4c-71d5-4860-864a-6dfe3692430a
ms.openlocfilehash: a70bd3070d8b1dcd06e55d330a01d29071293f6c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159391"
---
# <a name="synchronizing-data-for-multithreading"></a>マルチスレッド処理のためのデータの同期

複数のスレッドが同じオブジェクトのプロパティとメソッドを呼び出す場合は、これらの呼び出しを同期することが重要です。 同期しないと、1 つのスレッドが行っていることを別のスレッドが中断し、オブジェクトが無効な状態になってしまう可能性があります。 メンバーがこのように中断されないように保護されているクラスを、スレッドセーフと呼びます。  
  
.NET には、インスタンスや静的メンバーへのアクセスを同期するためのいくつかの方法が用意されています。  
  
- 同期されたコード領域。 <xref:System.Threading.Monitor> クラス、またはこのクラスに対するコンパイラ サポートを使用して、パフォーマンスを向上させながら、同期を必要とするコード ブロックだけを同期できます。  
  
- 手動での同期。 .NET クラス ライブラリによって提供されている同期オブジェクトを使用できます。 「[同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)」を参照してください。これには、<xref:System.Threading.Monitor> クラスの説明が含まれています。  
  
- 同期されたコンテキスト。 .NET Framework および Xamarin アプリケーションでは、<xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> を使用することで、<xref:System.ContextBoundObject> オブジェクトの単純な自動同期を有効にすることができます。  
  
- <xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間のコレクション クラス。 これらのクラスには、同期された追加操作および削除操作が組み込まれています。 詳しくは、「[スレッド セーフなコレクション](../../../docs/standard/collections/thread-safe/index.md)」を参照してください。  
  
 共通言語ランタイムにはスレッド モデルが用意されていて、要件に応じたさまざまな方法で同期することができる多数のカテゴリにクラスを分類できます。 次の表に、各同期カテゴリで提供されるフィールドおよびメソッドに対する同期サポートを示します。  
  
|カテゴリ|グローバル フィールド|静的フィールド|静的メソッド|インスタンス フィールド|インスタンス メソッド|特定のコード ブロック|  
|--------------|-------------------|-------------------|--------------------|---------------------|----------------------|--------------------------|  
|同期なし|いいえ|いいえ|いいえ|いいえ|いいえ|いいえ|  
|同期されたコンテキスト|いいえ|いいえ|いいえ|はい|はい|いいえ|  
|同期されたコード領域|いいえ|いいえ|マークされている場合にのみ|いいえ|マークされている場合にのみ|マークされている場合にのみ|  
|手動での同期|マニュアル|マニュアル|マニュアル|マニュアル|マニュアル|マニュアル|  
  
## <a name="no-synchronization"></a>同期なし  
 これは、オブジェクトに対する既定の設定です。 すべてのスレッドが、すべてのメソッドまたはフィールドにいつでもアクセスできます。 ただし、これらのオブジェクトにアクセスできるスレッドは一度に 1 つだけです。  
  
## <a name="manual-synchronization"></a>手動での同期  
 .NET クラス ライブラリには、スレッドを同期するための多数のクラスがあります。 「[同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)」を参照してください。  
  
## <a name="synchronized-code-regions"></a>同期されたコード領域  
 <xref:System.Threading.Monitor> クラスまたはコンパイラ キーワードを使用して、コード ブロック、インスタンス メソッド、静的メソッドを同期できます。 同期された静的フィールドに対するサポートはありません。  
  
 Visual Basic と C# の両方が、コード ブロックに特定の言語キーワード (C# の `lock` ステートメント、Visual Basic の `SyncLock` ステートメント) のマークを付けることをサポートしています。 スレッドによってコードが実行されると、ロックの取得が試行されます。 別のスレッドによってロックが既に取得されている場合、ロックが使用可能になるまでスレッドはブロックされます。 同期されているコード ブロック部分の実行をスレッドが終了すると、終了方法に関係なく、ロックが解放されます。  
  
> [!NOTE]
> `lock` ステートメントおよび `SyncLock` ステートメントは、<xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> および <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> を使用して実装されるため、<xref:System.Threading.Monitor> の他のメソッドを、同期された領域内でこれらと組み合わせて使用できます。  
  
 また、<xref:System.Runtime.CompilerServices.MethodImplAttribute> を <xref:System.Runtime.CompilerServices.MethodImplOptions.Synchronized?displayProperty=nameWithType> の値で使用してメソッドを修飾することもできます。これにより、<xref:System.Threading.Monitor> またはメソッド全体をロックするためのコンパイラ キーワードの 1 つを使用した場合と同じ結果になります。  
  
 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> を使用すると、同期されたコード領域へのアクセスの待機などのブロック操作から、スレッドを切り離すことができます。 また、この **Thread.Interrupt** を使用することで、<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> などの操作からスレッドを切り離すこともできます。  
  
> [!IMPORTANT]
> `typeof(MyType)` メソッド (Visual Basic では`GetType(MyType)`) を保護するために、型 (C# の場合は`MyType::typeid`、Visual Basic の場合は`static`、C++ の場合は`Shared`) をロックしないでください。 代わりにプライベート静的オブジェクトを使用します。 同様に、C# の `this` (Visual Basic の場合は `Me`) を使用してインスタンス メソッドをロックしないでください。 代わりにプライベート オブジェクトを使用します。 クラスやインスタンスは、独自のコード以外のコードでもロックできますが、デッドロックやパフォーマンスの問題が発生する可能性があります。  
  
### <a name="compiler-support"></a>コンパイラ サポート  
 Visual Basic と C# は、どちらも <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> と <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> を使用してオブジェクトをロックする言語キーワードをサポートします。 Visual Basic は [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) ステートメントをサポートしており、C# は [lock](../../csharp/language-reference/keywords/lock-statement.md) ステートメントをサポートしています。  
  
 両方とも、コード ブロックで例外がスローされると、**lock** または **SyncLock** によって取得されたロックは自動的に解放されます。 C# コンパイラおよび Visual Basic コンパイラは **try**/**finally** ブロックを生成します。tryブロックは先頭に **Monitor.Enter** を含み、**finally** ブロックは **Monitor.Exit** を含みます。 **lock** ブロックまたは **SyncLock** ブロック内部で例外がスローされると、**finally** ハンドラーが実行され、任意のクリーンアップ作業を行えるようになります。  
  
## <a name="synchronized-context"></a>同期されたコンテキスト  

.NET Framework と Xamarin のアプリケーションでのみ、任意の <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> で <xref:System.ContextBoundObject> を使用して、すべてのインスタンス メソッドとフィールドを同期できます。 同じコンテキスト ドメイン内のすべてのオブジェクトが同じロックを共有します。 複数のスレッドがメソッドやフィールドにアクセスできますが、これらのオブジェクトに一度にアクセスできるのは 1 つのスレッドだけです。  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute>
- [スレッドおよびスレッド処理](../../../docs/standard/threading/threads-and-threading.md)
- [同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)
- [SyncLock ステートメント](../../visual-basic/language-reference/statements/synclock-statement.md)
- [lock ステートメント](../../csharp/language-reference/keywords/lock-statement.md)
