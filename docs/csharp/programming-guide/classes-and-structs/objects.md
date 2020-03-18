---
title: オブジェクト - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- objects [C#], about objects
- variables [C#]
ms.assetid: af4a5230-fbf3-4eea-95e1-8b883c2f845c
ms.openlocfilehash: 09b290713f3bc2a7a7824bb19c98138943ad5b2c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77673382"
---
# <a name="objects-c-programming-guide"></a>オブジェクト (C# プログラミング ガイド)
クラスまたは構造体の定義は、型の動作を指定する設計図に似ています。 オブジェクトは基本的に、設計図に従って割り当てられて構成されたメモリのブロックです。 プログラムでは、同じクラスのオブジェクトを多数作成できます。 オブジェクトはインスタンスとも呼ばれ、名前付きの変数または配列やコレクションに格納できます。 クライアント コードとは、これらの変数を使ってメソッドを呼び出し、オブジェクトのパブリック プロパティにアクセスするコードです。 C# などのオブジェクト指向言語では、一般的なプログラムは動的に対話する複数のオブジェクトで構成されています。  
  
> [!NOTE]
> 静的な型の動作方法は、ここで説明する動作方法とは異なります。 詳細については、「[静的クラスと静的クラス メンバー](./static-classes-and-static-class-members.md)」を参照してください。
  
## <a name="struct-instances-vs-class-instances"></a>構造体インスタンスとクラス インスタンス  
 クラスは参照型であるため、クラスのオブジェクトの変数は、マネージド ヒープ上のオブジェクトのアドレスへの参照を保持します。 同じ型の 2 番目のオブジェクトが最初のオブジェクトに割り当てられた場合、両方の変数がそのアドレスにあるオブジェクトを参照します。 この点については、後で詳しく説明します。  
  
 クラスのインスタンスは、[new 演算子](../../language-reference/operators/new-operator.md)を使って作成されます。 次の例では、`Person` が型で、`person1` と `person 2` がその型のインスタンスつまりオブジェクトです。  
  
 [!code-csharp[csProgGuideStatements#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#30)]  
  
 構造体は値型であるため、構造体オブジェクトの変数はオブジェクト全体のコピーを保持します。 構造体のインスタンスも `new` 演算子を使って作成できますが、次の例で示すように、これは必要ではありません。  
  
 [!code-csharp[csProgGuideStatements#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#31)]  
  
 `p1` と `p2` のメモリはどちらも、スレッドのスタックに割り当てられます。 そのメモリは、それが宣言されている型またはメソッドと共に解放されます。 これは、割り当て時に構造体がコピーされる理由の 1 です。 これに対し、クラスのインスタンスに割り当てられたメモリは、そのオブジェクトに対するすべての参照がスコープ外になると、共通言語ランタイムによって自動的に解放 (ガベージ コレクション) されます。 C++ のようにクラスのオブジェクトを確定的に破棄することはできません。 .NET Framework のガベージ コレクションについて詳しくは、「[ガベージ コレクション](../../../standard/garbage-collection/index.md)」をご覧ください。  
  
> [!NOTE]
> マネージド ヒープ上のメモリの割り当てと解放は、共通言語ランタイムにおいて高度に最適化されています。 ほとんどの場合、ヒープへのクラス インスタンスの割り当てと、スタックへの構造体インスタンスの割り当てに、パフォーマンス コストの点で大きな違いはありません。
  
## <a name="object-identity-vs-value-equality"></a>オブジェクト ID と値の等価性  
 2 つのオブジェクトが等しいかどうかを比較するときは、最初に、2 つの変数がメモリ内の同じオブジェクトを表しているかどうかを知りたいのか、それともオブジェクトの 1 つ以上のフィールドの値が等しいかどうかを知りたいのかを、区別する必要があります。 値を比較する場合は、オブジェクトが値型 (構造体) のインスタンスか、または参照型 (クラス、デリゲート、配列) のインスタンスかを、検討する必要があります。  
  
- クラスの 2 つのインスタンスがメモリ内の同じ場所を参照しているかどうか (つまり、同じ *ID* か) を調べるには、静的な <xref:System.Object.Equals%2A> メソッドを使います (<xref:System.Object?displayProperty=nameWithType> は、ユーザー定義の構造体やクラスを含む、すべての値型と参照型の暗黙の基底クラスです)。  
  
- 2 つの構造体インスタンスのインスタンス フィールドが同じ値を持つかどうかを調べるには、<xref:System.ValueType.Equals%2A?displayProperty=nameWithType> メソッドを使います。 すべての構造体は <xref:System.ValueType?displayProperty=nameWithType> を暗黙的に継承するので、次の例で示すように、オブジェクトで直接メソッドを呼び出します。  
  
 [!code-csharp[csProgGuideStatements#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#32)]  
  
 <xref:System.ValueType?displayProperty=nameWithType> での `Equals` の実装は、構造体に存在するフィールドを特定できる必要があるため、リフレクションを使います。 独自の構造体を作成するときは、`Equals` メソッドをオーバーライドして、独自の型に固有の効率的な等値アルゴリズムを提供します。  
  
- クラスの 2 つのインスタンスのフィールドの値が等しいかどうかを調べるには、<xref:System.Object.Equals%2A> メソッドまたは [== 演算子](../../language-reference/operators/equality-operators.md#equality-operator-)を使用できる場合があります。 ただし、この方法を使用できるのは、その型のオブジェクトにおける "等値" の意味のカスタム定義が、クラスのオーバーライドまたはオーバーロードによって提供されている場合だけです。 クラスは、<xref:System.IEquatable%601> インターフェイスまたは <xref:System.Collections.Generic.IEqualityComparer%601> インターフェイスを実装することもできます。 どちらのインターフェイスも、値の等価性をテストするために使うことができるメソッドを提供します。 `Equals` をオーバーライドする独自のクラスを設計するときは、「[型の値の等価性を定義する方法](../statements-expressions-operators/how-to-define-value-equality-for-a-type.md)」および「<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType>」に記載されているガイドラインに従ってください。
  
## <a name="related-sections"></a>関連項目  
 詳細情報  
  
- [クラス](./classes.md)  
  
- [コンストラクター](./constructors.md)  
  
- [ファイナライザー](./destructors.md)  
  
- [イベント](../events/index.md)  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [object](../../language-reference/builtin-types/reference-types.md)
- [継承](./inheritance.md)
- [class](../../language-reference/keywords/class.md)
- [構造体型](../../language-reference/builtin-types/struct.md)
- [new 演算子](../../language-reference/operators/new-operator.md)
- [共通型システム](../../../standard/base-types/common-type-system.md)
