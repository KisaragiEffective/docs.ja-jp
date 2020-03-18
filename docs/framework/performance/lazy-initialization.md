---
title: 限定的な初期化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lazy initialization in .NET, introduction
ms.assetid: 56b4ae5c-4745-44ff-ad78-ffe4fcde6b9b
ms.openlocfilehash: 54776304e484fc7f1db2c56b102034ed0e8650c0
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130314"
---
# <a name="lazy-initialization"></a>限定的な初期化
オブジェクトの*遅延初期化*とは、初めて使用されるまでオブジェクトの作成が延期されることを意味します。 (このトピックでは、*レイジー初期化*と*レイジーインスタンス化*という用語は同義です)。遅延初期化は主に、パフォーマンスの向上、無駄な計算の回避、およびプログラムのメモリ要件の削減を目的として使用されます。 最も一般的なシナリオ:  
  
- あるオブジェクトを作成するとコストが高いのに、プログラムでそのオブジェクトが使用されない可能性もある。 たとえば、メモリ内の `Customer` オブジェクトに `Orders` プロパティがあるとします。このプロパティには、`Order` オブジェクトの大きな配列が含まれていますが、これを初期化するにはデータベースに接続する必要があります。 ユーザーが Orders データを表示しないのであれば、あるいは Orders データを計算に使用しないのであれば、システム メモリや計算処理周期を利用してそのデータを作成する理由がありません。 遅延初期化では `Lazy<Orders>` を利用して `Orders` オブジェクトを宣言することで、オブジェクトが使用されない場合のシステム リソースの無駄使いを回避できます。  
  
- あるオブジェクトを作成するとコストが高いとき、他の高額な演算が完了するまで、そのオブジェクトの作成を延期する。 たとえば、プログラムが起動時に複数のオブジェクト インスタンスを読み込むが、すぐに必要になるのはその中の一部のインスタンスだけという場合があります。 必要なオブジェクトが作成されるまで必要にならないオブジェクトの初期化を遅らせることで、プログラムの起動パフォーマンスを改善できます。  
  
 初期化を遅延させる独自のコードを記述できますが、代わりに <xref:System.Lazy%601> を使用することが推奨されます。 <xref:System.Lazy%601> とその関連型はスレッドセーフ対応であり、一貫性のある例外反映ポリシーを提供します。  
  
 次の表は、.NET Framework バージョン 4 で提供される、さまざまなシナリオの遅延初期化を可能にする型の一覧です。  
  
|[種類]|説明|  
|----------|-----------------|  
|<xref:System.Lazy%601>|あらゆるクラス ライブラリやユーザー定義の型を対象に、遅延初期化セマンティクスを提供するラッパー クラス。|  
|<xref:System.Threading.ThreadLocal%601>|<xref:System.Lazy%601> に似ていますが、スレッドローカル基準の遅延初期化セマンティクスを提供する点が異なります。 すべてのスレッドがその固有の値にアクセスできます。|  
|<xref:System.Threading.LazyInitializer>|オブジェクトの遅延初期化のために高度な `static` メソッドを提供します (Visual Basic の場合は `Shared`)。クラスのオーバーヘッドがありません。|  
  
