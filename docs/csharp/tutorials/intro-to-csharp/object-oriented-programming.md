---
title: オブジェクト指向プログラミング (C#)
description: C# は、抽象化、カプセル化、継承、ポリモーフィズムなど、オブジェクト指向プログラミングを完全にサポートします。
ms.date: 09/30/2020
ms.openlocfilehash: e6261e9018217094e0c3ce742ff0172b8559deaf
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624215"
---
# <a name="object-oriented-programming-c"></a>オブジェクト指向プログラミング (C#)

C# はオブジェクト指向言語です。 オブジェクト指向プログラミングで使用される 4 つの主要な手法は次のとおりです。

- "*抽象化*" とは、関連するプロパティ、メソッド、およびその他のメンバーのグループが 1 つの単位またはオブジェクトとして扱われることを意味します。
- "*カプセル化*" とは、型コンシューマーから不要な詳細を隠すことです。
- "*継承*" は、既存のクラスに基づいて新しいクラスを作成する機能です。
- "*ポリモーフィズム*" とは、同じプロパティまたはメソッドを異なる方法で実装している複数のクラスを、交換して使用できることです。

前のチュートリアルの [クラスの概要](introduction-to-classes.md)では、"*抽象化*" と "*カプセル化*" の両方について説明しました。 `BankAccount` クラスによって、銀行口座の概念に対する抽象化が提供されました。 その実装は、`BankAccount` クラスを使用したコードのいずれにも影響を与えずに変更できました。 `BankAccount`、`Transaction` クラスの両方で、これらの概念をコードで記述するために必要なコンポーネントのカプセル化が提供されます。

このチュートリアルでは、このアプリケーションを拡張し、"*継承*" と "*ポリモーフィズム*" を使用して新機能を追加します。 また、前のチュートリアルで学習した "*抽象化*" と "*カプセル化*" の手法を活用して、`BankAccount` クラスに機能を追加します。

## <a name="create-different-types-of-accounts"></a>さまざまな種類の口座を作成する

このプログラムを構築した後、あなたは機能を追加するように要求されます。 これは、銀行口座の種類が 1 つしかない場合に適しています。 時間の経過と共に、ニーズが変化し、関連する口座の種類が要求されます。

- 月末ごとに利息がつく、利息つき口座。
- 残高が負の値になる可能性のある与信枠。ただし、残高がある場合は、毎月利息を請求されます。
- 前払いのギフト カード口座。1 回の預金で始まり、支払いのみが可能です。 毎月初めに 1 回補充できます。

これらの各種口座はすべて、前のチュートリアルで定義した `BankAccount` クラスに似ています。 そのコードをコピーし、クラスの名前を変更して、変更を加えることができます。 この手法は短期的にはうまくいきますが、時間の経過と共に作業量が多くなります。 あらゆる変更を、影響を受けるすべてのクラス間でコピーする必要があります。

代わりに、前のチュートリアルで作成した `BankAccount` クラスからメソッドとデータを継承する、新しい銀行口座の種類を作成できます。 これらの新しいクラスでは、各種類に必要な特定の動作で `BankAccount` クラスを拡張できます。

```csharp
public class InterestEarningAccount : BankAccount
{
}

public class LineOfCreditAccount : BankAccount
{
}

public class GiftCardAccount : BankAccount
{
}
```

これらの各クラスは、共有の "*基底クラス*" である `BankAccount` クラスから、共有の動作を "*継承*" します。 その "*派生クラス*" ごとに、新しい異なる機能の実装を記述します。  これらの派生クラスには、`BankAccount` クラスで定義されているすべての動作が既に備わっています。

新しいクラスは、それぞれ別のソース ファイルに作成することをお勧めします。 [Visual Studio](https://visualstudio.com) では、プロジェクトを右クリックして *[クラスの追加]* を選択すると、新しいファイルに新しいクラスを追加できます。 [Visual Studio Code](https://code.visualstudio.com) では、 *[ファイル]* 、 *[新規]* の順に選択すると新しいソース ファイルを作成できます。 どちらのツールでも、クラスと一致するようにファイル名を指定します。*InterestEarningAccount.cs*、*LineOfCreditAccount.cs*、*GiftCardAccount.cs* です。

前述のサンプルのようなクラスを作成すると、どの派生クラスもコンパイルされないことがわかります。 コンストラクターによって、オブジェクトの初期化が行われます。 派生クラスのコンストラクターでは、派生クラスを初期化し、その派生クラスに含まれる基底クラスのオブジェクトを初期化する方法を指定する必要があります。 通常、適切な初期化は追加のコードなしで行われます。 `BankAccount` クラスでは、次のシグネチャを持つ 1 つのパブリック コンストラクターが宣言されます。

```csharp
public BankAccount(string name, decimal initialBalance)
```

コンストラクターを自分で定義した場合、コンパイラによって既定のコンストラクターが生成されることはありません。 これは、各派生クラスで明示的にこのコンストラクターを呼び出す必要があることを意味します。 基底クラスのコンストラクターに引数を渡すことができるコンストラクターを宣言します。  次のコードは、`InterestEarningAccount` のコンストラクターを示しています。

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="DerivedConstructor":::

この新しいコンストラクターのパラメーターは、基底クラスのコンストラクターのパラメーターと型と名前が一致しています。 `: base()` 構文を使用して、基底クラスのコンストラクターへの呼び出しを示すことができます。 複数のコンストラクターを定義するクラスもあります。この構文を使用することで、呼び出す基底クラスのコンストラクターを選択できます。 コンストラクターを更新したら、各派生クラスのコードを開発できます。 新しいクラスの要件は、次のように記述できます。

- 利息つき口座:
  - 月末の残高の 2% が振り込まれます。
- 与信枠:
  - 残高を負の値にすることができますが、与信限度額よりも絶対値を大きくすることはできません。
  - 月末の残高が 0 ではない場合は、毎月利息を請求されます。
  - 与信限度額を超えた引き出しごとに手数料が発生します。
- ギフト カード口座:
  - 毎月 1 回、月の最終日に指定した金額を補充できます。

これら 3 種類の口座には、すべて月末ごとに実行されるアクションがあることがわかります。 ただし、口座の種類ごとに異なるタスクが実行されます。 このコードを実装するために、"*ポリモーフィズム*" を使用します。 `BankAccount` クラスで 1 つの `virtual` メソッドを作成します。

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="DeclareMonthEndTransactions":::

前のコードでは、`virtual` キーワードを使用して、派生クラスで異なる実装を提供できる基底クラスのメソッドを宣言する方法を示しています。 `virtual` メソッドは、任意の派生クラスで再実装することを選択できるメソッドです。 派生クラスでは、`override` キーワードを使用して新しい実装を定義します。 通常、これは "基底クラスの実装のオーバーライド" と呼ばれます。 `virtual` キーワードによって、派生クラスで動作をオーバーライドする可能性があることを指定できます。 また、派生クラスで動作をオーバーライドしなければならない `abstract` メソッドを宣言することもできます。 基底クラスでは `abstract` メソッドの実装が提供されません。 次に、作成した 2 つの新しいクラスの実装を定義する必要があります。 まずは `InterestEarningAccount` です。

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="ApplyMonthendInterest":::

`LineOfCreditAccount` に次のコードを追加します。 このコードでは、残高の符号を反転させて、口座から引き出される正の利息請求額を計算しています。

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ApplyMonthendInterest":::

`GiftCardAccount` クラスには、月末の機能を実装するために 2 つの変更が必要です。 最初に、毎月追加できる省略可能な金額を含めるようにコンストラクターを変更します。

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="GiftCardAccountConstruction":::

このコンストラクターでは `monthlyDeposit` の値に対して既定値が指定されています。これにより、毎月の預金がない場合に呼び出し元は `0` を省略できます。 次に、`PerformMonthEndTransactions` メソッドをオーバーライドして、毎月の預金がコンストラクターで 0 以外の値に設定されていた場合にそれを追加するようにします。

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="AddMonthlyDeposit":::

このオーバーライドにより、コンストラクターで設定された毎月の預金が適用されます。 次のコードを `Main` メソッドに追加して、`GiftCardAccount` と `InterestEarningAccount` に対するこれらの変更内容をテストします。

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="FirstTests":::

結果を確認しましょう。 次に、`LineOfCreditAccount` に対して類似した一連のテスト コードを追加します。

```
    var lineOfCredit = new LineOfCreditAccount("line of credit", 0);
    // How much is too much to borrow?
    lineOfCredit.MakeWithdrawal(1000m, DateTime.Now, "Take out monthly advance");
    lineOfCredit.MakeDeposit(50m, DateTime.Now, "Pay back small amount");
    lineOfCredit.MakeWithdrawal(5000m, DateTime.Now, "Emergency funds for repairs");
    lineOfCredit.MakeDeposit(150m, DateTime.Now, "Partial restoration on repairs");
    lineOfCredit.PerformMonthEndTransactions();
    Console.WriteLine(lineOfCredit.GetAccountHistory());
 ```

上記のコードを追加してプログラムを実行すると、次のようなエラーが表示されます。

```console
Unhandled exception. System.ArgumentOutOfRangeException: Amount of deposit must be positive (Parameter 'amount')
   at OOProgramming.BankAccount.MakeDeposit(Decimal amount, DateTime date, String note) in BankAccount.cs:line 42
   at OOProgramming.BankAccount..ctor(String name, Decimal initialBalance) in BankAccount.cs:line 31
   at OOProgramming.LineOfCreditAccount..ctor(String name, Decimal initialBalance) in LineOfCreditAccount.cs:line 9
   at OOProgramming.Program.Main(String[] args) in Program.cs:line 29
```

> [!NOTE]
> 実際の出力には、プロジェクトを含むフォルダーへの完全なパスが含まれています。 簡潔にするため、フォルダー名は省略されています。 また、コード形式によっては、行番号が若干異なる場合があります。

このコードが失敗したのは、`BankAccount` によって初期残高は 0 より大きいと仮定されているためです。 `BankAccount` クラスに織り込まれているもう 1 つの前提は、残高を負の値にすることはできないということです。 代わりに、口座残高を超える引き出しは拒否されます。 これらの前提はどちらも変更する必要があります。 与信枠口座は 0 から始まり、通常は残高が負になります。 また、顧客がお金を借りすぎた場合、手数料が発生します。 そのトランザクションは承認されます。追加料金がかかるだけです。 最初の規則は、最小残高を指定する省略可能な引数を `BankAccount` コンストラクターに追加することによって実装できます。 既定では、 `0`です。 2 番目の規則には、派生クラスで既定のアルゴリズムを変更できるようにするメカニズムが必要です。 ある意味では、基底クラスが派生型に、過剰な引き出しがあった場合にどうするかを "尋ね" ます。 既定の動作では、例外をスローすることによってトランザクションを拒否します。

まず、省略可能な `minimumBalance` パラメーターを含む 2 番目のコンストラクターを追加しましょう。 この新しいコンストラクターでは、既存のコンストラクターで実行されるすべてのアクションが実行されます。 さらに、最小残高のプロパティも設定されます。 既存のコンストラクターの本体をコピーすることもできます。 ただし、今後は 2 か所を変更しなければならなくなります。 代わりに、"*コンストラクター チェーン*" を使用して、1 つのコンストラクターが別のコンストラクターを呼び出すようにすることができます。 次のコードは、2 つのコンストラクターと新しい追加フィールドを示しています。

 :::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="ConstructorModifications":::

上記のコードには、2 つの新しい手法が示されています。 まず、`minimumBalance` フィールドは `readonly` としてマークされています。 これは、オブジェクトを構築した後はこの値を変更できないことを意味します。 `BankAccount` の作成後に `minimumBalance` を変更することはできません。 次に、2 つのパラメーターを受け取るコンストラクターでは、実装として `: this(name, initialBalance, 0) { }` が使用されています。 `: this()` 式によって、3 つのパラメーターを持つ別のコンストラクターを呼び出します。 この手法を使用すると、オブジェクトを初期化するための実装を 1 つ用意して、クライアント コードでは多数のコンストラクターの中からいずれかを選択できるようにすることができます。

この実装では、初期残高が `0` よりも大きい場合にのみ `MakeDeposit` が呼び出されます。 これにより、預金は正である必要があるという規則を守りつつ、残高 `0` で与信枠口座を開設することができます。

これで `BankAccount` クラスに最小残高用の読み取り専用フィールドが設定されたので、最後の変更として、`MakeWithdrawal` メソッドのハードコーディングされた `0` を `minimumBalance` に変更します。

```csharp
if (Balance - amount < minimumBalance)
```

`BankAccount` クラスを拡張した後、次のコードに示すように、新しい基底コンストラクターを呼び出すように `LineOfCreditAccount` コンストラクターを変更できます。

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ConstructLineOfCredit":::

`LineOfCreditAccount` コンストラクターでは、`minimumBalance` パラメーターの意味と一致するように、`creditLimit` パラメーターの符号が変更されることがわかります。

## <a name="different-overdraft-rules"></a>過剰な引き出しに対する異なる規則

追加する最後の機能により、`LineOfCreditAccount` で、与信限度額の超過に対してトランザクションを拒否するのではなく、手数料を請求できるようになります。

1 つの手法は、必要な動作を実装する仮想関数を定義することです。 `BankAccount` クラスでは、`MakeWithdrawal` メソッドが 2 つのメソッドにリファクターされます。 新しいメソッドでは、引き出しによって残高が最小値を下回る場合に、指定されたアクションが実行されます。 既存の `MakeWithdrawal` メソッドには、次のコードが含まれています。

```csharp
public void MakeWithdrawal(decimal amount, DateTime date, string note)
{
    if (amount <= 0)
    {
        throw new ArgumentOutOfRangeException(nameof(amount), "Amount of withdrawal must be positive");
    }
    if (Balance - amount < minimumBalance)
    {
        throw new InvalidOperationException("Not sufficient funds for this withdrawal");
    }
    var withdrawal = new Transaction(-amount, date, note);
    allTransactions.Add(withdrawal);
}
```

見つかったコードを次のコードと置き換えます。

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="RefactoredMakeWithdrawal":::

追加されたメソッドは `protected` です。これは、派生クラスからのみ呼び出せることを意味します。 この宣言によって、他のクライアントがこのメソッドを呼び出せなくなります。 これは `virtual` でもあるため、派生クラスで動作を変更できます。 戻り値の型は `Transaction?` です。 `?` の注釈は、メソッドが `null` を返す可能性があることを示します。 次の実装を `LineOfCreditAccount` に追加して、引き出しの限度額を超えたときに手数料を請求します。

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="AddOverdraftFee":::

このオーバーライドでは、残高が過剰に引き出された場合に手数料のトランザクションが返されます。 引き出しが限度額を超えていない場合は、メソッドから `null` トランザクションが返されます。 これは手数料が発生していないことを示します。 `Program` クラスの `Main` メソッドに次のコードを追加して、これらの変更をテストします。

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="TestLineOfCredit":::

プログラムを実行し、結果を確認します。

## <a name="summary"></a>まとめ

うまくいかない場合は、このチュートリアルのソースを [GitHub リポジトリ](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/object-oriented-programming)で確認できます。

このチュートリアルでは、オブジェクト指向プログラミングで使用される多くの手法を示しました。

- 異なる種類の口座ごとにクラスを定義したとき、"*抽象化*" を使用しました。 これらのクラスによって、その種類の口座の動作が記述されました。
- 各クラスで多くの詳細情報を `private` にしたとき、"*カプセル化*" を使用しました。
- `BankAccount` クラスに既に作成されている実装を活用してコードを節約したとき、"*継承*" を使用しました。
- 派生クラスでオーバーライドしてその口座の種類に固有の動作を作成できる `virtual` メソッドを作成したとき、"*ポリモーフィズム*" を使用しました。

お疲れさまでした。これで、C# の概要に関する全チュートリアルを完了しました。 さらに学習する場合は、その他の[チュートリアル](../index.md)をお試しください。
