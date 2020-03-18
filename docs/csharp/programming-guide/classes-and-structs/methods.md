---
title: メソッド - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- methods [C#]
- C# language, methods
ms.assetid: cc738f07-e8cd-4683-9585-9f40c0667c37
ms.openlocfilehash: 114fa2973c50be9a4199db9729e3cd9ea6122866
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77626530"
---
# <a name="methods-c-programming-guide"></a>メソッド (C# プログラミング ガイド)

メソッドは、一連のステートメントが含まれているコード ブロックです。 必要なメソッド引数を指定してプログラムからメソッドを呼び出すと、メソッド内のステートメントが実行されます。 C# では、実行されるすべての命令がメソッドのコンテキストで実行されます。 `Main` メソッドは、すべての C# アプリケーションのエントリ ポイントです。プログラムが開始されると、このメソッドが共通言語ランタイム (CLR) によって呼び出されます。

> [!NOTE]
> この記事では、名前付きメソッドについて説明します。 匿名関数については、「[匿名関数](../statements-expressions-operators/anonymous-functions.md)」を参照してください。

## <a name="method-signatures"></a>メソッド シグネチャ

メソッドは、[クラス](../../language-reference/keywords/class.md)、[構造体](../../language-reference/builtin-types/struct.md)、または[インターフェイス](../interfaces/index.md)内で、アクセス レベル (`public` や `private` など)、オプションの修飾子 (`abstract` や `sealed` など)、戻り値、メソッドの名前、およびメソッド パラメーターを指定して宣言されます。 これらのまとまりがメソッドのシグネチャとなります。

> [!NOTE]
> メソッドのオーバーロードを可能にするために、メソッドの戻り値の型はメソッドのシグネチャには含まれません。 ただし、デリゲートとそれが指すメソッドの互換性を決定する場合には、メソッドのシグネチャの一部となります。

メソッド パラメーターはかっこで囲み、各パラメーターをコンマで区切ります。 かっこ内を空にすると、メソッドでパラメーターが不要なことを意味します。 このクラスには次の 4 つのメソッドが含まれています。

[!code-csharp[csProgGuideObjects#40](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#40)]

## <a name="method-access"></a>メソッド アクセス

オブジェクトでメソッドを呼び出すのは、フィールドにアクセスするのと似ています。 オブジェクト名の後に、ピリオド、メソッド名、かっこを追加します。 引数はかっこの中に記述し、コンマで区切ります。 `Motorcycle` クラスのメソッドの呼び出し例を次に示します。

[!code-csharp[csProgGuideObjects#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#41)]

## <a name="method-parameters-vs-arguments"></a>メソッドのパラメーターと引数

メソッド定義には、必要なパラメーターの名前と型を指定します。 呼び出し元のコードからメソッドを呼び出すときに、各パラメーターに引数と呼ばれる具体的な値を指定します。 引数にはパラメーター型との互換性が必要ですが、呼び出し元のコードで引数名を使用する場合、引数名がメソッドで定義されるパラメーター名と同じである必要はありません。 次に例を示します。

[!code-csharp[csProgGuideObjects#74](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#74)]

## <a name="passing-by-reference-vs-passing-by-value"></a>参照渡しと値渡し

既定では、[値の型](../../language-reference/builtin-types/value-types.md)のインスタンスがメソッドに渡されるときは、インスタンス自体ではなく、そのコピーが渡されます。 したがって、引数に加えた変更は、呼び出し元のメソッドにある元のインスタンスには影響しません。 値の型インスタンスを参照で渡すには、`ref` キーワードを使用します。 詳細については、「[値型パラメーターの引き渡し](./passing-value-type-parameters.md)」を参照してください。

参照型のオブジェクトがメソッドに渡されると、オブジェクトへの参照が渡されます。 つまり、メソッドは、オブジェクト自体ではなく、オブジェクトの場所を示す引数を受け取ります。 この参照を使用してオブジェクトのメンバーを変更した場合は、オブジェクトを値で渡しても、呼び出し元のメソッドの引数に変更が反映されます。

`class` キーワードを使用して参照型を作成する例を次に示します。

[!code-csharp[csProgGuideObjects#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#42)]

この型に基づくオブジェクトをメソッドに渡す場合は、オブジェクトへの参照が渡されます。 次の例では、`SampleRefType` 型のオブジェクトをメソッド `ModifyObject`に渡します。

[!code-csharp[csProgGuideObjects#75](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#75)]

この例は、基本的に前の例と同様に、引数を値でメソッドに渡しています。 しかし、参照型を使用しているため、結果は異なります。 `ModifyObject` のパラメーター `value` の `obj`フィールドで行われた変更によって、 `value` メソッドの引数 `rt`の `TestRefType` フィールドも変更されます。 `TestRefType` メソッドは出力として 33 を表示します。

参照型を参照渡しまたは値渡しで渡す方法の詳細については、「[参照型パラメーターの引き渡し](./passing-reference-type-parameters.md)」と「[参照型](../../language-reference/keywords/reference-types.md)」を参照してください。

## <a name="return-values"></a>戻り値

メソッドは、呼び出し元に値を返すことができます。 戻り値の型 (メソッド名の前に記述されている型) が `void`でない場合、メソッドは、 `return` キーワードを使用して値を返すことができます。 `return` キーワードに続いて戻り値の型に一致する値が記述されたステートメントは、その値をメソッドの呼び出し元に返します。

値を呼び出し元に返す方法には、値によって返す方法と、C# 7.0 以降の[参照](ref-returns.md)によって返す方法があります。 値が参照によって呼び出し元に返されるのは、`ref` キーワードがメソッド シグネチャで使用されていて、そのキーワードが各 `return` キーワードの後に続いている場合です。 たとえば、次のメソッド シグネチャと return ステートメントは、メソッドが変数名 `estDistance` を参照によって呼び出し元に返すことを示しています。

```csharp
public ref double GetEstimatedDistance()
{
    return ref estDistance;
}
```

また、 `return` キーワードは、メソッドの実行を中止します。 戻り値の型が `void`の場合、値を持たない `return` ステートメントは、メソッドの実行を中止するときに役立ちます。 `return` キーワードを使用しない場合、メソッドは、コード ブロックの最後に到達したときに実行を中止します。 戻り値の型が void 以外のメソッドで値を返すには、 `return` キーワードを使用する必要があります。 たとえば、次の 2 つのメソッドは、 `return` キーワードを使用して整数を返します。

[!code-csharp[csProgGuideObjects#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#44)]

メソッドから返された値を使用する場合、呼び出し元のメソッド内で同じ型の値を使用している場所では、メソッド呼び出し自体を値として使用できます。 戻り値は、変数に代入することもできます。 たとえば、次の 2 つのコードでは、同様の結果が得られます。

[!code-csharp[csProgGuideObjects#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#45)]

[!code-csharp[csProgGuideObjects#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#46)]

この場合、ローカル変数 `result`を使用して値を格納する手順はオプションです。 このローカル変数によってコードの読みやすさが向上することがあります。また、引数の元の値をメソッドのスコープ全体で保持する場合に必要になることがあります。

メソッドから参照によって返された値を使用する場合、値を変更するには、[ref ローカル](ref-returns.md#ref-locals)変数を宣言する必要があります。 たとえば、`Planet.GetEstimatedDistance` メソッドが <xref:System.Double> の値を参照によって返す場合は、次のようなコードを使用して、その値を ref ローカル変数として定義できます。

```csharp
ref int distance = plant
```

呼び出し元の関数から、配列の内容を変更するメソッド `M` に配列が渡された場合、`M` から多次元配列を返す必要はありません。  値の適切なスタイルまたは機能フローのために `M` から結果の配列を返すことはできますが、必須ではありません。変更された配列を返す必要がないのは、C# ではすべての参照型が値で渡され、配列参照の値がその配列へのポインターになるためです。 メソッド `M` では、次の例に示すように、配列の内容に対する変更は、配列への参照を含むコードによって監視できます。

```csharp
static void Main(string[] args)
{
    int[,] matrix = new int[2, 2];
    FillMatrix(matrix);
    // matrix is now full of -1
}

public static void FillMatrix(int[,] matrix)
{
    for (int i = 0; i < matrix.GetLength(0); i++)
    {
        for (int j = 0; j < matrix.GetLength(1); j++)
        {
            matrix[i, j] = -1;
        }
    }
}
```

詳細については、「 [return](../../language-reference/keywords/return.md)」を参照してください。

## <a name="async-methods"></a>非同期メソッド

非同期機能を使用することによって、明示的なコールバックを使用せずに、または複数のメソッドやラムダ式にわたって手動でコードを分割することなく、非同期メソッドを呼び出すことができます。

メソッドに [async](../../language-reference/keywords/async.md) 修飾子を付けると、そのメソッドで [await](../../language-reference/operators/await.md) 演算子を使用できます。 コントロールが非同期メソッドの await 式に到達すると、コントロールは呼び出し元に戻り、待機中のタスクが完了するまでメソッドの進行状況は中断されます。 タスクが完了すると、メソッドで実行を再開できます。

> [!NOTE]
> 非同期メソッドは、まだ完了していない待機中の最初のオブジェクトに達するか、または非同期メソッドの最後に達すると、呼び出し元に戻ります。

非同期メソッドの戻り値の型としては、 <xref:System.Threading.Tasks.Task%601>、 <xref:System.Threading.Tasks.Task>、または void を指定できます。 戻り値の型 void は主として、void の戻り値の型が必要なイベント ハンドラーの定義に使用されます。 void を返す非同期メソッドは待機できません。void を返すメソッドの呼び出し元は、このメソッドがスローする例外をキャッチできません。

次の例で、 `DelayAsync` は戻り値の型が <xref:System.Threading.Tasks.Task%601>である非同期メソッドです。 `DelayAsync` には、整数を返す `return` ステートメントがあります。 そのため、メソッド宣言 `DelayAsync` では、戻り値の型を `Task<int>`とする必要があります。 戻り値の型が `Task<int>`であるため、ステートメント `await` に示すように、 `DoSomethingAsync` 内の `int result = await delayTask`式を評価すると整数が生成されます。

`startButton_Click` メソッドは、戻り値の型が void の非同期メソッドの例です。 `DoSomethingAsync` が非同期メソッドであるため、 `DoSomethingAsync` を呼び出すタスクは、ステートメント `await DoSomethingAsync();`に示すように待機する必要があります。 `startButton_Click` メソッドでは `async` 式が使用されているため、 `await` 修飾子を使用して定義する必要があります。

[!code-csharp[csAsyncMethod#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csasyncmethod/cs/mainwindow.xaml.cs#2)]

非同期メソッドで [ref](../../language-reference/keywords/ref.md) パラメーターまたは [out](../../language-reference/keywords/out-parameter-modifier.md) パラメーターを宣言することはできませんが、これらのパラメーターが含まれたメソッドを呼び出すことはできます。

非同期メソッドの詳細については、「[Async および Await を使用した非同期プログラミング](../concepts/async/index.md)」、「[非同期プログラムにおける制御フロー](../concepts/async/control-flow-in-async-programs.md)」、「[非同期の戻り値の型](../concepts/async/async-return-types.md)」を参照してください。

## <a name="expression-body-definitions"></a>式本体の定義

メソッドの定義としては、式の結果を即座に返すか、またはメソッドの本文として 1 つのステートメントを含むものが一般的です。 `=>`を使用してこのようなメソッドを定義するための構文ショートカットがあります。

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

メソッドが `void` を返すか、非同期メソッドである場合は、メソッドの本文を (ラムダの場合と同様に) ステートメント式にする必要があります。 プロパティとインデクサーは読み取り専用にする必要があるため、 `get` アクセサー キーワードは使用しないでください。

## <a name="iterators"></a>反復子

反復子は、リストや配列など、コレクションに対するカスタム イテレーションを実行します。 反復子は、[yield return](../../language-reference/keywords/yield.md) ステートメントを使用して、各要素を 1 回に 1 つ返します。 [yield return](../../language-reference/keywords/yield.md) ステートメントに達すると、コードの現在の場所が記憶されます。 反復子が次回呼び出されたとき、この場所から実行が再開されます。

[foreach](../../language-reference/keywords/foreach-in.md) ステートメントを使用して、クライアント コードから反復子を呼び出します。

反復子の戻り値の型には、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601> を指定できます。

詳細については、「 [反復子](../concepts/iterators.md)」を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](index.md)
- [アクセス修飾子](access-modifiers.md)
- [静的クラスと静的クラス メンバー](static-classes-and-static-class-members.md)
- [継承](inheritance.md)
- [抽象クラスとシール クラス、およびクラス メンバー](abstract-and-sealed-classes-and-class-members.md)
- [params](../../language-reference/keywords/params.md)
- [return](../../language-reference/keywords/return.md)
- [out](../../language-reference/keywords/out.md)
- [ref](../../language-reference/keywords/ref.md)
- [パラメーターの引き渡し](passing-parameters.md)
