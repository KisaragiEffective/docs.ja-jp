---
title: アンマネージ リソースのクリーンアップ
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Close method
- Dispose method
- garbage collector
- garbage collection, unmanaged resources
- cleanup operations
- explicit cleanups
- unmanaged resource cleanup
- Finalize method
ms.assetid: a17b0066-71c2-4ba4-9822-8e19332fc213
ms.openlocfilehash: e05cfb949ee3f206f212ca7015f3ff4c22cd2a12
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73423037"
---
# <a name="cleaning-up-unmanaged-resources"></a>アンマネージ リソースのクリーンアップ

アプリケーションで作成するオブジェクトの大部分については、.NET のガベージ コレクターにメモリ管理を任せることができます。 しかし、アンマネージ リソースを含むオブジェクトを作成する場合は、アプリケーションでそのオブジェクトの使用が終了した時点で、そのリソースを明示的に解放する必要があります。 アンマネージ リソースの種類で最も一般的なのは、ファイル、ウィンドウ、ネットワーク接続、データベース接続などのオペレーティング システム リソースをラップしたオブジェクトです。 ガベージ コレクターは、アンマネージ リソースをカプセル化したオブジェクトを有効期間全体にわたって監視できますが、アンマネージ リソースを解放しクリーンアップする方法については情報を持っていません。

型でアンマネージ リソースを使用している場合は、次のようにする必要があります。

- [dispose パターン](implementing-dispose.md)を実装します。 これは、アンマネージ リソースの確定的解放を有効にするために <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の実装を提供する必要があります。 型のコンシューマーはオブジェクト (および使用するリソース) が不要になると <xref:System.IDisposable.Dispose%2A> を呼び出します。 <xref:System.IDisposable.Dispose%2A> メソッドはアンマネージ リソースを直ちに解放します。

- 型のコンシューマーが <xref:System.IDisposable.Dispose%2A> の呼び出しを忘れた場合にアンマネージ リソースを解放します。 この作業を実行する 2 つの方法があります。

  - アンマネージ リソースをラップするセーフ ハンドルを使用します。 この手法を使用することをお勧めします。 セーフ ハンドルは <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> クラスから派生し、堅牢な <xref:System.Object.Finalize%2A> メソッドを含みます。 セーフ ハンドルを使用するときは、単純に <xref:System.IDisposable> インターフェイスを実装し、<xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> の実装でセーフ ハンドルの <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> メソッドを呼び出します。 セーフ ハンドルのファイナライザーは、<xref:System.IDisposable.Dispose%2A> メソッドが呼び出されなかった場合、ガベージ コレクターによって自動的に呼び出されます。

    — または —

  - <xref:System.Object.Finalize%2A?displayProperty=nameWithType> メソッドをオーバーライドします。 終了処理では、型のコンシューマーが <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> を呼び出してアンマネージ リソースを確定的に破棄しなかった場合に、リソースを非確定的に解放できます。 ただし、オブジェクトの終了処理は複雑でエラーが発生しやすい操作であるため、独自のファイナライザーを用意する代わりに、セーフ ハンドルを使用することをお勧めします。

これで型のコンシューマーは、<xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> の実装を直接呼び出して、アンマネージ リソースで使用されるメモリを解放することができます。 <xref:System.IDisposable.Dispose%2A> メソッドを適切に実装すると、セーフ ハンドルの <xref:System.Object.Finalize%2A> メソッドまたは <xref:System.Object.Finalize%2A?displayProperty=nameWithType> メソッドの独自のオーバーライドは、<xref:System.IDisposable.Dispose%2A> メソッドが呼び出されなかった場合にリソースをクリーンアップするための安全装置になります。

## <a name="in-this-section"></a>このセクションの内容

[Dispose メソッドの実装](../../../docs/standard/garbage-collection/implementing-dispose.md) アンマネージ リソースを解放する [Dispose パターン](implementing-dispose.md)を実装する方法について説明します。

[IDisposable を実装するオブジェクトの使用](../../../docs/standard/garbage-collection/using-objects.md) 型のコンシューマーが <xref:System.IDisposable.Dispose%2A> の実装を確実に呼び出す方法について説明します。 このためには、C# の `using` ステートメントまたは Visual Basic の `Using` ステートメントを使用することをお勧めします。

## <a name="reference"></a>リファレンス

<xref:System.IDisposable?displayProperty=nameWithType>\
アンマネージ リソースの解放のための <xref:System.IDisposable.Dispose%2A> メソッドを定義します。

<xref:System.Object.Finalize%2A?displayProperty=nameWithType>\
アンマネージ リソースが <xref:System.IDisposable.Dispose%2A> メソッドによって解放されない場合に、オブジェクトの終了処理を提供します。

<xref:System.GC.SuppressFinalize%2A?displayProperty=nameWithType>\
終了処理を抑制します。 このメソッドは、慣例的に `Dispose` メソッドから呼び出され、ファイナライザーが実行されないようにします。
