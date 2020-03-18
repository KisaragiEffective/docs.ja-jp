---
title: オブジェクト初期化子とコレクション初期化子 - C# プログラミング ガイド
ms.date: 12/19/2018
helpviewer_keywords:
- object initializers [C#]
- collection initializers [C#]
ms.assetid: c58f3db5-d7d4-4651-bd2d-5a3a97357f61
ms.openlocfilehash: ae8741e2f29db0a470ad8d3b121375fbdeaff0d9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170196"
---
# <a name="object-and-collection-initializers-c-programming-guide"></a>オブジェクト初期化子とコレクション初期化子 (C# プログラミング ガイド)

C# では、1 つの命令文でオブジェクトまたはコレクションをインスタンス化し、1 つのステートメントでメンバーを割り当てることができます。

## <a name="object-initializers"></a>オブジェクト初期化子

オブジェクト初期化子を使用すると、オブジェクトの作成時にアクセスできるフィールドまたはプロパティに、コンストラクターを呼び出して代入ステートメントを使用しなくても、値を割り当てることができます。 オブジェクト初期化子の構文では、コンストラクターの引数を指定することも、引数 (およびかっこ構文) を省略することもできます。  以下の例では、名前付きの型である `Cat` でオブジェクト初期化子を使用する方法と、パラメーターなしのコンストラクターを呼び出す方法を示します。 `Cat` クラス内で自動実装プロパティが使用されています。 詳細については、「[自動実装プロパティ](auto-implemented-properties.md)」を参照してください。  
  
[!code-csharp[ObjectInitializer1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#CatDeclaration)]  
[!code-csharp[ObjectInitializer1a](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ObjectPropertyInitialization)]  

オブジェクト初期化子の構文では、インスタンスを作成できます。その後、新規作成されたオブジェクトは、割り当てられたプロパティと共に、割り当ての変数に代入されます。

C# 6 より、オブジェクト初期化子では、フィールドとプロパティを割り当てることに加え、インデクサーを設定できます。 次の基底 `Matrix` クラスをご覧ください。

[!code-csharp[ObjectInitializer2](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#MatrixDeclaration)]  

次のコードで単位行列を初期化できます。

[!code-csharp[ObjectInitializer2a](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#MatrixInitialization)]  

アクセス可能なセッターが含まれるアクセス可能なインデクサーを、引数の数や種類と関係なく、オブジェクト初期化子で式の 1 つとして使用できます。 インデックス引数は代入の左側となり、値は式の右側となります。  たとえば、`IndexersExample` に適切なインデクサーが指定されている場合、以下はすべて有効になります。

```csharp
var thing = new IndexersExample {
    name = "object one",
    [1] = '1',
    [2] = '4',
    [3] = '9',
    Size = Math.PI,
    ['C',4] = "Middle C"
}
```

上記のコードをコンパイルするには、`IndexersExample` 型に次のメンバーを指定する必要があります。

```csharp
public string name;
public double Size { set { ... }; }
public char this[int i] { set { ... }; }
public string this[char c, int i] {  set { ... }; }
```

## <a name="object-initializers-with-anonymous-types"></a>オブジェクト初期化子と匿名型

オブジェクト初期化子は、どのような場合にも使うことができますが、LINQ クエリ式で使うと特に有用です。 クエリ式では、次の宣言に示すように、オブジェクト初期化子を使うことによってのみ初期化できる[匿名型](./anonymous-types.md)が頻繁に使われます。  

```csharp
var pet = new { Age = 10, Name = "Fluffy" };  
```

匿名型を使うと、LINQ クエリ式の `select` 句によって元のシーケンスのオブジェクトを値と形状が元とは異なるオブジェクトに変換できます。 この方法は、シーケンス内の各オブジェクトの情報の一部のみを保存する場合に便利です。 次の例は、製品オブジェクト (`p`) に多くのフィールドおよびメソッドが含まれており、製品名および単価を含むオブジェクトのシーケンスを作成することにのみ関心があることを想定しています。  
  
[!code-csharp[ObjectInitializer3](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#AnonymousUse)]  

このクエリが実行されると、`productInfos` 変数には、次の例に示す `foreach` ステートメントでアクセスできるオブジェクトのシーケンスが格納されます。  

```csharp
foreach(var p in productInfos){...}  
```

作成される匿名型内の各オブジェクトには、2 つのパブリック プロパティがあります。これらのプロパティには、元のオブジェクトのプロパティまたはフィールドと同じ名前が付けられます。 匿名型を作成するときにフィールドの名前を変更することもできます。次の例では、フィールド `UnitPrice` の名前が `Price` に変更されます。  

```csharp
select new {p.ProductName, Price = p.UnitPrice};  
```

## <a name="collection-initializers"></a>コレクション初期化子

コレクション初期化子を使うと、<xref:System.Collections.IEnumerable> を実装するコレクション型を初期化するときに 1 つ以上の要素の初期化子を指定でき、適切なシグネチャの `Add` をインスタンス メソッドまたは拡張メソッドとして使用できます。 要素の初期化子は、単純な値、式またはオブジェクト初期化子です。 コレクション初期化子を使用すると、呼び出しを複数回指定する必要がなくなります。コンパイラによって呼び出しが自動的に追加されるためです。  
  
2 つの単純なコレクション初期化子を次の例に示します。  

```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };  
List<int> digits2 = new List<int> { 0 + 1, 12 % 3, MakeInt() };  
```

次のコレクション初期化子は、前の例で定義されている `Cat` クラスのオブジェクトをオブジェクト初期化子を使用して初期化します。 個々のオブジェクト初期化子は、かっこで囲まれ、コンマで区切られています。  
  
[!code-csharp[ListInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ListInitializer)]  
  
コレクションの [ メソッドで許容されている場合、コレクション初期化子の要素として ](../../language-reference/keywords/null.md)null`Add` を指定できます。  
  
[!code-csharp[ListInitializerNull](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#ListInitialerWithNull)]  
  
 コレクションがインデックスを読み取り/書き込みできる場合は、インデックス付きの要素を指定できます。
  
[!code-csharp[DictionaryInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#DictionaryIndexerInitializer)]  

上記のサンプルの場合、<xref:System.Collections.Generic.Dictionary%602.Item(%600)> を呼び出して値を設定するコードが生成されます。 C# 6 以前は、次の構文を使用して、ディクショナリやその他の連想コンテナーを初期化できます。 インデクサー構文の代わりに、括弧と代入によって 1 つのオブジェクトと複数の値が処理されていることにご注目ください。

[!code-csharp[DictionaryAddInitializer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#DictionaryAddInitializer)]  

この初期化子の例では、<xref:System.Collections.Generic.Dictionary%602.Add(%600,%601)> を呼び出し、3 つの項目をディクショナリに追加しています。 連想コレクションを初期化するこれら 2 つの異なる方法には、コンパイラによって生成されるメソッド呼び出しにより、動作にわずかな違いがあります。 いずれの場合も `Dictionary` クラスが使用されます。 他の型の場合、パブリック API に基づいて、いずれかのみサポートされることがあります。

## <a name="examples"></a>例

次の例では、オブジェクトの概念とコレクション初期化子の概念が組み合わさっています。

[!code-csharp[InitializerExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullExample)]  

次の例のオブジェクトは <xref:System.Collections.IEnumerable> を実装します。このオブジェクトには `Add` メソッドと複数のパラメーターが含まれ、`Add` メソッドのシグネチャに対応するリスト項目ごとにコレクション初期化子と複数の要素を使用します。

[!code-csharp[InitializerListExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullListExample)]  

`Add` メソッドでは、次の例で示すように、`params` キーワードを使用して可変数個の引数を受け取ることができます。 この例では、インデクサーのカスタム実装と、インデクサーを使用したコレクションの初期化を示しています。

[!code-csharp[InitializerListExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/BasicObjectInitializers.cs#FullDictionaryInitializer)]  

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [C# での LINQ](../../linq/index.md)
- [匿名型](anonymous-types.md)
