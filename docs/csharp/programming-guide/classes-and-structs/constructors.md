---
title: コンストラクター - C# プログラミング ガイド
ms.date: 05/05/2017
helpviewer_keywords:
- constructors [C#]
- classes [C#], constructors
- C# language, constructors
ms.assetid: df2e2e9d-7998-418b-8e7d-890c17ff6c95
ms.openlocfilehash: 8eedfaed111f01cc2ec55a2f42df66d4588bd42f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398401"
---
# <a name="constructors-c-programming-guide"></a>コンストラクター (C# プログラミング ガイド)

[クラス](../../language-reference/keywords/class.md)または[構造体](../../language-reference/builtin-types/struct.md)を作成するたびに、コンストラクターが呼び出されます。 クラスまたは構造体には、異なる引数を取るコンストラクターが複数含まれていることがあります。 プログラマーはコンストラクターを利用することで、既定値を設定したり、インスタンス化を制限したり、柔軟で読みやすいコードを記述したりできます。 詳細と例については、「[コンストラクターの使用](./using-constructors.md)」と「[インスタンス コンストラクター](./instance-constructors.md)」を参照してください。  

## <a name="parameterless-constructors"></a>パラメーターなしのコンストラクター
  
クラスにコンストラクターを指定しない場合、C# では既定でコンストラクターが 1 つ作成されます。そのコンストラクターによりオブジェクトがインスタンス化され、「[C# の既定値](../../language-reference/builtin-types/default-values.md)」の記事にある既定値にメンバー変数が設定されます。 構造体にコンストラクターを指定しない場合、C# では、*暗黙的なパラメーターなしのコンストラクター*を利用し、各フィールドがその既定値に自動的に初期化されます。 詳細と例については、「[インスタンス コンストラクター](instance-constructors.md)」を参照してください。  

## <a name="constructor-syntax"></a>コンストラクターの構文

コンストラクターは、名前がその型の名前と同じメソッドです。 メソッド シグネチャには、メソッド名とそのパラメーター リストだけが含まれます。戻り値の型は含まれません。 次の例は、`Person` という名前のクラスのコンストラクターを示しています。

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#1)]  

コンストラクターを 1 つのステートメントとして実装できる場合、[式の本体の定義](../statements-expressions-operators/expression-bodied-members.md)を利用できます。 次の例では、コンストラクターに *name* という名前の文字列パラメーターが 1 つある `Location` クラスが定義されています。 式の本体の定義により `locationName` フィールドに引数が割り当てられます。

[!code-csharp[expression-bodied-constructor](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]  

## <a name="static-constructors"></a>静的コンストラクター

前の例には、表示されるすべてのインスタンス コンストラクターがあり、それが新しいオブジェクトを作成します。 クラスまたは構造体には静的コンストラクターを与えることもできます。静的コンストラクターは型の静的メンバーを初期化します。  静的コンストラクターにはパラメーターがありません。 静的コンストラクターを指定して静的フィールドを初期化しない場合、C# コンパイラは、「[C# 型の既定値](../../language-reference/builtin-types/default-values.md)」の記事にある既定値に静的フィールドを初期化します。

次の例では、静的コンストラクターを使用して静的フィールドを初期化しています。

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#2)]  

式の本体の定義で静的コンストラクターを定義することもできます。次の例をご覧ください。

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#3)]  

詳細と例については、「[静的コンストラクター](./static-constructors.md)」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [コンストラクターの使用](./using-constructors.md)  
  
 [インスタンス コンストラクター](./instance-constructors.md)  
  
 [プライベート コンストラクター](./private-constructors.md)  
  
 [静的コンストラクター](./static-constructors.md)  
  
 [コピー コンストラクターを記述する方法](./how-to-write-a-copy-constructor.md)  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [クラスと構造体](./index.md)
- [ファイナライザー](./destructors.md)
- [static](../../language-reference/keywords/static.md)
- [初期化子がコンストラクターとは反対の順序で実行される理由その 1](https://docs.microsoft.com/archive/blogs/ericlippert/why-do-initializers-run-in-the-opposite-order-as-constructors-part-one)
