---
title: ref 戻り値と ref ローカル変数 (C# ガイド)
description: ref 戻り値と ref ローカル変数を定義して使用する方法について説明します。
ms.date: 04/04/2018
ms.openlocfilehash: 87a9538db60d69062f0fb48ed9683a9d4f972b91
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170074"
---
# <a name="ref-returns-and-ref-locals"></a>ref 戻り値と ref ローカル変数

C# 7.0 以降の C# は参照戻り値 (ref 戻り値) に対応しています。 参照戻り値を使用すると、メソッドから呼び出し元に、値ではなく、変数への参照を返すことができます。 呼び出し元は、返された変数を値渡しとして処理するか、参照渡しとして処理するかを選択できます。 呼び出し元は、それ自体が、返された値の参照である新しい変数を作成できます。これは ref ローカルと呼ばれています。

## <a name="what-is-a-reference-return-value"></a>参照戻り値とは

呼び出されたメソッドに参照によって引数を渡す*参照渡し*は、ほとんどの開発者によく知られています。 呼び出されたメソッドの引数リストには、参照によって渡された変数が含まれています。 呼び出されたメソッドがその値に対して行った変更は、呼び出し元によって観察されます。 "*参照戻り値*" とは、メソッドが何らかの変数への "*参照*" (または別名) を返すことを意味します。 その変数のスコープには、メソッドが含まれている必要があります。 その変数の有効期間は、メソッドから戻った後まで継続している必要があります。 呼び出し元によるメソッドの戻り値の変更は、メソッドによって返される変数に対して行われます。

メソッドが*参照戻り値*を返すという宣言があれば、それはそのメソッドが変数にエイリアスを返すことを示します。 この設計の意図は、多くの場合、呼び出し元のコードが変数の変更などを行うとき、そのコードにはエイリアス経由でその変数にアクセスさせるというものです。 参照で返すメソッドには戻り値の型 `void` を与えることができません。

メソッドが参照戻り値として返すことができる式には、いくつかの制限があります。 次のような制約があります。

- 戻り値の有効期間は、メソッドの実行より長くならないようにする必要があります。 言い換えると、値を返すメソッドに含まれているローカル変数にすることはできません。 クラスのインスタンスまたは静的フィールドを戻り値にすることができます。または、メソッドに渡される引数を戻り値にすることもできます。 ローカル変数を返そうとすると、コンパイラ エラー CS8168 "ローカル変数 'obj' は ref ローカル変数ではないため、参照渡しで返すことはできません" が生成されます。

- 戻り値はリテラル `null` にすることができません。 `null` を返すと、コンパイラ エラー CS8156 "参照渡しで返すことができないため、このコンテキストで使用できない式があります" が生成されます。

   ref 戻り値があるメソッドは、現在の値が null (インスタンス化されていない) 値か、値の型が [null 許容値型](../../language-reference/builtin-types/nullable-value-types.md)の変数にエイリアスを返すことができます。

- 戻り値は、定数、列挙型のメンバー、プロパティの値渡し戻り値、`class` または `struct` のメソッドにすることができません。 この規則に違反すると、コンパイラ エラー CS8156 "参照渡しで返すことができないため、このコンテキストで使用できない式があります" が生成されます。

さらに、参照戻り値は非同期メソッドでは許可されません。 非同期メソッドは実行が終了する前に戻る可能性があり、戻り値はまだ不明です。

## <a name="defining-a-ref-return-value"></a>ref 戻り値の定義

"*参照戻り値*" を返すメソッドは、次の 2 つの条件を満たす必要があります。

- メソッドのシグネチャで、戻り値の型の前にキーワード [ref](../../language-reference/keywords/ref.md) が含まれること。
- メソッド本体の各 [return](../../language-reference/keywords/return.md) ステートメントで、返されるインスタンスの名前の前に [ref](../../language-reference/keywords/ref.md) キーワードが含まれること。

これらの条件を満たし、`Person` という名前の `p` オブジェクトへの参照を返すメソッドを、次の例に示します。

```csharp
public ref Person GetContactInformation(string fname, string lname)
{
    // ...method implementation...
    return ref p;
}
```

## <a name="consuming-a-ref-return-value"></a>ref 戻り値の使用

ref 戻り値は、呼び出されるメソッドの範囲で、別の変数のエイリアスになります。 ref 戻り値の使用は、それが別名を与える変数の使用として解釈できます。

- その値を割り当てるとき、それが別名を与える変数に値を割り当てることになります。
- その値を読み取るとき、それが別名を与える変数の値を読み取ることになります。
- "*参照渡し*" で値を返す場合、その同じ変数の別名を返すことになります。
- "*参照渡し*" で別のメソッドに値を渡す場合、それが別名を与える変数への参照を渡すことになります。
- [ref ローカル](#ref-locals)をエイリアスにすると、同じ変数に新しいエイリアスが作られます。

## <a name="ref-locals"></a>ref ローカル変数

`GetContactInformation` メソッドが ref 戻り値として宣言されているとします。

```csharp
public ref Person GetContactInformation(string fname, string lname)
```

値渡し代入によって、変数の値が読み取られ、それが新しい変数に渡されます。

```csharp
Person p = contacts.GetContactInformation("Brandie", "Best");
```

先の代入では、ローカル変数として `p` が宣言されています。 その初期値は、`GetContactInformation` によって返された値の読み取りからコピーされます。 今後、`p` に値が代入されることで、`GetContactInformation` によって返された変数の値が変わることはありません。 変数 `p` は、返された変数のエイリアスではなくなります。

*ref ローカル*変数を宣言し、元の値にエイリアスをコピーします。 次の代入では、`p` は、`GetContactInformation` から返された変数のエイリアスになります。

```csharp
ref Person p = ref contacts.GetContactInformation("Brandie", "Best");
```

この後 `p` を使用することは、`GetContactInformation` によって返された変数を使用することと同じです。`p` はその変数のエイリアスであるためです。 `p` を変更すると、`GetContactInformation` から返される変数も変更されます。

`ref` キーワードは、ローカル変数宣言の前 "*および*" メソッド呼び出しの前の両方で使用します。

同じ方法で、参照渡しの値にアクセスできます。 場合によっては、参照渡しの値へのアクセスによって負荷がかかる可能性があるコピー操作が回避され、パフォーマンスが向上します。 たとえば、次のステートメントは、値の参照に使用される ref ローカル値をどのように定義できるかを示しています。

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct;
```

`ref` キーワードは、ローカル変数宣言の前 "*および*" 2 番目の例の値の前で使用します。 両方の例の、変数宣言と代入の両方の `ref` キーワードを含めないと、コンパイラ エラー CS8172 "値を使用して参照渡し変数を初期化することはできません" が生成されます。

C# 7.3 より前は、初期化後に別の記憶域を参照するように ref ローカル変数を割り当て直すことはできませんでした。 この制限はなくなりました。 再割り当ての例を次に示します。

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

 ref ローカル変数は、宣言時にやはり初期化する必要があります。

## <a name="ref-returns-and-ref-locals-an-example"></a>ref 戻り値と ref ローカル変数: 使用例

次の例では、整数値の配列を格納する `NumberStore` クラスを定義しています。 `FindNumber` メソッドは、引数として渡された数値に等しいかそれより大きい最初の数値を参照渡しで返します。 引数に等しいかそれより大きい数値がない場合、メソッドはインデックス 0 の数値を返します。

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStore.cs#1)]

次の例では、`NumberStore.FindNumber` メソッドを呼び出して、16 に等しいかそれより大きい最初の値を取得します。 呼び出し元は、メソッドによって返された値を 2 倍にします。 次の例の出力では、`NumberStore` インスタンスの配列要素の値に変更が反映されたことが示されています。

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStore.cs#2)]

参照戻り値がサポートされていない場合、このような操作は、配列要素のインデックスと値を返すことによって実行されます。 呼び出し元はこのインデックスを使用して、別のメソッド呼び出しで値を変更できます。 一方、インデックスを変更して配列の他の値にアクセスし、変更することも可能です。  

次の例では、C# 7.3 以降で ref ローカル変数の再割り当てを使用するように `FindNumber` メソッドを書き直す方法を示します。

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStoreUpdated.cs#1)]

この 2 番目のバージョンは、シークされる値が配列の末尾近くにあるようなシナリオのシーケンスが長い場合に、より効率的です。

## <a name="see-also"></a>参照

- [ref キーワード](../../language-reference/keywords/ref.md)
- [安全で効率的なコードを記述する](../../write-safe-efficient-code.md)
