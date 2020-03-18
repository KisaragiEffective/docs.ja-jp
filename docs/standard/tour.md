---
title: .NET のツアー
description: .NET のいくつかの優れた機能についてのガイド付きツアーです。
author: cartermp
ms.author: wiwagn
ms.date: 05/22/2017
ms.technology: dotnet-standard
ms.assetid: bbfe6465-329d-4982-869d-472e7ef85d93
ms.openlocfilehash: 61d4792b1f1b92dd59442ee38810da96c6cf63bd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78241144"
---
# <a name="tour-of-net"></a>.NET のツアー

.NET は、汎用的な開発プラットフォームです。 .NET には、複数のプラットフォームでのさまざまなシナリオを可能にする、複数のプログラミング言語、非同期および同時実行のプログラミング モデル、ネイティブな相互運用性のサポートなど、重要な機能がいくつかあります。

この記事は、.NET のいくつかの重要な機能についてのガイド付きツアーです。 .NET アーキテクチャの構成要素とそれらの用途については、「[.NET アーキテクチャ コンポーネント](components.md)」をご覧ください。

## <a name="how-to-run-the-code-samples"></a>コード サンプルの実行方法

コード サンプルを実行できるように開発環境を設定する方法については、[はじめに](get-started.md)をご覧ください。 このページのコード サンプルをコピーして環境に貼り付けて実行します。

## <a name="programming-languages"></a>プログラミング言語

.NET は複数のプログラミング言語をサポートしています。 .NET 実装では、[共通言語基盤 (CLI)](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/) が実装されています。CLI では特に、言語に依存しないランタイムと言語の相互運用性が指定されています。 つまり、任意の .NET 言語を選んで、.NET でアプリとサービスを作成します。

Microsoft が開発とサポートに力を注いでいる .NET 言語は、C#、F#、Visual Basic の 3 つです。

* C# はシンプル、強力、タイプセーフ、そしてオブジェクト指向でありながらも、C スタイル言語の表現力と簡潔さが維持されています。 C や類似の言語を使い慣れている人であれば、ほとんど問題なく C# に適応できます。 C# について詳しくは、「[C# ガイド](../csharp/index.yml)」 (C# ガイド) をご覧ください。

* F# はクロスプラットフォームの関数型プログラミング言語ですが、従来のオブジェクト指向および命令型プログラミングもサポートしています。 F# について詳しくは、「[F# ガイド](../fsharp/index.yml)」 (F# ガイド) をご覧ください。

* Visual Basic は、学習しやすい言語で、.NET 上で実行されるさまざまなアプリの構築に使用します。 .NET 言語の中で Visual Basic の構文は通常の人間の言語に最も近いため、ソフトウェア開発の経験のないユーザーでも使いやすい言語です。

## <a name="automatic-memory-management"></a>自動メモリ管理

.NET は、[ガベージ コレクション (GC) ](garbage-collection/index.md)を使ってプログラムの自動メモリ管理を行います。 GC はメモリ管理に対する遅延アプローチで動作します。この場合、メモリの即時収集よりもアプリのスループットが優先されます。 .NET GC について詳しくは、「[ガベージ コレクションの基礎](garbage-collection/fundamentals.md)」をご覧ください。

以下の 2 つの行はどちらもメモリを割り当てています。

