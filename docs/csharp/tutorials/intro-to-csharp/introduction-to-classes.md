---
title: クラスおよびオブジェクト - C# チュートリアルの概要
description: 初めての C# プログラムを作成し、オブジェクト指向の概念を確認します
ms.date: 10/11/2017
ms.custom: mvc
ms.openlocfilehash: b6ad72997647b80b981f1a1871e384791404bdf7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156594"
---
# <a name="explore-object-oriented-programming-with-classes-and-objects"></a>クラスおよびオブジェクトを使用したオブジェクト指向プログラミングについて確認します

このチュートリアルでは、開発用に使用できるマシンがあることを想定しています。 Windows、Linux、または macOS 上でローカルの開発環境を設定する手順については、.NET チュートリアル [Hello World in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (10 分で Hello World) に記載されています。 使用するコマンドの概要については、[開発ツールの概要](local-environment.md)のページと詳細へのリンクをご覧ください。

## <a name="create-your-application"></a>アプリケーションの作成

ターミナル ウィンドウで、「*classes*」という名前のディレクトリを作成します。 ここにアプリケーションを構築します。 このディレクトリに移動し、コンソール ウィンドウで「`dotnet new console`」と入力します。 このコマンドにより、アプリケーションが作成されます。 *Program.cs* を開きます。 内容は次のようになります。

```csharp
using System;

namespace classes
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

このチュートリアルでは、銀行口座を表す新しい型を作成します。 通常、開発者は各クラスを別々のテキスト ファイルで定義します。 この方法なら、プログラムのサイズが大きくなっても管理が容易です。 *classes* ディレクトリに「*BankAccount.cs*」という名前の新しいファイルを作成します。

このファイルには、***銀行口座***の定義が含まれます。 オブジェクト指向プログラミングでは、***クラス***の形式で型を作成することによりコードを整理します。 これらのクラスには、特定のエンティティを表すコードが含まれています。 `BankAccount` クラスは銀行口座を表します。 コードでは、メソッドとプロパティを使用した特定の操作を実装します。 このチュートリアルでは、銀行口座は次の動作をサポートします。

1. 銀行口座を一意に特定する 10 桁の数字をサポートしています。
1. 口座の名前、または所有者の名前を格納する文字列をサポートしています。
1. 残高を取得できます。
1. 預金を許可します。
1. 引き出しを許可します。
1. 初期残高は正の値である必要があります。
1. 引き出しによって残高が負の値になることはありません。

## <a name="define-the-bank-account-type"></a>銀行口座の型を定義する

動作を定義するクラスの基本を作成することから開始できます。 内容は次のようになります。

```csharp
using System;

namespace classes
{
    public class BankAccount
    {
        public string Number { get; }
        public string Owner { get; set; }
        public decimal Balance { get; }

        public void MakeDeposit(decimal amount, DateTime date, string note)
        {
        }

        public void MakeWithdrawal(decimal amount, DateTime date, string note)
        {
        }
    }
}
```

先に進む前に、構築したものを確認してみましょう。  `namespace` 宣言は、コードを論理的に整理する方法を提供します。 このチュートリアルで取り扱うコードは比較的小さいため、1 つの名前空間にすべてのコードを配置します。

`public class BankAccount` は、これから作成するクラスまたは型を定義します。 クラス宣言のあとにある `{` と `}` の内側はすべて、クラスの状態と動作を定義しています。 ***クラスには、5 つの***メンバー`BankAccount`があります。 最初の 3 つは***プロパティ***です。 プロパティはデータ要素であり、検証やその他の規則を適用するコードを持つことができます。 最後の 2 つは***メソッド***です。 メソッドは 1 つの機能を実行するコード ブロックです。 各メンバーの名前を確認すると、開発者がそのクラスの作用を把握するための十分な情報が得られます。

## <a name="open-a-new-account"></a>新しいアカウントを開く

実装する最初の機能は、銀行口座を開く機能です。 顧客が口座を開く場合、初期残高や口座の (1 名または複数名の) 所有者の情報を入力する必要があります。

`BankAccount` 型の新しいオブジェクトを作成すると、これらの値を割り当てる***コンストラクター***が定義されます。 ***コンストラクター***はクラスと同じ名前のメンバーです。 これは、そのクラス型のオブジェクトを初期化するために使用されます。 `BankAccount` 型に次のコンストラクターを追加します。

```csharp
public BankAccount(string name, decimal initialBalance)
{
    this.Owner = name;
    this.Balance = initialBalance;
}
```

[`new`](../../language-reference/operators/new-operator.md) を使用してオブジェクトを作成すると、コンストラクターが呼び出されます。 `Console.WriteLine("Hello World!");`Program.cs*の* の行を次のコードで置き換えます (`<name>` を自分の名前に置き換えます)。

```csharp
var account = new BankAccount("<name>", 1000);
Console.WriteLine($"Account {account.Number} was created for {account.Owner} with {account.Balance} initial balance.");
```

「`dotnet run`」と入力すると何が起こるか見てみましょう。  

口座番号が空であることに気付かれましたか? 次にこの問題を解決します。 口座番号はオブジェクトが作成されるときに割り当てられる必要があります。 しかし、それを作成する責任を呼び出し元に負わせるべきではありません。 `BankAccount` クラスのコードは、新しい口座番号の割り当て方を知っている必要があります。  そのための簡単な方法は、10 桁の数字で始めることです。 そして、新しい口座番号が作成されるごとに値を 1 増加します。 最後に、オブジェクトが作成されるときに現在の口座番号を格納します。

`BankAccount` クラスに次のメンバー宣言を追加します。

```csharp
private static int accountNumberSeed = 1234567890;
```

これがデータ メンバーです。 これは `private` であり、`BankAccount` クラス内のコードのみがこれにアクセスできます。 この方法により、プライベートな実装 (口座番号の生成方法) から (口座番号を持つなどの) パブリックな責任を分離できます。 `static` でもあるため、すべての `BankAccount` オブジェクトによって共有されます。 静的でない変数の値は `BankAccount` オブジェクトのインスタンスごとに一意です。 次の 2 行をコンストラクターに追加して、口座番号を割り当てます。

```csharp
this.Number = accountNumberSeed.ToString();
accountNumberSeed++;
```

「`dotnet run`」と入力して結果を表示します。

## <a name="create-deposits-and-withdrawals"></a>預金と引き出しを作成する

銀行口座クラスは、預金と引き出しを許可して正しく動作するようにする必要があります。 口座のすべてのトランザクションを記録する履歴を作成して、預金と引き出しを実装しましょう。 これには、単純にトランザクションごとに残高を更新する方法に比べていくつかのメリットがあります。 履歴は、すべてのトランザクションを監査して毎日の残高を管理するために使用できます。 必要に応じてすべてのトランザクションの履歴から残高を計算することにより、1 つのトランザクションの中で修正されたすべてのエラーが正しく残高に反映されて次の計算に使用されます。

トランザクションを表す新しい型を作成するところから始めましょう。 これは一切の責任を持たない単純型です。 いくつかのプロパティが必要になります。 「*Transaction.cs*」という名前の新しいファイルを作成します。 これに次のコードを追加します。

[!code-csharp[Transaction](~/samples/snippets/csharp/classes-quickstart/Transaction.cs)]

<xref:System.Collections.Generic.List%601> クラスに `Transaction` オブジェクトの `BankAccount` を追加しましょう。 次の宣言を追加します。

[!code-csharp[TransactionDecl](~/samples/snippets/csharp/classes-quickstart/BankAccount.cs#TransactionDeclaration)]

<xref:System.Collections.Generic.List%601> クラスでは、別の名前空間をインポートする必要があります。 *BankAccount.cs* の最初に次を追加します。

```csharp
using System.Collections.Generic;
```

`Balance` の報告方法を変更しましょう。  これは、すべてのトランザクションの値を合計することで確認できます。 `Balance` クラスの `BankAccount` の宣言を次のように変更します。

[!code-csharp[BalanceComputation](~/samples/snippets/csharp/classes-quickstart/BankAccount.cs#BalanceComputation)]

この例は、***プロパティ***の重要な一面を示しています。 これで、別のプログラマーが値を要求したときに残高が計算されるようになりました。 この計算処理は、すべてのトランザクションを列挙して、その合計値を現在の残高として提供します。

次に `MakeDeposit` メソッドと `MakeWithdrawal` メソッドを実装します。 これらのメソッドは、初期残高が正の値でなければならず、引き出し後の残高が負の値になってはいけない、という最後の 2 つの規則を適用します。

これにより、***例外***の概念が導入されます。 メソッドが作業を正常に完了できないことを示す標準的な方法は、例外をスローすることです。 例外の型とそれに関連付けられたメッセージがエラーを説明します。 `MakeDeposit` メソッドは、預金額が負の値になる場合に例外をスローします。 `MakeWithdrawal` メソッドは、引き出し額が負の値になる場合、または引き出しを適用した結果、残高が負の値になる場合に例外をスローします。

[!code-csharp[DepositAndWithdrawal](~/samples/snippets/csharp/classes-quickstart/BankAccount.cs#DepositAndWithdrawal)]

[`throw`](../../language-reference/keywords/throw.md) ステートメントが例外を**スロー**します。 現在のブロックの実行が終了し、コントロールによってコール スタックで最初に一致した `catch` ブロックに転送されます。 あとで `catch` ブロックを追加してこのコードをテストします。

残高を直接更新するのではなく、最初のトランザクションを追加するようにするため、コンストラクターを 1 か所変更する必要があります。 既に `MakeDeposit` メソッドは記述したので、このメソッドをコンストラクターから呼び出します。 完成したコンストラクターは次のようになります。

[!code-csharp[Constructor](~/samples/snippets/csharp/classes-quickstart/BankAccount.cs#Constructor)]

<xref:System.DateTime.Now?displayProperty=nameWithType> は、現在の日付と時刻を返すプロパティです。 `Main` メソッドにいくつかの預金と引き出しを追加することで、これをテストします。

```csharp
account.MakeWithdrawal(500, DateTime.Now, "Rent payment");
Console.WriteLine(account.Balance);
account.MakeDeposit(100, DateTime.Now, "Friend paid me back");
Console.WriteLine(account.Balance);
```

次に、残高が負の値になっているアカウントを作成してみることで、エラー条件のキャッチをテストします。

```csharp
// Test that the initial balances must be positive.
try
{
    var invalidAccount = new BankAccount("invalid", -55);
}
catch (ArgumentOutOfRangeException e)
{
    Console.WriteLine("Exception caught creating account with negative balance");
    Console.WriteLine(e.ToString());
}
```

[`try` と `catch` のステートメント](../../language-reference/keywords/try-catch.md)を使用して、例外をスローする可能性のあるコード ブロックをマークし、想定したエラーをキャッチします。 同じ方法で、残高が負の値になっている場合に例外をスローするコードをテストします。

```csharp
// Test for a negative balance.
try
{
    account.MakeWithdrawal(750, DateTime.Now, "Attempt to overdraw");
}
catch (InvalidOperationException e)
{
    Console.WriteLine("Exception caught trying to overdraw");
    Console.WriteLine(e.ToString());
}
```

ファイルを保存し、「`dotnet run`」と入力して試します。

## <a name="challenge---log-all-transactions"></a>課題 - すべてのトランザクションをログに記録する

このチュートリアルを完了すると、トランザクション履歴の `GetAccountHistory` を作成する `string` メソッドを記述できるようになります。 このメソッドを `BankAccount` 型に追加します。

[!code-csharp[History](~/samples/snippets/csharp/classes-quickstart/BankAccount.cs#History)]

これは、<xref:System.Text.StringBuilder> クラスを使用して、各トランザクションを 1 行で表す文を含んだ文字列をフォーマットします。 文字列をフォーマットするコードについては、このチュートリアルで先述しました。 新しい文字の 1 つは `\t` です。 これはタブを挿入して出力をフォーマットします。

次の行を追加して、*Program.cs* でテストします。

```csharp
Console.WriteLine(account.GetAccountHistory());
```

「`dotnet run`」と入力して結果を表示します。

## <a name="next-steps"></a>次のステップ

うまくいかない場合は、このチュートリアルのソースを [GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/classes-quickstart/)で確認できます。

これで、C# のチュートリアルの概要をすべて説明しました。 さらに詳しい情報については、その他の[チュートリアル](../index.md)を試してください。
