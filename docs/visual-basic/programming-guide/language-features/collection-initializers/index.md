---
title: コレクション初期化子
ms.date: 07/20/2015
f1_keywords:
- vb.CollectionInitializer
helpviewer_keywords:
- collection initializers [Visual Basic]
ms.assetid: a9290329-77b0-4fdf-ae75-8fc17287f469
ms.openlocfilehash: fbdd116298c530ae54677631eff7dac2f22c0fe2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346782"
---
# <a name="collection-initializers-visual-basic"></a>コレクション初期化子 (Visual Basic)

*コレクション初期化子*とは、コレクションを作成して一連の初期値を設定できる、短い構文です。 コレクション初期化子は、コレクションを既知の値のセットから作成する場合に便利です。値のセットの例として、メニュー オプションやカテゴリのリスト、数値の初期セット、曜日や月の名前の静的文字列のリスト、検証に使用する州のリストなどの地理的な場所が挙げられます。

コレクションの詳細については、「[コレクション](../../../../visual-basic/programming-guide/concepts/collections.md)」を参照してください。

コレクション初期化子は、`From` キーワードの後に中かっこ (`{}`) を使用して指定できます。 これは、「[配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)」で説明する配列リテラルの構文と似ています。 次の例では、コレクション初期化子を使用してコレクションを作成するさまざまな方法を示します。

