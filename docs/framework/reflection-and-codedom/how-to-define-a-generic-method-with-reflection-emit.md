---
title: '方法: リフレクション出力を使用してジェネリック メソッドを定義する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- reflection emit, generic methods
- generics [.NET Framework], dynamic types
ms.assetid: 93892fa4-90b3-4ec4-b147-4bec9880de2b
ms.openlocfilehash: d16f6728b01583fe3ffb8d892522f3892444c537
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130168"
---
# <a name="how-to-define-a-generic-method-with-reflection-emit"></a>方法: リフレクション出力を使用してジェネリック メソッドを定義する

最初の手順では、2 つの型パラメーターを持つ単純なジェネリック メソッドを作成する方法と、クラスの制約、インターフェイスの制約、および特殊な制約を型パラメーターに適用する方法を示します。

2 番目の手順では、メソッド本体を出力する方法と、ジェネリック メソッドの型パラメーターを使用して、ジェネリック型のインスタンスを作成し、それらのメソッドを呼び出す方法を示します。

3 番目の手順では、ジェネリック メソッドを呼び出す方法を示します。

> [!IMPORTANT]
> メソッドはジェネリック型に属し、その型の型パラメーターを使用するだけであるため、ジェネリックではありません。 メソッドがジェネリックになるのは、そのメソッドが独自の型パラメーター リストを持つ場合だけです。 この例のように、ジェネリック メソッドは非ジェネリック型で表すことができます。 ジェネリック型の非ジェネリック メソッド例については、「[方法: リフレクション出力を使用してジェネリック型を定義する](how-to-define-a-generic-type-with-reflection-emit.md)」を参照してください。

### <a name="to-define-a-generic-method"></a>ジェネリック メソッドを定義するには

