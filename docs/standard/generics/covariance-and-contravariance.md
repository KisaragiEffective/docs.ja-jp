---
title: ジェネリックの共変性と反変性
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generics, covariance and contravariance
- generics, variance
- covariance and contravariance in generics
- generic type parameters
ms.assetid: 2678dc63-c7f9-4590-9ddc-0a4df684d42e
ms.openlocfilehash: 909b03588d2a41f667bfa117a5cecb420b125088
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708398"
---
# <a name="covariance-and-contravariance-in-generics"></a>ジェネリックの共変性と反変性
共変性と反変性は、元の指定よりも強い派生型 (具体性が高い) と弱い派生型 (具体性が低い) を使用する能力を示す用語です。 ジェネリック型パラメーターは、ジェネリック型の代入と使用の柔軟性を向上させるために、共変性と反変性をサポートしています。 型システムにおいて、共変性、反変性、および不変性は、次のように定義されます。 各例では、基底クラスが `Base` という名前であり、派生クラスが `Derived`という名前であるとします。  
  
- `Covariance`  
  
     最初に指定された型よりも強い派生型を使用できるようにします。  
  
     `IEnumerable<Derived>` (Visual Basic では `IEnumerable(Of Derived)`) のインスタンスを `IEnumerable<Base>` 型の変数に割り当てることができます。  
  
- `Contravariance`  
  
     最初に指定された型よりも一般的な (弱い派生の) 型を使用できるようにします。  
  
     `Action<Base>` (Visual Basic では `Action(Of Base)`) のインスタンスを `Action<Derived>` 型の変数に割り当てることができます。  
  