## <a name="basic-lazy-initialization"></a>基本的な遅延初期化  
 `MyType` のような、遅延初期化型を定義するには、次の例のように、`Lazy<MyType>` を使用します (Visual Basic の場合は `Lazy(Of MyType)`)。 <xref:System.Lazy%601> コンストラクターにデリゲートが渡されない場合、値プロパティの最初のアクセスで <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> を使用することで、ラップされた型が作成されます。 型にパラメーターなしのコンストラクターがない場合は、実行時の例外がスローされます。  
  
 次の例では、`Orders` は、データベースから取得された `Order` オブジェクトの配列を含むクラスであると想定しています。 `Customer` オブジェクトには `Orders` のインスタンスが含まれていますが、ユーザーの操作によっては、`Orders` オブジェクトからのデータは必要ないことがあります。  
  
 [!code-csharp[Lazy#1](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#1)]
 [!code-vb[Lazy#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#1)]  
  
 次の例のように、作成時に特定のコンストラクター オーバーロードを呼び出す <xref:System.Lazy%601> コンストラクターにデリゲートを渡し、他にも初期化手順が必要であればそれを実行することもできます。  
  
 [!code-csharp[Lazy#2](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#2)]
 [!code-vb[Lazy#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#2)]  
  
 遅延オブジェクトの作成後、遅延変数の <xref:System.Lazy%601.Value%2A> プロパティが初めてアクセスされるまで、`Orders` のインスタンスは作成されません。 最初のアクセスで、ラップされた型が作成され、返され、今後のアクセスのために保存されます。  
  
 [!code-csharp[Lazy#3](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#3)]
 [!code-vb[Lazy#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#3)]  
  
 <xref:System.Lazy%601> オブジェクトは常に、初期化に利用されたものと同じオブジェクトまたは値を返します。 そのため、<xref:System.Lazy%601.Value%2A> プロパティは読み取り専用です。 <xref:System.Lazy%601.Value%2A> が参照型を保存する場合、それに新しいオブジェクトを割り当てることはできません。 (ただし、設定可能なパブリックフィールドとプロパティの値は変更できます)。<xref:System.Lazy%601.Value%2A> に値型が格納されている場合、その値を変更することはできません。 それでも、変数コンストラクターを呼び出し、新しい変数を作成することはできます。新しい引数を利用します。  
  
 [!code-csharp[Lazy#4](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#4)]
 [!code-vb[Lazy#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#4)]  
  
 先に見たような新しい遅延インスタンスの場合、その <xref:System.Lazy%601.Value%2A> が初めてアクセスされるまで `Orders` はインスタンス化されません。  
  
### <a name="thread-safe-initialization"></a>スレッドセーフな初期化  
 既定では、<xref:System.Lazy%601> オブジェクトはスレッド セーフです。 つまり、コンストラクターがスレッドセーフの種類を指定しない場合、スレッドセーフな <xref:System.Lazy%601> オブジェクトが作成されます。 マルチスレッド シナリオの場合、スレッドセーフな <xref:System.Lazy%601> オブジェクトの <xref:System.Lazy%601.Value%2A> プロパティにアクセスした最初のスレッドが、すべてのスレッドのすべての後続アクセスで、オブジェクトを初期化します。すべてのスレッドが同じデータを共有します。 そのため、どのスレッドがオブジェクトを初期化するかは問題ではありません。競合状態に害はありません。  
  
> [!NOTE]
> 例外キャッシュを利用すれば、エラー状態にもこの一貫性が及びます。 詳細については、次のセクションの「[遅延オブジェクトの例外](lazy-initialization.md#ExceptionsInLazyObjects)」を参照してください。  
  
 次の例では、同じ `Lazy<int>` インスタンスで、3 つの別個のスレッドに対して同じ値が与えられています。  
  
 [!code-csharp[Lazy#8](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#8)]
 [!code-vb[Lazy#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#8)]  
  
 各スレッドで別個のデータが必要であれば、このトピックの後半で説明するように、<xref:System.Threading.ThreadLocal%601> 型を使用してください。  
  
 一部の <xref:System.Lazy%601> コンストラクターには、`isThreadSafe` という名前のブール値パラメーターがあります。これを利用し、複数のスレッドから <xref:System.Lazy%601.Value%2A> プロパティにアクセスするかどうかを指定します。 1 つだけのスレッドからこのプロパティにアクセスする場合、適度なパフォーマンス上の利点が得られるように、`false` を渡します。 複数のスレッドからプロパティにアクセスする場合、`true` を渡し、初期化時、1 つのスレッドが例外をスローするような競合状態を適切に処理するように <xref:System.Lazy%601> インスタンスに指示します。  
  
 <xref:System.Lazy%601> の一部のコンストラクターには、`mode` という名前の <xref:System.Threading.LazyThreadSafetyMode> パラメーターが含まれています。 このコンストラクターでは、スレッド セーフ モードが 1 つ増えます。 次の表は、スレッド セーフを指定するコンストラクター パラメーターの影響を <xref:System.Lazy%601> オブジェクトのスレッド セーフが受けるしくみをまとめたものです。 各コンストラクターに含まれるこのようなパラメーターは多くても 1 つです。  
  
|オブジェクトのスレッド セーフ|`LazyThreadSafetyMode` `mode` パラメーター|ブール値 `isThreadSafe` パラメーター|スレッド セーフ パラメーターなし|  
|---------------------------------|---------------------------------------------|--------------------------------------|---------------------------------|  
|完全スレッドセーフ。一度に 1 つだけのスレッドが値の初期化を試行します。|<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>|`true`|はい。|  
|スレッド セーフではない。|<xref:System.Threading.LazyThreadSafetyMode.None>|`false`|該当しない。|  
|完全スレッドセーフ。値の初期化に際し、スレッドの競合が起こります。|<xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>|該当しない。|該当しない。|  
  
 表に示されているように、`mode` パラメーターに <xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> を指定することは、`isThreadSafe` パラメーターに `true` を指定することと同じであり、<xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> を指定することは `false` を指定することと同じです。  
  
 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> を指定すると、<xref:System.Lazy%601> インスタンスの初期化を複数のスレッドが試行できます。 1 つだけのスレッドがこの競合を征します。他のスレッドはすべて、競合を征したスレッドにより初期化された値を受け取ります。 初期化中、あるスレッドで例外がスローされた場合、そのスレッドは、競合を征したスレッドが設定した値を受け取ることがありません。 例外はキャッシュされません。<xref:System.Lazy%601.Value%2A> プロパティへの後続のアクセス試行で初期化が行われます。 これは、他のモードでの例外の扱いとは異なります。それについては、次のセクションで説明します。 詳細については、<xref:System.Threading.LazyThreadSafetyMode> 列挙型のページをご覧ください。  
  
<a name="ExceptionsInLazyObjects"></a>   
## <a name="exceptions-in-lazy-objects"></a>遅延オブジェクトの例外  
 前述のように、<xref:System.Lazy%601> オブジェクトは常に、初期化に使用されたものと同じオブジェクトまたは値を返します。そのため、<xref:System.Lazy%601.Value%2A> プロパティは読み取り専用です。 例外キャッシュを有効にすると、この不変性は例外動作にも及びます。 遅延初期化されたオブジェクトで例外キャッシュが有効になっていて、<xref:System.Lazy%601.Value%2A> プロパティに最初にアクセスしたときに初期化メソッドから例外がスローされた場合、その後の <xref:System.Lazy%601.Value%2A> プロパティへのアクセスが試行されるたびに同じ例外がスローされます。 言い換えると、マルチスレッドのシナリオであっても、ラップされた型のコンストラクターが再び呼び出されることはありません。 そのため、<xref:System.Lazy%601> オブジェクトがあるアクセスで例外をスローし、後続のアクセスで値を返すということはありません。  
  
 初期化メソッド (`valueFactory` パラメーター) を取る <xref:System.Lazy%601?displayProperty=nameWithType> コンストラクターを使用すると、例外キャッシュが有効になります。たとえば、`Lazy(T)(Func(T))` コンストラクターの使用時に有効になります。 そのコンストラクターは <xref:System.Threading.LazyThreadSafetyMode> 値 (`mode` パラメーター) も取る場合、<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> または <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> を指定します。 初期化メソッドを指定すると、この 2 つのモードで例外キャッシュが有効になります。 初期化メソッドは非常に単純にすることができます。 たとえば、の `T`: `new Lazy<Contents>(() => new Contents(), mode)` の場合はパラメーターなしのコンストラクターをC#、Visual Basic では `New Lazy(Of Contents)(Function() New Contents())` を呼び出すことができます。 初期化メソッドを指定しない <xref:System.Lazy%601?displayProperty=nameWithType> コンストラクターを使用する場合、`T` についてパラメーターなしのコンストラクターからスローされる例外はキャッシュされません。 詳細については、<xref:System.Threading.LazyThreadSafetyMode> 列挙型のページをご覧ください。  
  
> [!NOTE]
> `isThreadSafe` コンストラクター パラメーターを `false` に設定するか、`mode` コンストラクター パラメーターを <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> に設定して <xref:System.Lazy%601> オブジェクトを作成した場合、1 つのスレッドから <xref:System.Lazy%601> オブジェクトにアクセスするか、独自の同期を提供する必要があります。 これは、例外キャッシュを含む、オブジェクトのあらゆる側面に適用されます。  
  
 前のセクションで説明したように、<xref:System.Lazy%601> オブジェクトの作成に <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> を指定した場合、例外の処理が異なります。 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly> の場合、<xref:System.Lazy%601> インスタンスの初期化にあたり、複数のスレッドが競合できます。 その場合、例外はキャッシュされません。<xref:System.Lazy%601.Value%2A> プロパティに対するアクセス試行は初期化完了まで続行されます。  
  
 次の表は、<xref:System.Lazy%601> コンストラクターが例外キャッシュを制御するしくみをまとめたものです。  
  
|コンストラクター|スレッド セーフ モード|初期化メソッドを使用します|例外がキャッシュされます|  
|-----------------|------------------------|--------------------------------|---------------------------|  
|Lazy(T)()|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|Ｘ|Ｘ|  
|Lazy(T)(Func(T))|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|[はい]|[はい]|  
|Lazy(T)(Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) または `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|Ｘ|Ｘ|  
|Lazy(T)(Func(T), Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) または `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|[はい]|[はい]|  
|Lazy(T)(LazyThreadSafetyMode)|ユーザー指定|Ｘ|Ｘ|  
|Lazy(T)(Func(T), LazyThreadSafetyMode)|ユーザー指定|[はい]|ユーザーが <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly> を指定する場合は、いいえ。指定しない場合は、はい。|  
  
## <a name="implementing-a-lazy-initialized-property"></a>遅延初期化プロパティを実装する  
 遅延初期化を利用してパブリック プロパティを実装するには、プロパティのバッキング フィールドを <xref:System.Lazy%601> として定義し、プロパティの `get` アクセサーから <xref:System.Lazy%601.Value%2A> プロパティを返します。  
  
 [!code-csharp[Lazy#5](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#5)]
 [!code-vb[Lazy#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#5)]  
  
 <xref:System.Lazy%601.Value%2A> プロパティは読み取り専用です。そのため、それを公開するプロパティには `set` アクセサーがありません。 読み取り/書き込みプロパティに <xref:System.Lazy%601> オブジェクトのバッキングが必要な場合、`set` アクセサーは新しい <xref:System.Lazy%601> オブジェクトを作成し、それをバッキング ストアに割り当てる必要があります。 `set` アクセサーは、`set` アクセサーに渡された新しいプロパティ値を返すラムダ式を作成し、そのラムダ式を新しい <xref:System.Lazy%601> オブジェクトのコンストラクターに渡す必要があります。 <xref:System.Lazy%601.Value%2A> プロパティの次のアクセスで新しい <xref:System.Lazy%601> が初期化されます。そのため、その <xref:System.Lazy%601.Value%2A> プロパティは、プロパティに割り当てられた新しい値を返します。 このような入り組んだしくみになるのは、<xref:System.Lazy%601> にビルドされるマルチスレッド保護を保存するためです。 そうでなければ、プロパティ アクセサーは <xref:System.Lazy%601.Value%2A> プロパティにより返された最初の値をキャッシュし、その値だけを変更することになり、自分のスレッド セーフ コードを記述しなければならなくなります。 <xref:System.Lazy%601> オブジェクトがバッキングする読み取り/書き込みプロパティで追加の初期化が必要になるため、パフォーマンスは許容できないものになる可能性があります。 さらに、シナリオによっては、セッターとゲッターの競合状態を回避するために、追加の調整が必要になる場合があります。  
  
## <a name="thread-local-lazy-initialization"></a>スレッドローカルの遅延初期化  
 一部のマルチスレッド シナリオでは、各スレッドに独自のプライベート データを与えると効果的になります。 そのようなデータは*スレッドローカル データ*と呼ばれています。 .NET Framework バージョン 3.5 以前では、`ThreadStatic` 属性を静的変数に適用し、スレッドローカルにすることができました。 ただし、`ThreadStatic` 属性を使用すると、検出が困難なエラーが発生することがあります。 たとえば、次の例のように、基本的な初期化ステートメントであっても、それにアクセスする最初のスレッドでのみ、変数が初期化されます。  
  
 [!code-csharp[Lazy#6](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#6)]
 [!code-vb[Lazy#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#6)]  
  
 その他すべてのスレッドでは、その既定値 (ゼロ) を利用して変数は初期化されます。 .NET Framework バージョン 4 における代替として、<xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 型を使用し、インスタンスベースで、指定した <xref:System.Action%601> デリゲートによりすべてのスレッドで初期化されるスレッドローカル変数を作成できます。 次の例では、`counter` にアクセスするすべてのスレッドでその開始値が 1 になります。  
  
 [!code-csharp[Lazy#7](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#7)]
 [!code-vb[Lazy#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#7)]  
  
 <xref:System.Threading.ThreadLocal%601> は、<xref:System.Lazy%601> とほとんど同じ方法でそのオブジェクトをラップしますが、本質的な違いとして次があります。  
  
- 各スレッドは、他のスレッドからアクセスできない独自のプライベート データを利用してスレッドローカル変数を初期化します。  
  
- <xref:System.Threading.ThreadLocal%601.Value%2A?displayProperty=nameWithType> プロパティは編集可能なプロパティです。何回でも変更できます。 これは例外伝達に影響を与えることがあります。たとえば、ある `get` 操作で例外が発生したが、次の操作で値を初期化できたということがあります。  
  
- 初期化デリゲートが指定されない場合、<xref:System.Threading.ThreadLocal%601> は、型の初期値を利用し、そのラップされた型を初期化します。 この点では、<xref:System.Threading.ThreadLocal%601> は <xref:System.ThreadStaticAttribute> 属性と一致しています。  
  
 次の例は、`ThreadLocal<int>` インスタンスにアクセスするすべてのスレッドがデータの独自のコピーを取得する様子を示しています。  
  
 [!code-csharp[Lazy#9](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#9)]
 [!code-vb[Lazy#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#9)]  
  
## <a name="thread-local-variables-in-parallelfor-and-foreach"></a>Parallel.For と ForEach のスレッドローカル変数  
 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> メソッドを利用してデータ ソースを並列で繰り返すとき、スレッドローカル データのサポートが組み込まれたオーバーロードを利用できます。 そのようなメソッドでは、ローカル デリゲートを利用してデータの作成、アクセス、クリーンアップを行うことで、スレッドローカルになります。 詳細については、「[方法: スレッド ローカル変数を使用する Parallel.For ループを記述する](../../standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)」と「[How to: Write a Parallel.ForEach Loop with Thread-Local Variables (方法: パーティション ローカル変数を使用する Parallel.ForEach ループを記述する)](../../standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)」を参照してください。  
  
## <a name="using-lazy-initialization-for-low-overhead-scenarios"></a>オーバーヘッドが低いシナリオで遅延初期化を使用する  
 大量のオブジェクトを遅延初期化しなければならないシナリオでは、<xref:System.Lazy%601> で各オブジェクトをラッピングするとき、あまりにも多くのメモリまたは計算処理リソースが必要になるのか判断することがあります。 あるいは、遅延初期化の公開方法について厳格な要件が与えられることがあります。 そのような場合、<xref:System.Threading.LazyInitializer?displayProperty=nameWithType> クラスの `static` (Visual Basic の場合、`Shared`) メソッドを利用し、<xref:System.Lazy%601> のインスタンスでラッピングせずに、各オブジェクトを遅延初期化できます。  
  
 次の例では、1 つの <xref:System.Lazy%601> オブジェクトで `Orders` オブジェクト全体をラッピングする代わりに、必要な場合にのみ、個々の `Order` オブジェクトを遅延初期化したと想定しています。  
  
 [!code-csharp[Lazy#10](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#10)]
 [!code-vb[Lazy#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#10)]  
  
 この例では、ループの繰り返しの旅に初期化プロシージャが呼び出されていることに注意してください。 マルチスレッド シナリオでは、初期化プロシージャを呼び出した最初のスレッドの値をすべてのスレッドで確認できます。 後続のスレッドも初期化プロシージャを呼び出しますが、その結果は利用されません。 この種類の潜在的競合状態が許容できない場合、ブール値引数と同期オブジェクトを取る <xref:System.Threading.LazyInitializer.EnsureInitialized%2A?displayProperty=nameWithType> のオーバーロードを使用します。  
  
## <a name="see-also"></a>関連項目

- [マネージド スレッド処理の基本](../../standard/threading/managed-threading-basics.md)
- [スレッドおよびスレッド処理](../../standard/threading/threads-and-threading.md)
- [タスク並列ライブラリ (TPL)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [方法: オブジェクトの遅延初期化を実行する](how-to-perform-lazy-initialization-of-objects.md)
