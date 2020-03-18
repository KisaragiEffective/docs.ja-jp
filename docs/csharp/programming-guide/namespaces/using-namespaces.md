---
title: 名前空間の使用 - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- cs.names
helpviewer_keywords:
- fully qualified names [C#]
- namespaces [C#], how to use
ms.assetid: 1fe8bf39-addc-438a-bd9e-86410e32381d
ms.openlocfilehash: 5193fc7aaae83cbc0c75e81835244eaaaece69a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75700199"
---
# <a name="using-namespaces-c-programming-guide"></a>名前空間の使用 (C# プログラミング ガイド)

C# プログラム内では名前空間が 2 つの方法でよく使用されます。 最初の方法では、.NET Framework クラスで名前空間を使用して、その多くのクラスを整理します。 2 つ目の方法では、独自の名前空間を宣言します。これは、より大きなプログラミング プロジェクトでクラス名とメソッド名のスコープを制御するのに役立ちます。  
  
## <a name="accessing-namespaces"></a>名前空間へのアクセス

 ほとんどの C# アプリケーションは `using` ディレクティブのセクションから始まります。 このセクションには、アプリケーションが頻繁に使用する名前空間がリストされ、包含されているメソッドが使用されるたびにプログラマが完全修飾名を指定しなくても済むようにします。  
  
 たとえば、次の行を記述したとします。  
  
 [!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]  
  
 この場合、プログラムの開始位置で、以下のコードを使用できます。  
  
 [!code-csharp[csProgGuide#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#31)]  
  
 次の表記の代わりに、  
  
 [!code-csharp[csProgGuide#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#30)]  
  
## <a name="namespace-aliases"></a>名前空間エイリアス

 [`using` ディレクティブ](../../language-reference/keywords/using-directive.md)を使って、名前空間のエイリアスを作成することもできます。 エイリアス化された名前空間のメンバーにアクセスするには、[名前空間エイリアス修飾子 `::`](../../language-reference/operators/namespace-alias-qualifier.md) を使います。 次の例では、名前空間エイリアスを作成して使用する方法を示します。
  
[!code-csharp[csProgGuideNamespaces#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#5)]
  
## <a name="using-namespaces-to-control-scope"></a>名前空間を用いたスコープの制御

 `namespace` キーワードを使用して、スコープを宣言します。 プロジェクト内でスコープを作成すると、コードの編成が容易になり、グローバルに一意の型を作成できます。 次の例では、入れ子関係にある 2 つの名前空間で `SampleClass` というクラスを定義します。 [メンバー アクセス `.` 演算子](../../language-reference/operators/member-access-operators.md#member-access-operator-)は、呼び出されるメソッドを区別するために使用されます。  
  
 [!code-csharp[csProgGuideNamespaces#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#8)]  
  
## <a name="fully-qualified-names"></a>完全修飾名

 名前空間と型には、論理階層を示す完全修飾名で表された一意のタイトルが割り当てられています。 たとえば、ステートメント `A.B` は、`A` が名前空間または型の名前であり、`B` はその内部で入れ子になっていることを意味します。  
  
 入れ子になっているクラスと名前空間を次の例に示します。 完全修飾名は、各エンティティの後のコメントとして示されています。  
  
 [!code-csharp[csProgGuideNamespaces#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#9)]  
  
 上のコード セグメントの各要素の意味は次のとおりです。  
  
- 名前空間 `N1` はグローバル名前空間のメンバーです。 その完全修飾名は `N1` です。  
  
- 名前空間 `N2` は `N1` のメンバーです。 その完全修飾名は `N1.N2` です。  
  
- クラス `C1` は `N1` のメンバーです。 その完全修飾名は `N1.C1` です。  
  
- クラス名 `C2` は、このコードでは 2 回使用されています。 ただし、完全修飾名は異なります。 `C2` の最初のインスタンスは `C1` 内で宣言されているため、その完全修飾名は `N1.C1.C2` となります。 `C2` の 2 つ目のインスタンスは名前空間 `N2` 内で宣言されているため、その完全修飾名は `N1.N2.C2` となります。  
  
 上のコード セグメントを使用して、次のように新しいクラス メンバー `C3` を名前空間 `N1.N2` に追加することができます。  
  
 [!code-csharp[csProgGuideNamespaces#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#10)]  
  
 通常、[名前空間エイリアス修飾子 `::`](../../language-reference/operators/namespace-alias-qualifier.md) は名前空間エイリアスを参照するために使い、`global::` はグローバル名前空間を参照するために使います。`.` は型またはメンバーを修飾するために使います。  
  
 名前空間ではなく型を参照するエイリアスで `::` を使用するのは誤りです。 次に例を示します。  
  
 [!code-csharp[csProgGuideNamespaces#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#11)]  
  
 [!code-csharp[csProgGuideNamespaces#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#12)]  
  
 `global` という語は定義済みのエイリアスではないため、`global.X` には特別な意味がないことに注意してください。 `::` と共に使用したときにのみ特別な意味が与えられます。  
  
 `global::` は常にグローバル名前空間を参照し、エイリアスを参照しないので、global という名前のエイリアスを定義すると、コンパイラ警告 CS0440 が生成されます。 たとえば、次のコード行では警告が生成されます。  
  
 [!code-csharp[csProgGuideNamespaces#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces2.cs#13)]  
  
 エイリアスで `::` を使用することをお勧めします。そうすれば、追加の型が予想外に導入されることを防ぐことができます。 たとえば、次の例について考えてみます。  
  
 [!code-csharp[csProgGuideNamespaces#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#14)]  
  
 [!code-csharp[csProgGuideNamespaces#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#15)]  
  
 このコードは動作しますが、`Alias` という名前の型が後で導入された場合、`Alias.` は代わりにその型にバインドされます。 `Alias::Exception` を使用すれば、`Alias` は名前空間エイリアスとして扱われ、型と間違われることがなくなります。  

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [名前空間](./index.md)
- [演算子 .](../../language-reference/operators/member-access-operators.md#member-access-operator-)
- [:: 演算子](../../language-reference/operators/namespace-alias-qualifier.md)
- [extern エイリアス](../../language-reference/keywords/extern-alias.md)