1. 作業を始める前に、高水準言語を使用して記述したときに、ジェネリック メソッドがどのように表されるかを確認しておくと役立ちます。 次のコードは、ジェネリック メソッドを呼び出すコードと共に、このトピックのプログラム例に含まれています。 このメソッドは、`TInput` と `TOutput` の 2 つの型パラメーターを持ちます。2 つ目の型パラメーターは、参照型 (`class`) であることが必要です。さらに、パラメーターなしのコンストラクター (`new`) を持ち、`ICollection(Of TInput)` (C# では `ICollection<TInput>`) を実装する必要があります。 このインターフェイスの制約によって、<xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> メソッドを使用して、メソッドによって作成される `TOutput` コレクションに要素を確実に追加できるようになります。 メソッドは、`input` の配列である仮パラメーター `TInput` を 1 つ持ちます。 メソッドは、`TOutput` 型のコレクションを作成し、`input` の要素をコレクションにコピーします。

    [!code-csharp[GenericMethodHowTo#20](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#20)]
    [!code-vb[GenericMethodHowTo#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#20)]

2. 動的アセンブリと動的モジュールを定義して、ジェネリック メソッドが属する型を格納します。 この場合、アセンブリには `DemoMethodBuilder1` という名前のモジュールが 1 つだけ含まれます。モジュール名はアセンブリ名に拡張子が付いたものです。 この例では、アセンブリをディスクに保存したうえで実行するため、<xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType> を指定します。 [Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) を使用すると、DemoMethodBuilder1.dll をチェックし、手順 1. に示すメソッドの Microsoft Intermediate Language (MSIL) と比較できます。

    [!code-csharp[GenericMethodHowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#2)]
    [!code-vb[GenericMethodHowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#2)]

3. ジェネリック メソッドが属する型を定義します。 型はジェネリックである必要はありません。 ジェネリック メソッドは、ジェネリック型と非ジェネリック型のどちらにでも属することができます。 この例では、型はクラスであり、ジェネリックではありません。型の名前は `DemoType` です。

    [!code-csharp[GenericMethodHowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#3)]
    [!code-vb[GenericMethodHowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#3)]

4. ジェネリック メソッドを定義します。 ジェネリック メソッドの仮パラメーターの型が、ジェネリック メソッドのジェネリック型パラメーターで指定されている場合は、<xref:System.Reflection.Emit.TypeBuilder.DefineMethod%28System.String%2CSystem.Reflection.MethodAttributes%29> メソッド オーバーロードを使用してメソッドを定義します。 メソッドのジェネリック型パラメーターはまだ定義されていないため、<xref:System.Reflection.Emit.TypeBuilder.DefineMethod%2A> の呼び出しでメソッドの仮パラメーターの型を指定することはできません。 この例では、メソッドの名前は `Factory` です。 メソッドは、パブリックかつ `static` (Visual Basic では `Shared`) です。

    [!code-csharp[GenericMethodHowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#4)]
    [!code-vb[GenericMethodHowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#4)]

5. パラメーターの名前を格納している文字列の配列を `DemoMethod` メソッドに渡して、<xref:System.Reflection.Emit.MethodBuilder.DefineGenericParameters%2A?displayProperty=nameWithType> のジェネリック型パラメーターを定義します。 これにより、メソッドはジェネリック メソッドになります。 次のコードによって、`Factory` を型パラメーター `TInput` と `TOutput` を持つジェネリック メソッドにします。 コードを読みやすくするために、これらの名前の変数を作成し、2 つの型パラメーターを表す <xref:System.Reflection.Emit.GenericTypeParameterBuilder> オブジェクトを保持します。

    [!code-csharp[GenericMethodHowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#5)]
    [!code-vb[GenericMethodHowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#5)]

6. オプションで、型パラメーターに特殊な制約を追加します。 特殊な制約は、<xref:System.Reflection.Emit.GenericTypeParameterBuilder.SetGenericParameterAttributes%2A> メソッドを使用して追加します。 この例では、`TOutput` は参照型であり、パラメーターなしのコンストラクターを持つように制約されます。

    [!code-csharp[GenericMethodHowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#6)]
    [!code-vb[GenericMethodHowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#6)]

7. オプションで、型パラメーターにクラスの制約とインターフェイスの制約を追加します。 この例では、型パラメーター `TOutput` は、`ICollection(Of TInput)` (C# では `ICollection<TInput>`) インターフェイスを実装する型に制約されます。 これにより、<xref:System.Collections.Generic.ICollection%601.Add%2A> メソッドを使用して要素を確実に追加できます。

    [!code-csharp[GenericMethodHowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#7)]
    [!code-vb[GenericMethodHowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#7)]

8. <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A> メソッドを使用して、メソッドの仮パラメーターを定義します。 この例では、`Factory` メソッドは、`TInput` の配列であるパラメーターを 1 つ持ちます。 この型は、<xref:System.Type.MakeArrayType%2A> を表す <xref:System.Reflection.Emit.GenericTypeParameterBuilder> で `TInput` メソッドを呼び出すことによって作成されます。 <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A> の引数は、<xref:System.Type> オブジェクトの配列です。

    [!code-csharp[GenericMethodHowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#8)]
    [!code-vb[GenericMethodHowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#8)]

9. <xref:System.Reflection.Emit.MethodBuilder.SetReturnType%2A> メソッドを使用して、メソッドの戻り値の型を定義します。 この例では、`TOutput` のインスタンスが返されます。

    [!code-csharp[GenericMethodHowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#9)]
    [!code-vb[GenericMethodHowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#9)]

10. <xref:System.Reflection.Emit.ILGenerator> を使用して、メソッド本体を出力します。 詳細については、メソッド本体を出力する際の手順を参照してください。

    > [!IMPORTANT]
    > ジェネリック型のメソッド呼び出しを出力し、それらの型の型引数がジェネリック メソッドの型パラメーターである場合、`static` クラスの <xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29><xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29>、<xref:System.Reflection.Emit.TypeBuilder.GetField%28System.Type%2CSystem.Reflection.FieldInfo%29>、<xref:System.Reflection.Emit.TypeBuilder> の各メソッド オーバーロードを使用して、メソッドの構築された形式を取得します。 メソッド本体を出力する手順にこの例が示されています。

11. メソッドを格納する型を完成させ、アセンブリを保存します。 ジェネリック メソッドを呼び出すための手順には、完成したメソッドを呼び出す 2 とおりの方法が示されています。

    [!code-csharp[GenericMethodHowTo#14](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#14)]
    [!code-vb[GenericMethodHowTo#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#14)]

<a name="procedureSection1"></a>

### <a name="to-emit-the-method-body"></a>メソッド本体を出力するには

1. コード ジェネレーターを取得し、ローカル変数とラベルを宣言します。 ローカル変数を宣言するには、<xref:System.Reflection.Emit.ILGenerator.DeclareLocal%2A> メソッドを使用します。 `Factory` メソッドには、4 つのローカル変数があります。メソッドによって返される新しい `retVal` を保持する `TOutput`、`ic` (C# では `TOutput`) にキャストされた `ICollection(Of TInput)` を保持する `ICollection<TInput>`、`input` オブジェクトの入力配列を保持する `TInput`、および配列を反復処理するための `index` です。 また、ラベルも 2 つあります。ループに入るためのラベル (`enterLoop`) およびループの先頭に使用するラベル (`loopAgain`) であり、<xref:System.Reflection.Emit.ILGenerator.DefineLabel%2A> メソッドを使用して定義されます。

    メソッドは、まず、<xref:System.Reflection.Emit.OpCodes.Ldarg_0> オペコードを使用して引数を読み込み、`input` オペコードを使用してローカル変数 <xref:System.Reflection.Emit.OpCodes.Stloc_S> に格納します。

    [!code-csharp[GenericMethodHowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#10)]
    [!code-vb[GenericMethodHowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#10)]

2. `TOutput` メソッドのジェネリック メソッド オーバーロードを使用して、<xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> のインスタンスを作成するコードを出力します。 このオーバーロードを使用する場合、指定した型はパラメーターなしのコンストラクターを持つ必要があります。`TOutput` にこの制約を追加したのはこのためです。 `TOutput` を <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> に渡して、構築されたジェネリック メソッドを作成します。 メソッドを呼び出すコードを出力したら、`retVal` を使用してローカル変数 <xref:System.Reflection.Emit.OpCodes.Stloc_S> に格納するコードを出力します。

    [!code-csharp[GenericMethodHowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#11)]
    [!code-vb[GenericMethodHowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#11)]

3. 新しい `TOutput` オブジェクトを `ICollection(Of TInput)` にキャストし、ローカル変数 `ic` に格納するコードを出力します。

    [!code-csharp[GenericMethodHowTo#31](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#31)]
    [!code-vb[GenericMethodHowTo#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#31)]

4. <xref:System.Reflection.MethodInfo> メソッドを表す <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> を取得します。 このメソッドは、`ICollection(Of TInput)` (C# では `ICollection<TInput>`) で動作するため、この構築された型に固有の `Add` メソッドを取得する必要があります。 <xref:System.Type.GetMethod%2A> は、<xref:System.Reflection.MethodInfo> で構築された型ではサポートされていないため、`icollOfTInput` メソッドを使用して、<xref:System.Type.GetMethod%2A> からこの <xref:System.Reflection.Emit.GenericTypeParameterBuilder> を直接取得することはできません。 代わりに、<xref:System.Type.GetMethod%2A> ジェネリック インターフェイスのジェネリック型の定義を格納する `icoll` で、<xref:System.Collections.Generic.ICollection%601> を呼び出します。 次に、<xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29>`static` メソッドを使用して、構築された型の <xref:System.Reflection.MethodInfo> を作成します。 次のコードでこれを示します。

    [!code-csharp[GenericMethodHowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#12)]
    [!code-vb[GenericMethodHowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#12)]

5. 32 ビット整数 0 を読み込み、変数に格納することにより、変数 `index` を初期化するコードを出力します。 `enterLoop` ラベルに分岐するコードを出力します。 このラベルはループ内部にあるため、まだマークされていません。 ループのコードは、次の手順で出力されます。

    [!code-csharp[GenericMethodHowTo#32](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#32)]
    [!code-vb[GenericMethodHowTo#32](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#32)]

6. ループのコードを出力します。 まず、<xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A> ラベルを使用して `loopAgain` を呼び出し、ループの先頭をマークします。 これで、このラベルを使用する分岐ステートメントは、コードのこのポイントに分岐するようになります。 次の手順では、`TOutput` にキャストされた `ICollection(Of TInput)` オブジェクトをスタックにプッシュします。 このオブジェクトはすぐに必要なわけではありませんが、`Add` メソッドを呼び出すための適切な位置にあることが必要です。 次に、入力配列がスタックにプッシュされ、現在のインデックスを格納する変数 `index` が配列にプッシュされます。 <xref:System.Reflection.Emit.OpCodes.Ldelem> オペコードによってインデックスと配列がスタックからポップされ、インデックスが付けられた配列要素がスタックにプッシュされます。 これで、スタックは <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> メソッドの呼び出しに対応できるようになりました。このメソッドは、スタックからコレクションと新しい要素をポップし、その要素をコレクションに追加します。

    ループのコードの残りの部分では、インデックスをインクリメントし、ループが終了したかどうかを確認するテストを行います。インデックスと 32 ビット整数 1 がスタックにプッシュされて加算され、その合計がスタックに残されます。合計は `index` に格納されます。 <xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A> を呼び出して、このポイントをループのエントリ ポイントとして設定します。 インデックスが再び読み込まれます。 入力配列がスタックにプッシュされ、その長さを取得するために <xref:System.Reflection.Emit.OpCodes.Ldlen> が出力されます。 現在、インデックスと長さはスタックにあります。<xref:System.Reflection.Emit.OpCodes.Clt> を出力してこれらを比較します。 インデックスが長さ未満の場合、<xref:System.Reflection.Emit.OpCodes.Brtrue_S> はループの先頭に分岐して戻ります。

    [!code-csharp[GenericMethodHowTo#13](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#13)]
    [!code-vb[GenericMethodHowTo#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#13)]

7. `TOutput` オブジェクトをスタックにプッシュし、メソッドから返すコードを出力します。 ローカル変数 `retVal` と `ic` には、新しい `TOutput` への参照が格納されます。`ic` は、<xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> メソッドにアクセスする目的でのみ使用されます。

    [!code-csharp[GenericMethodHowTo#33](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#33)]
    [!code-vb[GenericMethodHowTo#33](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#33)]

<a name="procedureSection2"></a>

### <a name="to-invoke-the-generic-method"></a>ジェネリック メソッドを呼び出すには

1. `Factory` は、ジェネリック メソッドの定義です。 これを呼び出すには、ジェネリック型パラメーターに型を割り当てる必要があります。 これを行うには、<xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> メソッドを使用します。 次のコードでは、<xref:System.String> に `TInput`、`List(Of String)` に `List<string>` (C# では `TOutput`) を指定して、構築されたジェネリック メソッドを作成し、メソッドの文字列形式を表示します。

    [!code-csharp[GenericMethodHowTo#21](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#21)]
    [!code-vb[GenericMethodHowTo#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#21)]

2. 遅延バインディングによってメソッドを呼び出すには、<xref:System.Reflection.MethodBase.Invoke%2A> メソッドを使用します。 次のコードでは、唯一の要素として文字列の配列を格納する <xref:System.Object> の配列を作成し、ジェネリック メソッドの引数リストとして渡します。 このメソッドは <xref:System.Reflection.MethodBase.Invoke%2A> であるため、`static` の 1 つ目のパラメーターは null 参照です。 戻り値は `List(Of String)` にキャストされ、最初の要素が表示されます。

    [!code-csharp[GenericMethodHowTo#22](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#22)]
    [!code-vb[GenericMethodHowTo#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#22)]

3. デリゲートを使用してメソッドを呼び出すには、構築されたジェネリック メソッドのシグネチャと一致するデリゲートを用意する必要があります。 汎用デリゲートを作成すると、これを簡単に行うことができます。 次のコードでは、`D` メソッド オーバーロードを使用して、プログラム例で定義した <xref:System.Delegate.CreateDelegate%28System.Type%2CSystem.Reflection.MethodInfo%29?displayProperty=nameWithType> 汎用デリゲートのインスタンスを作成し、デリゲートを呼び出します。 デリゲートは、遅延バインディングによる呼び出しよりもパフォーマンスが優れています。

    [!code-csharp[GenericMethodHowTo#23](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#23)]
    [!code-vb[GenericMethodHowTo#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#23)]

4. 出力されたメソッドは、保存されたアセンブリを参照するプログラムから呼び出すこともできます。

## <a name="example"></a>例

ジェネリック メソッド `DemoType` と共に、非ジェネリック型 `Factory` を作成するコード例を次に示します。 このメソッドは、入力の種類を指定する `TInput` と、出力の種類を指定する `TOutput` の 2 つのジェネリック型パラメーターを持ちます。 型パラメーター `TOutput` は、`ICollection<TInput>` (Visual Basic では `ICollection(Of TInput)`) を実装し、参照型であり、パラメーターなしのコンストラクターを持つように制約されています。

メソッドは、`TInput` の配列である仮パラメーターを 1 つ持ちます。 メソッドは、入力配列のすべての要素を格納する `TOutput` のインスタンスを返します。 `TOutput` は <xref:System.Collections.Generic.ICollection%601> ジェネリック インターフェイスを実装するジェネリック コレクション型に指定できます。

コードを実行すると、動的アセンブリが DemoGenericMethod1.dll という名前で保存され、[Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) を使用してチェックできます。

> [!NOTE]
> コードを出力する方法を習得するためのよい方法は、出力しようとしているタスクを実行するプログラムを Visual Basic、C#、または Visual C++ で記述し、逆アセンブラーを使用して、コンパイラによって生成された MSIL をチェックすることです。

このコード例には、出力されたメソッドに相当するソース コードが含まれています。 出力されたメソッドは、遅延バインディングによって呼び出されます。また、このコード例で宣言された汎用デリゲートを使用することによっても呼び出されます。

[!code-csharp[GenericMethodHowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#1)]
[!code-vb[GenericMethodHowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#1)]

## <a name="see-also"></a>関連項目

- <xref:System.Reflection.Emit.MethodBuilder>
- [方法: リフレクション出力を使用してジェネリック型を定義する](how-to-define-a-generic-type-with-reflection-emit.md)
