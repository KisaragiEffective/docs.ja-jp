---
title: .NET のジェネリック
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generic methods, type inference
- generics [.NET Framework], collections
- generic interfaces [.NET Framework]
- constructed generic types
- nested generic types
- generic type definitions
- generic classes [.NET Framework]
- generics [.NET Framework], interfaces
- generics [.NET Framework], about
- generics [.NET Framework]
- generic collections [.NET Framework]
- generic delegates [.NET Framework]
- generic type arguments
- generics [.NET Framework], delegates
- generics [.NET Framework], features
- constraints [.NET Framework]
- generic types
- generic type parameters
ms.assetid: 2994d786-c5c7-4666-ab23-4c83129fe39c
ms.openlocfilehash: 7f20e5108ad8bff602f5b761e65f093d987f2608
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156310"
---
# <a name="generics-in-net"></a>.NET のジェネリック

ジェネリックを使用すると、操作対象のデータ型に厳密に合わせてメソッド、クラス、構造体、またはインターフェイスを調整できます。 たとえば、任意の型のキーと値が許可される <xref:System.Collections.Hashtable> クラスを使用する代わりに、 <xref:System.Collections.Generic.Dictionary%602> ジェネリック クラスを使用して、キーに使用できる型と、値に使用できる型を指定できます。 ジェネリックの利点として、コードの再利用性やタイプ セーフの向上などを挙げることができます。  

