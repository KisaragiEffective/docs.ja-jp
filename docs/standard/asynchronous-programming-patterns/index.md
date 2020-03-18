---
title: 非同期プログラミングのパターン
ms.date: 10/16/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- asynchronous design patterns, .NET
- .NET Framework, asynchronous design patterns
ms.assetid: 4ece5c0b-f8fe-4114-9862-ac02cfe5a5d7
ms.openlocfilehash: e1efe9c3eb57f317def91e527506c358eb086679
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160054"
---
# <a name="asynchronous-programming-patterns"></a>非同期プログラミングのパターン

.NET には、非同期操作を実行するための 3 つのパターンが用意されています。  

- **タスク ベースの非同期パターン (TAP)** 。1 つのメソッドを使用して非同期操作の開始と完了を表します。 TAP は、.NET Framework 4 で導入されました。 **.NET で非同期プログラミングを行う場合、これが推奨される方法となります。** C# の [async](../../csharp/language-reference/keywords/async.md) キーワードと [await](../../csharp/language-reference/operators/await.md) キーワード、および Visual Basic の [Async](../../visual-basic/language-reference/modifiers/async.md) 演算子と [Await](../../visual-basic/language-reference/operators/await-operator.md) 演算子により、TAP の言語サポートが追加されます。 詳細については、「[タスク ベースの非同期パターン (TAP)](task-based-asynchronous-pattern-tap.md)」を参照してください。  

- **イベント ベースの非同期パターン (EAP)** は、非同期動作を提供するための、イベント ベースの従来のモデルです。 これは、`Async` サフィックスを持つメソッドと、1 つ以上のイベント、イベント ハンドラー デリゲート型、および `EventArg` 派生型を必要とします。 EAP は、.NET Framework 2.0 で導入されました。 新規の開発では推奨されなくなりました。 詳細については、「[イベント ベースの非同期パターン (EAP)](event-based-asynchronous-pattern-eap.md)」を参照してください。  

- **非同期プログラミング モデル (APM)** パターン (<xref:System.IAsyncResult> パターンとも呼ばれます) は、<xref:System.IAsyncResult> インターフェイスを使用して非同期動作を提供する従来のモデルです。 このパターンでは、同期操作に `Begin` と `End` メソッドが必要になります (たとえば、非同期書き込み操作を実装するための `BeginWrite` と `EndWrite`)。 このパターンは、新規の開発では推奨されなくなりました。 詳細については、「[非同期プログラミング モデル (APM)](asynchronous-programming-model-apm.md)」を参照してください。  
  
## <a name="comparison-of-patterns"></a>パターンの比較

3 つのパターンで非同期操作がどのようにモデリングされるかを簡単に比較するために、指定された量のデータを、指定のバッファーの指定されたオフセットに読み込む `Read` メソッドを考えます。  
  
```csharp  
public class MyClass  
{  
    public int Read(byte [] buffer, int offset, int count);  
}  
```  

TAP のこのメソッドに対応する部分では、次の単一の `ReadAsync` メソッドが公開されます。  
  
```csharp
public class MyClass  
{  
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
}  
```

EAP の場合は次の型とメンバーのセットが公開されます。  
  
```csharp  
public class MyClass  
{  
    public void ReadAsync(byte [] buffer, int offset, int count);  
    public event ReadCompletedEventHandler ReadCompleted;  
}  
```  
  
APM の対応する部分では `BeginRead` メソッドと `EndRead` メソッドが公開されます。  
  
```csharp  
public class MyClass  
{  
    public IAsyncResult BeginRead(  
        byte [] buffer, int offset, int count,
        AsyncCallback callback, object state);  
    public int EndRead(IAsyncResult asyncResult);  
}  
```  

## <a name="see-also"></a>参照

- [非同期の詳細](../async-in-depth.md)
- [C# の非同期プログラミング](../../csharp/async.md)
- [F# の非同期プログラミング](../../fsharp/tutorials/asynchronous-and-concurrent-programming/async.md)
- [Async および Await を使用した非同期プログラミング (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)
