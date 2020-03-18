---
title: async - C# リファレンス
ms.date: 05/22/2017
f1_keywords:
- async_CSharpKeyword
helpviewer_keywords:
- async keyword [C#]
- async method [C#]
- async [C#]
ms.assetid: 16f14f09-b2ce-42c7-a875-e4eca5d50674
ms.openlocfilehash: 92e94d6fe1c07ab5cd8f29d040401a737a1db78e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173654"
---
# <a name="async-c-reference"></a>async (C# リファレンス)

`async` 修飾子を使用して、メソッド、[ラムダ式](../../programming-guide/statements-expressions-operators/lambda-expressions.md)、または[匿名メソッド](../operators/delegate-operator.md)が非同期であることを指定します。 この修飾子が使用されているメソッドまたは式を、"*非同期メソッド*" と呼びます。 次の例では、`ExampleMethodAsync` という名前の非同期メソッドを定義します。
  
```csharp  
public async Task<int> ExampleMethodAsync()  
{  
    // . . . .  
}  
```  

非同期プログラミングに慣れていない場合、または、非同期メソッドで [`await` 演算子](../operators/await.md)を使って、実行時間が長くなる可能性のある処理を、呼び出し元のスレッドをブロックすることなく実行する方法を理解していない場合は、「[async および await を使用した非同期プログラミング](../../programming-guide/concepts/async/index.md)」の概要を参照してください。 次のコードは、非同期メソッド内のコードで、<xref:System.Net.Http.HttpClient.GetStringAsync%2a?displayProperty=nameWithType> メソッドを呼び出します。
  
```csharp  
string contents = await httpClient.GetStringAsync(requestUrl);  
```  
  
非同期メソッドは、最初の `await` 式に到達するまで同期的に実行されますが、この時点で、待機していたタスクが完了するまで中断されます。 次のセクションの例で示すように、その間はメソッドの呼び出し元に制御が戻ります。  
  
`async` キーワードで修飾されているメソッドに `await` 式またはステートメントが含まれていない場合、メソッドは同期的に実行されます。 `await` ステートメントが含まれていない非同期メソッドが存在する場合は、その状態がエラーを示す可能性があるため、コンパイラによって警告が通知されます。 「[コンパイラの警告 (レベル 1) CS4014](../compiler-messages/cs4014.md)」をご覧ください。  
  
 `async` は、メソッド、ラムダ式、または匿名メソッドを修飾する場合にのみキーワードとなるため、コンテキスト キーワードです。 それ以外の場合は、識別子として解釈されます。  
  
## <a name="example"></a>例  
次の例は、非同期のイベント ハンドラー `StartButton_Click` と非同期メソッド `ExampleMethodAsync` との間の制御構造および制御フローを示しています。 非同期メソッドの結果は、Web ページの文字数です。 このコードは、Visual Studio で Windows Presentation Foundation (WPF) アプリまたは Windows ストア アプリを作成する場合に適しています。アプリの設定に関するコード内のコメントを参照してください。  

このコードは、Visual Studio で、Windows Presentation Foundation (WPF) アプリまたは Windows ストア アプリとして実行できます。 `StartButton` という名前のボタン コントロールと、`ResultsTextBox` という名前のテキストボックス コントロールが必要です。 次のように、名前とハンドラーを必ず設定してください。  

```xaml
<Button Content="Button" HorizontalAlignment="Left" Margin="88,77,0,0" VerticalAlignment="Top" Width="75"  
        Click="StartButton_Click" Name="StartButton"/>  
<TextBox HorizontalAlignment="Left" Height="137" Margin="88,140,0,0" TextWrapping="Wrap"
         Text="&lt;Enter a URL&gt;" VerticalAlignment="Top" Width="310" Name="ResultsTextBox"/>  
```
  
WPF アプリとしてコードを実行するには:  

- 次のコードを、MainWindow.xaml.cs の `MainWindow` クラスに貼り付けます。  
- System.Net.Http に対する参照を追加します。  
- System.Net.Http に対する `using` ディレクティブを追加します。  
  
Windows ストア アプリとしてコードを実行するには:  

- 次のコードを、MainPage.xaml.cs の `MainPage` クラスに貼り付けます。  
- System.Net.Http と System.Threading.Tasks に対する using ディレクティブを追加します。  
  
[!code-csharp[wpf-async](../../../../samples/snippets/csharp/language-reference/keywords/async/wpf/mainwindow.xaml.cs#1)]
  
> [!IMPORTANT]
> タスクの詳細、およびタスクを待機している間に実行されるコードの詳細については、「[Async および Await を使用した非同期プログラミング](../../programming-guide/concepts/async/index.md)」をご覧ください。 同様の要素を使用する WPF 例の完全版については、「[チュートリアル: Async と Await を使用した Web へのアクセス](../../programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)」をご覧ください。  
  
## <a name="return-types"></a>戻り値の型  
非同期メソッドの戻り値の型を次に示します。

- <xref:System.Threading.Tasks.Task>
- <xref:System.Threading.Tasks.Task%601>
- [void](../builtin-types/void.md)。 `async void` メソッドは、呼び出し元でそれらのメソッドを `await` できず、正常終了またはエラー状態を報告するために別のメカニズムを実装する必要があるため、一般に、イベント ハンドラー以外のコードには推奨されません。
- C# 7.0 以降、アクセス可能な `GetAwaiter` を持つ任意の型です。 `System.Threading.Tasks.ValueTask<TResult>` 型はこの実装例で、 NuGet パッケージ `System.Threading.Tasks.Extensions` を追加することで使用できます。

非同期メソッドでは [in](./in-parameter-modifier.md)、[ref](./ref.md)、[out](./out-parameter-modifier.md) パラメーターを宣言できません。また、[参照戻り値](../../programming-guide/classes-and-structs/ref-returns.md)を指定することもできません。ただし、これらのパラメーターを持つメソッドを呼び出すことはできます。  
  
メソッドの `Task<TResult>`return[ ステートメントで ](./return.md) 型のオペランドを指定している場合、非同期メソッドの戻り値の型として `TResult` を指定します。 メソッドの完了時に意味のある値を返さない場合は、`Task` を使用します。 これにより、メソッドの呼び出しでは `Task` が返されますが、`Task` の完了時に、`await` を待機している `Task` 式はすべて、`void` に評価されます。  
  
戻り値の型 `void` は主として、その戻り値の型が要求されるイベント ハンドラーの定義に使用されます。 `void` を返す非同期メソッドの呼び出し元は、このメソッドを待機できず、このメソッドがスローする例外をキャッチできません。  

C# 7.0 以降、`GetAwaiter` メソッドを持つ別の型 (通常は値の型) を返して、コードのパフォーマンスが重要なセクションでメモリ割り当てを最小限に抑えます。

使用例を含む詳細については、「[非同期の戻り値の型](../../programming-guide/concepts/async/async-return-types.md)」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.CompilerServices.AsyncStateMachineAttribute>
- [await](../operators/await.md)
- [チュートリアル: Async と Await を使用した Web へのアクセス](../../programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Async および Await を使用した非同期プログラミング](../../programming-guide/concepts/async/index.md)
