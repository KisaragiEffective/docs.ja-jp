---
title: Properties
description: C# のプロパティについて説明します。C# のプロパティには、検証、計算値、遅延評価、プロパティ変更通知の機能があります。
ms.technology: csharp-fundamentals
ms.date: 04/25/2018
ms.openlocfilehash: bda8a4f58f71b57248296dd4ba9f9bf4cbed40d4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398689"
---
# <a name="properties"></a>Properties

C# のプロパティは、非常に優れた機能です。 開発者は C# で定義されている構文を使用して、設計の意図を正確に表すコードを記述できます。

プロパティは、アクセスされたときにはフィールドのように振る舞います。
ただし、フィールドとは異なり、プロパティの実装ではアクセサーを使用します。プロパティがアクセスされたときや値を割り当てられたときに実行されるステートメントをアクセサーで定義します。

## <a name="property-syntax"></a>プロパティの構文

プロパティの構文は、フィールドを自然に拡張したものです。 フィールドで格納場所を定義します。

[!code-csharp[Person class with public fields](../../samples/snippets/csharp/properties/Person.cs#1)]

プロパティの定義には、プロパティの値を取得する `get` アクセサーとプロパティに値を割り当てる `set` アクセサーの宣言が含まれます。

[!code-csharp[Person class with public properties](../../samples/snippets/csharp/properties/Person.cs#2)]

上記の構文は "*自動プロパティ*" の構文です。 コンパイラによって、プロパティをバックアップするフィールドの格納場所が生成されます。 また、`get` アクセサーと `set` アクセサーの本体もコンパイラによって実装されます。

場合によっては、その型の既定以外の値にプロパティを初期化する必要があります。  C# では、プロパティの右中かっこの後で値を設定することにより可能です。 `FirstName` プロパティの初期値は `null` より空の文字列の方がよい場合があります。 その場合は次に示すように指定します。

[!code-csharp[Person class with properties and initializer](../../samples/snippets/csharp/properties/Person.cs#3)]

この記事で後述するように、特定の初期化は読み取り専用プロパティに最も役に立ちます。

格納場所は、下に示すように、開発者が定義することもできます。

[!code-csharp[Person class with properties and backing field](../../samples/snippets/csharp/properties/Person.cs#4)]

プロパティの実装が 1 つの式の場合は、*式形式のメンバー*を get アクセス操作子または set アクセス操作子に使用できます。

[!code-csharp[Person class with properties and expression bodied getters and setters](../../samples/snippets/csharp/properties/Person.cs#5)]

この記事では、該当する箇所ではこの簡単な構文を使います。

上に示したプロパティの定義は、読み取り/書き込みプロパティです。 set アクセサーの `value` に注目してください。 `set` アクセサーには常に、`value` という名前のパラメーターが 1 つあります。 `get` アクセサーは、プロパティの型に変換可能な値を返す必要があります (この例では `string`)。

これが構文の基本です。 さまざまな設計手法をサポートするバリエーションが多数あります。 これらを詳しく確認しながら、各種シナリオに応じた構文の選択肢を見てみましょう。

## <a name="scenarios"></a>シナリオ

ここまでに示した例は、検証が行われない読み取り/書き込みプロパティという、プロパティ定義の中でも単純なものでした。 目的のコードを `get` アクセサーと `set` アクセサーで記述することで、さまざまなシナリオに対応できます。

### <a name="validation"></a>検証

`set` アクセサーにコードを記述すると、プロパティが表す値を常に有効な値にすることができます。 たとえば、`Person` クラスに設定されているルールの 1 つに、名前は無指定または空白文字にできないというものがあるとします。 これは次のように記述できます。

[!code-csharp[Validating property setters](../../samples/snippets/csharp/properties/Person.cs#6)]

上記の例は、プロパティ セッターの検証の一部として `throw` 式を使用して簡略化できます。

[!code-csharp[Validating property setters](../../samples/snippets/csharp/properties/Person.cs#7)]

上記の例では、名前を無指定または空白文字にしてはいけないというルールが強制的に適用されます。 もし開発者が下のように指定すると、

```csharp
hero.FirstName = "";
```

この割り当てに対して `ArgumentException` がスローされます。 プロパティの set アクセサーの戻り値は void でなければならないため、例外をスローすることで set アクセサーにエラーを報告します。

この構文を拡張して、シナリオに必要なあらゆる要素に対応できます。 たとえば、各種プロパティ間の関係をチェックしたり、外部条件に対して検証したりできます。 C# で有効なステートメントは、すべてプロパティ アクセサーでも有効です。

### <a name="read-only"></a>読み取り専用

ここまでのプロパティ定義はすべて、パブリック アクセサーを持つ読み取り/書き込みプロパティでした。 これ以外にも、プロパティに有効なアクセシビリティがあります。
たとえば、読み取り専用プロパティを作成したり、set アクセサーと get アクセサーに異なるアクセシビリティを設定したりすることができます。 具体例として、`Person` クラスで、クラス内の他のメソッドからのみ `FirstName` プロパティの値を変更できるようにしたい場合は、 set アクセサーのアクセシビリティを `private` ではなく `public` に設定します。

[!code-csharp[Using a private setter for a publicly readonly property](../../samples/snippets/csharp/properties/Person.cs#8)]

これで、`FirstName` プロパティにはどのコードからもアクセスできる一方で、値の割り当ては `Person` クラス内の他のコードからしかできなくなります。

制限を設定するアクセス修飾子を set アクセサーと get アクセサーのどちらか 1 つに追加することもできます。 個々のアクセサーには、プロパティ定義のアクセス修飾子よりも制限が強いアクセス修飾子を設定する必要があります。 上記は、`FirstName` プロパティが `public` ですが set アクセサーが `private` であるため、有効です。 `private` なアクセサーを持つ `public` なプロパティを宣言することはできません。 プロパティの宣言では、`protected`、`internal`、`protected internal`、`private` を宣言することもできます。

`get` アクセサーに制限の高い修飾子を設定することも有効です。 たとえば、`public` なプロパティで、`get` アクセサーを `private` に制限できます。 ただし、このようなシナリオは実際にはほとんどありません。

また、コンストラクターやプロパティの初期化子でのみ設定できるように、プロパティに対する変更を制限することもできます。 `Person` クラスを次のように変更することができます。

[!code-csharp[A readonly auto implemented property](../../samples/snippets/csharp/properties/Person.cs#9)]

この機能は、読み取り専用プロパティとして公開されるコレクションを初期化する場合に最もよく使われます。

```csharp
public class Measurements
{
    public ICollection<DataPoint> points { get; } = new List<DataPoint>();
}
```

### <a name="computed-properties"></a>計算されたプロパティ

プロパティが返す値は、メンバー フィールドの値でなくてもかまいません。 計算された値を返すプロパティを作成できます。 姓と名を連結する計算をしてフルネームを返すように `Person` オブジェクトを拡張してみましょう。

[!code-csharp[A computed property](../../samples/snippets/csharp/properties/Person.cs#10)]

上の例では、"[文字列補間](./language-reference/tokens/interpolated.md)" 機能を使用して、フルネームを表す書式設定された文字列を作成しています。

*式形式のメンバー*を使用することもできます。式形式のメンバーを使用すると、計算された `FullName` プロパティを簡潔な方法で作成できます。

[!code-csharp[A computed property using an expression bodied member](../../samples/snippets/csharp/properties/Person.cs#11)]

*式形式のメンバー*では、式が 1 つだけ含まれたメソッドを定義する*ラムダ式*構文を使用します。 ここでは、その式が Person オブジェクトのフルネームを返しています。

### <a name="cached-evaluated-properties"></a>キャッシュ済みの評価されたプロパティ

計算されたプロパティの概念をストレージと組み合わせて、*キャッシュ済みの評価されたプロパティ*を作成できます。  たとえば、`FullName` プロパティを更新して、プロパティが最初にアクセスされたときに文字列が書式設定されるようにすることができます。

[!code-csharp[Caching the value of a computed property](../../samples/snippets/csharp/properties/Person.cs#12)]

ただし、上記のコードにはバグが含まれています。 コードによって `FirstName` プロパティと `LastName` プロパティのいずれかの値が更新されると、以前に評価された `fullName` フィールドは無効になります。 `set` フィールドが再計算されるように、`FirstName` プロパティと `LastName` プロパティの `fullName` アクセサーを変更します。

[!code-csharp[Invalidating the cache correctly](../../samples/snippets/csharp/properties/Person.cs#13)]

上の最終版では、必要になった場合にのみ `FullName` プロパティが評価されます。
以前に計算されたものが有効であれば、それが使用されます。 状態が変化したことで、以前に計算されたバージョンが無効になると、再計算が行われます。 このクラスを使用するにあたって、開発者は実装の詳細を知っている必要はありません。 内部で変化があっても Person オブジェクトの使用には影響しません。 これが、プロパティを使用してオブジェクトのデータ メンバーを公開するする重要な利点です。

### <a name="attaching-attributes-to-auto-implemented-properties"></a>自動実装プロパティに属性をアタッチする

C# 7.3 以降、自動実装プロパティのコンパイラの生成したバッキング フィールドにフィールド属性をアタッチできるようになりました。 たとえば、一意の整数 `Person` プロパティを追加する `Id` クラスのリビジョンについて考えてみましょう。
自動実装プロパティを使用して `Id` プロパティを記述しますが、この設計では `Id` プロパティの永続化を呼び出しません。 <xref:System.NonSerializedAttribute> は、プロパティではなく、フィールドにのみアタッチすることができます。 次の例のように、属性に対して <xref:System.NonSerializedAttribute> 指定子を使用して `Id` プロパティのバッキング フィールドに `field:` をアタッチできます。

[!code-csharp[Attaching attributes to a backing field](../../samples/snippets/csharp/properties/Person.cs#14)]

この手法は、自動実装プロパティのバッキング フィールドにアタッチする任意の属性に利用できます。

### <a name="implementing-inotifypropertychanged"></a>INotifyPropertyChanged を実装する

プロパティ アクセサーでコードを記述する必要があるシナリオとして、値が変更されたことをデータ バインディング クライアントに通知するための <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスのサポートというものもあります。 プロパティの値が変更されると、オブジェクトはその変更を示す <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged?displayProperty=nameWithType> イベントを発生させます。 データ バインディング ライブラリは、その変更に基づいて表示要素を更新します。 下のコードは、この Person クラスの `INotifyPropertyChanged` プロパティに `FirstName` を実装する方法を示しています。

[!code-csharp[invalidating the cache correctly](../../samples/snippets/csharp/properties/Person.cs#15)]

`?.` は "*null 条件演算子*" と呼ばれる演算子です。 演算子の右側を評価する前に、null 参照がないかをチェックします。 チェックの結果、`PropertyChanged` イベントに対するサブスクライバーがない場合は、イベントを発生させるコードは実行されません。 その場合、評価は行われず `NullReferenceException` がスローされます。 詳細については、[`events`](events-overview.md) を参照してください。 上記の例では、新たに `nameof` という演算子を使用して、記号としてのプロパティ名を文字列に変換しています。
`nameof` を使用すると、プロパティ名にタイプミスが含まれるというエラーを減らすことができます。

<xref:System.ComponentModel.INotifyPropertyChanged> の実装も、アクセサーでコードを記述することで目的のシナリオをサポートできるケースの一例です。

## <a name="summing-up"></a>まとめ

プロパティは、クラスまたはオブジェクトに含まれた一種のスマート フィールドです。 オブジェクトの外部からは、オブジェクト内にあるフィールドのように見えます。 一方、プロパティは、C# の機能をどれでも自由に使用して実装できます。
検証、各種アクセシビリティ、遅延評価など、目的のシナリオで必要となる要素はすべて提供できます。
