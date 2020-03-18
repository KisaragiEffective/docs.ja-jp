---
title: out パラメーター修飾子 - C# リファレンス
ms.date: 03/26/2019
helpviewer_keywords:
- parameters [C#], out
- out parameters [C#]
ms.openlocfilehash: f963188d77685bb81f7dc9fb3794e343114fe3c0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173563"
---
# <a name="out-parameter-modifier-c-reference"></a>out パラメーター修飾子 (C# リファレンス)
`out` キーワードによって、参照により引数が渡されます。 仮パラメーターを引数 (変数にする必要があります) の別名にします。 つまり、パラメーターに対するすべての操作は引数に対して行われます。 これは、[ref](ref.md) キーワードと似ていますが、`ref` では、変数を初期化してから渡す必要があります。 [in](in-parameter-modifier.md) キーワードとも似ていますが、`in` では、呼び出されたメソッドで引数の値を変更することはできません。 `out` パラメーターを使用するには、メソッド定義と呼び出し元のメソッドの両方で `out` キーワードを明示的に使用する必要があります。 次に例を示します。  
  
[!code-csharp-interactive[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#1)]  

> [!NOTE]
> `out` キーワードは、ジェネリック型パラメーターと共に使用すると、型パラメーターが共変であることを指定することもできます。 このコンテキストでの `out` キーワードの使用方法の詳細については、「[out (ジェネリック修飾子)](out-generic-modifier.md)」を参照してください。
  
`out` の引数として渡される変数は、メソッド呼び出しで渡される前に初期化する必要はありません。 ただし、呼び出されたメソッドでは、メソッドから制御が返される前に値を割り当てる必要があります。  
  
`in`、`ref`、および `out` キーワードは、オーバーロード解決のためのメソッド シグネチャの一部とは見なされません。 したがって、唯一の違いが、1 つのメソッドは `ref` または `in` 引数を受け取り、もう一方のメソッドは `out` 引数を受け取ることである場合、メソッドはオーバーロードできません。 たとえば、次のコードはコンパイルされません。  
  
```csharp
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
}
```
  
ただし、一方のメソッドが `ref`、`in`、または `out` 引数を受け取り、もう一方のメソッドにいずれの修飾子もない場合は、オーバーロードを実行できます。この例を次に示します。  
  
[!code-csharp[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#2)]  

コンパイラは、呼び出しサイトのパラメーター修飾子と、メソッド呼び出しで使用されるパラメーター修飾子を照合して、最適なオーバーロードを選択します。

プロパティは変数ではないため、`out` パラメーターとして渡すことはできません。
  
次の種類のメソッドには、`in`、`ref`、`out` キーワードを使用することはできません。  
  
- [async](./async.md) 修飾子を使用して定義した Async メソッド。  
  
- [yield return](./yield.md) または `yield break` ステートメントを含む Iterator メソッド。  

## <a name="declaring-out-parameters"></a>`out` パラメーターの宣言

`out` 引数を含むメソッドの宣言は、複数の値を返すための従来の回避策です。 C#7.0 以降、同様のシナリオでは[タプル](../../tuples.md)を検討してください。 次の例では `out` を使用して、1 つのメソッド呼び出しで 3 つの変数を返します。 3 番目の引数が null に割り当てられることに注意してください。 これにより、必要に応じてメソッドが値を返すことができます。  
  
[!code-csharp-interactive[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#3)]  

## <a name="calling-a-method-with-an-out-argument"></a>`out` 引数を含むメソッドの呼び出し

C# 6 以前では、変数を別のステートメントで宣言してから `out` 引数として渡す必要があります。 次の例では、`number` という名前の変数を宣言してから、文字列を数値に変換する [Int32.TryParse](xref:System.Int32.TryParse(System.String,System.Int32@)) メソッドに渡しています。

[!code-csharp-interactive[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#4)]  

C# 7.0 以降では、`out` 変数を、別の変数宣言内ではなく、メソッド呼び出しの引数リスト内で宣言できます。 これにより、よりコンパクトで読みやすいコードが生成されます。また、メソッド呼び出しの前に誤って変数に値を割り当てることもなくなります。 次の例は前の例と似ていますが、`number`Int32.TryParse[ メソッドの呼び出しで ](xref:System.Int32.TryParse(System.String,System.Int32@)) 変数を定義している点が異なります。

[!code-csharp-interactive[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#5)]  

前の例では、`number` 変数は `int` として厳密に型指定されています。 次の例のように、暗黙的に型指定されたローカル変数を宣言することもできます。

[!code-csharp-interactive[cs-out-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/OutParameterModifier.cs#6)]  

## <a name="c-language-specification"></a>C# 言語仕様  
[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](./index.md)
- [メソッド パラメーター](./method-parameters.md)
