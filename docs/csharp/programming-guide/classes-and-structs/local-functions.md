---
title: ローカル関数 - C# プログラミング ガイド
ms.date: 06/14/2017
helpviewer_keywords:
- local functions [C#]
ms.openlocfilehash: b6924b8981af5115a474eeb6b2e5376dd1b17ff5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170236"
---
# <a name="local-functions-c-programming-guide"></a>ローカル関数 (C# プログラミング ガイド)

C# 7.0 以降、C# では*ローカル関数*がサポートされています。 ローカル関数は、別のメンバーの入れ子になっているタイプのプライベート メソッドです。 親メンバーからのみ呼び出すことができます。 ローカル関数は次の要素で宣言し、呼び出すことができます。

- メソッド (特に反復子メソッドと非同期メソッド)
- コンストラクター
- プロパティ アクセサー
- イベント アクセサー
- 匿名メソッド
- ラムダ式
- ファイナライザー
- その他のローカル関数

ただし、ローカル関数は、式形式のメンバーの内部では宣言できません。

> [!NOTE]
> 場合によっては、ラムダ式を使用して、ローカル関数でもサポートされている機能を実装できます。 比較については、「[ローカル関数とラムダ式の比較](../../local-functions-vs-lambdas.md)」を参照してください。

ローカル関数を使用すると、コードの意図が明確になります。 コードを見た人は、メソッドが親メソッドによってのみ呼び出し可能であることがわかります。 また、チーム プロジェクトの場合は、別の開発者がクラスや構造体の別の場所から誤ってメソッドを直接呼び出すことができなくなります。

## <a name="local-function-syntax"></a>ローカル関数の構文

ローカル関数は、親メンバーの内側に、入れ子になったメソッドとして定義されます。 その定義の構文は次のとおりです。

```csharp
<modifiers: async | unsafe> <return-type> <method-name> <parameter-list>
```

ローカル関数では、[async](../../language-reference/keywords/async.md) および [unsafe](../../language-reference/keywords/unsafe.md) 修飾子を使用できます。

メソッドのパラメーターを含め、親メンバーに定義されたすべてのローカル変数は、ローカル関数からアクセス可能です。

メソッド定義とは異なり、ローカル関数の定義にメンバー アクセス修飾子を含めることはできません。 すべてのローカル関数はプライベートであるため、`private` キーワードなどのアクセス修飾子が含まれていると、コンパイラ エラー CS0106 "修飾子 'private' がこの項目に対して有効ではありません" が生成されます。

> [!NOTE]
> C# 8.0 より前では、ローカル関数に `static` 修飾子を含めることはできません。 `static` キーワードが含まれていると、コンパイラ エラー CS0106 "修飾子 'static' がこの項目に対して有効ではありません" が生成されます。

さらに、ローカル関数またはローカル関数のパラメーターと型パラメーターには属性を適用できません。

次の例は、`AppendPathSeparator` というメソッドに対してプライベートな `GetText` というローカル関数を定義しています。

[!code-csharp[LocalFunctionExample](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions1.cs)]  

## <a name="local-functions-and-exceptions"></a>ローカル関数と例外

ローカル関数の便利な機能の 1 つは、例外を直ちに検出できることです。 メソッド反復子の場合、例外は返されたシーケンスを列挙する時点でしか検出されず、反復子を取得した時点では検出されません。 非同期メソッドの場合、非同期メソッドでスローされた例外は、タスクの戻りを待機中に検出されます。

次の例は、指定した範囲にある奇数を列挙する `OddSequence` メソッドを定義しています。 100 より大きい数値を `OddSequence` 列挙子メソッドに渡しているため、メソッドは <xref:System.ArgumentOutOfRangeException> をスローします。 この例の出力が示すように、例外は列挙子を取得したときではなく、数値を反復処理した時点でのみ検出されます。

[!code-csharp[LocalFunctionIterator1](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-iterator1.cs)]

これに対して、次の例に示すようにローカル関数から列挙子を返すことによって、列挙子を取得する前の検証を実行する時点で例外をスローできます。

[!code-csharp[LocalFunctionIterator2](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-iterator2.cs)]

ローカル関数は、非同期操作の外部で例外を処理するために同様の方法で使用できます。 通常、非同期メソッドでスローされた例外では <xref:System.AggregateException> の内部例外を確認する必要があります。 ローカル関数を使用すると、コードを迅速に失敗させ (Fail Fast)、例外のスローと検出の両方を同時に行うことができます。

次の例は、`GetMultipleAsync` という非同期メソッドを使用して、指定した秒数だけ一時停止した後、その秒数のランダムな倍数である値を返します。 遅延の最大値は 5 秒です。値が 5 より大きい場合は <xref:System.ArgumentOutOfRangeException> が発生します。 次の例に示すように、`GetMultipleAsync` メソッドに渡された値が 6 の場合にスローされる例外は、<xref:System.AggregateException> メソッドの実行開始後、`GetMultipleAsync` にラップされます。

[!code-csharp[LocalFunctionAsync](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-async1.cs)]

メソッド反復子と同様、非同期メソッドを呼び出す前に検証を実行するように、この例のコードをリファクターすることができます。 次の例の出力に示されているように、<xref:System.ArgumentOutOfRangeException> は <xref:System.AggregateException> にラップされていません。

[!code-csharp[LocalFunctionAsync](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-async2.cs)]

## <a name="see-also"></a>参照

- [メソッド](methods.md)
