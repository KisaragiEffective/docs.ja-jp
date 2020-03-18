---
title: プロパティの使用 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- set accessor [C#]
- get accessor [C#]
- properties [C#], about properties
ms.assetid: f7f67b05-0983-4cdb-96af-1855d24c967c
ms.openlocfilehash: d873f626b660bb6bd94710add4543e21e11823d6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77452020"
---
# <a name="using-properties-c-programming-guide"></a>プロパティの使用 (C# プログラミング ガイド)

プロパティは、フィールドとメソッドの両方の側面を結合します。 オブジェクトのユーザーにとってプロパティは、プロパティへのアクセスに同じ構文を必要とするフィールドのように見えます。 クラスの実装者にとってプロパティは、[get](../../language-reference/keywords/get.md) アクセサーと [set](../../language-reference/keywords/set.md) アクセサーの両方またはいずれかを表す 1 つまたは 2 つのコード ブロックです。 `get` アクセサーのコード ブロックはプロパティが読み取られる時に実行され、`set` アクセサーのコード ブロックはプロパティに新しい値が割り当てられるときに実行されます。 `set` アクセサーのないプロパティは読み取り専用と見なされます。 `get` アクセサーのないプロパティは書き込み専用と見なされます。 両方のアクセサーを持つプロパティは、読み取り/書き込みです。

フィールドとは異なり、プロパティは変数には分類されません。 そのため、プロパティを [ref](../../language-reference/keywords/ref.md) または [out](../../language-reference/keywords/out-parameter-modifier.md) パラメーターとして渡すことはできません。

プロパティには次のようなさまざまな用途があります。変更を許可する前にデータを検証したり、データをそのデータが実際に他のソース (データベースなど) から取得されるクラスで透過的に公開したり、イベントの発生や他のフィールドの値を変更するなど、データが変更されたときに、アクションを実行したりすることができます。

プロパティはクラス ブロックで宣言できます。フィールドのアクセス レベル、プロパティの型、プロパティの名前、`get` アクセサーと `set` アクセサーの両方またはいずれかを宣言するコード ブロックの順で指定します。 次に例を示します。

[!code-csharp[csProgGuideProperties#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#7)]

この例では、`Month` アクセサーが `set` が 1 から 12 までの値に設定されていることを確認できるように、`Month` がプロパティとして宣言されています。 `Month` プロパティは、プライベート フィールドを使用して実際の値を追跡します。 プロパティのデータの実際の場所は、プロパティの "バッキング ストア" と呼ばれることがよくあります。 プロパティがプライベート フィールドをバッキング ストアとして使用するのは一般的なことです。 フィールドは、プロパティを呼び出すことでのみ変更できるようにするため、プライベートとマークされます。 パブリックおよびプライベートのアクセス制限の詳細については、「[アクセス修飾子](./access-modifiers.md)」を参照してください。

自動実装プロパティは、単純なプロパティ宣言の簡単な構文を提供します。 詳細については、「[自動実装プロパティ](auto-implemented-properties.md)」を参照してください。

## <a name="the-get-accessor"></a>get アクセサー

`get` アクセサーの本体は、メソッドの本体と似ています。 プロパティの型の値を返す必要があります。 `get` アクセサーの実行は、フィールドの値を読み取ることに相当します。 たとえば、`get` アクセサーからプライベート変数を返し、最適化が有効になっている場合、`get` アクセサー メソッドへの呼び出しはコンパイラによってインライン化されるため、メソッド呼び出しのオーバーヘッドはありません。 ただし、仮想 `get` アクセサー メソッドはインライン化できません。これは、コンパイラがコンパイル時にどのメソッドが実際に実行時に呼び出されるかを認識しないからです。 次に、プライベート フィールド `get` の値を返す `_name` アクセサーを示します。

[!code-csharp[csProgGuideProperties#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#8)]

プロパティを参照するとき、割り当ての対象を除き、`get` アクセサーがプロパティの値を読み取るために呼び出されます。 次に例を示します。

[!code-csharp[csProgGuideProperties#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#9)]

`get` アクセサーは [return](../../language-reference/keywords/return.md) または [throw](../../language-reference/keywords/throw.md) ステートメントで終わる必要があります。コントロールはアクセサー本体をフロー オフすることはできません。

`get` アクセサーを使用してオブジェクトの状態を変更するのは、悪いプログラミング スタイルです。 たとえば、次のアクセサーでは、`_number` フィールドにアクセスされるたびにオブジェクトの状態が変更される副作用が発生します。

[!code-csharp[csProgGuideProperties#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#10)]

`get` アクセサーは、フィールド値を返すまたは計算してから返すために使用できます。 次に例を示します。

[!code-csharp[csProgGuideProperties#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#11)]

前のコード セグメントで `Name` プロパティに値を割り当てない場合、値 `NA` が返されます。

## <a name="the-set-accessor"></a>set アクセサー

`set` アクセサーは、戻り値の型が [void](../../language-reference/builtin-types/void.md) のメソッドと似ています。 型がプロパティの型の `value` と呼ばれる暗黙のパラメーターを使用します。 次の例では、`set` アクセサーが `Name` プロパティに追加されます。

[!code-csharp[csProgGuideProperties#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#12)]

プロパティに値を割り当てるときに、新しい値を提供する引数を使用して `set` アクセサーが呼び出されます。 次に例を示します。

[!code-csharp[csProgGuideProperties#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#13)]

`value` アクセサーでローカル変数の宣言に暗黙のパラメーター名 `set` を使用すると、エラーになります。

## <a name="remarks"></a>解説

プロパティは `public`、`private`、`protected`、`internal`、`protected internal`、`private protected` のいずれかでマークできます。 これらのアクセス修飾子により、クラスのユーザーがプロパティにアクセスできる方法が定義されます。 同じプロパティの `get` と `set` アクセサーは、異なるアクセス修飾子を持つことができます。 たとえば、`get` を `public` にして、型の外部からの読み取り専用アクセスを許可して、`set` を `private` または `protected` にすることができます。 詳細については、「[アクセス修飾子](./access-modifiers.md)」を参照してください。

`static` キーワードを使用して、プロパティを静的プロパティとして宣言できます。 その場合、クラスのインスタンスが存在しなくても、呼び出し元がいつでもプロパティを使用できるようになります。 詳細については、「[静的クラスと静的クラス メンバー](./static-classes-and-static-class-members.md)」を参照してください。

プロパティは、[virtual](../../language-reference/keywords/virtual.md) キーワードを使用して仮想プロパティとしてマークできます。 その場合、派生クラスでは、[override](../../language-reference/keywords/override.md) キーワードを使用してプロパティの動作をオーバーライドできます。 これらのオプションの詳細については、「[継承](inheritance.md)」を参照してください。

仮想プロパティをオーバーライドするプロパティは、[sealed](../../language-reference/keywords/sealed.md) にすることもできます。その場合、派生クラスでは、プロパティが仮想でなくなります。 最後に、プロパティは[抽象](../../language-reference/keywords/abstract.md)として宣言できます。 つまり、クラスに実装はなく、派生クラスが独自の実装を記述する必要があります。 これらのオプションの詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](abstract-and-sealed-classes-and-class-members.md)」を参照してください。
  
> [!NOTE]
> [静的](../../language-reference/keywords/virtual.md)プロパティのアクセサーで[virtual](../../language-reference/keywords/abstract.md)、[abstract](../../language-reference/keywords/override.md)、または [override](../../language-reference/keywords/static.md) 修飾子を使用すると、エラーになります。

## <a name="example"></a>例

この例では、インスタンス、静的、および読み取り専用のプロパティを示します。 キーボードから従業員の名前を受け取り、`NumberOfEmployees` を 1 だけインクリメントし、従業員の名前と番号を表示します。

[!code-csharp[csProgGuideProperties#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#2)]

## <a name="example"></a>例

この例では、派生クラスで同じ名前を持つ別のプロパティによって非表示にされている基底クラスのプロパティにアクセスする方法を示します。

[!code-csharp[csProgGuideProperties#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#3)]

前の例で重要な点を次に示します。

- 派生クラスのプロパティ `Name` により基底クラス内のプロパティ `Name` が非表示になっています。 このような場合、`new` 修飾子は派生クラスのプロパティの宣言で使用されます。

     [!code-csharp[csProgGuideProperties#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#4)]  

- キャスト `(Employee)` は基底クラスで非表示のプロパティにアクセスするために使用されます。

     [!code-csharp[csProgGuideProperties#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#5)]

     メンバーを非表示にする詳細については、「[new 修飾子](../../language-reference/keywords/new-modifier.md)」を参照してください。

## <a name="example"></a>例

この例では、`Cube` と `Square` の 2 つのクラスが抽象クラス `Shape` を実装し、その抽象 `Area` プロパティをオーバーライドします。 プロパティでの [override](../../language-reference/keywords/override.md) 修飾子の使用に注意してください。 プログラムは、入力として辺を受け入れ、四角形と立方体の面積を計算します。 プログラムはまた、入力として面積を受け入れ、四角形と立方体の対応する辺を計算します。

[!code-csharp[csProgGuideProperties#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#6)]

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [Properties](properties.md)
- [インターフェイスのプロパティ](interface-properties.md)
- [自動実装プロパティ](auto-implemented-properties.md)