## <a name="defining-and-using-generics"></a>ジェネリックの定義と使用
 ジェネリックは、格納または使用される 1 つ以上の型のプレースホルダー (型パラメーター) を持つクラス、構造体、インターフェイス、およびメソッドです。 ジェネリック コレクション クラスでは、格納するオブジェクトの型のプレースホルダーとして、型パラメーターを使用することがあります。型パラメーターは、そのフィールドの型やそのメソッドのパラメーター型として出現します。 ジェネリック メソッドでは、その戻り値の型として、またはいずれかの仮パラメーターの型として、型パラメーターを使用します。 単純なジェネリック クラスの定義を次のコードに示します。  
  
 [!code-cpp[Conceptual.Generics.Overview#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#2)]
 [!code-csharp[Conceptual.Generics.Overview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#2)]
 [!code-vb[Conceptual.Generics.Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#2)]  
  
 ジェネリック クラスのインスタンスを作成する場合は、型パラメーターを置き換える実際の型を指定します。 これで、構築されたジェネリック クラスと呼ばれる新しいジェネリック クラスが確立され、この中では、型パラメーターが、出現するすべての場所で、選択した型に置き換えられています。 次のコードに示すように、結果は選択した型に合わせたタイプ セーフなクラスになります。  
  
 [!code-cpp[Conceptual.Generics.Overview#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#3)]
 [!code-csharp[Conceptual.Generics.Overview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#3)]
 [!code-vb[Conceptual.Generics.Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#3)]  

### <a name="generics-terminology"></a>ジェネリックの用語  
 .NET におけるジェネリックの説明では、次の用語が使用されます。  
  
- *ジェネリック型定義* は、テンプレートとして機能するクラス、構造体、またはインターフェイスの宣言で、格納または使用できる型のプレースホルダーを含みます。 たとえば、 <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> クラスには、キーと値の 2 つの型を含めることができます。 ジェネリック型定義は単なるテンプレートであるため、ジェネリック型定義のクラス、構造体、またはインターフェイスのインスタンスを作成することはできません。  
  
- *ジェネリック型パラメーター*(または *型パラメーター*) は、ジェネリック型定義またはジェネリック メソッド定義のプレースホルダーです。 <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> ジェネリック型には、そのキーと値の型を表す 2 つの型パラメーター `TKey` と `TValue`があります。  
  
- *構築ジェネリック型*(または *構築型*) は、ジェネリック型定義のジェネリック型パラメーターに型を指定することによって得られる結果です。  
  
- *ジェネリック型引数* は、ジェネリック型パラメーターを置き換える任意の型です。  
  
- 一般的な用語である *ジェネリック型* には、構築型とジェネリック型定義の両方が含まれます。  
  
- ジェネリック型パラメーターの*共変性*と*反変性*を使用すると、型引数がターゲットの構築型よりも強い派生型 (共変性) または弱い派生型 (反変性) である構築ジェネリック型を使用できます。 共変性と反変性は、"*変性*" と総称されます。 詳細については、「[共変性と反変性](../../../docs/standard/generics/covariance-and-contravariance.md)」を参照してください。  
  
- *制約* は、ジェネリック型パラメーターに適用される制限です。 たとえば、型パラメーターを、 <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> ジェネリック インターフェイスを実装する型に制限して、型のインスタンスを並べ替えることができるようにできます。 また、型パラメーターを、特定の基底クラスを持つ型、パラメーターなしのコンストラクターを持つ型、または参照型や値型に制約することもできます。 ジェネリック型のユーザーは、制約を満たさない型引数に置き換えることはできません。  
  
- *ジェネリック メソッド定義* は、ジェネリック型パラメーターのリストと仮パラメーターのリストの 2 つのパラメーター リストを持つメソッドです。 次のコードに示すように、型パラメーターは、戻り値の型または仮パラメーターの型として指定できます。  
  
 [!code-cpp[Conceptual.Generics.Overview#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#4)]
 [!code-csharp[Conceptual.Generics.Overview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#4)]
 [!code-vb[Conceptual.Generics.Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#4)]  
  
 ジェネリック メソッドはジェネリック型または非ジェネリック型で指定できます。 メソッドがジェネリック型に属している、またはメソッドに仮パラメーターがあり、その型が外側の型のジェネリック パラメーターであるという理由だけでは、そのメソッドがジェネリックであるとは言えないことに注意してください。 メソッドは、独自の型パラメーター リストを持つ場合にのみジェネリックとなります。 次のコードでは、 `G` メソッドのみがジェネリックです。  
  
 [!code-cpp[Conceptual.Generics.Overview#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#5)]
 [!code-csharp[Conceptual.Generics.Overview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#5)]
 [!code-vb[Conceptual.Generics.Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#5)]  
  
## <a name="advantages-and-disadvantages-of-generics"></a>ジェネリックの利点と欠点
 ジェネリック コレクションやジェネリック デリゲートを使用することには多くの利点があります。  
  
- インライン関数はタイプ セーフです。 ジェネリックによって、タイプ セーフの負担がユーザーからコンパイラに移ります。 コンパイル時に正しいデータ型が適用されるため、これをテストするためのコードを記述する必要はありません。 型キャストの必要性や、ランタイム エラーが発生する可能性が減少します。  
  
- コードは少なければ少ないほど再利用が容易になります。 基本型から継承してメンバーをオーバーライドする必要はありません。 たとえば、 <xref:System.Collections.Generic.LinkedList%601> はすぐに使える状態になっています。 たとえば、次の変数宣言で文字列のリンク リストを作成できます。  
  
     [!code-cpp[HowToGeneric#24](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/source2.cpp#24)]
     [!code-csharp[HowToGeneric#24](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/source2.cs#24)]
     [!code-vb[HowToGeneric#24](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/source2.vb#24)]  
  
- パフォーマンスが向上します。 通常、ジェネリック コレクション型では、値型をボックス化する必要がないため、値型の格納と操作のパフォーマンスが向上します。  
  
- 汎用デリゲートによって、複数のデリゲート クラスを作成せずにタイプ セーフなコールバックを使用できます。 たとえば、 <xref:System.Predicate%601> 汎用デリゲートを使用すると、特定の型を対象とした独自の検索条件を実装するメソッドを作成し、 <xref:System.Array> 、 <xref:System.Array.Find%2A>、 <xref:System.Array.FindLast%2A>などの <xref:System.Array.FindAll%2A>型のメソッドと共に自分のメソッドを使用することができます。  
  
- ジェネリックによって、動的に生成されるコードが簡略化されます。 動的に生成されるコードでジェネリックを使用する場合、型を生成する必要がありません。 これにより、アセンブリ全体を生成する代わりに軽量の動的メソッドを使用できるシナリオの数が増えます。 詳細については、「[How to: Define and Execute Dynamic Methods](../../../docs/framework/reflection-and-codedom/how-to-define-and-execute-dynamic-methods.md)」 (方法: 動的メソッドを定義および実行する) および 「<xref:System.Reflection.Emit.DynamicMethod>」 を参照してください。  
  
 ジェネリックの制限事項を次に示します。  
  
- ジェネリック型は、 <xref:System.MarshalByRefObject> などのほとんどの基本クラスから派生できます (制約を使用して、ジェネリック型パラメーターが <xref:System.MarshalByRefObject>のような基本クラスから派生することを要求できます)。 ただし、.NET Framework では、コンテキスト バインドのジェネリック型はサポートしていません。 ジェネリック型は、 <xref:System.ContextBoundObject>から派生できますが、その型のインスタンスを作成しようとすると、 <xref:System.TypeLoadException>が発生します。  
  
- 列挙型にジェネリック型パラメーターを含めることはできません。 列挙型が、単なる偶然によってジェネリックのみになる可能性はあります (たとえば、Visual Basic、C#、または C++ を使用して定義されたジェネリック型に入れ子にされているため)。 詳細については、[「Common Type System」](../../../docs/standard/base-types/common-type-system.md) の「列挙型」を参照してください。  
  
- 軽量の動的メソッドをジェネリックにすることはできません。  
  
- Visual Basic、C#、および C++ で、ジェネリック型に囲まれている入れ子にされた型は、外側のすべての型の型パラメーターに型が割り当てられていない限り、インスタンス化できません。 言い換えると、リフレクションでは、これらの言語を使用して定義されている入れ子にされた型には、その外側のすべての型の型パラメーターが含まれます。 これによって、外側の型の型パラメーターを、入れ子にされた型のメンバー定義で使用できます。 詳細については、「 <xref:System.Type.MakeGenericType%2A>」の「入れ子にされた型」を参照してください。  
  
    > [!NOTE]
    > 動的アセンブリでコードを出力するか [Ilasm.exe (IL アセンブラー)](../../../docs/framework/tools/ilasm-exe-il-assembler.md) を使用して定義されている入れ子にされた型に、外側の型の型パラメーターを含める必要はありません。ただし、これらを含んでいない場合、型パラメーターは入れ子にされたクラスのスコープから外れます。  
  
     詳細については、「 <xref:System.Type.MakeGenericType%2A>」の「入れ子にされた型」を参照してください。  

## <a name="class-library-and-language-support"></a>クラス ライブラリと言語サポート  
 .NET では、次の名前空間に多数のジェネリック コレクション クラスが用意されています。  
  
- <xref:System.Collections.Generic> 名前空間には、<xref:System.Collections.Generic.List%601> ジェネリック クラスや <xref:System.Collections.Generic.Dictionary%602> ジェネリック クラスなど、.NET で用意されているほとんどのジェネリック コレクション型が含まれています。  
  
- <xref:System.Collections.ObjectModel> 名前空間には、<xref:System.Collections.ObjectModel.ReadOnlyCollection%601> ジェネリック クラスなど、クラスのユーザーにオブジェクト モデルを公開するために役立つ追加のジェネリック コレクション型が含まれています。  
  
 並べ替えと等価比較を実装するためのジェネリック インターフェイスは、イベント ハンドラー、変換、および検索述語の汎用デリゲート型と共に、 <xref:System> 名前空間に用意されています。  
  
 ジェネリックのサポートは、ジェネリック型とジェネリック メソッドを調べるために <xref:System.Reflection> 名前空間に追加され、ジェネリック型とジェネリック メソッドを含む動的アセンブリを出力するために <xref:System.Reflection.Emit> に追加され、さらにジェネリックを含むソース グラフを生成するために <xref:System.CodeDom> に追加されています。  
  
 共通言語ランタイムでは、Microsoft Intermediate Language (MSIL) のジェネリック型をサポートするために、新しいオペコードとプレフィックスを提供しています ( <xref:System.Reflection.Emit.OpCodes.Stelem>、 <xref:System.Reflection.Emit.OpCodes.Ldelem>、 <xref:System.Reflection.Emit.OpCodes.Unbox_Any>、 <xref:System.Reflection.Emit.OpCodes.Constrained>、 <xref:System.Reflection.Emit.OpCodes.Readonly>など)。  
  
 Visual C++、C#、および Visual Basic のすべてで、ジェネリックの定義と使用が完全にサポートされています。 言語サポートの詳細については、「[Visual Basic におけるジェネリック型](../../visual-basic/programming-guide/language-features/data-types/generic-types.md)」、「[ジェネリックの概要](../../csharp/programming-guide/generics/index.md)」、および「[Overview of Generics in Visual C++](/cpp/windows/overview-of-generics-in-visual-cpp)」 (Visual C++ のジェネリックの概要) を参照してください。

## <a name="nested-types-and-generics"></a>入れ子にされた型とジェネリック  
 ジェネリック型に入れ子にされている型は、外側のジェネリック型の型パラメーターに依存している可能性があります。 共通言語ランタイムでは、独自のジェネリック型パラメーターがない場合でも、入れ子にされた型をジェネリックと見なします。 入れ子にされた型のインスタンスを作成するときに、外側のすべてのジェネリック型の型引数を指定する必要があります。  

## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[ .NET の汎用コレクション](../../../docs/standard/generics/collections.md)|.NET のジェネリック コレクション クラスとその他のジェネリック型について説明します。|  
|[配列とリストの操作に使用する汎用デリゲート](../../../docs/standard/generics/delegates-for-manipulating-arrays-and-lists.md)|配列またはコレクションの要素に対して実行される変換、検索述語、およびアクションの汎用デリゲートについて説明します。|  
|[ジェネリック インターフェイス](../../../docs/standard/generics/interfaces.md)|ジェネリック型のファミリ間に共通する機能を提供するジェネリック インターフェイスについて説明します。|  
|[共変性と反変性](../../../docs/standard/generics/covariance-and-contravariance.md)|ジェネリック型パラメーターの共変性と反変性について説明します。|  
|[ 一般的に使用されるコレクション型](../../../docs/standard/collections/commonly-used-collection-types.md)|ジェネリック型など、.NET のコレクション型の特性と使用シナリオの概要について説明します。|  
|[ジェネリック コレクションの使用に適した状況](../../../docs/standard/collections/when-to-use-generic-collections.md)|ジェネリック コレクション型を使用するケースを決定するための一般的な規則について説明します。|  
|[方法: リフレクション出力を使用してジェネリック型を定義する](../../../docs/framework/reflection-and-codedom/how-to-define-a-generic-type-with-reflection-emit.md)|ジェネリック型とジェネリック メソッドを含む動的アセンブリの生成方法について説明します。|  
|[Generic Types in Visual Basic](../../visual-basic/programming-guide/language-features/data-types/generic-types.md)|ジェネリック型の使用方法や定義方法に関するトピックなど、Visual Basic ユーザー向けにジェネリック機能について説明します。|  
|[ジェネリックの概要](../../csharp/programming-guide/generics/index.md)|C# ユーザー向けにジェネリック型の定義方法や使用方法の概要について説明します。|  
|[Visual C++ のジェネリックの概要](/cpp/windows/overview-of-generics-in-visual-cpp)|ジェネリックとテンプレートの違いなど、C++ ユーザー向けにジェネリック機能について説明します。|  

## <a name="reference"></a>リファレンス  
 <xref:System.Collections.Generic>  
  
 <xref:System.Collections.ObjectModel>  
  
 <xref:System.Reflection.Emit.OpCodes?displayProperty=nameWithType>  