[!code-csharp[MemoryManagement](../../samples/snippets/csharp/snippets/tour/MemoryManagement.csx#L1-L2)]

ガベージ コレクターがスケジュールされた実行によってメモリを解放する際に割り当て解除が自動的に行われるため、メモリの割り当てを解除するための類似したキーワードはありません。

ガベージ コレクターは、"*メモリの安全性*" の確保に役立つサービスの 1 つです。 割り当てられているメモリのみにプログラムがアクセスする場合、そのプログラムはメモリ セーフです。 たとえば、ランタイムでは、配列の範囲を超えた割り当てられていないメモリにアプリがアクセスしていないことを確認します。

次の例では、メモリの安全性を確保するため、ランタイムにより <xref:System.IndexOutOfRangeException> 例外がスローされます。

[!code-csharp[MemoryManagement](../../samples/snippets/csharp/snippets/tour/MemoryManagement.csx#L4-L5)]

## <a name="working-with-unmanaged-resources"></a>アンマネージ リソースの操作

一部のオブジェクトは、*アンマネージ リソース*を参照します。 アンマネージ リソースは、.NET ランタイムで自動的に維持されないリソースです。 たとえば、ファイル ハンドルは、アンマネージ リソースです。 <xref:System.IO.FileStream> オブジェクトはマネージ オブジェクトですが、アンマネージドのファイル ハンドルを参照します。 <xref:System.IO.FileStream> の使用が終わったら、ファイル ハンドルを解放する必要があります。

.NET では、アンマネージ リソースを参照するオブジェクトは <xref:System.IDisposable> インターフェイスを実装します。 オブジェクトの使用が終わったら、すべてのアンマネージ リソースを解放する、オブジェクトの <xref:System.IDisposable.Dispose> メソッドを呼び出します。 そのようなオブジェクトに対し、.NET 言語では次の例に示すように便利な [`using` ステートメント](../csharp/language-reference/keywords/using.md)が提供されています。

[!code-csharp[UnmanagedResources](../../samples/snippets/csharp/snippets/tour/UnmanagedResources.csx#L1-L6)]

`using` ブロックが完了すると、.NET ランタイムがファイル ハンドルを開放する `stream` オブジェクトの <xref:System.IDisposable.Dispose> メソッドを自動的に呼び出します。 これは、例外によってコントロールがブロックを離れた場合にも行われます。

詳しくは、次のトピックをご覧ください。

* C# の場合は、「[using ステートメント (C# リファレンス)](../csharp/language-reference/keywords/using-statement.md)」を参照してください。
* F# の場合は、「[リソースの管理:use キーワード](../fsharp/language-reference/resource-management-the-use-keyword.md)」を参照してください。
* Visual Basic の場合は、「[Using ステートメント (Visual Basic)](../visual-basic/language-reference/statements/using-statement.md)」を参照してください。

## <a name="type-safety"></a>タイプ セーフ

オブジェクトは特定の型のインスタンスです。 指定のオブジェクトが許可される操作は、その型から決まります。 `Dog` 型には `Jump` と `WagTail` メソッドを含むことができますが、`SumTotal` メソッドを含むことはできません。 プログラムは、特定の型に属するメソッドのみを呼び出します。 それ以外のどの呼び出しを行っても、コンパイル時エラーまたはランタイム例外が発生します (動的機能または `object` を使用した場合)。

.NET 言語は、オブジェクト指向で、基本クラスと派生クラスの階層を含みます。 .NET ランタイムではオブジェクトの階層に応じたオブジェクトのキャストと呼び出しのみが許可されます。 .NET 言語で定義されているすべての型が、基本の <xref:System.Object> 型から派生していることを覚えておいてください。

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L19-L23)]

タイプ セーフは、アクセサー キーワードの忠実性を保証することで、カプセル化の支援を行うためにも使用されます。 アクセサー キーワードは、指定した型のメンバーへのアクセスを他のコードによって制御する成果物です。 これらは通常、その動作を管理するために使用される型に含まれる、さまざまな種類のデータに使用されます。

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L3-L3)]

C#、Visual Basic、F# は、ローカルな*型推論*をサポートします。 型推論は、コンパイラが右側にある式から左側にある式の型を推論するという意味です。 タイプ セーフの破損、または回避を意味するわけではありません。 結果の型には、推論されるすべてを含む厳密な型が含まれます。 前の例の `dog` を書き換えて型の推論を導入し、残りの部分はそのままとします。

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L28-L34)]

F# は、C# や Visual Basic のメソッド ローカルな型推論よりさらに進んだ型推論機能を備えています。 詳しくは、「[型の推定](../fsharp/language-reference/type-inference.md)」 (型推論) をご覧ください。

## <a name="delegates-and-lambdas"></a>デリゲートとラムダ

デリゲートは、メソッド シグネチャで表されます。 そのシグネチャを持つ任意のメソッドをデリゲートに割り当てることができます。割り当てたメソッドは、デリゲートが呼び出されると実行されます。

デリゲートは、C++ の関数ポインターに似ています。ただし、タイプ セーフであるという点は異なります。 CLR 型システム内で接続されていないメソッドの一種です。 通常のメソッドはクラスに接続されており、静的な、またはインスタンスが呼び出す規則によってのみ直接呼び出すことができます。

.NET では、デリゲートはイベント ハンドラー (非同期の操作を定義するとき) やラムダ式 (LINQ の基軸である) でよく使用されます。 詳細について、「[デリゲートとラムダ](delegates-lambdas.md)」を参照してください。

## <a name="generics"></a>ジェネリック

ジェネリックを使用することで、プログラマーがクラスを設計する際に "*型パラメーター*" を導入することができ、これによってクライアント コード (その型のユーザー) が型パラメーターの代わりに使用する正確な型を指定できるようになります。