[!code-vb[VbVbalrCollectionInitializers#1](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#1)]

> [!NOTE]
> C# にもコレクション初期化子はあります。 C# のコレクション初期化子には、Visual Basic のコレクション初期化子と同じ機能があります。 C# のコレクション初期化子の詳細については、「[オブジェクト初期化子とコレクション初期化子](../../../../csharp/programming-guide/classes-and-structs/object-and-collection-initializers.md)」を参照してください。

## <a name="syntax"></a>構文

コレクション初期化子は、次のコードのように、先頭に `{}` キーワードが付いた、中かっこ (`From`) で囲まれたコンマ区切りの値の一覧で構成されます。

[!code-vb[VbVbalrCollectionInitializers#2](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#2)]

<xref:System.Collections.Generic.List%601> や <xref:System.Collections.Generic.Dictionary%602> などのコレクションを作成する場合、次のコードに示すように、コレクション初期化子の前に、コレクションの種類を指定する必要があります。

[!code-vb[VbVbalrCollectionInitializers#13](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#13)]

> [!NOTE]
> コレクション初期化子とオブジェクト初期化子を組み合わせて、同じコレクション オブジェクトを初期化することはできません。 コレクション初期化子内のオブジェクトを初期化するには、オブジェクト初期化子を使用します。

## <a name="creating-a-collection-by-using-a-collection-initializer"></a>コレクション初期化子を使用したコレクションの作成

コレクション初期化子を使用してコレクションを作成する場合、コレクション初期化子内の各値は、コレクション内の適切な `Add` メソッドに渡されます。 たとえば、コレクション初期化子を使用して <xref:System.Collections.Generic.List%601> 作成する場合、コレクション初期化子内の各文字列値は <xref:System.Collections.Generic.List%601.Add%2A> メソッドに渡されます。 コレクション初期化子を使用してコレクションを作成する場合は、指定した型は有効なコレクション型である必要があります。 有効なコレクションの型には、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するクラスや、<xref:System.Collections.CollectionBase> クラスを継承するクラスなどがあります。 指定した型には、次の条件を満たす `Add` メソッドも公開されている必要があります。

- `Add` メソッドは、コレクション初期化子の呼び出し元の範囲から使用できる必要があります。 そのコレクションのパブリックでないメソッドにアクセスできるシナリオでコレクション初期化子を使用する場合は、`Add` メソッドは、パブリックである必要はありません。

- `Add` メソッドはインスタンス メンバー、またはコレクション クラスの `Shared` メンバーまたは拡張メソッドである必要があります。

- オーバーロード解決規則に基づいて、コレクション初期化子で提供される型に対応付けることができる `Add` メソッドが存在する必要があります。

 たとえば、コレクション初期化子を使用して、`List(Of Customer)` コレクションを作成する例を次に示します。 このコードを実行すると、`Customer` の各オブジェクトは汎用リストの `Add(Customer)` メソッドに渡されます。

[!code-vb[VbVbalrCollectionInitializers#9](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#9)]

次では、コレクション初期化子を使用しない同等のコード例を示します。

[!code-vb[VbVbalrCollectionInitializers#10](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#10)]

`Add` オブジェクトのコンストラクターと一致するパラメーターを持つ `Customer` メソッドがコレクションにある場合、次のセクションで説明するように、コレクション初期化子内に `Add` メソッドのパラメーター値を入れ子にすることができます。 コレクションにこのような `Add` メソッドがない場合、それを拡張メソッドとして作成できます。 コレクションの拡張メソッドとして `Add` メソッドを作成する方法の例については、「[方法: コレクション初期化子で使用される拡張メソッドを作成または追加する](../../../../visual-basic/programming-guide/language-features/collection-initializers/how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)」を参照してください。 コレクション初期化子で使用できるカスタム コレクションを作成する方法の例については、「[方法: コレクション初期化子を使用してコレクションを作成する](../../../../visual-basic/programming-guide/language-features/collection-initializers/how-to-create-a-collection-used-by-a-collection-initializer.md)」を参照してください。

## <a name="nesting-collection-initializers"></a>コレクション初期化子を入れ子にする

作成するコレクションの `Add` メソッドで固有のオーバーロードを指定するため、コレクション初期化子内の値を入れ子にすることができます。 `Add` メソッドに渡す値は、配列リテラルまたはコレクション初期化子の場合と同じように、コンマで区切り、中かっこ (`{}`) で囲む必要があります。

入れ子にした値でコレクションを作成する場合、入れ子になった値一覧の各要素は、要素の型に一致する `Add` メソッドに引数として渡されます。 たとえば、次のコード例では、キーは <xref:System.Collections.Generic.Dictionary%602> 型で値は `Integer` 型の `String` が作成されます。 一連の入れ子になった値は、それぞれ <xref:System.Collections.Generic.Dictionary%602.Add%2A> の `Dictionary` メソッドと照合されます。

[!code-vb[VbVbalrCollectionInitializers#5](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#5)]

前のコード例は、次のコードと同じです。

[!code-vb[VbVbalrCollectionInitializers#6](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#6)]

入れ子の最初のレベルの一連の入れ子になった値のみが、それぞれそのコレクション型の `Add` メソッドに送信されます。 より深いレベルの入れ子は配列リテラルとして扱われ、入れ子になった一連の値はどのコレクションの `Add` メソッドとも照合されません。

## <a name="related-topics"></a>関連トピック

|タイトル|説明|
|---|---|
|[方法: コレクション初期化子で使用される拡張メソッドを作成または追加する](../../../../visual-basic/programming-guide/language-features/collection-initializers/how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)|コレクション初期化子の値をコレクションに入力するために使用できる `Add` と呼ばれる拡張メソッドを作成する方法を示します。|
|[方法: コレクション初期化子を使用してコレクションを作成する](../../../../visual-basic/programming-guide/language-features/collection-initializers/how-to-create-a-collection-used-by-a-collection-initializer.md)|`Add` を実装するコレクション クラスに `IEnumerable` メソッドを含めてコレクション初期化子を使用できるようにする方法を説明します。|

## <a name="see-also"></a>参照

- [コレクション](../../../../visual-basic/programming-guide/concepts/collections.md)
- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md)
- [自動実装プロパティ](../../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)
- [方法: Visual Basic で配列変数を初期化する](../../../../visual-basic/programming-guide/language-features/arrays/how-to-initialize-an-array-variable.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [方法: 項目のリストを作成する](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)
