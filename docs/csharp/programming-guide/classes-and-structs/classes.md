---
title: クラス - C# プログラミング ガイド
description: クラスの型と、クラスの型を作成する方法について説明します
ms.date: 08/21/2018
helpviewer_keywords:
- classes [C#]
- C# language, classes
ms.assetid: e8848524-7273-429f-8aba-c658d5eff5ad
ms.openlocfilehash: aadf555fb47963eab323bbb6105227c5b119e6f4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170313"
---
# <a name="classes-c-programming-guide"></a>クラス (C# プログラミング ガイド)

## <a name="reference-types"></a>参照型  
[class](../../language-reference/keywords/class.md) として定義された型は、*参照型*です。 実行時には、参照型の変数を宣言すると、[new](../../language-reference/keywords/null.md) 演算子を使用してクラスのインスタンスを明示的に作成するまで、変数には値 [null](../../language-reference/operators/new-operator.md) が格納されています。または、次の例に示すように、別の場所で作成された可能性がある、互換性のある型のオブジェクトを代入することもできます。

```csharp
//Declaring an object of type MyClass.
MyClass mc = new MyClass();

//Declaring another object of the same type, assigning it the value of the first object.
MyClass mc2 = mc;
```

オブジェクトが作成されると、その特定のオブジェクトに対してマネージド ヒープ上で十分なメモリが割り当てられ、変数にはそのオブジェクトの場所への参照のみが格納されます。 マネージド ヒープを使用する型では、メモリの割り当て時と、CLR の自動メモリ管理機能 ("*ガベージ コレクション*") による再要求時の両方についてオーバーヘッドが発生します。 しかし、ガベージ コレクションも高度に最適化されるため、ほとんどのシナリオでは、パフォーマンス上の問題が発生することはありません。 ガベージ コレクションの詳細については、「[自動メモリ管理とガベージ コレクション](../../../standard/garbage-collection/gc.md)」を参照してください。  
  
## <a name="declaring-classes"></a>クラスの宣言

 クラスは、次の例に示すように、[class](../../language-reference/keywords/class.md) キーワードと、その後に続ける一意の識別子を使用して宣言します。

 ```csharp
//[access modifier] - [class] - [identifier]
 public class Customer
 {
    // Fields, properties, methods and events go here...
 }
```

 `class` キーワードは、アクセス レベルの後に配置します。 この例では、[public](../../language-reference/keywords/public.md) が使用されているため、誰でもこのクラスのインスタンスを作成できます。 `class` キーワードの後にクラスの名前を記述します。 クラスの名前を、有効な C# の[識別子名](../inside-a-program/identifier-names.md)にする必要があります。 定義の残りの部分がクラス本体で、そこで動作とデータを定義します。 クラスのフィールド、プロパティ、メソッド、およびイベントは*クラス メンバー*と総称されます。  
  
## <a name="creating-objects"></a>オブジェクトの作成

クラスとオブジェクトは、同義的に使用されることがありますが、これらは異なるものです。 クラスはオブジェクトの型を定義しますが、オブジェクト自体ではありません。 オブジェクトは、クラスに基づく具体的なエンティティであり、クラスのインスタンスと呼ばれることもあります。  
  
 オブジェクトを作成するには、次のように、[new](../../language-reference/operators/new-operator.md) キーワードの後にオブジェクトの基になるクラスの名前を指定します。  

 ```csharp
 Customer object1 = new Customer();
 ```

 クラスのインスタンスを作成すると、そのオブジェクトへの参照が返されます。 前の例の `object1` は、`Customer` に基づくオブジェクトへの参照です。 この参照は、新しいオブジェクトを参照しますが、オブジェクト データ自体を含みません。 実際、オブジェクト参照は、オブジェクトを作成しなくても作成できます。  

```csharp
 Customer object2;
```

 上のような、オブジェクトを参照しないオブジェクト参照を作成するのはお勧めしません。実行時にこのような参照を通じてオブジェクトへのアクセスを試みると失敗するからです。 ただし、新しいオブジェクトを作成するか、既存のオブジェクトに割り当てると、このような参照でオブジェクトを参照できるようになります。次に例を示します。  

 ```csharp
 Customer object3 = new Customer();
 Customer object4 = object3;
```
  
 上のコードでは、同じオブジェクトを参照する 2 つのオブジェクト参照が作成されます。 そのため、`object3` を通じて行われたオブジェクトの変更は、後で `object4` を使用するときに反映されます。 これは、クラスに基づくオブジェクトが参照によって参照されるからです。このためクラスは参照型と呼ばれています。  
  
## <a name="class-inheritance"></a>クラスの継承  

クラスは、オブジェクト指向プログラミングの基本的な特性である "*継承*" を完全にサポートします。 クラスを作成するときは、[sealed](../../language-reference/keywords/sealed.md) として定義されているものを除く、他のすべてのインターフェイスまたはクラスから継承できます。また、作成したクラスから他のクラスを継承し、クラスの仮想メソッドをオーバーライドすることもできます。

継承は、*派生*を使用して行われます。派生とは、データの動作の継承元である*基底クラス*を使用してクラスを宣言することを意味します。 基底クラスは、派生クラス名の後に、コロンと基底クラス名を追加して指定します。次に例を示します。  

 ```csharp
 public class Manager : Employee
 {
     // Employee fields, properties, methods and events are inherited
     // New Manager fields, properties, methods and events go here...
 }
 ```

クラスで基底クラスを宣言している場合、基底クラスのすべてのメンバー (コンストラクター以外) が継承されます。 詳細については、「[継承](inheritance.md)」を参照してください。
  
C++ と異なり、C# のクラスは 1 つの基底クラスから直接継承することしかできません。 ただし、基底クラス自体が別のクラスを継承している場合があるため、1 つのクラスに複数の基底クラスが間接的に継承されることもあります。 さらに、クラスは複数のインターフェイスを直接実装できます。 詳細については、「[インターフェイス](../interfaces/index.md)」を参照してください。  
  
クラスは[抽象](../../language-reference/keywords/abstract.md)としても宣言できます。 抽象クラスには、シグネチャ定義が存在し、実装は存在しない抽象メソッドが含まれています。 抽象クラスはインスタンス化できません。 抽象クラスを使用するには、抽象メソッドを実装する派生クラスを介する必要があります。 これとは対照的に、[シール](../../language-reference/keywords/sealed.md) クラスは、他のクラスに派生させることはできません。 詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](abstract-and-sealed-classes-and-class-members.md)」を参照してください。  
  
クラス定義は、別々のソース ファイルに分割できます。 詳細については、「[部分クラスと部分メソッド](partial-classes-and-methods.md)」を参照してください。  
  
## <a name="example"></a>例

次の例では、[自動実装プロパティ](auto-implemented-properties.md)、メソッド、およびコンストラクターという特殊なメソッドをそれぞれ 1 つずつ含むパブリック クラスを定義しています。 詳しくは、[プロパティ](properties.md)、[メソッド](methods.md)、および[コンス トラクター](constructors.md)に関するトピックを参照してください。 このクラスのインスタンスは、`new` キーワードによってインスタンス化されます。  
  
[!code-csharp[Class Example](~/samples/snippets/csharp/programming-guide/classes-and-structs/class-example.cs)]
  
## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [オブジェクト指向プログラミング](../concepts/object-oriented-programming.md)
- [ポリモーフィズム](polymorphism.md)
- [識別子名](../inside-a-program/identifier-names.md)
- [[メンバー]](members.md)
- [メソッド](methods.md)
- [コンストラクター](constructors.md)
- [ファイナライザー](destructors.md)
- [オブジェクト](objects.md)