ジェネリックは、プログラマが汎用的なデータ構造を実装するために追加されました。 それ以前は、`List` などの型をジェネリックにするには、`object` 型の要素を使用する必要がありました。 これにより、軽微なランタイム エラーの可能性があることは言うまでもなく、パフォーマンスやセマンティックのさまざまな問題が発生することがありました。 一般的なランタイム エラーは、たとえば、データ構造に整数と文字列の両方が含まれる場合に、リストのメンバーの処理中に <xref:System.InvalidCastException> がスローされるということです。

以下のサンプルに、<xref:System.Collections.Generic.List%601> 型のインスタンスを使用して実行される基本的なプログラムを示します。

[!code-csharp[GenericsShort](../../samples/snippets/csharp/snippets/tour/GenericsShort.csx)]

詳細については、トピック「[ジェネリック型 (ジェネリック) の概要](generics.md)」を参照してください。

## <a name="async-programming"></a>非同期プログラミング

非同期プログラミングは、ランタイムでの非同期サポート、フレームワーク ライブラリ、.NET 言語構成要素が含まれる、.NET のファースト クラスの概念です。 内部は、オペレーティング システムを利用して可能な限り効率的に I/O バウンドなジョブを実行する、(`Task` などの) オブジェクトに基づいています。

.NET の非同期プログラミングの詳細を理解するには、最初にトピック「[非同期の概要](async.md)」を参照してください。

## <a name="language-integrated-query-linq"></a>統合言語クエリ (LINQ)

LINQ は、データ操作のための単純な宣言型コードを記述できる、C# および Visual Basic の強力な一連の機能です。 データは (メモリ内オブジェクト、SQL データベース、XML ドキュメントなどの) さまざまな形式にすることができますが、記述する LINQ コードは通常、どのデータ ソースでも違いがないように見えます。

詳細および一部のサンプルを確認するには、トピック「[LINQ (統合言語クエリ)](using-linq.md)」を参照してください。

## <a name="native-interoperability"></a>ネイティブ相互運用性

どのオペレーティング システムにも、システム サービスを提供するアプリケーション プログラミング インターフェイス (API) が含まれています。 .NET には、その API を呼び出すためのいくつかの方法が用意されています。

ネイティブの相互運用を行う主な方法が、"プラットフォーム呼び出し" (略して P/Invoke) を使用するものです。これは、Linux および Windows のプラットフォームに渡ってサポートされています。 Windows 限定でネイティブの相互運用を行う方法が "COM 相互運用" です。これは、マネージド コードで [COM コンポーネント](/cpp/atl/introduction-to-com)を操作する場合に使用します。 これは、P/Invoke インフラストラクチャ上に構築されますが、動作は少し異なります。

Java および Objective-C に対する Mono (つまり Xamarin) の相互運用性サポートの多くが同じようにして構築されています。つまり、同じ原則を使用しているということです。

ネイティブ相互運用性の詳細については、記事「[ネイティブ相互運用性](native-interop/index.md)」を参照してください。

## <a name="unsafe-code"></a>アンセーフ コード

言語サポートに応じて、CLR の `unsafe` コードによって、ネイティブ メモリにアクセスしたりポインターの算術演算を実行したりできるようになります。 この操作は、特定のアルゴリズムおよびシステム相互運用性のために必要です。 アンセーフ コードの使用は強力ですが、システム API との相互運用を行ったり、最も効率的なアルゴリズムを実装したりする必要がなければ、推奨されません。 アンセーフ コードは、環境が異なると同じように実行されない可能性があり、さらにガベージ コレクターとタイプ セーフの利点が得られない場合があります。 可能な限りアンセーフ コードのみに制限し、そのコードを徹底的にテストすることをお勧めします。

`StringBuilder` クラスから `ToString()` メソッドを変更した例を次に示します。 次のように `unsafe` コードを使用してメモリのチャンクを直接移動して、アルゴリズムを効率的に実装する方法を示しています。

[!code-csharp[Unsafe](../../samples/snippets/csharp/snippets/tour/Unsafe.csx)]

## <a name="next-steps"></a>次の手順

C# の機能については、「[C# のツアー](../csharp/tour-of-csharp/index.md)」をご覧ください。

F# の機能については、「[F# のツアー](../fsharp/tour.md)」をご覧ください。

独自のコードの記述を開始する場合は、「[Getting Started](get-started.md)」 (はじめに) をご覧ください。

.NET の重要なコンポーネントについては、「[.NET アーキテクチャ コンポーネント](components.md)」をご覧ください。
