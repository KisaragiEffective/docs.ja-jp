---
title: インスタンス コンストラクター - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- constructors [C#], instance constructors
- instance constructors [C#]
ms.assetid: 24663779-c1e5-4af4-a942-ca554e4c542d
ms.openlocfilehash: 621b8ca7510b0b9916c9c46f201ff77402c3c655
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75964724"
---
# <a name="instance-constructors-c-programming-guide"></a>インスタンス コンストラクター (C# プログラミング ガイド)

インスタンス コンストラクターは、[new](../../language-reference/operators/new-operator.md) 式を使って[クラス](../../language-reference/keywords/class.md)のオブジェクトを作成するときに、インスタンス メンバー変数を作成および初期化するために使われます。 [静的](../../language-reference/keywords/static.md)クラスを初期化する場合、または非静的クラスの静的変数を初期化する場合は、静的コンストラクターを定義します。 詳細については、「[静的コンストラクター](./static-constructors.md)」を参照してください。  
  
 次に示すのは、インスタンス コンストラクターの例です。  
  
 [!code-csharp[csProgGuideObjects#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#5)]  
  
> [!NOTE]
> わかりやすくするため、このクラスにはパブリック フィールドが含まれています。 パブリック フィールドを使うと、プログラム内の任意の場所にある任意のメソッドが、制限も検証もなしにオブジェクトの内部動作にアクセスできるため、パブリック フィールドは推奨されるプログラミング手法ではありません。 データ メンバーは、一般にプライベートにする必要があり、クラスのメソッドとプロパティを介してのみアクセスする必要があります。  
  
 このインスタンス コンストラクターは、`Coords` クラスに基づくオブジェクトが作成されるたびに呼び出されます。 引数を受け取らないこのようなコンストラクターは、*パラメーターなしのコンストラクター*と呼ばれます。 ただし、コンストラクターを追加すると便利な場合がよくあります。 たとえば、データ メンバーの初期値を指定できるコンストラクターを `Coords` クラスに追加できます。  
  
 [!code-csharp[csProgGuideObjects#76](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#76)]  
  
 これにより、次のように、既定値または特定の初期値で `Coords` オブジェクトを作成できます。  
  
 [!code-csharp[csProgGuideObjects#77](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#77)]  
  
 クラスにコンストラクターがない場合は、パラメーターなしのコンストラクターが自動的に生成され、既定値を使ってオブジェクトのフィールドが初期化されます。 たとえば、[int](../../language-reference/builtin-types/integral-numeric-types.md) は 0 に初期化されます。 値の既定値の詳細については、「[C# 型の既定値](../../language-reference/builtin-types/default-values.md)」を参照してください。 したがって、`Coords` クラスのパラメーターなしのコンストラクターは、すべてのデータ メンバーをゼロに初期化するため、クラスの動作を変更することなくまとめて削除することができます。 複数のコンストラクターを使う完全な例は後の例 1 で、自動的に生成されたコンストラクターの例は例 2 で示します。  
  
 インスタンス コンストラクターを使って、基底クラスのインスタンス コンストラクターを呼び出すこともできます。 次のように、クラスのコンストラクターは、初期化子を使って基底クラスのコンストラクターを呼び出すことができます。  
  
 [!code-csharp[csProgGuideObjects#78](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#78)]  
  
 この例では、`Circle` クラスは、半径と高さを表す値を、`Shape` の派生元である `Circle` によって提供されるコンストラクターに渡しています。 `Shape` と `Circle` を使う完全な例は、例 3 をご覧ください。  
  
## <a name="example-1"></a>例 1  
 次の例で示すクラスには、引数を持たないクラス コンストラクターと、2 つの引数を持つクラス コンストラクターがあります。  
  
 [!code-csharp[csProgGuideObjects#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#4)]  
  
## <a name="example-2"></a>例 2  
 この例の `Person` クラスにはコンストラクターがありません。このような場合は、パラメーターなしのコンストラクターが自動的に提供され、フィールドは既定値に初期化されます。  
  
 [!code-csharp[csProgGuideObjects#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#8)]  
  
 `age` の既定値は `0`、`name` の既定値は `null` であることに注意してください。
  
## <a name="example-3"></a>例 3  
 次の例では、基底クラスの初期化子の使い方を示します。 `Circle` クラスは汎用クラス `Shape` から派生し、`Cylinder` クラスは `Circle` クラスから派生しています。 各派生クラスのコンストラクターでは、その基底クラスの初期化子が使われています。  
  
 [!code-csharp[csProgGuideObjects#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#9)]  
  
 基底クラスのコンストラクターの呼び出しに関する他の例については、「[virtual](../../language-reference/keywords/virtual.md)」、「[override](../../language-reference/keywords/override.md)」、「[base](../../language-reference/keywords/base.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [コンストラクター](./constructors.md)
- [ファイナライザー](./destructors.md)
- [static](../../language-reference/keywords/static.md)
