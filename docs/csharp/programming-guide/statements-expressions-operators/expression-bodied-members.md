---
title: 式形式のメンバー - C# プログラミング ガイド
ms.date: 02/06/2019
helpviewer_keywords:
- expression-bodied members[C#]
- C# language, expresion-bodied members
ms.openlocfilehash: f212bb707d3dd2d4a7cc917d335a83cff01ed0cf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75711988"
---
# <a name="expression-bodied-members-c-programming-guide"></a>式形式のメンバー (C# プログラミング ガイド)

式本体の定義を使用すると、簡潔でわかりやすい形式でメンバーの実装を指定できます。 サポートされる任意のメンバー (メソッドやプロパティなど) に関するロジックが単一の式で構成される場合は、常に式本体の定義を使用できます。 式本体の定義には、次の一般的な構文があります。

```csharp
member => expression;
```

この *expression* には有効な式を指定します。

式本体の定義のサポートは、メソッドと読み取り専用プロパティのために C# 6 で導入され、C# 7.0 で拡張されました。 式本体の定義は、次の表の方メンバーで使用できます。

|メンバー  |サポートが開始されたバージョン |
|---------|---------|
|[方法](#methods)  |C# 6 |
|[読み取り専用プロパティ](#read-only-properties)   |C# 6  |
|[Property](#properties)  |C# 7.0 |
|[コンストラクター](#constructors)   |C# 7.0 |
|[ファイナライザー](#finalizers)     |C# 7.0 |
|[Indexer](#indexers)       |C# 7.0 |

## <a name="methods"></a>メソッド

式形式のメソッドは、型がメソッドの戻り値の型と一致する値を返す単一の式、または、`void` を返すメソッドの場合は何らかの処理を実行する単一の式で構成されます。 たとえば、一般的に、<xref:System.Object.ToString%2A> メソッドをオーバーライドする型には、現在のオブジェクトの文字列形式を返す単一の式が含まれています。

次の例では、式本体の定義を使用して `Person` メソッドをオーバーライドする <xref:System.Object.ToString%2A> クラスを定義します。 また、名前をコンソールに表示する `DisplayName` メソッドも定義します。 `return` 式本体の定義に `ToString` キーワードが使用されていない点に注意してください。

[!code-csharp[expression-bodied-methods](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-methods.cs)]  

詳細については、「[メソッド (C# プログラミング ガイド)](../classes-and-structs/methods.md)」を参照してください。

## <a name="read-only-properties"></a>読み取り専用プロパティ

C# 6 以降では、式本体の定義を使用して読み取り専用プロパティを実装することができます。 そのためには、次の構文を使用します。

```csharp
PropertyType PropertyName => expression;
```

次の例では、プライベート `Location` フィールドの値を返す式本体の定義として読み取り専用の `Name` プロパティを実装する `locationName` クラスを定義します。

[!code-csharp[expression-bodied-read-only-property](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-readonly.cs#1)]  

プロパティの詳細については、「[プロパティ (C# プログラミング ガイド)](../classes-and-structs/properties.md)」を参照してください。

## <a name="properties"></a>Properties

C# 7.0 以降では、式本体の定義を使用してプロパティ `get` と `set` アクセサーを実装することができます。 これを実行する方法を次の例に示します。

[!code-csharp[expression-bodied-property-get-set](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]

プロパティの詳細については、「[プロパティ (C# プログラミング ガイド)](../classes-and-structs/properties.md)」を参照してください。

## <a name="constructors"></a>コンストラクター

一般的に、コンストラクターの式本体の定義は、コンストラクターの引数を処理したり、インスタンスの状態を初期化したりする単一の代入式またはメソッド呼び出しから構成されます。

次の例では、コンストラクターに `Location`name*という名前の文字列パラメーターが 1 つある* クラスが定義されています。 式の本体の定義により `Name` プロパティに引数が割り当てられます。

[!code-csharp[expression-bodied-constructor](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]  

詳細については、「[コンストラクター (C# プログラミング ガイド)](../classes-and-structs/constructors.md)」を参照してください。

## <a name="finalizers"></a>ファイナライザー

一般的に、ファイナライザーの式本体の定義には、アンマネージ リソースをリリースするステートメントなどのクリーンアップ ステートメントが含まれています。

次の例では、式本体の定義を使用して、ファイナライザーが呼び出されたことを示すファイナライザーを定義します。

[!code-csharp[expression-bodied-finalizer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-destructor.cs#1)]  

詳細については、「[ファイナライザー (C# プログラミング ガイド)](../classes-and-structs/destructors.md)」を参照してください。

## <a name="indexers"></a>インデクサー

プロパティと同様に、`get` アクセサーが値を返す単一のステートメントで構成される場合、または `set` アクセサーがシンプルな代入を実行する場合、インデクサーの `get` と `set` アクセサーは、式本体の定義で構成されます。

次の例では、`Sports` というクラスを定義します。このクラスには、複数のスポーツ名を含む内部 <xref:System.String> 配列があります。 インデクサーの `get` および `set` アクセサーはいずれも、式本体の定義として実装されます。

[!code-csharp[expression-bodied-indexer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-indexers.cs#1)]

詳細については、「[インデクサー (C# プログラミング ガイド)](../indexers/index.md)」を参照してください。