- `Invariance`  
  
     最初に指定された型のみを使用できることを意味します。そのため、不変のジェネリック型パラメーターは、共変でも反変でもありません。  
  
     `List<Base>` (Visual Basic では `List(Of Base)`) のインスタンスを `List<Derived>` 型の変数に割り当てることはできず、その逆もできません。  
  
 共変の型パラメーターでは、次のコードで示されているように、通常の[ポリモーフィズム](../../csharp/programming-guide/classes-and-structs/polymorphism.md)と非常によく似た代入を行うことができます。  
  
 [!code-csharp[CoContraSimpleIEnum#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontrasimpleienum/cs/example.cs#1)]
 [!code-vb[CoContraSimpleIEnum#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontrasimpleienum/vb/example.vb#1)]  
  
 <xref:System.Collections.Generic.List%601> クラスは <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するため、 `List<Derived>` (Visual Basic では`List(Of Derived)` ) は `IEnumerable<Derived>`を実装します。 共変の型パラメーターが後の処理を行います。  
  
 一方、反変性は直感に反するように見えます。 次の例では、 `Action<Base>` 型 (Visual Basic では`Action(Of Base)` ) のデリゲートを作成し、次にそのデリゲートを `Action<Derived>`型の変数に代入します。  
  
 [!code-csharp[CoContraSimpleAction#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontrasimpleaction/cs/example.cs#1)]
 [!code-vb[CoContraSimpleAction#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontrasimpleaction/vb/example.vb#1)]  
  
 これは逆方向のように見えますが、コンパイルして実行できるタイプ セーフ コードです。 ラムダ式は代入先のデリゲートに一致するため、 `Base` 型のパラメーターを 1 つ受け取って戻り値を返さないメソッドを定義します。 `Action<Derived>` デリゲートの型パラメーター `T` は反変であるため、結果として得られたデリゲートは <xref:System.Action%601> 型の変数に代入できます。 `T` はパラメーター型を指定するため、コードはタイプ セーフです。 `Action<Base>` 型のデリゲートが `Action<Derived>`型のデリゲートであるかのように呼び出される場合、その引数は `Derived`型である必要があります。 メソッドのパラメーターは `Base`型であるため、この引数は、基になるメソッドに常に安全に渡すことができます。  
  
 一般に、共変の型パラメーターはデリゲートの戻り値の型として使用でき、反変の型パラメーターはパラメーター型として使用できます。 インターフェイスについては、共変の型パラメーターをインターフェイスのメソッドの戻り値の型として使用でき、反変の型パラメーターをインターフェイスのメソッドのパラメーター型として使用できます。  
  
 共変性と反変性は、"*変性*" と総称されます。 共変または反変としてマークされていないジェネリック型パラメーターは、 *不変*と呼ばれます。 共通言語ランタイムにおける変性について、簡潔な概要を示します。  
  
- .NET Framework 4 のバリアント型パラメーターは、ジェネリック インターフェイス型と汎用デリゲート型に制限されています。  
  
- ジェネリック インターフェイス型や汎用デリゲート型では、共変と反変の両方の型パラメーターを使用できます。  
  
- 変性が適用されるのは参照型のみです。バリアント型パラメーターに対して値型を指定すると、その型パラメーターが、結果の構築型で不変になります。  
  
- 変性は、デリゲートの組み合わせには適用されません。 つまり、 `Action<Derived>` 型と `Action<Base>` 型 (Visual Basic では`Action(Of Derived)` と `Action(Of Base)` ) の 2 つのデリゲートがある場合、結果はタイプ セーフになりますが、2 つ目のデリゲートに 1 つ目のデリゲートを組み合わせることはできません。 変性によって 2 つ目のデリゲートを `Action<Derived>`型の変数に代入できますが、デリゲートを組み合わせることができるのは、それらの型が完全に一致している場合だけです。

<a name="InterfaceCovariantTypeParameters"></a>
## <a name="generic-interfaces-with-covariant-type-parameters"></a>共変の型パラメーターを持つジェネリック インターフェイス  
 .NET Framework 4 以降には、共変の型パラメーターを持つジェネリック インターフェイスがいくつかあります (<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.Generic.IEnumerator%601>、<xref:System.Linq.IQueryable%601>、<xref:System.Linq.IGrouping%602> など)。 これらのインターフェイスのすべての型パラメーターは共変のみであるため、型パラメーターはメンバーの戻り値の型だけに使用されます。  
  
 共変の型パラメーターの例を以下に示します。 ここでは 2 つの型が定義されています。 `Base` には、 `PrintBases` (Visual Basic では `IEnumerable<Base>` ) を受け取って要素を出力する`IEnumerable(Of Base)` という静的メソッドがあります。 `Derived` は `Base` を継承します。 この例は、空の `List<Derived>` (Visual Basic では`List(Of Derived)` ) を作成し、その型を `PrintBases` に渡して、キャストすることなく、 `IEnumerable<Base>` 型の変数に代入できることを示しています。 <xref:System.Collections.Generic.List%601> は、共変の型パラメーターを 1 つ持つ <xref:System.Collections.Generic.IEnumerable%601>を実装します。 `IEnumerable<Derived>` のインスタンスを `IEnumerable<Base>`の代わりに使用できるのは、この共変の型パラメーターがあるためです。  
  
 [!code-csharp[CoContravarianceInClrGenericI#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravarianceinclrgenerici/cs/example.cs#1)]
 [!code-vb[CoContravarianceInClrGenericI#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravarianceinclrgenerici/vb/example.vb#1)]  
  
## <a name="generic-interfaces-with-contravariant-generic-type-parameters"></a>反変のジェネリック型パラメーターを持つジェネリック インターフェイス  
 .NET Framework 4 以降には、反変の型パラメーターを持つジェネリック インターフェイスがいくつかあります (<xref:System.Collections.Generic.IComparer%601>、<xref:System.IComparable%601>、<xref:System.Collections.Generic.IEqualityComparer%601> など)。 これらのインターフェイスの型パラメーターは反変のみであるため、これらの型パラメーターは、インターフェイスのメンバーのパラメーター型としてのみ使用されます。  
  
 反変の型パラメーターの例を以下に示します。 この例では、`MustInherit` プロパティを使用して抽象 (Visual Basic では `Shape` ) `Area` クラスを定義しています。 また、 `ShapeAreaComparer` (Visual Basic では `IComparer<Shape>` ) を実装する`IComparer(Of Shape)` クラスを定義しています。 <xref:System.Collections.Generic.IComparer%601.Compare%2A?displayProperty=nameWithType> メソッドの実装は `Area` プロパティの値に基づくため、 `ShapeAreaComparer` を使用して、領域で `Shape` オブジェクトを並べ替えることができます。  
  
 `Circle` クラスは `Shape` を継承し、 `Area`をオーバーライドします。 この例では、 <xref:System.Collections.Generic.SortedSet%601> (Visual Basic では `Circle` ) を受け取るコンストラクターを使用して、 `IComparer<Circle>` オブジェクトの`IComparer(Of Circle)` を作成します。 ただし、 `IComparer<Circle>`を渡す代わりに、 `ShapeAreaComparer` を実装する `IComparer<Shape>`オブジェクトを渡します。 この例では、`Shape`ジェネリック インターフェイスの型パラメーターは反変であるため、コードがより強い派生型 (`Circle`) の比較子を要求している場合に、より弱い派生型 ( <xref:System.Collections.Generic.IComparer%601> ) の比較子を渡すことができます。  
  
 新しい `Circle` オブジェクトを `SortedSet<Circle>`に追加すると、新しい要素が既存の要素と比較されるたびに `IComparer<Shape>.Compare` オブジェクトの`IComparer(Of Shape).Compare` メソッド (Visual Basic では `ShapeAreaComparer` メソッド) が呼び出されます。 このメソッドのパラメーターの型 (`Shape`) は、渡される型 (`Circle`) より弱い派生型なので、この呼び出しはタイプ セーフです。 反変性により、 `ShapeAreaComparer` で、単一の型のコレクションおよび `Shape`から派生した型の混合コレクションを並べ替えることができるようになります。  
  
 [!code-csharp[CoContravarianceInClrGenericI2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravarianceinclrgenerici2/cs/example.cs#1)]
 [!code-vb[CoContravarianceInClrGenericI2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravarianceinclrgenerici2/vb/example.vb#1)]  

## <a name="generic-delegates-with-variant-type-parameters"></a>バリアント型パラメーターを持つ汎用デリゲート  
 .NET Framework 4 には、<xref:System.Func%602> などの `Func` 汎用デリゲートに、共変の戻り値の型と反変のパラメーターの型があります。 `Action` などの <xref:System.Action%602>汎用デリゲートには、反変のパラメーターの型があります。 したがって、より強い派生型のパラメーターと、より弱い派生型の戻り値 ( `Func` 汎用デリゲートの場合) を持つ変数に、デリゲートを代入できます。  
  
> [!NOTE]
> `Func` 汎用デリゲートの最後のジェネリック型パラメーターは、デリゲート シグネチャの戻り値の型を指定します。 他のジェネリック型パラメーターは反変 (`out` キーワード) ですが、この最後のジェネリック型パラメーターは共変 (`in` キーワード) です。  
  
 次に例を示します。 コードの最初の部分では、 `Base`という名前のクラスと、 `Derived` を継承する `Base`という名前のクラスを定義しています。その他に、 `static` という名前の`Shared` (Visual Basic では `MyMethod`) メソッドを持つクラスも定義されています。 このメソッドは、`Base` のインスタンスを受け取り、`Derived` のインスタンスを返します (引数が `Derived` のインスタンスの場合は、それが `MyMethod` によって返されます。引数が `Base` のインスタンスの場合は、`MyMethod` によって `Derived` の新しいインスタンスが返されます)。`Main()` では、`Func<Base, Derived>` を表す `Func(Of Base, Derived)` (Visual Basic では `MyMethod`) のインスタンスを作成して、変数 `f1` に格納しています。  
  
 [!code-csharp[CoContravarianceDelegates#2](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#2)]
 [!code-vb[CoContravarianceDelegates#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#2)]  
  
 コードの 2 番目の部分は、このデリゲートを `Func<Base, Base>` (Visual Basic では`Func(Of Base, Base)` ) 型の変数に代入できることを示しています。これは、戻り値の型が共変であるためです。  
  
 [!code-csharp[CoContravarianceDelegates#3](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#3)]
 [!code-vb[CoContravarianceDelegates#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#3)]  
  
 コードの 3 番目の部分は、このデリゲートを `Func<Derived, Derived>` (Visual Basic では`Func(Of Derived, Derived)` ) 型の変数に代入できることを示しています。これは、パラメーターの型が反変であるためです。  
  
 [!code-csharp[CoContravarianceDelegates#4](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#4)]
 [!code-vb[CoContravarianceDelegates#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#4)]  
  
 コードの最後の部分は、このデリゲートを `Func<Derived, Base>` (Visual Basic では`Func(Of Derived, Base)` ) 型の変数に代入できることを示しています。これは、反変のパラメーターの型と共変の戻り値の型の両方の効果の組み合わせによるものです。  
  
 [!code-csharp[CoContravarianceDelegates#5](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#5)]
 [!code-vb[CoContravarianceDelegates#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#5)]  
  
### <a name="variance-in-generic-and-non-generic-delegates"></a>汎用デリゲートと非汎用デリゲートの変性  
 上のコードでは、 `MyMethod` のシグネチャが、構築された汎用デリゲート `Func<Base, Derived>` (Visual Basic では`Func(Of Base, Derived)` ) のシグネチャと厳密に一致しています。 この例から、より強い派生型のパラメーターとより弱い派生型の戻り値を持つ変数やメソッド パラメーターにこの汎用デリゲートを格納できることと、そのためには、すべてのデリゲート型が汎用デリゲート型 <xref:System.Func%602>から構築されている必要があることがわかります。  
  
 これは重要なポイントです。 汎用デリゲートの型パラメーターにおける共変性と反変性の効果は、通常のデリゲート バインディングにおける共変性と反変性の効果 (「[デリゲートの変性 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」および「[デリゲートの変性 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」を参照) に似ていますが、 デリゲート バインディングの変性は、バリアント型パラメーターを持つ汎用デリゲート型だけでなく、すべてのデリゲート型で使用できます。 さらに、デリゲート バインディングの変性では、より限定的なパラメーターの型とより限定的でない戻り値の型を持つ任意のデリゲートにメソッドをバインドできますが、汎用デリゲートの代入を使用できるのは、両方のデリゲート型が同じジェネリック型定義から構築されている場合のみです。  
  
 デリゲート バインディングの変性とジェネリック型パラメーターの変性の両方の効果を組み合わせた例を以下に示します。 ここでは、3 つの型を含む型階層を定義しています。`Type1`が最も弱い派生型で、`Type3`が最も強い派生型です。 通常のデリゲート バインディングの変性を使用して、パラメーターの型が `Type1` で戻り値の型が `Type3` のメソッドを、パラメーターの型が `Type2` で戻り値の型が `Type2`の汎用デリゲートにバインドしています。 その結果、得られた汎用デリゲートを、ジェネリック型パラメーターの共変性と反変性を使用して、 `Type3` 型のパラメーターと `Type1`型の戻り値を持つ汎用デリゲート型の変数に代入しています。 2 回目の代入では、変数型とデリゲート型の両方が同じジェネリック型定義 (この場合は <xref:System.Func%602>) から構築されている必要があります。  
  
 [!code-csharp[CoContravarianceDelegatesGenRelaxed#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegatesgenrelaxed/cs/example.cs#1)]
 [!code-vb[CoContravarianceDelegatesGenRelaxed#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegatesgenrelaxed/vb/example.vb#1)]  

## <a name="defining-variant-generic-interfaces-and-delegates"></a>バリアント ジェネリック インターフェイスとバリアント汎用デリゲートの定義
 .NET Framework 4 以降では、Visual Basic と C# の両方で、インターフェイスやデリゲートのジェネリック型パラメーターを共変または反変としてマークするためのキーワードが提供されています。  
  
> [!NOTE]
> ジェネリック型パラメーターの分散注釈は .NET Framework Version 2.0 以降の共通言語ランタイムでサポートされていますが、 .NET Framework 4 より前で、これらの注釈を含むジェネリック クラスを定義するには、クラスを [Ilasm.exe (IL アセンブラー)](../../../docs/framework/tools/ilasm-exe-il-assembler.md) を使ってコンパイル、またはそれを動的アセンブリに出力することで、Microsoft Intermediate Language (MSIL) を使用する方法しかありませんでした。  
  
 共変の型パラメーターをマークするには、 `out` キーワード (Visual Basic では`Out` キーワード、 `+` MSIL アセンブラー [では](../../../docs/framework/tools/ilasm-exe-il-assembler.md)) を使用します。 共変の型パラメーターは、インターフェイスに属するメソッドの戻り値として使用したり、デリゲートの戻り値の型として使用したりできます。 インターフェイス メソッドのジェネリック型制約として使用することはできません。  
  
> [!NOTE]
> インターフェイスのメソッドに汎用デリゲート型のパラメーターがある場合は、インターフェイス型の共変の型パラメーターを使用してデリゲート型の反変の型パラメーターを指定できます。  
  
 反変の型パラメーターをマークするには、 `in` キーワード (Visual Basic では`In` キーワード、 `-` MSIL アセンブラー [では](../../../docs/framework/tools/ilasm-exe-il-assembler.md)) を使用します。 反変の型パラメーターは、インターフェイスに属するメソッドのパラメーターの型として使用したり、デリゲートのパラメーターの型として使用したりできます。 インターフェイス メソッドのジェネリック型制約として使用することもできます。  
  
 バリアント型パラメーターを持つことができるのは、インターフェイス型とデリゲート型だけです。 インターフェイス型やデリゲート型は、共変と反変の両方の型パラメーターを持つことができます。  
  
 Visual Basic と C# では、共変および反変の型パラメーターの使用規則に違反したり、インターフェイスとデリゲート以外の型の型パラメーターに共変性や反変性の注釈を追加したりすることは許可されません。 [MSIL アセンブラー](../../../docs/framework/tools/ilasm-exe-il-assembler.md) ではそのようなチェックは行われませんが、規則に違反する型を読み込もうとすると <xref:System.TypeLoadException> がスローされます。  
  
 詳細およびコード例については、「[ジェネリック インターフェイスの変性 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)」および「[ジェネリック インターフェイスの変性 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)」を参照してください。  

## <a name="list-of-variant-generic-interface-and-delegate-types"></a>バリアント ジェネリック インターフェイス型とバリアント汎用デリゲート型の一覧
 .NET Framework 4 では、次のインターフェイス型およびデリゲート型に、共変または反変、またはその両方の型パラメーターが含まれます。  
  
|種類|共変の型パラメーター|反変の型パラメーター|  
|----------|-------------------------------|-----------------------------------|  
|<xref:System.Action%601> ～ <xref:System.Action%6016>||はい|  
|<xref:System.Comparison%601>||はい|  
|<xref:System.Converter%602>|はい|はい|  
|<xref:System.Func%601>|はい||  
|<xref:System.Func%602> ～ <xref:System.Func%6017>|はい|はい|  
|<xref:System.IComparable%601>||はい|  
|<xref:System.Predicate%601>||はい|  
|<xref:System.Collections.Generic.IComparer%601>||はい|  
|<xref:System.Collections.Generic.IEnumerable%601>|はい||  
|<xref:System.Collections.Generic.IEnumerator%601>|はい||  
|<xref:System.Collections.Generic.IEqualityComparer%601>||はい|  
|<xref:System.Linq.IGrouping%602>|はい||  
|<xref:System.Linq.IOrderedEnumerable%601>|はい||  
|<xref:System.Linq.IOrderedQueryable%601>|はい||  
|<xref:System.Linq.IQueryable%601>|はい||  
  
## <a name="see-also"></a>関連項目

- [共変性と反変性 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/index.md)
- [共変性と反変性 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/index.md)
- [デリゲートの変性 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
- [デリゲートの変性 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
