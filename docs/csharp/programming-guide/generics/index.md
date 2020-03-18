---
title: ジェネリック - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generics
- generics [C#]
ms.assetid: 75ea8509-a4ea-4e7a-a2b3-cf72482e9282
ms.openlocfilehash: c7252180c9c98a8ca99c8cc6b3faaf8b1b8f0749
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79167493"
---
# <a name="generics-c-programming-guide"></a>ジェネリック (C# プログラミング ガイド)

ジェネリックにより、.NET Framework に型パラメーターという概念が導入されます。これを使用すると、クラスやメソッドがクライアント コードによって宣言され、インスタンス化されるまで、1 つ以上の型の指定を遅延させるクラスやメソッドを設計することができます。 たとえば、ジェネリック型パラメーター `T` を使用すると、次に示すように、ランタイム キャストやボックス化操作を使うコストやリスクを発生させることなく他のクライアント コードで使用できる、単一のクラスを記述できます。

[!code-csharp[csProgGuideGenerics#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#1)]

ジェネリックのクラスとメソッドにより、非ジェネリックの場合には不可能な方法で、再利用性、タイプ セーフ、効率性が同時に実現されます。 ジェネリックは、コレクションとそれを操作するメソッドとともに使用されるのが通常です。 <xref:System.Collections.Generic> 名前空間には、ジェネリック ベースのコレクション クラスがいくつか含まれています。 <xref:System.Collections.ArrayList> などの非ジェネリック コレクションは推奨されません。これらは互換性のために保持されています。 詳細については、「[.NET のジェネリック](../../../standard/generics/index.md)」を参照してください。

もちろん、カスタムのジェネリック型やジェネリック メソッドを作成して、タイプ セーフで効率的な独自の汎用ソリューションや設計パターンを実現することもできます。 次のコード例では、デモンストレーション用の簡単なジェネリックのリンク リスト クラスを示します。 (通常は、独自のクラスを作成するのではなく、.NET Framework クラス ライブラリに用意されている <xref:System.Collections.Generic.List%601> クラスを使用してください。)この例では、通常、具体的な型を使用して、リストに格納する項目の型を示す場面で、型パラメーター `T` を使用しています。 このパラメーターは、次のように使用されています。

- `AddHead` メソッドのメソッド パラメーターの型として使用。
- 入れ子になった `Node` クラスの `Data` プロパティの戻り値の型として使用。
- 入れ子になったクラスのプライベート メンバー `data` の型として使用。

 入れ子になった `Node` クラスで `T` を使用できることに注意してください。 `GenericList<T>` が `GenericList<int>` のような具象型でインスタンス化されると、`T` の部分はそれぞれ `int` に置き換えられます。

[!code-csharp[csProgGuideGenerics#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#2)]

次のコード例では、クライアント コードでジェネリックの `GenericList<T>` クラスを使用して、整数のリストを作成する方法を示しています。 このコードの型引数を変更するだけで、文字列やその他の任意のカスタム型のリストを作成するように簡単に修正できます。

[!code-csharp[csProgGuideGenerics#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#3)]

## <a name="generics-overview"></a>ジェネリックの概要

- ジェネリック型は、コードの再利用、タイプ セーフ、およびパフォーマンスを最大化するために使用します。
- ジェネリックの最も一般的な用途は、コレクション クラスの作成です。
- .NET Framework クラス ライブラリには、複数の新しいジェネリック コレクション クラスが <xref:System.Collections.Generic> 名前空間に含まれています。 <xref:System.Collections> 名前空間の <xref:System.Collections.ArrayList> などのクラスの代わりとして、できる限りこれらを使用してください。
- 独自のジェネリック インターフェイス、クラス、メソッド、イベント、およびデリゲートを作成できます。
- ジェネリック クラスは、特定のデータ型のメソッドへのアクセスを有効にするように制限できます。
- ジェネリック データ型で使用される型に関する情報は、実行時にリフレクションを使用して取得できます。

## <a name="related-sections"></a>関連項目

- [ジェネリック型パラメーター](generic-type-parameters.md)
- [型パラメーターの制約](constraints-on-type-parameters.md)
- [ジェネリック クラス](generic-classes.md)
- [ジェネリック インターフェイス](generic-interfaces.md)
- [ジェネリック メソッド](generic-methods.md)
- [汎用デリゲート](generic-delegates.md)
- [C++ テンプレートと C# ジェネリックの違い](differences-between-cpp-templates-and-csharp-generics.md)
- [ジェネリックとリフレクション](generics-and-reflection.md)
- [ランタイムのジェネリック](generics-in-the-run-time.md)

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語の仕様](~/_csharplang/spec/types.md#constructed-types)」を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Collections.Generic>
- [C# プログラミング ガイド](../index.md)
- [型](../types/index.md)
- [\<typeparam>](../xmldoc/typeparam.md)
- [\<typeparamref>](../xmldoc/typeparamref.md)
- [.NET のジェネリック](../../../standard/generics/index.md)
